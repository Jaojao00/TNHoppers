# 🛡️ Chính Sách Bảo Mật & An Toàn Hệ Thống (Security Policy)

> **Dự án:** TNHoppers — Hệ Thống Phễu Không Dây & Hút Vật Phẩm Tối Tân  
> **Chủ quản & Kỹ sư bảo mật:** **Tài Nguyễn (TN)**  
> **Kênh tiếp nhận bảo mật:** [tainguyenhr.dev@gmail.com](mailto:tainguyenhr.dev@gmail.com) | Discord: [https://discord.gg/kwqU3N4nkH](https://discord.gg/kwqU3N4nkH)

---

## 📌 I. PHIÊN BẢN ĐƯỢC HỖ TRỢ BẢO MẬT (SUPPORTED VERSIONS)

Chúng tôi cam kết duy trì tính toàn vẹn và cung cấp các bản vá bảo mật khẩn cấp (Hotfixes) cho các phiên bản đang hoạt động chính thức:

| Phiên Bản | Môi Trường Khuyên Dùng | Trạng Thái Hỗ Trợ | Hỗ Trợ Vá Lỗ Hổng |
| :--- | :--- | :--- | :--- |
| **v0.6.x** | Paper / Purpur 1.20.x – 1.21.x (Java 21) | **Hoạt động chính thức (Active)** | ✅ Hỗ trợ đầy đủ (Ưu tiên cao nhất) |
| **v0.5.x** | Paper 1.20.x | **Ngừng duy trì (Deprecated)** | ⚠️ Chỉ nhận vá lỗi nghiêm trọng (Critical) |
| **< v0.5.0** | Legacy Builds | **Hết hạn hỗ trợ (End-of-Life)** | ❌ Không hỗ trợ — Vui lòng nâng cấp lên bản mới |

---

## 🏛️ II. CÁC TẦNG PHÒNG THỦ AN NINH CỐT LÕI (SECURITY ARCHITECTURE)

**TNHoppers** được lập trình theo tiêu chuẩn khắt khe dành cho các máy chủ công cộng quy mô lớn (High-Traffic Public Servers), tích hợp sẵn các cơ chế phòng vệ chống gian lận và khai thác lỗi:

### 1. ⚔️ Cơ Chế Giao Dịch Nguyên Tử & Chống Dup Đồ (Atomic Transactions & Anti-Duplication)
* **Giao dịch nguyên tử (`InventoryTransferTransaction.java`):** Quá trình chuyển đồ không bao giờ giả định trước kết quả. Hệ thống luôn thử nghiệm mô phỏng việc thêm đồ vào kho rương đích, tính toán chính xác số lượng thực tế được nhận (`transferred`), sau đó mới trừ tương ứng ở nguồn.
* **Chống Overflow & Desync:** Nếu rương đích bị đầy bất ngờ trong quá trình chuyển hoặc bị phá hủy cùng tick, vật phẩm thừa lập tức được giữ lại an toàn ở kho phễu hoặc rơi có kiểm soát, triệt tiêu 100% rủi ro mất đồ hoặc nhân bản vật phẩm (Dupe bug).
* **Chuẩn hóa Rương đôi (Double Chest Normalization):** Tự động phát hiện và ánh xạ về nửa khối rương chính (`Chest.isDoubleChest()`), ngăn chặn các hành vi gửi gói tin giả mạo nhằm liên kết hai lần vào cùng một rương để nhân đôi vật phẩm.

### 2. 🏝️ Tường Lửa Bảo Vệ Đảo & Chống Trộm Xuyên Đảo (Island Isolation Firewall)
* **Tích hợp SuperiorSkyblock2 đa tầng:**
  * **Tầng Đặt Phễu (`canBuild`):** Chỉ người chơi có quyền xây dựng trên đảo mới được phép đặt phễu.
  * **Tầng Quản Trị GUI (`canOpenGui`):** Chỉ chủ phễu hoặc thành viên có quyền truy cập rương trên đảo mới có thể mở menu quản lý.
  * **Tầng Liên Kết (`canLink`):** Ngăn chặn tuyệt đối việc đứng từ đảo này liên kết sang rương của đảo khác (`Cross-island Linking`).
  * **Tầng Runtime Transfer (`canTransfer`):** Kiểm tra liên tục ở thời gian thực mỗi chu kỳ chuyển đồ. Ngay cả khi người chơi bị kick hoặc giáng quyền khỏi đảo sau khi đã đặt phễu, hệ thống lập tức phong tỏa phễu, vô hiệu hóa khả năng hút/chuyển đồ.

### 3. 🛑 Chống Tấn Công Từ Chối Dịch Vụ & Giật Lag (Anti-DoS & Lag Exploitation)
* **Hạn mức chu kỳ an toàn (Cycle Budgets):** Mỗi chu kỳ quét hút đồ (`SuctionEngine`) đều chịu sự ràng buộc của 4 hạn mức cứng:
  * `max-hoppers-per-cycle`: Giới hạn số phễu duyệt mỗi nhịp.
  * `max-entities-per-cycle`: Giới hạn số thực thể quét mỗi nhịp.
  * `max-entities-per-hopper-per-cycle`: Ngăn chặn việc một cụm farm quái xả hàng chục nghìn item độc chiếm tài nguyên CPU.
  * `max-items-per-cycle`: Ngăn tràn bộ nhớ đệm luân chuyển.
* **Bộ lọc thực thể thông minh (Entity Filtering):** Động cơ quét Chunk chỉ duyệt đúng thực thể `Item`, bỏ qua 100% các thực thể quái, vật nuôi, ArmorStand... tránh tình trạng spam entity làm quá tải server.

### 4. 🗄️ An Toàn Cơ Sở Dữ Liệu & Cách Ly Luồng (Database & Thread Confinement)
* **Single-Writer Thread Confinement:** Mọi thao tác ghi dữ liệu vào SQLite đều được chuyển qua một Executor đơn luồng chuyên biệt. Loại bỏ hoàn toàn xung đột ghi đồng thời, lỗi `SQLITE_BUSY` hay nghẽn luồng xử lý chính (Main Thread Hang).
* **PreparedStatements 100%:** Tuyệt đối không ghép chuỗi SQL thủ công; loại trừ mọi nguy cơ tấn công SQL Injection thông qua tên phễu hay dữ liệu NBT tùy biến.

---

## 🔐 III. TÍNH TOÀN VẸN MÃ & XÁC THỰC BẢN PHÁT HÀNH (CODE INTEGRITY)

Để bảo vệ máy chủ của bạn khỏi các mã độc (backdoor, mã độc gián điệp, force-op), **chỉ tải TNHoppers từ các kênh phát hành chính thức của tác giả Tài Nguyễn (TN)**:

* 🌐 **Kênh phát hành chính thức:** [GitHub Releases: Jaojao00/TNHoppers](https://github.com/Jaojao00/TNHoppers/releases)

### Bảng Mã Băm Xác Thực Bản Phát Hành (SHA-256 Checksum):
Trước khi đưa file `.jar` vào thư mục `plugins/` của máy chủ, bạn nên xác thực mã băm SHA-256 để đảm bảo tệp tin nguyên bản và không bị chỉnh sửa:

| Tệp Tin | Phiên Bản | Mã Băm SHA-256 |
| :--- | :--- | :--- |
| `TNHoppers-0.6.0.jar` | **v0.6.0 (Protected Build)** | `35d1143fd49d82f0b7fef94a57992424cd5a1a0d02c386889d1208cdbf740d7b` |

*Lệnh kiểm tra mã băm trên máy chủ của bạn:*
```powershell
# Trên Windows PowerShell:
Get-FileHash -Path "TNHoppers-0.6.0.jar" -Algorithm SHA256

# Trên Linux Terminal:
sha256sum TNHoppers-0.6.0.jar
```

---

## 🚨 IV. QUY TRÌNH BÁO CÁO LỖ HỔNG (VULNERABILITY DISCLOSURE)

Chúng tôi trân trọng và đánh giá cao mọi đóng góp từ cộng đồng quản trị viên, lập trình viên và các chuyên gia bảo mật. Nếu bạn phát hiện bất kỳ lỗ hổng bảo mật nào (như nguy cơ dup đồ, bypass quyền bảo vệ đảo, hoặc crash server):

### ⚠️ QUY TẮC CỐT LÕI: KHÔNG CÔNG KHAI LỖ HỔNG TRƯỚC CỘNG ĐỒNG
* **KHÔNG** mở Issue công khai trên GitHub đối với các lỗi liên quan đến bảo mật / dup đồ.
* **KHÔNG** chia sẻ phương thức khai thác trên các diễn đàn hoặc nhóm chat công cộng.

### Các Bước Báo Cáo Kín:
1. **Gửi thông tin trực tiếp tới:**
   * 📧 **Email:** [tainguyenhr.dev@gmail.com](mailto:tainguyenhr.dev@gmail.com)
   * 💬 **Discord:** Nhắn tin riêng (DM) trực tiếp cho **Tài Nguyễn** tại máy chủ [Discord Support](https://discord.gg/kwqU3N4nkH).
2. **Nội dung báo cáo khuyên dùng:**
   * Mô tả ngắn gọn về lỗ hổng (Dup đồ, bypass quyền đảo, crash...).
   * Các bước chi tiết để tái hiện lỗi (Steps to reproduce).
   * Phiên bản Paper / Purpur và phiên bản TNHoppers đang thử nghiệm.
   * Video hoặc ảnh chụp màn hình minh họa (nếu có).

### Cam Kết Xử Lý (Service Level Agreement - SLA):
* ⏱️ **Phản hồi ban đầu:** Trong vòng **24 - 48 giờ** kể từ khi nhận được báo cáo.
* 🛠️ **Phát hành bản vá (Hotfix):** Trong vòng **3 - 7 ngày** (tùy thuộc vào mức độ phức tạp).
* 🎖️ **Ghi nhận công trạng:** Tên của bạn sẽ được vinh danh trong phần **Security Hall of Fame** của bản phát hành nếu bạn mong muốn!

---

*Chân thành cảm ơn sự đồng hành và hỗ trợ của bạn trong việc xây dựng một hệ sinh thái Minecraft an toàn, ổn định và chuyên nghiệp!*
