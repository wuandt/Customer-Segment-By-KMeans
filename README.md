## Dự án phân cụm khách hàng với thuật toán K-Means

1. **Bối cảnh và mục đích**:  
   Bạn được tuyển dụng vào một chuỗi khách sạn cao cấp để phân tích hiệu suất kinh doanh dựa trên dữ liệu đặt phòng từ tháng 3 năm 2023 đến đầu tháng 2 năm 2025. Ban quản lý khách sạn đang đối mặt với nhiều vấn đề kinh doanh và họ cần bạn tìm ra insights quan trọng của từng nhóm khách hàng để cải thiện trải nghiệm khách hàng và tối ưu doanh thu.

2. **Tổng quan dữ liệu**:  
   Bộ dữ liệu mô phỏng hệ thống đặt phòng của khách sạn gồm 6 bảng dữ liệu quan trọng.

   | Tên bảng       | Mô tả                                  |
   |--------------|----------------------------------------|
   | Customers    | Danh sách khách hàng                  |
   | Rooms        | Thông tin các phòng trong khách sạn   |
   | Bookings     | Lịch sử đặt phòng của khách hàng      |
   | Payments     | Các khoản thanh toán của khách       |
   | Services     | Dịch vụ bổ sung mà khách sạn cung cấp |
   | Service_Usage | Dữ liệu khách đã sử dụng dịch vụ nào |

**Bảng Customers**
- Chứa thông tin về khách hàng đã từng đặt phòng trong khách sạn.
- Khóa chính: 'customer_id`.
- Mối quan hệ: Liên kết với `Bookings` (Mỗi khách hàng có thể có nhiều đặt phòng).
| Tên Cột          | Kiểu Dữ Liệu          | Mô Tả                                |
|------------------|----------------------|--------------------------------------|
| `customer_id`    | `INTEGER (PK)`       | ID khách hàng (Primary Key)          |
| `full_name`      | `VARCHAR(255)`       | Họ và tên khách hàng                 |
| `email`          | `VARCHAR(255)`       | Email khách hàng (duy nhất)         |
| `phone`          | `VARCHAR(20)`        | Số điện thoại                        |
| `created_at`     | `TIMESTAMP`          | Ngày khách hàng đăng ký             |


