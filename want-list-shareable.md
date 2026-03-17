---
description: Save a product you want to your Notion Want List, grabbed directly from the current browser page
allowed-tools: mcp__notion__notion-create-pages, WebFetch
---

# Add to Want List

Save the product on the current browser page to your Notion Want List.

## Before using this command
You need to set up your own Notion database first:
1. Create a new database in Notion called "🛍️ Want List"
2. Add these columns:
   - Product Name (Title)
   - Brand (Text)
   - URL (URL)
   - Price (Text)
   - Category (Select: Skincare, Clothing, Shoes, Accessories, Other)
   - Date Added (Date)
   - Did I Buy It? (Select: Not Yet, Yes 🛍️, Decided Against It ✅)
   - Why I Want It (Text)
   - Notes (Text)
3. Replace YOUR_COLLECTION_ID below with your database's collection ID
4. Replace YOUR_NOTION_DATABASE_URL below with your database URL

How to find your collection ID:
- Open your Notion database
- Click the "..." menu → Connections → Copy link
- The ID is the string after "notion.so/" and before "?"

## Step 1 — Get product details from the page

Read the current browser page and extract:
- **Product Name** — full product name as listed
- **Brand** — the brand or company name
- **URL** — the exact current page URL
- **Price** — the listed price (e.g. "$64" or "$64.00")
- **Category** — pick the closest match: Skincare, Clothing, Shoes, Accessories, or Other

If $ARGUMENTS is provided, use it as additional context (e.g. a specific colorway or variant).

## Step 2 — Add to Notion

Use the notion-create-pages tool to add a new row to the Want List database.

Data source ID: `collection://YOUR_COLLECTION_ID`

Set these fields:
- `Product Name` → full product name
- `Brand` → brand name
- `userDefined:URL` → page URL
- `Price` → price as a string
- `Category` → one of: Skincare, Clothing, Shoes, Accessories, Other
- `date:Date Added:start` → today's date in YYYY-MM-DD format
- `date:Date Added:is_datetime` → 0
- `Did I Buy It?` → "Not Yet"
- `Why I Want It` → leave blank (fill in later)
- `Notes` → leave blank (fill in later)

## Step 3 — Confirm

Print a short confirmation in the terminal:

✅ Added to your Want List:
  Product: [product name]
  Brand: [brand]
  Price: [price]
  Notion: YOUR_NOTION_DATABASE_URL

## Notes
- If you can't find the price on the page, leave it blank and note it in the confirmation
- If the page has multiple products (e.g. a collection page), ask which specific product to save
