# 🔌 TNHoppers — Developer API Documentation (Hướng Dẫn Tích Hợp Plugin)

> **Tác giả:** Tài Nguyễn (TN)  
> **Phiên bản:** `v0.6.4+`  
> **Yêu cầu nền tảng:** Paper / Purpur 1.20 – 1.21.x (Java 21 LTS)

---

## 📌 1. Cấu hình Dự Án (Setup Dependency)

### Cách 1: Thêm file JAR vào Local Repository (Khuyên dùng)
Tải file `TNHoppers-0.6.2.jar` và thêm vào dự án Maven hoặc Gradle của bạn:

#### **Maven (`pom.xml`):**
```xml
<dependency>
    <groupId>me.tainguyen</groupId>
    <artifactId>TNHoppers</artifactId>
    <version>0.6.2</version>
    <scope>provided</scope>
</dependency>
```

*(Nếu dùng file JAR cục bộ trong thư mục `libs/`:)*
```xml
<dependency>
    <groupId>me.tainguyen</groupId>
    <artifactId>TNHoppers</artifactId>
    <version>0.6.2</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/libs/TNHoppers-0.6.2.jar</systemPath>
</dependency>
```

#### **Gradle (`build.gradle`):**
```groovy
dependencies {
    compileOnly files('libs/TNHoppers-0.6.2.jar')
}
```

---

### Khai báo trong `plugin.yml`:
Thêm `TNHoppers` vào `depend` (bắt buộc) hoặc `softdepend` (tùy chọn) trong tệp `plugin.yml` của bạn:

```yaml
name: MyCustomPlugin
version: 1.0.0
main: com.example.MyPlugin
softdepend: [TNHoppers] # hoặc depend: [TNHoppers]
```

---

## ⚡ 2. Khởi tạo & Lấy API Instance

Bạn có thể truy xuất API theo **2 cách chuẩn**:

### Cách 1: Singleton trực tiếp (Nhanh nhất)
```java
import me.tainguyen.tnhoppers.api.TNHoppersAPI;

TNHoppersAPI api = TNHoppersAPI.get();
```

### Cách 2: Qua Bukkit ServicesManager (An toàn cho softdepend)
```java
import me.tainguyen.tnhoppers.api.TNHoppersAPI;
import org.bukkit.Bukkit;
import org.bukkit.plugin.RegisteredServiceProvider;

public class MyPluginHook {
    private TNHoppersAPI api;

    public void hook() {
        if (Bukkit.getPluginManager().isPluginEnabled("TNHoppers")) {
            RegisteredServiceProvider<TNHoppersAPI> rsp = 
                    Bukkit.getServicesManager().getRegistration(TNHoppersAPI.class);
            if (rsp != null) {
                this.api = rsp.getProvider();
                getLogger().info("Hooked into TNHoppers API successfully!");
            }
        }
    }
}
```

---

## 🛠️ 3. Các Tính Năng API Chi Tiết & Code Mẫu

### A. Kiểm tra Phễu & Lấy Thông Tin
```java
Block block = location.getBlock();

// 1. Kiểm tra khối có phải là phễu TNHoppers không
if (api.isCustomHopper(block)) {
    // 2. Lấy loại phễu: "NORMAL" hoặc "CHUNK"
    String type = api.getHopperType(block);
    
    // 3. Lấy UUID chủ sở hữu
    UUID owner = api.getHopperOwner(block);
    
    // 4. Lấy dữ liệu đầy đủ (Snapshot bất biến)
    api.getHopperData(block).ifPresent(data -> {
        boolean suctionOn = data.suctionEnabled();
        double radius = data.customRadius();
        int linkCount = data.destinations().size();
    });
}

// 5. Kiểm tra item trên tay người chơi
ItemStack handItem = player.getInventory().getItemInMainHand();
if (api.isCustomHopper(handItem)) {
    player.sendMessage("Bạn đang cầm 1 Phễu TNHoppers!");
}
if (api.isLinkWand(handItem)) {
    player.sendMessage("Bạn đang cầm Gậy Liên Kết (Link Wand)!");
}
```

---

### B. Tạo Vật Phẩm Phễu Cho Shop / Crate / Nhiệm Vụ (Item Generation)
```java
// Tạo Phễu Thường (Cấp 1 mặc định)
ItemStack normalHopper = api.createHopperItem("NORMAL");

// Tạo Phễu Chunk (Cấp 1 mặc định)
ItemStack chunkHopper = api.createHopperItem("CHUNK");

// Tạo Phễu VIP Max Cấp (Tốc độ 5, Số lượng 5, Liên kết 5, Khoảng cách 5, Kho ảo 4):
ItemStack vipHopper = api.createHopperItem("CHUNK", 5, 5, 5, 5, 4);
player.getInventory().addItem(vipHopper);

// Tạo Gậy Liên Kết Link Wand
ItemStack wand = api.createLinkWand();
player.getInventory().addItem(wand);
```

---

### C. Quản Lý Liên Kết Rương Không Dây (Wireless Links)
```java
Block hopper = ...;
Block chest = ...;

// Lấy danh sách vị trí tất cả rương đích đang liên kết
List<Location> linkedChests = api.getLinkedDestinations(hopper);

// Tạo liên kết mới bằng code (không cần dùng gậy)
boolean linked = api.linkContainer(hopper, chest);
if (linked) {
    player.sendMessage("§aĐã tự động liên kết rương đích thành công!");
}

// Hủy liên kết rương
boolean unlinked = api.unlinkContainer(hopper, chest);
```

---

### D. Nâng Cấp Phễu Bằng Code (Upgrades)
```java
Block hopper = ...;

// Lấy cấp độ nâng cấp hiện tại
int speed = api.getSpeedLevel(hopper);       // 1 -> 5
int amount = api.getAmountLevel(hopper);     // 1 -> 5
int links = api.getLinksLevel(hopper);       // 1 -> 5
int distance = api.getDistanceLevel(hopper); // 1 -> 5
int storage = api.getStorageLevel(hopper);   // 1 -> 4 (Kho ảo)

// Nâng cấp trực tiếp:
HopperUpgrades current = api.getUpgrades(hopper);
HopperUpgrades upgraded = current.withSpeedLevel(5).withStorageLevel(4);
api.setUpgrades(hopper, upgraded);
```

---

### E. Điều Khiển Bán Kính & Hút Đồ Rơi (Suction & Radius)
```java
Block hopper = ...;

// Bật / Tắt tính năng hút đồ
api.setSuctionEnabled(hopper, true);

// Lấy bán kính hút thực tế (block)
double currentRadius = api.getSuctionRadius(hopper);

// Đặt bán kính hút tùy chỉnh (Ví dụ: đặt 8 block)
api.setSuctionRadius(hopper, 8.0);
```

---

### F. Thao Tác Với Kho Lưu Trữ Ảo (Virtual Deep Storage: 10,000 - 250,000 Items)
```java
Block hopper = ...;
Block chest = ...;

// 1. Kiểm tra số lượng item đang lưu trữ trong kho ảo của liên kết này
long storedItems = api.getVirtualStorageCount(hopper, chest);

// 2. Lấy sức chứa tối đa theo cấp nâng cấp của phễu (10k, 50k, 100k, 250k)
int maxCapacity = api.getMaxStorageCapacity(hopper);

// 3. Nạp vật phẩm vào kho ảo bằng code
ItemStack diamonds = new ItemStack(Material.DIAMOND, 64);
int accepted = api.depositVirtualStorage(hopper, chest, diamonds);

// 4. Rút vật phẩm từ kho ảo vào túi đồ của người chơi
int withdrawn = api.withdrawVirtualStorage(player, hopper, chest, "DIAMOND", 64);

// 5. Xả vật phẩm từ kho ảo vào ô trống của rương vật lý
int drained = api.drainVirtualStorageToContainer(hopper, chest, 64);
```

---

## 📡 4. Lắng Nghe Sự Kiện (Custom Bukkit Events)

TNHoppers kích hoạt các sự kiện Bukkit tiêu chuẩn trong gói `me.tainguyen.tnhoppers.api.events.*`. Bạn có thể đăng ký lắng nghe hoặc hủy sự kiện (`setCancelled(true)`):

### 1. Sự kiện Liên kết (`HopperLinkEvent`)
```java
@EventHandler
public void onHopperLink(HopperLinkEvent event) {
    Block hopper = event.getHopper();
    Block chest = event.getDestination();
    Player player = event.getPlayer(); // Có thể null nếu do API gọi

    // Chặn người chơi liên kết nếu không đủ điều kiện
    if (player != null && !player.hasPermission("myplugin.allowlink")) {
        player.sendMessage("§cBạn không có quyền liên kết rương này!");
        event.setCancelled(true);
    }
}
```

### 2. Sự kiện Hút Đồ Rơi (`HopperSuctionEvent`)
```java
@EventHandler
public void onHopperSuction(HopperSuctionEvent event) {
    Item itemEntity = event.getItemEntity();
    ItemStack stack = event.getItemStack();

    // Không cho phễu hút Kim Cương rơi ra từ sự kiện đặc biệt
    if (stack.getType() == Material.DIAMOND) {
        event.setCancelled(true);
    }
}
```

### 3. Sự kiện Chuyển Đồ Không Dây (`HopperTransferEvent`)
```java
@EventHandler
public void onHopperTransfer(HopperTransferEvent event) {
    Block hopper = event.getHopper();
    Block destination = event.getDestination();
    ItemStack item = event.getItemStack();

    // Log hoặc can thiệp quá trình chuyển đồ
}
```

### 4. Sự kiện Tràn Đồ Vào Kho Ảo (`VirtualStorageOverflowEvent`)
```java
@EventHandler
public void onDeepStorageOverflow(VirtualStorageOverflowEvent event) {
    ItemStack item = event.getOverflowItem();
    long currentVirtualCount = event.getCurrentVirtualCount();
    int maxCap = event.getMaxCapacity();

    // Có thể huỷ để chặn đồ tràn vào kho ảo nếu rương đầy
}
```

### 5. Sự kiện Nâng Cấp Phễu (`HopperUpgradeEvent`)
```java
@EventHandler
public void onHopperUpgrade(HopperUpgradeEvent event) {
    HopperUpgradeEvent.UpgradeType type = event.getUpgradeType();
    int newLevel = event.getNewLevel();
    Player player = event.getPlayer();

    // Can thiệp hoặc thưởng quà cho người chơi khi nâng cấp
}
```
