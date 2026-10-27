# 🔑 Bảng Quyền Hạn & Lệnh Quản Trị — TNHoppers

**Tác giả (Author):** Tài Nguyễn (TN)  
**Plugin:** `TNHoppers` (Paper 1.20+ / 1.21.x)  
**Giấy phép:** MIT License  

---

## 1. 📋 Bảng Lệnh Quản Trị (Commands)

| Lệnh | Cú pháp | Quyền hạn yêu cầu | Mô tả chi tiết |
| :--- | :--- | :---: | :--- |
| `/tnhoppers hopper` | `/tnhoppers hopper` | `tnhoppers.admin` | Phát 1 phễu không dây TNHoppers (có đánh dấu thẻ `custom_hopper` và gán UUID người sở hữu). |
| `/tnhoppers wand` | `/tnhoppers wand` | `tnhoppers.admin` | Phát 1 gậy liên kết Link Wand (có đánh dấu thẻ `link_wand`, tự hết hạn phiên sau 60 giây). |

---

## 2. 🛡 Bảng Quyền Hạn (Permissions)

| Node Quyền Hạn (Permission) | Mặc định | Mục đích sử dụng |
| :--- | :---: | :--- |
| `tnhoppers.admin` | `op` | Quyền Quản Trị Viên: Cho phép nhận phễu, nhận Link Wand, và mở/quản lý bất kỳ phễu nào trên server mà không bị giới hạn quyền đảo. |

---

## 3. 🏝 Quyền Hạn Tích Hợp Đảo (SuperiorSkyblock2 Privileges)

Khi plugin hoạt động cùng **SuperiorSkyblock2**, các hành động của người chơi sẽ tuân theo các đặc quyền phân quyền trên đảo:

| Hành động | Đặc quyền đảo yêu cầu | Ghi chú bảo mật |
| :--- | :---: | :--- |
| **Đặt phễu TNHoppers** | `BUILD` | Ngăn chặn người lạ hoặc khách tham quan đặt trộm phễu trên đảo của người khác. |
| **Phá phễu TNHoppers** | `BUILD` | Chỉ thành viên có quyền xây dựng mới được phá phễu (trả lại phễu PDC an toàn). |
| **Mở Menu Hub Quản Lý** | `CHEST` | Chỉ chủ sở hữu phễu hoặc thành viên có quyền mở rương trên đảo mới mở được GUI phễu. |
| **Tạo liên kết tới rương đích** | `CHEST` | Bắt buộc phải có quyền mở rương tại vị trí rương đích. Chặn tuyệt đối liên kết xuyên đảo. |
| **Chuyển & Hút đồ thời gian thực** | Thành viên đảo | Nếu chủ phễu bị kick khỏi đảo, phễu sẽ tự động ngừng truyền và ngừng hút vật phẩm ngay lập tức. |
