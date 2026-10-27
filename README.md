# 🌪️ TNHoppers — Hệ Thống Phễu Không Dây & Hút Đồ Tối Tân

> **Tác giả & Bản quyền:** © 2026 **Tài Nguyễn (TN)**  
> **Nền tảng hỗ trợ:** Paper / Purpur / Folia 1.20+ / 1.21.x  
> **Ngôn ngữ & Môi trường:** Java 21 / Maven 3.9+  
> **Giấy phép (License):** [MIT License](LICENSE) — Cam kết 100% mã nguồn mở, không làm rối / không obfuscate.

---

## 📖 I. GIỚI THIỆU TỔNG QUAN (INTRODUCTION)

### 1. Bối cảnh ra đời
Trong các máy chủ Minecraft hiện đại (đặc biệt là thể loại **Skyblock**, **Survival**, **Towny**), hệ thống đường ống phễu truyền thống (vanilla hoppers) luôn là một trong những nguyên nhân hàng đầu gây sụt giảm TPS và nghẽn luồng xử lý do hàng nghìn phễu liên tục quét và đẩy item mỗi tick. 

Các plugin phễu cũ thời kỳ Minecraft 1.8–1.13 (như `MFHoppers`) hiện nay đã quá lỗi thời: thường xuyên rò rỉ RAM (memory leaks), không hỗ trợ các tính năng NBT mới, không tương thích với Paper API hiện đại và thiếu cơ chế bảo vệ đảo nghiêm ngặt.

**TNHoppers** ra đời như một giải pháp thay thế toàn diện, được nghiên cứu và thiết kế kiến trúc hoàn toàn mới từ đầu bởi **Tài Nguyễn (TN)**. Plugin kế thừa ý tưởng phễu không dây tiện lợi nhưng xây dựng trên nền tảng kỹ thuật hiện đại nhất của Java 21 và Paper 1.21.x, hướng đến hiệu năng đỉnh cao và độ an toàn tuyệt đối cho các máy chủ công cộng.

### 2. Triết lý thiết kế cốt lõi
- **Chống lag cực đoan (Zero-Lag Engineering):** Thao tác chuyển đồ không dây diễn ra đồng bộ theo chu kỳ kiểm soát (budgeted ticks), không tạo chuỗi phễu vanilla kéo dài.
- **Không rò rỉ bộ nhớ (Zero Memory Leak):** Bộ đệm Chunk Cache chỉ lưu trữ tọa độ bất biến (`BlockPosition`), tuyệt đối không lưu giữ tham chiếu đối tượng nặng như `org.bukkit.World`, `Chunk` hay `Block`.
- **An toàn giao dịch (Atomic Inventory Transactions):** Cơ chế tính toán số lượng thực tế nhận được khi thêm đồ, chống hoàn toàn nguy cơ nhân bản (duplication) hoặc mất mát vật phẩm khi rương đầy hay server giật lag.
- **Tương thích bảo vệ đảo (Skyblock-Ready):** Tích hợp sâu với **SuperiorSkyblock2**, ngăn chặn mọi hành vi hút trộm hoặc chuyển đồ xuyên đảo trái phép.

---

## 🎮 II. CƠ CHẾ SỬ DỤNG (HOW TO USE & MECHANICS)

### 1. Phân loại Phễu & Cách nhận
Hệ thống cung cấp **2 dòng phễu chuyên biệt** tùy theo nhu cầu:
- 🔸 **Normal Hopper (Phễu thường):** Hút vật phẩm rơi tự do theo bán kính hình cầu (`RADIUS`). Bán kính mặc định từ 3 blocks và có thể nâng cấp mở rộng lên tới 16 blocks. Thích hợp cho các máy farm tự động nhỏ lẻ, xưởng chế tạo, kho phân loại.
- 🔹 **Chunk Hopper (Phễu Chunk):** Hút **TOÀN BỘ** vật phẩm rơi tự do trong phạm vi toàn bộ một Chunk (`16x16` từ tầng bedrock đến nóc thế giới). Thích hợp cho các cụm farm quái, farm đá đỏ, máy thu hoạch tự động quy mô siêu lớn.

**Cách nhận và tương tác:**
- **Lệnh nhận:** Gõ `/tnhoppers get <normal|chunk>` hoặc mở bảng điều khiển `/tnhoppers` để bấm nhận trực tiếp.
- **Bảo toàn cấp độ & Loại phễu (Level & Type Preservation):** Khi phễu bị đập vỡ bằng cúp (hoặc bị nổ bởi TNT/Creeper), phễu sẽ rơi ra vật phẩm đặc biệt lưu toàn bộ cấp độ nâng cấp và loại phễu (`NORMAL` hoặc `CHUNK`) trong `PersistentDataContainer (PDC)`. Khi đặt lại, phễu sẽ tự động khôi phục nguyên vẹn mọi cấp độ mà không bị mất!
- **Mở giao diện:** Người chơi có thể **Nhấp chuột phải HOẶC Nhấp chuột trái** vào phễu để mở menu quản lý. (Nếu muốn đập vỡ phễu, người chơi chỉ cần **Sneak (Ngồi xổm) + Chuột trái**).

---

### 2. Tạo liên kết không dây bằng Link Wand
1. **Lấy gậy liên kết:** Sử dụng lệnh `/tnhoppers wand` (hoặc lấy từ menu `/tnhoppers`) để nhận **Link Wand**.
2. **Chọn nguồn (Source):** Cầm Link Wand và **Nhấp chuột phải** vào phễu TNHoppers của bạn.
3. **Chọn đích (Destination):** Đi tới thùng chứa mong muốn (Rương, Rương bẫy, Thùng gỗ, Shulker...) và **Nhấp chuột phải** vào đó.
   - **Tự động chuẩn hóa rương đôi (Double Chest Normalization):** Tự động chọn nửa rương chính làm mốc, ngăn chặn liên kết trùng lặp 2 nửa của một rương.
   - **Giới hạn tự động:** Kiểm tra khoảng cách và số lượng liên kết cho phép (dựa trên cấp độ nâng cấp của phễu).
4. **Hết hạn an toàn:** Phiên liên kết tự động hủy sau **60 giây** nếu đổi đồ trên tay hoặc thoát game.

---

### 3. Quản lý Liên Kết & Cấu hình Bộ Lọc Riêng Từng Rương (Per-Link Filtering)
Trong menu chính của phễu, chọn mục **Quản lý liên kết (Links)**:
- **Xem danh sách rương đích:** Mỗi ô đại diện cho một rương đích đã kết nối (kèm thông tin thế giới, tọa độ và trạng thái bộ lọc).
- 🖱️ **Nhấp chuột TRÁI vào rương đích:** Mở giao diện **Cấu hình bộ lọc riêng cho rương đó**:
  - **Chế độ Whitelist (Danh sách trắng):** Chỉ cho phép các vật phẩm bạn đã đặt mẫu được chuyển vào rương này.
  - **Chế độ Blacklist (Danh sách đen):** Các vật phẩm trong danh sách sẽ bị chặn lại, không cho vào rương này.
  - **Lọc nghiêm ngặt (Strict NBT Filtering):** Bật tùy chọn này nếu muốn so khớp chính xác tên tùy biến, mô tả (lore), phù phép (enchantments) hoặc thuộc tính bình thuốc.
  - 📋 **Sao chép & Dán (Copy/Paste Filter):** Nhấp nút **Sao chép bộ lọc** để lưu cấu hình vào bộ nhớ tạm, sau đó sang rương đích khác bấm **Dán bộ lọc** để áp dụng ngay lập tức!
- 🖱️ **Nhấp chuột PHẢI vào rương đích:** Gỡ liên kết rương đích an toàn.
- 🔄 **Chế độ chuyển đồ (Slot 45):** Nhấp để chuyển đổi giữa:
  - `INSERT_BY_ORDER`: Ưu tiên lấp đầy rương đích theo thứ tự đã định sẵn.
  - `DISTRIBUTE_EQUALLY`: Chia đều luân phiên giữa các rương nhận.
- 🗑️ **Tự hủy rác không khớp (Slot 46 - Auto-Void Unmatched):**
  - **BẬT:** Tự động tiêu hủy các vật phẩm không khớp với bất kỳ bộ lọc rương nào để tránh làm kẹt phễu và sàn farm.
  - **TẮT:** Giữ lại các vật phẩm chưa phân loại trong 5 ô chứa của phễu.

---

### 4. Bảng nâng cấp phễu (Interactive Upgrades GUI)
Người chơi có thể đầu tư tiền tệ Vault hoặc XP Levels để cường hóa 4 nhánh chỉ số:
1. ⚡ **Tốc độ (Speed):** Giảm thời gian chờ giữa các lần chuyển đồ (từ 20 ticks xuống 15, 10, 5, 2 ticks/lần).
2. 📦 **Số lượng (Amount):** Tăng lượng vật phẩm chuyển mỗi chu kỳ (từ 1 item lên 4, 16, 32, 64 item/lần).
3. 🔗 **Số liên kết (Links):** Mở rộng số lượng rương kết nối cùng lúc (từ 1 rương lên 3, 5, 8, 15 rương).
4. 📏 **Khoảng cách / Bán kính (Distance & Radius):** 
   - Với **Normal Hopper**: Tăng khoảng cách liên kết rương (lên đến 250 blocks) VÀ mở rộng bán kính hút đồ rơi (lên đến 16 blocks).
   - Với **Chunk Hopper**: Tăng khoảng cách liên kết rương không dây tối đa.

---

### 5. Cơ chế hút đồ và điều hướng trực tiếp (Smart Direct Routing)
Khi phễu hút vật phẩm rơi tự do từ sàn farm:
- **Chuyển thẳng tới rương đích (Direct Routing):** Vật phẩm được chuyển **thẳng đến các rương đích phù hợp với bộ lọc** mà không cần qua bước trung gian ở kho đồ của phễu.
- **Tự hủy rác (Voiding):** Nếu tính năng `voidUnmatched` được bật, các vật phẩm không có rương nào nhận (ví dụ thịt thối, xương trong farm khoáng sản) sẽ bị xóa bỏ trực tiếp.
- **Đệm dự phòng:** Nếu không bật xóa rác và tất cả rương đích bị đầy, item sẽ được lưu an toàn vào 5 ô chứa của phễu.
- **Tôn trọng quyền đảo & quyền nhặt:** Tích hợp kiểm tra quyền SuperiorSkyblock2 và thời gian nhặt của người đánh rơi (`pickup-delay`).

---

### 6. Bảng điều khiển lệnh `/tnhoppers` (Command Menu)
Gõ lệnh `/tnhoppers` (hoặc `/tnhoppers menu`) để mở ngay bảng điều khiển tổng quan:
- 🌪️ **Lấy Phễu Thường (Normal Hopper):** Nhận 1 phễu hút theo bán kính.
- 🌐 **Lấy Phễu Chunk (Chunk Hopper):** Nhận 1 phễu hút toàn chunk 16x16.
- 🪄 **Lấy Link Wand:** Nhận gậy liên kết.
- 📜 **Danh Sách Phễu Đang Nạp:** Xem toàn bộ phễu đang hoạt động trong RAM, **nhấp chuột để Dịch Chuyển Tức Thời (Teleport)** đến kiểm tra phễu.
- ℹ️ **Trạng Thái Hệ Thống:** Xem phiên bản, cơ chế hoạt động, tình trạng module.
- 🔄 **Nạp Lại Cấu Hình (Reload):** Tải lại toàn bộ cấu hình `config.yml`.

---

## ⚙️ III. CHI TIẾT KỸ THUẬT & KIẾN TRÚC LÕI (TECHNICAL ARCHITECTURE)

```
                     ┌──────────────────────────────────────────────┐
                     │            TNHoppers Plugin Main             │
                     └──────┬────────────────────────────────┬──────┘
                            │                                │
            ┌───────────────▼──────────────┐  ┌──────────────▼──────────────┐
            │   Persistent Storage Layer   │  │    Realtime In-Memory       │
            │   (SQLite WAL Single-Writer) │  │  (ChunkHopperCache + PDC)   │
            └───────────────┬──────────────┘  └──────────────┬──────────────┘
                            │                                │
            ┌───────────────┴────────────────────────────────┴──────────────┐
            │                     Core Engine Subsystems                    │
            ├───────────────────────────────┬───────────────────────────────┤
            │    WirelessTransferEngine     │         SuctionEngine         │
            │  (Order / Distribute Modes)   │     (Radius / Chunk Modes)    │
            ├───────────────────────────────┼───────────────────────────────┤
            │         FilterMatcher         │        UpgradeManager         │
            │    (Strict NBT / Material)    │      (Vault & XP Fallback)    │
            ├───────────────────────────────┴───────────────────────────────┤
            │                   ProtectionAdapter Layer                     │
            │         (SuperiorSkyblock2 Realtime Permission Hook)          │
            └───────────────────────────────────────────────────────────────┘
```

### 1. Wireless Transfer Engine (Động cơ truyền tải)
- Chạy trên luồng chính định kỳ an toàn (`runTaskTimer`), tuyệt đối không đụng chạm đến inventory bất đồng bộ để ngăn ngừa crash server.
- **Hỗ trợ 2 chế độ điều phối:**
  - `INSERT_BY_ORDER`: Đổ đầy rương đích thứ nhất trước khi chuyển sang rương đích thứ hai theo thứ tự ưu tiên liên kết.
  - `DISTRIBUTE_EQUALLY`: Chia đều vật phẩm luân phiên xoay vòng cho tất cả các rương đích đã liên kết.
- **Chống vòng lặp vô hạn (Anti-Circular Loop):** Không bao giờ cho phép liên kết đích đến một khối `Material.HOPPER` khác, triệt tiêu hoàn toàn khả năng người chơi tạo vòng lặp phễu đẩy vòng tròn làm treo CPU.

### 2. Protection Adapter & Tích hợp SuperiorSkyblock2
Plugin sử dụng mô hình Adapter độc lập (`ProtectionAdapter`):
- Nếu máy chủ **có cài** SuperiorSkyblock2: Kích hoạt `SuperiorSkyblockAdapter`.
- Nếu máy chủ **không cài** SuperiorSkyblock2: Tự động chuyển sang `NoOpProtectionAdapter` (phễu vẫn vận hành trơn tru mà không yêu cầu bắt buộc dependency).
- **Xác thực quyền động theo thời gian thực:**
  - Lúc đặt phễu & tạo liên kết: Kiểm tra quyền `BUILD` và `CHEST`. Chặn triệt để liên kết xuyên giữa 2 đảo khác nhau (`allow-cross-island-linking: false`).
  - Trong quá trình truyền và hút đồ: Định kỳ kiểm tra lại tư cách thành viên của chủ phễu. Nếu người chơi bị đuổi khỏi đảo (kicked/banned khỏi đảo), toàn bộ phễu của người đó trên đảo sẽ lập tức ngừng hoạt động, không thể hút hay chuyển thêm bất kỳ vật phẩm nào.

### 3. Tối ưu hóa Suction Engine (Chi phí Chunk Mode)
- Ở chế độ `CHUNK`, plugin sử dụng bộ nhớ đệm `chunkEntitiesCache` tồn tại trong phạm vi chu kỳ tick. Dù một chunk có 50 chiếc phễu cùng hoạt động, lệnh lấy danh sách thực thể `chunk.getEntities()` chỉ được gọi **đúng 1 lần duy nhất** cho chunk đó, loại bỏ hoàn toàn chi phí tạo mảng rác (garbage collection overhead).
- Kết hợp ngân sách giới hạn (`max-entities-per-cycle`, `max-items-per-cycle`) giúp máy chủ duy trì ổn định 20.0 TPS ngay cả khi người chơi thả hàng loạt vật phẩm trong các máy farm quái quy mô lớn.

### 4. Hệ thống lưu trữ SQLite WAL & Chống nổ
- Áp dụng kết nối JDBC duy nhất (`single-writer executor`) kết hợp chế độ `WAL (Write-Ahead Logging)` và `busy_timeout 5000ms`, hàng đợi xử lý tối đa 4.096 tác vụ. Việc lưu trữ diễn ra mượt mà, không bao giờ gây khựng hình server.
- **Xử lý vụ nổ (Explosion Clean-up):** Khi phễu bị nổ bởi TNT hoặc Creeper, plugin chặn drop item phễu vanilla, dọn dẹp cơ sở dữ liệu SQLite, xóa cache và trả lại đúng phễu TNHoppers có gắn thẻ PDC cho người chơi.
- **Xử lý rương đích bị phá:** Khi một rương đích bị đập vỡ, toàn bộ các phễu đang trỏ tới rương đó sẽ tự động gỡ bỏ liên kết đích trong SQLite và cache.

---

## 💻 IV. LỆNH & QUYỀN HẠN (COMMANDS & PERMISSIONS)

| Lệnh | Cú pháp | Mô tả chi tiết | Quyền hạn yêu cầu | Mặc định |
| :--- | :--- | :--- | :---: | :---: |
| `/tnhoppers` | `/tnhoppers [menu]` | Mở Bảng Điều Khiển Tổng Quan (Control Panel) dạng GUI | Không yêu cầu | Mọi người |
| `/tnhoppers hopper` | `/tnhoppers hopper` | Nhận 1 phễu không dây TNHoppers (gắn PDC) | `tnhoppers.admin` | OP |
| `/tnhoppers wand` | `/tnhoppers wand` | Nhận 1 gậy liên kết Link Wand (hạn 60s) | `tnhoppers.admin` | OP |
| `/tnhoppers reload` | `/tnhoppers reload` | Nạp lại toàn bộ tệp cấu hình `config.yml` | `tnhoppers.admin` | OP |

---

## 🛠️ V. CHI TIẾT TỆP CẤU HÌNH (CONFIG.YML)

Tệp `plugins/TNHoppers/config.yml` được chú thích chi tiết, cho phép bạn tinh chỉnh mọi hành vi của plugin:

```yaml
# ==========================================
#          TNHoppers - Tài Nguyễn (TN)
# ==========================================

storage:
  sqlite-file: "tnhoppers.db"       # Tên file cơ sở dữ liệu SQLite
  busy-timeout-ms: 5000             # Thời gian chờ ghi tối đa khi DB bận
  journal-mode: "WAL"               # Chế độ ghi nhật ký tốc độ cao
  synchronous: "NORMAL"             # Đồng bộ an toàn

wireless:
  enabled: true                     # Bật/tắt tính năng chuyển đồ không dây
  default-max-links: 1              # Số lượng rương đích mặc định ở Level 1
  default-max-distance: 15          # Khoảng cách liên kết tối đa (blocks) ở Level 1
  default-mode: "INSERT_BY_ORDER"   # Chế độ mặc định: INSERT_BY_ORDER hoặc DISTRIBUTE_EQUALLY

transfer:
  check-interval-ticks: 2           # Chu kỳ tick chạy bộ kiểm tra truyền tải
  interval-ticks: 20                # Thời gian trễ cơ bản (20 ticks = 1 giây)
  max-items-per-cycle: 1            # Số lượng item chuyển mỗi lần ở Level 1

suction:
  enabled: false                    # Mặc định tắt; bật true sau khi cấu hình xong
  mode: "RADIUS"                    # Chế độ hút: "RADIUS" (bán kính) hoặc "CHUNK" (toàn chunk)
  radius: 3.0                       # Bán kính hút (áp dụng cho RADIUS mode, từ 1.0 đến 16.0)
  interval-ticks: 10                # Chu kỳ tick quét hút đồ rơi
  max-hoppers-per-cycle: 128        # Giới hạn số lượng phễu được xử lý trong 1 tick
  max-entities-per-cycle: 128       # Giới hạn tổng số entity item quét trong 1 tick
  max-entities-per-hopper-per-cycle: 16 # Số entity tối đa cho mỗi phễu quét/tick
  max-items-per-cycle: 64           # Số lượng item tối đa được hút trong 1 tick
  respect-item-owner: true          # Không hút item đang thuộc quyền sở hữu của người khác

upgrades:
  purchases-enabled: true           # Bật nâng cấp tốn phí (qua Vault hoặc XP Level)
  costs:
    speed-base: 1000.0              # Giá cơ bản nâng cấp Tốc độ (nhân cấp độ)
    amount-base: 2500.0             # Giá cơ bản nâng cấp Số lượng item
    links-base: 1500.0              # Giá cơ bản nâng cấp Số lượng rương đích
    distance-base: 2000.0           # Giá cơ bản nâng cấp Khoảng cách

integrations:
  superior-skyblock2:
    enabled: true                   # Bật kiểm tra quyền đảo SuperiorSkyblock2
    require-island-to-place: true   # Bắt buộc phải đặt phễu trên đảo (cấm đặt ở hoang dã)
    allow-cross-island-linking: false # Cấm liên kết rương xuyên qua đảo người khác
    validate-island-membership-on-transfer: true # Kiểm tra tư cách đảo liên tục khi chuyển đồ
  vault:
    enabled: true                   # Bật tích hợp tiền tệ Vault (nếu thiếu sẽ dùng XP Level)

logging:
  debug: false                      # Bật log chi tiết khi cần theo dõi gỡ lỗi
```

---

## 🔨 VI. HƯỚNG DẪN CÀI ĐẶT & BIÊN DỊCH (BUILD GUIDE)

### 1. Cài đặt trên máy chủ
1. Tải file `TNHoppers-0.6.0-SNAPSHOT.jar` từ thư mục `target/`.
2. Thả file vào thư mục `plugins/` của máy chủ Paper (1.20 trở lên).
3. (Tùy chọn) Cài đặt **Vault** (cùng plugin kinh tế như EssentialsX) và **SuperiorSkyblock2** nếu máy chủ là Skyblock.
4. Khởi động lại server để plugin tự tạo cơ sở dữ liệu và cấu hình mặc định.

### 2. Biên dịch từ mã nguồn (Compile with Maven)
Yêu cầu máy tính có cài đặt **JDK 21** trở lên và **Apache Maven**:

```bash
# Di chuyển vào thư mục dự án TNHoppers
cd TNHoppers

# Thực hiện dọn dẹp, chạy unit tests và đóng gói
mvn clean test package
```
Sau khi build hoàn tất (`BUILD SUCCESS`), file JAR thành phẩm dạng shaded sẽ nằm tại:
`target/TNHoppers-0.6.0-SNAPSHOT.jar`.

---

## 📜 VII. BẢN QUYỀN & TÁC QUYỀN (LICENSE & CREDITS)

- Dự án thuộc bản quyền chính thức của **Tài Nguyễn (TN)**.
- Được cấp phép dưới [Giấy phép MIT](LICENSE): Bạn có toàn quyền sử dụng, sửa đổi, tích hợp vào hệ thống máy chủ thương mại hoặc phi thương mại, với điều kiện giữ nguyên thông báo bản quyền gốc của tác giả.
