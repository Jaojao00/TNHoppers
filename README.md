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

### 1. Nhận và đặt phễu TNHoppers
1. **Lấy phễu:** Admin sử dụng lệnh `/tnhoppers hopper` (hoặc cấu hình nhận qua Shop/Crates).
2. **Đặt phễu:** Đặt phễu xuống vị trí mong muốn trên đảo hoặc vùng đất của bạn.
   - *Lưu ý:* Phễu TNHoppers được đánh dấu bằng thẻ `PersistentDataContainer (PDC)` mang mã nhận diện riêng và lưu quyền sở hữu UUID của người đặt.
   - Nếu bạn cố tình đặt ở vùng hoang dã (Wilderness) khi server bật giới hạn đảo, phễu sẽ tự động từ chối đặt.

### 2. Tạo liên kết không dây bằng Link Wand
1. **Lấy gậy liên kết:** Sử dụng lệnh `/tnhoppers wand` để nhận **Link Wand** (Gậy liên kết).
2. **Chọn nguồn (Source):** Cầm Link Wand và **Nhấp chuột phải** vào phễu TNHoppers của bạn. Hệ thống sẽ ghi nhận phiên liên kết (hiển thị hiệu ứng hoặc tin nhắn xác nhận).
3. **Chọn đích (Destination):** Đi tới thùng chứa mong muốn (Rương, Rương bẫy, Thùng gỗ, Hộp Shulker...) và **Nhấp chuột phải** vào đó.
   - **Tự động chuẩn hóa rương đôi (Double Chest Normalization):** Nếu thùng chứa là rương đôi, plugin sẽ tự động chọn nửa rương chính làm mốc, ngăn chặn triệt để tình trạng liên kết trùng lặp 2 nửa của một rương.
   - **Giới hạn tự động:** Plugin sẽ kiểm tra khoảng cách và số lượng liên kết cho phép (dựa trên cấp độ nâng cấp hiện tại của phễu).
4. **Hết hạn an toàn:** Phiên liên kết sẽ tự động hủy sau **60 giây** nếu bạn đổi đồ trên tay hoặc thoát game để tránh ghi đè nhầm lẫn.

### 3. Hub điều khiển & Giao diện quản lý (GUI Suite)
Chỉ cần **Nhấp chuột phải** (bằng tay không hoặc vật phẩm thông thường) vào phễu TNHoppers của bạn để mở bảng điều khiển chính:

#### A. Menu Hub Trung Tâm (`§8TNHoppers`)
- **Quản lý liên kết (Link Manager):** Hiển thị trực quan tất cả các thùng chứa đang được phễu truyền tới. Nhấp vào để gỡ liên kết ngay lập tức (xóa an toàn khỏi SQLite và Cache).
- **Rương Cho Phép (Whitelist Chest):** Mở giao diện rương 54 ô riêng biệt để bỏ các vật phẩm được phép đi qua.
- **Rương Chặn (Blacklist Chest):** Mở giao diện rương 54 ô riêng biệt để bỏ các vật phẩm bị chặn lại.
- **Nâng cấp phễu (Hopper Upgrades):** Mở giao diện mua cấp độ tăng sức mạnh cho phễu.

#### B. Rương Cho Phép & Rương Chặn Riêng Biệt (Chest GUIs)
Thay vì dùng 1 menu gộp phức tạp, TNHoppers tách biệt thành **2 rương giao diện độc lập**:
- 🟢 **Rương Cho Phép (`§2Rương Cho Phép (Whitelist)`):**
  - Bỏ trực tiếp hoặc **Shift-Click** vật phẩm mẫu vào rương (tạo mẫu "bóng ma" *ghost item*, không mất đồ thật).
  - Nhấp vào ô mẫu để xóa khỏi danh sách.
  - Nút **Strict NBT Mode:** Tùy chọn so khớp nghiêm ngặt toàn bộ ItemMeta (tên, lore, phù phép, potion, CustomModelData) hoặc chỉ so sánh theo Material.
  - Nút **Lưu & Kích hoạt Whitelist:** Lưu vào SQLite và kích hoạt ngay lập tức cho phễu.
- 🔴 **Rương Chặn (`§4Rương Chặn (Blacklist)`):**
  - Mở giao diện rương để quản lý các vật phẩm cấm đi qua phễu.
  - Tương tự, hỗ trợ Shift-Click thêm mẫu, xóa mẫu, bật/tắt Strict NBT và nút **Lưu & Kích hoạt Blacklist**.

#### C. Bảng nâng cấp phễu (Interactive Upgrades GUI)
Người chơi có thể đầu tư tiền tệ Vault hoặc Cấp độ kinh nghiệm (XP Levels) để cường hóa 4 nhánh chỉ số:
1. ⚡ **Tốc độ (Speed):** Giảm thời gian chờ giữa các lần chuyển đồ (đẩy đồ nhanh hơn gấp nhiều lần).
2. 📦 **Số lượng (Amount):** Tăng lượng vật phẩm chuyển được trong một chu kỳ (ví dụ từ 1 item lên 4, 8, 16 item/lần).
3. 🔗 **Số liên kết (Links):** Cho phép phễu kết nối tới nhiều rương đích cùng lúc (1, 2, 4, 8... rương).
4. 📏 **Khoảng cách (Distance):** Mở rộng tầm với không dây giữa phễu và rương đích (từ 15 blocks lên 30, 50, 100 blocks).

### 4. Cơ chế hút vật phẩm rơi chuyển thẳng tới rương đích
Khi tính năng hút đồ được bật trong `config.yml`, phễu TNHoppers sẽ tự động gom các item rơi tự do trong phạm vi (bán kính `RADIUS` hoặc toàn bộ `CHUNK`):
- **Chuyển thẳng tới rương đích đã liên kết:** Item sau khi hút sẽ được đưa **ngay lập tức vào các rương đích được liên kết** (tuân theo chế độ `INSERT_BY_ORDER` hoặc `DISTRIBUTE_EQUALLY`), không bị nghẽn lại ở 5 ô chứa nhỏ của phễu.
- **Cơ chế đệm dự phòng (Hopper Fallback Buffer):** Nếu tất cả các rương đích đã đầy hoặc phễu chưa được liên kết với rương nào, vật phẩm hút sẽ được lưu an toàn vào 5 ô chứa của phễu.
- **Tương thích bộ lọc:** Item rơi chỉ được hút nếu thỏa mãn bộ lọc hiện tại của phễu (Whitelist/Blacklist).
- **Bảo vệ quyền nhặt:** Tôn trọng người chơi vừa đánh rơi đồ (`pickup-delay` và `respect-item-owner`). Phễu không bao giờ cướp đồ của người chơi khác khi item còn đang khóa nhặt.

### 5. Bảng điều khiển lệnh `/tnhoppers` (Command Control Panel)
Gõ lệnh `/tnhoppers` (hoặc `/tnhoppers menu`) để mở ngay bảng điều khiển tổng quan:
- 🌪️ **Lấy Phễu TNHoppers:** Nhận nhanh phễu không dây (cần quyền `tnhoppers.admin`).
- 🪄 **Lấy Link Wand:** Nhận nhanh gậy liên kết (cần quyền `tnhoppers.admin`).
- 📜 **Danh Sách Phễu Đang Nạp:** Xem toàn bộ phễu đang hoạt động trong RAM của máy chủ. Admin có thể **nhấp chuột vào bất kỳ phễu nào để Dịch Chuyển Tức Thời (Teleport)** đến tận nơi kiểm tra thực địa!
- ℹ️ **Trạng Thái Hệ Thống:** Xem phiên bản, tác giả, trạng thái động cơ chuyển đồ, trạng thái Suction và bảo vệ đảo.
- 🔄 **Nạp Lại Cấu Hình (Reload):** Tải lại toàn bộ cấu hình `config.yml` ngay từ GUI mà không cần reload server.

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
