# Phân tích vi phạm SOLID trong ClubManager

## 1. Single Responsibility Principle (SRP)

- **Vị trí:** Các phương thức tạo câu lạc bộ, quản lý thành viên, tạo sự kiện và lập báo cáo trong lớp `ClubManager`.
- **Vi phạm:** Lớp `ClubManager` đảm nhận nhiều trách nhiệm nghiệp vụ khác nhau thay vì chỉ có một lý do để thay đổi.
- **Hậu quả:** Khi thay đổi chức năng quản lý thành viên hoặc báo cáo, lớp này có thể phải sửa nhiều phần code, làm tăng độ phức tạp và nguy cơ phát sinh lỗi.
- **Hướng khắc phục:** Tách trách nhiệm thành `ClubService`, `EventService` và `ReportService`.

## 2. Open/Closed Principle (OCP)

- **Vị trí:** Đoạn điều kiện `if/else` lựa chọn kênh thông báo trong phần xử lý thông báo của `ClubManager`.
- **Vi phạm:** Khi bổ sung một kênh thông báo mới, chẳng hạn SMS, chương trình phải sửa đoạn điều kiện hiện có.
- **Hậu quả:** Việc mở rộng chức năng có thể ảnh hưởng đến code đã hoạt động và làm tăng nguy cơ lỗi.
- **Hướng khắc phục:** Sử dụng `INotificationService` cùng `NotificationServiceFactory` để tạo dịch vụ thông báo phù hợp với từng kênh.

## 3. Liskov Substitution Principle (LSP)

- **Vị trí:** Cần đối chiếu các lớp triển khai interface trong phiên bản code ban đầu để xác định trường hợp cụ thể.
- **Vi phạm:** Chưa đủ bằng chứng từ code hiện có để khẳng định một lớp triển khai vi phạm LSP. Vi phạm chỉ xảy ra khi lớp triển khai không tuân thủ hợp đồng hành vi của interface hoặc lớp cơ sở.
- **Hậu quả nếu xảy ra:** Có thể thay thế một implementation bằng implementation khác nhưng chương trình hoạt động sai hoặc phát sinh ngoại lệ ngoài dự kiến.
- **Hướng khắc phục:** Quy định hợp đồng rõ ràng cho các interface và bảo đảm mọi lớp triển khai tuân thủ hợp đồng đó.

## 4. Interface Segregation Principle (ISP)

- **Vị trí:** Cần kiểm tra các interface trong phiên bản code ban đầu để xác định interface nào chứa những phương thức mà lớp triển khai không cần sử dụng.
- **Vi phạm:** Chưa đủ bằng chứng để khẳng định interface cụ thể nào vi phạm ISP. Vi phạm xảy ra khi một lớp buộc phải phụ thuộc hoặc triển khai những phương thức không liên quan đến chức năng của nó.
- **Hậu quả nếu xảy ra:** Các lớp phụ thuộc vào những chức năng không cần thiết, khiến code khó bảo trì và thay đổi.
- **Hướng khắc phục:** Chia interface lớn thành các interface nhỏ theo từng nhóm trách nhiệm, chẳng hạn `IClubRepository`, `IEventRepository` và `IMemberRepository`.

## 5. Dependency Inversion Principle (DIP)

- **Vị trí:** Các đoạn code trong `ClubManager` khởi tạo hoặc sử dụng trực tiếp những lớp triển khai cụ thể để lưu trữ dữ liệu, ghi log hoặc gửi thông báo.
- **Vi phạm:** Lớp nghiệp vụ phụ thuộc trực tiếp vào chi tiết triển khai thay vì phụ thuộc vào abstraction.
- **Hậu quả:** Khó thay thế thành phần hạ tầng, khó kiểm thử độc lập và khó mở rộng chương trình.
- **Hướng khắc phục:** Sử dụng interface và constructor injection để truyền các dependency vào service, chẳng hạn `IClubRepository`, `IAppLogger` và `INotificationService`.
