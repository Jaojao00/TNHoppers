[![Version](https://img.shields.io/badge/Version-v0.6.2%20Release-blue.svg)](https://github.com/Jaojao00/TNHoppers/releases) [![Paper](https://img.shields.io/badge/Paper-1.20%20--%201.21.x-red.svg)](https://papermc.io) [![Java](https://img.shields.io/badge/Java-21%2B-orange.svg)](https://www.java.com) [![Discord](https://img.shields.io/badge/Discord-Support-5865F2.svg?logo=discord&logoColor=white)](https://discord.gg/kwqU3N4nkH)

---

## 🌪️ What is TNHoppers?

Are traditional vanilla hoppers dragging your server's **TPS** down and inflating **MSPT** into the red zone? Do massive mob farms, sugarcane towers, and item sorters with hundreds of hoppers lag your Skyblock or Survival realm?

**TNHoppers** is a high-performance, next-generation **Wireless Hopper & Suction System** built from the ground up for modern **Paper and Purpur 1.20 - 1.21.x** servers. Designed by **Tài Nguyễn (TN)**, it completely eliminates laggy vanilla hopper chains by routing items wirelessly through a budgeted, event-driven engine backed by a zero-allocation live view cache and SQLite WAL storage.

---

## 🔥 Key Features at a Glance

### 1. ⚡ 2 Specialized Hopper Types
* 🔸 **Normal Hopper (Radius Mode):** Collects free-falling ground items within a customizable spherical radius (upgradable from **3 to 16 blocks**). Perfect for compact farms, mob grinders, and private storage rooms.
* 🔹 **Chunk Hopper (Full-Chunk Vacuum):** Instantly vacuums **EVERY** dropped item across the entire **16×16 chunk** from bedrock to sky limit! Runs with a chunk-wide entity cache that calls `getEntities()` exactly once per tick cycle regardless of how many hoppers reside in that chunk.

### 2. 🪄 Wireless Link Wand & Smart Routing
* Link hoppers directly to target containers (Chests, Trapped Chests, Barrels, Shulkers) using the **Link Wand** (`/tnhoppers wand`).
* **Double Chest Normalization:** Automatically resolves large double chests to their canonical master tile, preventing accidental double-registration or dupe exploits.
* **Two Transfer Modes:**
  * `INSERT_BY_ORDER`: Fills linked chests in sequential priority order.
  * `DISTRIBUTE_EQUALLY`: Distributes incoming items in a fair round-robin cycle across all linked containers.

### 3. 🧙‍♂️ Brand New in v0.6.2: Auto-Sorter Wizard (Slot 48)
* Say goodbye to manually configuring item filters chest-by-chest!
* Simply prepare your sample items in your player inventory (Hotbar slots 1–9).
* Click the **Hopper Minecart** button in the Links GUI:
  * **Left-Click:** Automatically assigns a **Whitelist** filter to each linked chest in order, and **automatically renames each chest** to a clean Vietnamese/English item title (e.g. `Raw Iron`, `Lapis Lazuli`, `Diamond`)!
  * **Right-Click:** Automatically assigns a **Blacklist** filter to each linked chest.
* **100% Non-Mutating:** Player inventory is completely read-only. Includes overwrite confirmation prompts and automatic rollback if database persistence fails.

### 4. 🏷️ Custom Chest Renaming (Slot 47)
* Assign friendly names to your linked chests (e.g. `Ore Chest`, `Farm Drops`, `Trash Bin`) directly in chat or via Shift-Click in GUI.

### 5. 📦 Visual Per-Link GUI Filtering (54-Slot Chest)
* **Whitelist & Blacklist Modes:** Click any linked chest in the GUI to open a full 54-slot container. Drop item samples right in!
* **Strict ItemMeta Matching:** Toggle strict mode to accurately match custom display names, lore, enchantments, and potion effects.
* **Filter Clipboard (Copy & Paste):** Copy a filter profile with 1 click and paste it across multiple destination chests instantly.
* **Auto-Void Unmatched:** Enable the trash-voiding bucket (Slot 46) to automatically destroy unmatched items (e.g. rotten flesh or poison potatoes) so hoppers never clog!

### 6. 💎 4-Branch Interactive Upgrades GUI
Players can invest in-game economy or XP levels to upgrade 4 core stats:
1. ⚡ **Speed:** Reduces transfer delay from 20 ticks down to 15, 10, 5, or 2 ticks/transfer.
2. 📦 **Amount:** Increases items moved per transfer from 1 up to 4, 16, 32, or 64 items.
3. 🔗 **Max Links:** Expands simultaneous linked chest connections from 1 up to 15 chests.
4. 📏 **Distance & Radius:** Expands suction radius up to 16 blocks and wireless linking distance up to 250 blocks.
* Fully supports **Vault Economy** with automatic fallback to **XP Levels**.

### 7. 🛡️ Deep Island Protection (SuperiorSkyblock2 Ready)
* Checks island permissions dynamically: players must hold `BUILD` privileges at the hopper and `CHEST` privileges at the destination container.
* **Cross-Island Lockdown:** Blocks players from routing items across island boundaries.
* **Real-time Member Validation:** If a player is kicked or banned from an island, their hoppers immediately freeze.
* **NoOp Fallback:** Works seamlessly as a standalone plugin on non-Skyblock servers (Survival, SMP, Towny).

### 8. 🚀 Rock-Solid Technical Architecture
* **PDC Level Preservation:** When hoppers are broken or exploded by TNT/Creeper, they drop with all upgrade levels and hopper type saved inside `PersistentDataContainer`. Placing it back restores all stats immediately!
* **Zero-Allocation Live View (`activeHoppersView()`):** Traverses active hoppers with a weakly consistent iterator, eliminating garbage collection spikes.
* **Symmetrical Transactional Rollback:** Both container slot arrays and world entity item stacks are snapshot-protected against unexpected server lag or event cancellation.
* **SQLite WAL Single-Writer:** Asynchronous single-writer queue with 5000ms busy timeout and `PRAGMA wal_checkpoint(TRUNCATE)` shutdown verification.
* **19 Automated Unit Tests:** 100% verified test coverage for mass conservation and cache consistency.

---

## 💻 Commands & Permissions

| Command | Usage | Description | Required Permission | Default |
| :--- | :--- | :--- | :---: | :---: |
| `/tnhoppers` | `/tnhoppers [menu]` | Opens the Interactive Control Panel GUI | `tnhoppers.use` | Everyone (`true`) |
| `/tnhoppers get` | `/tnhoppers get <normal\|chunk>` | Gives a TNHoppers item with PDC metadata | `tnhoppers.admin` | OP |
| `/tnhoppers wand` | `/tnhoppers wand` | Gives a Link Wand (60-second session) | `tnhoppers.admin` | OP |
| `/tnhoppers reload` | `/tnhoppers reload` | Hot-reloads `config.yml` | `tnhoppers.admin` | OP |

---

## ⚙️ Configuration Preview (`config.yml`)

```yaml
storage:
  sqlite-file: "tnhoppers.db"
  busy-timeout-ms: 5000
  journal-mode: "WAL"
  synchronous: "NORMAL"

wireless:
  enabled: true
  default-max-links: 1
  default-max-distance: 15
  default-mode: "INSERT_BY_ORDER"

transfer:
  check-interval-ticks: 2
  interval-ticks: 20
  max-items-per-cycle: 1
  max-hoppers-per-cycle: 512

suction:
  enabled: true
  mode: "RADIUS"
  radius: 3.0
  interval-ticks: 10
  max-hoppers-per-cycle: 128
  max-entities-per-cycle: 128
  max-items-per-cycle: 64
  respect-item-owner: true

upgrades:
  purchases-enabled: true
  costs:
    speed-base: 1000.0
    amount-base: 2500.0
    links-base: 1500.0
    distance-base: 2000.0

integrations:
  superior-skyblock2:
    enabled: true
    require-island-to-place: true
    allow-cross-island-linking: false
    validate-island-membership-on-transfer: true
  vault:
    enabled: true
```

---

## 🔨 Installation Guide

1. Download `TNHoppers-0.6.2.jar`.
2. Drop it into your server's `plugins/` directory.
3. *(Optional but recommended)* Install **Vault** + Economy Provider (e.g. EssentialsX) and **SuperiorSkyblock2**.
4. Restart your server. Everything will generate automatically!

---

## 📞 Support & Community

* 💬 **Discord Support:** [https://discord.gg/kwqU3N4nkH](https://discord.gg/kwqU3N4nkH)
* 📧 **Author Email:** [tainguyenhr.dev@gmail.com](mailto:tainguyenhr.dev@gmail.com)
* 📖 **Official Wiki:** [Read Full Documentation](https://github.com/Jaojao00/TNHoppers/blob/main/WIKI.md)
* 📦 **GitHub Releases:** [Download Latest Builds](https://github.com/Jaojao00/TNHoppers/releases)

---

**© 2026 Tài Nguyễn (TN) — All Rights Reserved.**
