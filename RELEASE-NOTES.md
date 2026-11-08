<p align="center">
  <img src="https://raw.githubusercontent.com/Jaojao00/TNHoppers/main/tnhoperlogo.jpg" alt="TNHoppers Logo" width="200" />
</p>

# 🚀 Bản Phát Hành TNHoppers v0.6.3 (Break Protection, Virtual Deep Storage & 24/24 Tests Passed)

**Phiên bản:** `v0.6.3`  
**Ngày phát hành:** 09/11/2026  
**Tác giả:** Tài Nguyễn (TN)  
**Tương thích:** Paper / Purpur 1.20 - 1.21.x (Java 21+)  

---

### 🛡️ Các tính năng đột phá trong bản phát hành này:

1. **[Tính năng mới] 🔒 Lớp Bảo Vệ Phá Hủy & Chống Nổ Tuyệt Đối (Break & Explosion Protection)**:
   - **Bảo vệ Rương đích liên kết:** Ngăn chặn tuyệt đối mọi hành vi đập phá rương/thùng chứa đang được liên kết với phễu TNHoppers (`BlockBreakEvent` bị hủy ngay lập tức, phát âm thanh cảnh báo lỗi và gửi thông báo màu đỏ yêu cầu gỡ liên kết trước).
   - **Chuẩn hóa Rương Đôi (Double Chest Normalization):** Nhận diện chính xác cả nửa bên trái và bên phải của rương đôi. Đập vào bất kỳ nửa nào cũng được bảo vệ an toàn.
   - **Bảo vệ Phễu TNHoppers:** Phễu đang có liên kết rương đích sẽ không thể bị phá cho đến khi toàn bộ liên kết được gỡ bỏ trong GUI.
   - **Miễn nhiễm nổ 100%:** Cả phễu và rương liên kết đều được lọc ra khỏi danh sách phá hủy của TNT, Creeper, Wither và Ender Dragon trong `ExplosionCleanupListener`.

2. **[Tính năng mới] 📦 Kho Lưu Trữ Ảo Mở Rộng (Virtual Deep Storage — 10k đến 250k Items)**:
   - **Dung tích đệm ảo nâng cấp 4 cấp độ:**
     - **Cấp 1:** `10,000` items (Mặc định khi tạo liên kết)
     - **Cấp 2:** `50,000` items ($5,000 Vault / XP)
     - **Cấp 3:** `100,000` items ($15,000 Vault / XP)
     - **Cấp 4:** `250,000` items ($50,000 Vault / XP — Cấp tối đa)
   - **Tự động tràn vào kho ảo (Overflow Buffer):** Khi rương vật lý đầy 100%, đồ hút hoặc chuyển đến sẽ tự động lưu vào kho ảo của liên kết đó mà không bị nghẹt hay rơi vãi ra ngoài.
   - **Tự động xả đồ mượt mà (Auto-Drain):** Khi người chơi lấy bớt đồ trong rương vật lý tạo ô trống, phễu sẽ tự động trích xuất các item từ kho ảo xả dần vào rương theo chu kỳ tick.
   - **Trình xem & Rút đồ từ Kho Ảo (Deep Storage Viewer):**
     - Mở trực tiếp bằng cách **Nhấp chuột giữa (Middle-click)** vào rương liên kết trong menu Links, hoặc bấm nút Kho Ảo (Slot 47) trong menu Bộ lọc.
     - Cho phép người chơi rút 1 item (Chuột Phải), 1 Stack 64 items (Chuột Trái), hoặc rút toàn bộ (Shift + Click).
     - Nút **"Xả Vào Rương Ngay" (Slot 53)** cho phép ép xả toàn bộ đồ từ kho ảo vào ô trống rương đích tức thì.
   - **Bảo vệ chống mất đồ khi hủy liên kết:** Nếu kho ảo của rương đích còn chứa vật phẩm, hệ thống lập tức từ chối gỡ liên kết và cảnh báo người chơi phải rút hết đồ ra trước.
   - **Cơ sở dữ liệu SQLite WAL (`hopper_virtual_storage`):** Dữ liệu kho ảo được lưu an toàn xuống SQLite, không bao giờ mất đồ khi restart server hay chunk bị dỡ nạp.

3. **[Nâng cấp GUI] ⚙️ Nhánh Nâng Cấp Dung Tích Kho Ảo (Slot 13 - Barrel)**:
   - Bảng điều khiển Upgrades GUI bổ sung Slot 13 biểu tượng Thùng Gỗ (Barrel) hiển thị chi tiết cấp độ, sức chứa hiện tại, sức chứa tiếp theo và giá nâng cấp bằng tiền Vault / XP.

4. **🧪 24/24 Bài Kiểm Thử Độc Lập Đạt 100% (Surefire Unit Tests)**:
   - Bổ sung `VirtualStorageCapacityTest` kiểm thử tính toán sức chứa 4 cấp độ và tính bất biến của dữ liệu.
   - Bổ sung `testDestinationLinkedAndBreakProtection` trong `ChunkHopperCacheTest` kiểm thử cơ chế phát hiện liên kết rương và bảo vệ phá hủy.
   - Toàn bộ **24 bài kiểm thử độc lập** đều vượt qua (**0 failures, 0 errors, 0 skipped**).
   - Đóng gói ProGuard Obfuscated Jar `TNHoppers-0.6.2.jar` (14.3 MB) tối ưu hóa kích thước và bảo mật mã nguồn độc quyền của **Tài Nguyễn (TN)**.

---

# 🚀 Bản Phát Hành TNHoppers v0.6.2 (Live View Cache, Symmetrical Rollback & Hardened Auto-Sorter)

**Phiên bản:** `v0.6.2`  
**Ngày phát hành:** 06/11/2026  
**Tác giả:** Tài Nguyễn (TN)  
**Tương thích:** Paper / Purpur 1.20 - 1.21.x (Java 21+)  

---

### 🛡️ Các cải tiến cốt lõi trong v0.6.2:

1. **[P1] Live View Phễu Hoạt Động (`activeHoppersView()`)**:
   - Tách biệt rõ ràng khái niệm giữa immutable snapshot và live view: đổi tên phương thức chính thức thành `activeHoppersView()`.
   - Phương thức trả về view không thể sửa đổi (`Collections.unmodifiableCollection`) từ `ConcurrentHashMap.values()`.
   - Lấy phần tử kế tiếp qua iterator có chi phí khấu hao trung bình $O(1)$; quét toàn bộ $N$ phễu hoạt động có độ phức tạp thời gian $O(N)$.
   - Iterator nhất quán yếu (weakly consistent), cho phép thêm/xóa phễu và dỡ nạp chunk đồng thời mà không ném ngoại lệ `ConcurrentModificationException` (được kiểm chứng bằng unit test).

2. **[P0] Giao Dịch Chuyển Đồ Đối Xứng & Bảo Toàn Khối Lượng (Symmetrical Rollback)**:
   - Trong `InventoryTransferTransaction.java`, bổ sung `simulateInsert` tính toán sức chứa trước khi thực hiện.
   - Triển khai `insertWithRollback`: Chụp ảnh nhanh mảng `ItemStack[]` của inventory đích trước khi ghi, đồng thời giữ trạng thái gốc của thực thể item ngoài thế giới (`entityInitialStack`).
   - Nếu tác vụ `onCommit` hoặc trừ thực thể item gặp ngoại lệ, cả kho lưu trữ lẫn thực thể item đều được hoàn trả nguyên vẹn về trạng thái ban đầu.
   - Đảm bảo an toàn tuyệt đối trước các lỗi runtime, plugin can thiệp hoặc rương đầy trong quá trình chuyển đồng bộ. Định luật bảo toàn khối lượng được kiểm thử với 6 test cases trong `TransferResultTest`.

3. **[P1] SQLite WAL Checkpoint Chẩn Đoán & Khả Năng Phục Hồi**:
   - Thực thi `PRAGMA wal_checkpoint(TRUNCATE)` khi đóng server và đọc chi tiết 3 cột trạng thái (`busy`, `log`, `checkpointed`).
   - Ghi nhận cảnh báo rõ ràng nếu checkpoint bị hoãn lại do luồng đọc đang giải phóng khóa (`busy = 1`), khẳng định các frame WAL còn lại sẽ được SQLite tự động phục hồi an toàn khi khởi động lại.
   - Bắt buộc kiểm tra timeout xả hàng đợi ghi 15 giây; tuyệt đối không can thiệp xóa file `-wal` thủ công.

4. **🧙‍♂️ Gia Cố Tính Năng Tự Động Phân Loại Đồ (Auto-Sorter Wizard)**:
   - **Chống ghi đè vô ý:** Nếu các rương đích đã cài đặt bộ lọc hoặc tên tùy chỉnh từ trước, hệ thống yêu cầu người chơi nhấp lại lần thứ hai trong vòng 10 giây để xác nhận ghi đè.
   - **Lấy mẫu vật phẩm bất biến (Read-Only Sampling):** Chỉ đọc và clone mẫu đồ với số lượng 1, tuyệt đối không làm thay đổi hay tiêu hao đồ trong túi người chơi.
   - **Phân biệt ItemMeta đa chiều:** Tách biệt chính xác các vật phẩm cùng `Material` nhưng khác `ItemMeta` (Tên tùy chỉnh, Phù phép, Thuốc Potion, CustomModelData, Sách phù phép). Tự động kích hoạt cờ `strict = true` khi đồ mẫu có metadata đặc biệt.
   - **Hoàn tác cấu hình khi lỗi database:** Giám sát toàn bộ batch ghi qua `CompletableFuture.allOf`. Nếu việc lưu vào SQLite gặp sự cố, cache trong RAM lập tức rollback về trạng thái ban đầu và báo lỗi cho người chơi.

5. **🧪 19/19 Báo Cáo Kiểm Thử Tự Động Surefire (XML & TXT)**:
   - Toàn bộ **19 bài kiểm thử độc lập** (`ChunkHopperCacheTest`: 4, `TransferResultTest`: 6, `LinkDestinationTest`: 4, `HopperUpgradesTest`: 3, `BlockPositionTest`: 2) đều vượt qua thành công (**0 failures, 0 errors**).
   - Thư mục `target/surefire-reports` được đóng gói nguyên vẹn trong tệp `TNHoppers-v0.6.2-FullProject.zip` để bất kỳ ai cũng có thể giải nén và kiểm chứng độc lập.

---

# 🚀 Bản Phát Hành TNHoppers v0.6.1 (Gia Cố & Nâng Cấp Độ Bền Vững)

**Phiên bản:** `v0.6.1`  
**Ngày phát hành:** 06/11/2026  
**Tác giả:** Tài Nguyễn (TN)  
**Tương thích:** Paper / Purpur 1.20 - 1.21.x (Java 21+)  

---

### 🛡️ Các cải tiến & gia cố quan trọng trong v0.6.1:
1. **[P0] Chấm dứt ghi đè cấu hình Suction**:
   - Gỡ bỏ hoàn toàn việc ép buộc ghi đè giá trị `suction.enabled`, `interval-ticks`, `respect-item-owner` và `call-pickup-event` khi khởi động.
   - Tôn trọng 100% mọi cấu hình do quản trị viên thiết lập; chỉ bổ sung các tùy chọn còn thiếu.

2. **[P0] Gia cố giao dịch hút item chống Duplicate & Desync**:
   - Cơ chế khấu trừ tức thời (Instant Deduct): Số lượng item trên thế giới được trừ ngay khi mỗi phần đồ được nhận thành công vào rương/phễu.
   - Bắt trọn vẹn ngoại lệ, loại bỏ hoàn toàn khả năng nhân bản vật phẩm khi rương đầy, ngắt quãng hoặc lỗi kết nối.
   - Hỗ trợ kiểm tra quyền sở hữu đồ rơi (`respect-item-owner` kiểm tra cả owner và thrower).

3. **[P1] Tối ưu hóa hiệu năng quét phễu (Performance Cursor)**:
   - Thay thế việc duyệt toàn bộ chunk của mọi thế giới bằng con trỏ ngân sách (Round-Robin Budget Cursor) qua `plugin.cache().snapshotAll()`.
   - Bổ sung cấu hình `transfer.max-hoppers-per-cycle: 512` giúp server hàng nghìn chunk luôn giữ vững 20.0 TPS.

4. **[P1] Khắc phục race-condition khi tải nâng cấp bất đồng bộ**:
   - Đồng bộ hóa việc nạp hồ sơ nâng cấp (`loadUpgradeProfile`) lên luồng chính.
   - Áp dụng `setUpgradesIfAbsent` ngăn việc kết quả nạp database đến muộn ghi đè cấp độ người chơi vừa mua.

5. **[P1] Phân quyền lệnh chuẩn hóa (`tnhoppers.use` & `tnhoppers.admin`)**:
   - Cho phép toàn bộ người chơi thường mở bảng điều khiển `/tnhoppers` hoặc `/tnhoppers menu`.
   - Phân định rõ ràng quyền quản trị `tnhoppers.admin` cho các lệnh lấy đồ, nạp cấu hình và dịch chuyển.

6. **[P1] An toàn dữ liệu SQLite khi Shutdown**:
   - Tăng thời gian chờ đợi ghi cơ sở dữ liệu khi tắt server lên 15 giây.
   - Tự động thực thi `PRAGMA wal_checkpoint(PASSIVE)` đồng bộ sạch nhật ký WAL trước khi ngắt kết nối.

---

# 🚀 Bản Phát Hành TNHoppers v0.6.0 (Release Notes)

**Phiên bản:** `v0.6.0`  
**Ngày phát hành:** 27/10/2026  
**Tác giả:** Tài Nguyễn (TN)  
**Tương thích:** Paper / Purpur 1.20 - 1.21.x (Java 21+)  

---

### 🎉 Các tính năng trong bản phát hành v0.6.0:

1. **Phân loại 2 dòng Hopper chuyên biệt**:
   - **Normal Hopper**: Hút vật phẩm rơi theo bán kính hình cầu (`RADIUS`), có thể nâng cấp bán kính (lên đến 16 block), tốc độ, số lượng và số rương liên kết.
   - **Chunk Hopper**: Hút TOÀN BỘ vật phẩm rơi tự do trong phạm vi cả Chunk (`16x16`), lý tưởng cho các cụm farm quái, farm cây, farm khoáng sản quy mô lớn.
   - Bảo toàn cấp độ & loại phễu khi đập vỡ hoặc bị nổ: Dữ liệu nâng cấp được lưu trực tiếp vào PersistentDataContainer (PDC), rớt ra item giữ nguyên Lore và khôi phục ngay khi đặt lại.

2. **Cấu hình Bộ Lọc Riêng cho Từng Liên Kết (Per-Link Filtering)**:
   - Nhấp chuột trái vào từng rương đích trong menu Links để mở giao diện cấu hình bộ lọc riêng biệt cho rương đó:
     - **Chế độ Whitelist**: Chỉ cho phép các vật phẩm được chỉ định chuyển vào rương này.
     - **Chế độ Blacklist**: Chặn các vật phẩm được chỉ định, cho phép các món khác đi qua.
     - **Lọc nghiêm ngặt (Strict NBT)**: So khớp chính xác cả tên hiển thị, mô tả (lore), phù phép (enchantments), bình thuốc và CustomModelData.
   - **Sao chép & Dán cấu hình (Copy/Paste Filter)**: Nút sao chép cấu hình bộ lọc vào bộ nhớ tạm và dán sang các rương khác chỉ bằng một cú nhấp chuột.
   - **Xóa rác không khớp (Auto-Void Unmatched Items)**: Tùy chọn tự hủy ngay các vật phẩm không khớp với bất kỳ rương đích nào để chống tắc nghẽn phễu và sàn farm.

3. **Điều hướng Chuyển đồ Trực tiếp & Phân phối Thông minh**:
   - Vật phẩm sau khi được hút sẽ được điều hướng truyền thẳng đến các rương đích thỏa điều kiện bộ lọc của từng rương.
   - Chỉ lưu vào phễu nếu tất cả rương đích bị đầy hoặc chưa liên kết rương nào.
   - Nút chuyển đổi trực quan giữa 2 chế độ:
     - `INSERT_BY_ORDER`: Lấp đầy lần lượt từng rương theo thứ tự ưu tiên.
     - `DISTRIBUTE_EQUALLY`: Chia đều luân phiên giữa các rương nhận.

4. **Bảng Điều Khiển & Lệnh Trực Quan `/tnhoppers`**:
   - `/tnhoppers` (hoặc `/tnhoppers menu`): Mở GUI Bảng Điều Khiển Quản Trị Viên:
     - Nhận Normal Hopper / Chunk Hopper.
     - Nhận Gậy Liên Kết (Link Wand).
     - Xem danh sách toàn bộ phễu đang hoạt động trong RAM kèm tính năng nhấp để Dịch chuyển (Teleport).
     - Kiểm tra trạng thái hệ thống và nạp lại cấu hình (Reload).
   - Hỗ trợ Tab-Completion đầy đủ: `/tnhoppers [menu|get <normal|chunk>|wand|reload]`.
   - Mở menu phễu bằng cả Chuột Trái hoặc Chuột Phải (Sneak + Chuột Trái để phá khối).

5. **SuperiorSkyblock2 Protection & SQLite WAL**:
   - Kiểm tra ranh giới đảo, quyền `BUILD` / `CHEST`, chặn liên kết xuyên đảo, bảo vệ thời gian thực.
   - Hàng đợi bất đồng bộ Single-Writer SQLite, cơ chế giao dịch nguyên tử chống dup đồ, cam kết không lag giật TPS.

---

### 📦 Tệp phân phối:
- `TNHoppers-0.6.0.jar`: File plugin shaded hoàn chỉnh (đã tích hợp SQLite JDBC và các thư viện cần thiết).
- `config.yml`: File cấu hình mẫu mặc định.

---

### 💬 Hỗ Trợ & Liên Hệ:
- **Discord:** [https://discord.gg/kwqU3N4nkH](https://discord.gg/kwqU3N4nkH)
- **Gmail:** [tainguyenhr.dev@gmail.com](mailto:tainguyenhr.dev@gmail.com)
