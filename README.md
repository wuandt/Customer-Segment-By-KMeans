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

| **Tên Cột**     | **Kiểu Dữ Liệu**     | **Mô Tả**                            |
|----------------|----------------------|--------------------------------------|
| `customer_id`  | `INTEGER (PK)`       | ID khách hàng (Primary Key)          |
| `full_name`    | `VARCHAR(255)`       | Họ và tên khách hàng                 |
| `email`        | `VARCHAR(255)`       | Email khách hàng (duy nhất)         |
| `phone`        | `VARCHAR(20)`        | Số điện thoại                        |
| `created_at`   | `TIMESTAMP`          | Ngày khách hàng đăng ký              |

**Bảng Rooms**
- Lưu thông tin về các phòng khách sạn
- Khóa chính: 'room_id'
- Mối quan hệ: Liên kết với Bookings (Mỗi phòng có thể được đặt nhiều lần)

| **Tên Cột**        | **Kiểu Dữ Liệu**      | **Mô Tả**                                                                 |
|--------------------|------------------------|--------------------------------------------------------------------------|
| `room_id`          | `INTEGER (PK)`         | ID phòng (Primary Key)                                                   |
| `room_number`      | `VARCHAR(10)`          | Số phòng                                                                 |
| `room_type`        | `VARCHAR(50)`          | Loại phòng (Standard, Deluxe, Suite, Executive, Presidential)           |
| `price_per_night`  | `DECIMAL(10,2)`        | Giá mỗi đêm                                                              |
| `status`           | `VARCHAR(20)`          | Trạng thái phòng (`Available`, `Booked`)                                |

**Bảng Bookings**
- Ghi nhận lịch sử đặt phòng của khách
- Khóa chính: 'booking_id'
- Mối quan hệ:
  + customer_id liên kết với Customers
  + room_id liên kết với Rooms
  + booking_id liên kết với Payments & Service_Usage

| **Tên Cột**     | **Kiểu Dữ Liệu**     | **Mô Tả**                                                                 |
|------------------|----------------------|--------------------------------------------------------------------------|
| `booking_id`     | `INTEGER (PK)`       | ID đặt phòng (Primary Key)                                               |
| `customer_id`    | `INTEGER (FK)`       | ID khách hàng (Foreign Key từ `Customers`)                               |
| `room_id`        | `INTEGER (FK)`       | ID phòng khách sạn (Foreign Key từ `Rooms`)                              |
| `check_in`       | `DATE`               | Ngày check-in                                                             |
| `check_out`      | `DATE`               | Ngày check-out                                                            |
| `status`         | `VARCHAR(20)`        | Trạng thái (`Confirmed`, `Cancelled`, `Pending`)                         |
| `created_at`     | `TIMESTAMP`          | Ngày đặt phòng                                                            |

**Bảng Payment**
- Ghi nhận các khoản thanh toán của khách hàng
- Khóa chính: 'payment_id'
- Mối quan hệ:
  + booking_id liên kết với Bookings (Mỗi đặt phòng có thể có nhiều thanh toán)

| **Tên Cột**       | **Kiểu Dữ Liệu**     | **Mô Tả**                                                                 |
|-------------------|----------------------|--------------------------------------------------------------------------|
| `payment_id`      | `INTEGER (PK)`       | ID thanh toán (Primary Key)                                              |
| `booking_id`      | `INTEGER (FK)`       | ID đặt phòng (Foreign Key từ `Bookings`)                                 |
| `amount`          | `DECIMAL(10,2)`      | Số tiền thanh toán                                                        |
| `payment_method`  | `VARCHAR(50)`        | Phương thức thanh toán (`Credit Card`, `Cash`, `PayPal`, `Bank Transfer`, `Crypto`) |
| `payment_date`    | `TIMESTAMP`          | Ngày thanh toán                                                           |

**Bảng Services**
- Chứa danh sách các dịch vụ bổ sung trong khách sạn
- Khóa chính: 'service_id'
- Mối quan hệ: Liên kết với Service_Usage (Mỗi dịch vụ có thể được khách hàng sử dụng nhiều lần)

| **Tên Cột**     | **Kiểu Dữ Liệu**     | **Mô Tả**                                                                 |
|------------------|----------------------|--------------------------------------------------------------------------|
| `service_id`     | `INTEGER (PK)`       | ID dịch vụ (Primary Key)                                                 |
| `service_name`   | `VARCHAR(255)`       | Tên dịch vụ (`Breakfast`, `Laundry`, `Spa`, `Gym`, `Airport Pickup`, `Room Service`, `Mini Bar`, `Tour Package`, `VIP Lounge`) |
| `price`          | `DECIMAL(10,2)`      | Giá dịch vụ                                                              |

**Bảng Service_Usage**
- Ghi nhận các dịch vụ mà khách hàng đã sử dụng trong khách sạn
- Khóa chính: 'usage_id'
- Mối quan hệ:
  + booking_id liên kết với Bookings
  + service_id liên kết với Services

| **Tên Cột**     | **Kiểu Dữ Liệu**     | **Mô Tả**                                                                 |
|------------------|----------------------|--------------------------------------------------------------------------|
| `usage_id`       | `INTEGER (PK)`       | ID sử dụng dịch vụ (Primary Key)                                         |
| `booking_id`     | `INTEGER (FK)`       | ID đặt phòng (Foreign Key từ `Bookings`)                                 |
| `service_id`     | `INTEGER (FK)`       | ID dịch vụ (Foreign Key từ `Services`)                                   |
| `quantity`       | `INTEGER`            | Số lượng sử dụng                                                         |
| `total_price`    | `DECIMAL(10,2)`      | Tổng tiền = `quantity * price`                                           |



