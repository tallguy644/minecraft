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

The site talks to Tebex's **Headless API** right from the browser: it loads your packages live (so prices and kit contents are managed in Tebex, not in code), signs the player in with their Minecraft username, and sends them to Tebex's checkout. Until a token is set, it shows sample kits as a preview.

### 1. Edit the basics
In `index.html`, near the bottom, set `serverName`, `serverIp` and `discord` in the `CONFIG` block.

### 2. Create your Tebex store
1. Sign up at <https://tebex.io> and create a webstore for **Minecraft: Java Edition**.
2. Set up payouts in the Tebex creator panel.
3. Find your store's **Public Token** on the API keys page of the creator panel and paste it into `CONFIG.tebexToken`.
   Use the **public** token only — never put your private/secret key in this file, because the site is public.

### 3. Connect your Forge server
1. In the creator panel, add a game server and copy its **secret key**.
2. Download the Tebex build for **Forge 1.20.1** (Tebex's official plugin download; if there isn't an official Forge build for your version, the community **Tebex Forged** mod on CurseForge fills the gap). Put the `.jar` in your server's `mods/` folder.
3. Restart, then run in the server console: `tebex secret <your-secret-key>`

### 4. Make one package per kit
For each kit: **Packages → Create**. Its category becomes the filter on the site.

- **Description** — write a short sentence, then a bullet list of what's inside. The site shows the sentence as the blurb and the bullets as the item list.
- **Image** (optional) — shown as the kit's icon.
- **Commands** — what runs in-game when it's paid for, e.g.:

```
give {username} minecraft:iron_pickaxe 1
give {username} minecraft:iron_chestplate 1
give {username} minecraft:torch 64
give {username} minecraft:cooked_beef 32
tellraw {username} {"text":"Thanks for supporting the server! Your kit has been delivered.","color":"gold"}
```

For modded items, hold the item in-game and press **F3 + H** to show advanced tooltips — the item ID (like `modname:item_name`) appears under the name. Use that in the `give` command.

Settings worth using on each package:
- **Require player to be online** → on, so items don't drop into an empty world.
- **Purchase limit / expiry** → if you want one-time or monthly kits.

### Test before going live
Make a cheap test package (or use Tebex's test/sandbox options), buy it from the site, and confirm the items appear in-game.

### How checkout works for players
1. Click **Buy** on a kit, tick the terms box.
2. Tebex asks for their Minecraft username (once per basket).
3. They land on Tebex's checkout with the kit in the basket and pay.
4. Tebex sends them back to the site with a thank-you message, and the Forge plugin delivers the kit when they're online.

### 5. Publish on GitHub Pages
Repo **Settings → Pages → Source: Deploy from branch → `main` / root**. Your site appears at `https://<username>.github.io/<repo>/`. You can point a custom domain at it later.

## Stay within Mojang's rules
Minecraft's Usage Guidelines allow servers to sell things that affect gameplay **only if** they don't give a competitive advantage or make other players' experience worse. Kits that save early-game time and contain items anyone can earn are the usual safe approach; avoid selling end-game gear that can't be obtained normally, or anything that beats other players in PvP. Read the current text yourself before launching: <https://www.minecraft.net/en-us/usage-guidelines>.

## Files
| File | What it is |
|---|---|
| `index.html` | The whole site — styles, store, and the config block |
| `README.md` | This guide |
