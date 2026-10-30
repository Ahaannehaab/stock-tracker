# Outlet Stock Tracker

A single-page stock tracker for three café outlets (Jurong, Orchard, Tampines). It reads and saves stock counts and reorder levels in Supabase.

## Features

- A tab per outlet, with category filters and a "low items only" switch
- − / + and Save on each item for stock on hand and reorder level
- Items at or below their reorder level are marked **Low**
- **Order from suppliers**: the shortfall (reorder level minus stock on hand) totalled across all outlets, with a copy button

## How it works

`index.html` is the whole app: plain HTML, CSS and JavaScript with no build step. It signs people in with supabase-js (loaded from jsDelivr), then calls the Supabase REST API as that user:

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

## Sign-in and users

People sign in with a username and password through Supabase Auth. Behind the scenes a username like `apple` is the Auth email `apple@example.com`.

- Only people listed in `stock_tracker_members` can read or change the inventory. Everyone else, signed in or not, is refused by the database's row-level security.
- Admins see a **Users** tab where they can add someone with a starting password (via the `add_stock_tracker_user` function) or remove their access.
- New users are asked to change their starting password the first time they sign in. Anyone can change their own password from the header.

The publishable key in `index.html` is safe to publish: on its own it can't read or change anything.
