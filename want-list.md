**Claude Commands have since merged with Claude Skills. But this document keeps old naming: Commands. Feel free to copy this document into claude and ask it to update any command reference and folder name to skills. 
---
description: Save a product you want to your Notion Want List — works with or without a browser
allowed-tools: mcp__notion__notion-create-pages, WebFetch
---

# 🛍️ Want List — Claude Code Command

Save any product to your Notion Want List in seconds. Works two ways:

| Mode | How to launch | How it works |
|------|--------------|--------------|
| With Chrome | `claude --chrome` | Reads the product page you have open automatically |
| Without Chrome | `claude` | Paste a URL and Claude fetches the page for you |

---

## Installation

**1. Create the commands directory** in your project folder if it doesn't exist:
```
.claude/commands/
```

**2. Save this file** as `want-list.md` inside that folder:
```
your-project/.claude/commands/want-list.md
```

**3. Use it** — open Claude Code from your project folder and type:
```
/want-list
```

> The filename becomes the command — `want-list.md` → `/want-list`

---

## Setup (one time)

### Step 1 — Connect Notion MCP

```bash
claude mcp add
```

Follow the prompts to connect Notion. You only need to do this once.

### Step 2 — Create your Notion database

Once Notion MCP is connected, just ask Claude to build it for you:

> *"Create a Notion database called 🛍️ Want List with these columns: Product Name (Title), Brand (Text), URL (URL), Price (Text), Category (Select: Skincare, Clothing, Shoes, Accessories, Other), Date Added (Date), Did I Buy It? (Select: Not Yet, Yes 🛍️, Decided Against It ✅), Why I Want It (Text), Notes (Text)"*

Or create it manually in Notion with these columns:

| Column | Type | Options |
|--------|------|---------|
| Product Name | Title | — |
| Brand | Text | — |
| URL | URL | — |
| Price | Text | — |
| Category | Select | Skincare, Clothing, Shoes, Accessories, Other |
| Date Added | Date | — |
| Did I Buy It? | Select | Not Yet, Yes 🛍️, Decided Against It ✅ |
| Why I Want It | Text | — |
| Notes | Text | — |

### Step 3 — Add your database info to this file (if you created your Notion database manually)

Replace the two placeholders below with your own values:

- `YOUR_COLLECTION_ID` — found by opening your database → `...` menu → Connections → Copy link. It's the string after `notion.so/` and before `?`
- `YOUR_NOTION_DATABASE_URL` — the full URL of your Notion database

---

## Command Logic

### Get product details

**With Chrome:** reads the current browser page automatically.  
**Without Chrome:** prompts for a product URL, then fetches the page via WebFetch.

Extracts:
- **Product Name** — full name as listed on the page
- **Brand** — brand or company name
- **URL** — exact page URL
- **Price** — listed price (e.g. `$64.00`)
- **Category** — closest match from: Skincare, Clothing, Shoes, Accessories, Other

If `$ARGUMENTS` is provided, use it as additional context (e.g. a colorway, variant, or URL).

### Save to Notion

Uses `notion-create-pages` to add a new row to the Want List database.

Data source ID: `collection://YOUR_COLLECTION_ID`

| Field | Value |
|-------|-------|
| `Product Name` | Full product name |
| `Brand` | Brand name |
| `userDefined:URL` | Page URL |
| `Price` | Price as a string |
| `Category` | Skincare / Clothing / Shoes / Accessories / Other |
| `date:Date Added:start` | Today's date (YYYY-MM-DD) |
| `date:Date Added:is_datetime` | 0 |
| `Did I Buy It?` | Not Yet |
| `Why I Want It` | *(leave blank)* |
| `Notes` | *(leave blank)* |

### Confirm

```
✅ Added to your Want List:
   Product: [product name]
   Brand:   [brand]
   Price:   [price]
   Notion:  YOUR_NOTION_DATABASE_URL
```

---

## Tips

- If the price isn't found on the page, it will be left blank and flagged in the confirmation
- On collection pages with multiple products, Claude will ask which one to save
- Without Chrome, you can pass the URL directly: `/want-list https://example.com/product`
