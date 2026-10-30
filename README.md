# Outlet Stock Tracker

A single-page stock tracker for three café outlets (Jurong, Orchard, Tampines). It reads and saves stock counts and reorder levels in Supabase.

## Features

- A tab per outlet, with category filters and a "low items only" switch
- − / + and Save on each item for stock on hand and reorder level
- Items at or below their reorder level are marked **Low**
- **Order from suppliers**: the shortfall (reorder level minus stock on hand) totalled across all outlets, with a copy button

## How it works

`index.html` is the whole app: plain HTML, CSS and JavaScript with no build step. It calls the Supabase REST API with the project's publishable key:

| Data | Supabase object |
|---|---|
| Outlets | `outlets` |
| Items and categories | `items` |
| Latest count per outlet and item | `current_stock` view |
| Saving stock | upsert into `stock_counts` (one count per outlet, item and day) |
| Saving reorder levels | update `outlet_items.reorder_level` |

The Supabase URL and key are set at the top of the `<script>` block in `index.html`.

## Deploy on Vercel

1. In Vercel, choose **Add New → Project** and import this repository.
2. Leave **Framework Preset** on **Other**. There's no build command and no output directory to set.
3. Deploy. Vercel serves `index.html` at the site root.

## Security

The publishable key is meant to be public, but the database's access rules currently let anyone with the key update stock counts and reorder levels. Anyone who can open the deployed site can therefore change the figures. Before sharing the link widely, add a staff login (Supabase Auth) and restrict the write rules to signed-in users.
