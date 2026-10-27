# 🚀 Bản Phát Hành TNHoppers v0.6.0 (Release Notes)

**Phiên bản:** `v0.6.0`  
**Ngày phát hành:** 27/10/2026  
**Tác giả:** Tài Nguyễn (TN)  
**Tương thích:** Paper / Purpur 1.20 - 1.21.x (Java 21+)  

---

### 🎉 Các tính năng trong bản phát hành v0.6.0:
1. **Wireless Transfer Engine**: Truyền chuyển item không dây an toàn, hỗ trợ 2 chế độ `INSERT_BY_ORDER` và `DISTRIBUTE_EQUALLY`, cơ chế giao dịch nguyên tử chống duplicate item.
2. **Filter Matcher Suite**: Lọc linh hoạt theo `Material` hoặc `Strict NBT` (ItemMeta, tên, lore, enchantments, CustomModelData), hỗ trợ `WHITELIST` và `BLACKLIST`.
3. **Interactive GUI Suite**:
   - Menu Hub chính (`§8TNHoppers`).
   - Quản lý liên kết: xem và gỡ liên kết tức thì.
   - Trình chỉnh sửa bộ lọc với cơ chế *Ghost items* (Shift-Click không mất đồ thật).
   - Hệ thống nâng cấp 4 nhánh (Tốc độ, Số lượng, Số liên kết, Khoảng cách) tích hợp Vault Economy và XP level fallback.
4. **SuperiorSkyblock2 Protection Adapter**: Kiểm tra ranh giới đảo, quyền `BUILD` / `CHEST`, chặn liên kết xuyên đảo, xác thực quyền động thời gian thực.
5. **Suction Engine**: Hút item rơi tự do theo bán kính (`RADIUS`) hoặc phạm vi chunk (`CHUNK`), kiểm soát ngân sách per-tick, cache mảng entity per-tick và tôn trọng item owner.
6. **SQLite Single-Writer & WAL**: Bộ nhớ bền vững, hàng đợi 4096 tác vụ không lock main thread, an toàn khi nổ TNT/Creeper và rương đích bị phá vỡ.
7. **ChunkHopperCache**: Không lưu giữ tham chiếu đối tượng Bukkit World/Block, cam kết không rò rỉ RAM (Zero Memory Leaks).

---

### 📦 Tệp phân phối:
- `TNHoppers-0.6.0.jar`: File plugin shaded hoàn chỉnh (đã bao gồm SQLite JDBC và các thư viện cần thiết).
- `config.yml`: File cấu hình mẫu mặc định.
