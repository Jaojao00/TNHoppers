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

### 6. 💎 5-Branch Interactive Upgrades GUI
Players can invest in-game economy or XP levels to upgrade 5 core stats:
1. ⚡ **Speed:** Reduces transfer delay from 20 ticks down to 15, 10, 5, or 2 ticks/transfer.
2. 📦 **Amount:** Increases items moved per transfer from 1 up to 4, 16, 32, or 64 items.
3. 🔗 **Max Links:** Expands simultaneous linked chest connections from 1 up to 15 chests.
4. 📏 **Distance & Radius:** Expands suction radius up to 16 blocks and wireless linking distance up to 250 blocks.
5. 🌌 **Virtual Deep Storage:** Expands virtual overflow buffer from **10,000** up to **50,000**, **100,000**, and **250,000** items!
* Fully supports **Vault Economy** with automatic fallback to **XP Levels**.

### 7. 🌌 Virtual Deep Storage Buffer (10k → 250k Items)
* **Never Lose Items:** When physical destination chests are full, items automatically route into a secure virtual buffer!
* **Upgradable Capacities:** Level 1: 10,000 items | Level 2: 50,000 items | Level 3: 100,000 items | Level 4: 250,000 items.
* **Auto-Drain Engine:** As soon as slots open up in physical chests, virtual items automatically drain back into inventory slots!
* **Deep Storage Viewer GUI:** Middle-click any linked chest or press Slot 47 in filter to inspect stored items and withdraw stacks anytime.
* **Anti-Loss Safeguard:** Unlinking a container is blocked if virtual items are still stored in its buffer.

### 8. 🛡️ Active Break & Explosion Protection
* **Accidental Break Protection:** Linked hoppers and linked destination containers (Chests, Double Chests, Barrels, Shulkers) cannot be broken by players while linked. Players receive a warning sound and message requiring them to unlink in the GUI first.
* **Explosion Immunity:** Complete immunity against TNT, Creeper, and Wither explosions for all active linked hoppers and destination chests.
* **SuperiorSkyblock2 Ready:** Dynamic permission validation (BUILD & CHEST privileges), cross-island lockdown, and instant freeze on member kick/ban.
* **NoOp Fallback:** Works seamlessly as a standalone plugin on non-Skyblock servers (Survival, SMP, Towny).

### 9. 🚀 Rock-Solid Technical Architecture
* **PDC Level Preservation:** When unlinked hoppers are broken, they drop with all upgrade levels and hopper type saved inside `PersistentDataContainer`. Placing it back restores all stats immediately!
* **Zero-Allocation Live View (`activeHoppersView()`):** Traverses active hoppers with a weakly consistent iterator, eliminating garbage collection spikes.
* **Symmetrical Transactional Rollback:** Both container slot arrays and world entity item stacks are snapshot-protected against unexpected server lag or event cancellation.
* **SQLite WAL Single-Writer:** Asynchronous single-writer queue with 5000ms busy timeout and `PRAGMA wal_checkpoint(TRUNCATE)` shutdown verification.
* **24 Automated Unit Tests:** 100% verified test coverage for mass conservation, virtual deep storage overflow, and cache consistency.

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
    storage-base: 5000.0

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

## 📋 Dependencies & System Requirements

### ⚡ Required Dependencies:
* **Java 21+ Runtime:** [Eclipse Temurin Java 21](https://adoptium.net/temurin/releases/?version=21) or Oracle Java 21.
  > **Note on Minecraft 1.20:** TNHoppers is compiled using Java 21 LTS bytecode (records, modern pattern matching). While vanilla 1.20.0–1.20.4 initially defaulted to Java 17, Paper/Purpur 1.20 runs smoothly on Java 21. Ensure your server startup command points to a Java 21+ JRE.
* **Server Software:** [Paper](https://papermc.io/downloads/paper), [Purpur](https://purpurmc.org/), or Folia for Minecraft 1.20 – 1.21.x.

### 🔌 Optional Integrations (Soft Dependencies):
* **[Vault](https://www.spigotmc.org/resources/vault.34315/) (Economy):** Enables purchasing hopper upgrades with server currency. *If Vault is absent, TNHoppers automatically falls back to vanilla player XP levels.*
* **[SuperiorSkyblock2](https://bg-software.com/superiorskyblock/) (Island Protection):** Enables island boundary checks, BUILD & CHEST permission validation, and anti-theft cross-island protection. *If not installed, TNHoppers operates in standalone NoOp mode for Survival/SMP.*

---

## 🔨 Installation Guide

1. Ensure your server is running on **Java 21+** and **Paper 1.20 - 1.21.x**.
2. Download `TNHoppers-0.6.2.jar`.
3. Drop it into your server's `plugins/` directory.
4. *(Optional)* Install **Vault** and/or **SuperiorSkyblock2**.
5. Restart your server. Everything will generate automatically!

---

## 📞 Support & Community

* 💬 **Discord Support:** [https://discord.gg/kwqU3N4nkH](https://discord.gg/kwqU3N4nkH)
* 📧 **Author Email:** [tainguyenhr.dev@gmail.com](mailto:tainguyenhr.dev@gmail.com)
* 📖 **Official Wiki:** [Read Full Documentation](https://github.com/Jaojao00/TNHoppers/blob/main/WIKI.md)
* 📦 **GitHub Releases:** [Download Latest Builds](https://github.com/Jaojao00/TNHoppers/releases)

---

**© 2026 Tài Nguyễn (TN) — All Rights Reserved.**
