# Tekkit Reborn — Server Website & Kit Store

A one-page website for a **Tekkit The Resurrection 2** (Forge 1.20.1) Minecraft server, with a kit store that delivers items straight into players' inventories.

- Plain HTML/CSS/JS — no framework, no build step, no Base44.
- Hosted free on **GitHub Pages**.
- Payments + in-game delivery handled by **Tebex** (the standard Minecraft server store platform; it has a Forge plugin).
- Live online/player count via the free mcsrvstat.us API.

## How the pieces fit

```
Player on website ──► picks kit ──► Tebex checkout (pays)
                                         │
                                         ▼
                    Tebex Forge plugin on your server runs
                    the commands you set, e.g. /give {username} ...
                                         │
                                         ▼
                              Items appear in-game
```

The website never touches money or card data — Tebex does.

## Setup (about 30 minutes)

### 1. Edit the config
Open `index.html` and change the `CONFIG` and `KITS` blocks near the bottom: server name, IP, Discord link, kit names, prices and contents.

### 2. Create your Tebex store
1. Sign up at <https://tebex.io> and create a webstore for **Minecraft: Java Edition**.
2. Set up payouts (PayPal/Stripe) in the Tebex panel.
3. Put your store address in `CONFIG.tebexStore` (e.g. `https://tekkitreborn.tebex.io`).

### 3. Install the Tebex plugin on your server
1. In the Tebex panel go to **Game Servers → Connect** and copy the secret key.
2. Download the **Tebex Forge** build that matches **1.20.1** and drop the `.jar` into your server's `mods/` folder.
3. Restart, then run in the server console: `tebex secret <your-key>`

### 4. Make one Tebex package per kit
For each kit: **Packages → Create**. Set the price, then add commands that run when the player is online. Examples:

```
give {username} minecraft:iron_pickaxe 1
give {username} minecraft:iron_chestplate 1
give {username} minecraft:torch 64
give {username} minecraft:cooked_beef 32
tellraw {username} {"text":"Thanks for supporting the server! Your kit has been delivered.","color":"gold"}
```

For modded items, hold the item in-game and press **F3 + H** to show advanced tooltips — the item ID (like `modname:item_name`) appears under the name. Use that in the `give` command.

Settings worth using on each package:
- **Require player to be online** → on (so items don't vanish).
- **Purchase limit / expiry** → match the "Limit" shown on the website card.

Copy each package's numeric ID into the matching kit's `tebexId` in `index.html`. Until a kit has an ID, its Buy button tells the player checkout isn't connected yet.

### 5. Publish on GitHub Pages
Repo **Settings → Pages → Source: Deploy from branch → `main` / root**. Your site appears at `https://<username>.github.io/<repo>/`. You can point a custom domain at it later.

## Stay within Mojang's rules
Minecraft's Usage Guidelines allow servers to sell things that affect gameplay **only if** they don't give a competitive advantage or make other players' experience worse. Kits that save early-game time and contain items anyone can earn are the usual safe approach; avoid selling end-game gear that can't be obtained normally, or anything that beats other players in PvP. Read the current text yourself before launching: <https://www.minecraft.net/en-us/usage-guidelines>.

## Files
| File | What it is |
|---|---|
| `index.html` | The whole site — styles, store, and the config block |
| `README.md` | This guide |
