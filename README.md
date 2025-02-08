# **Kiểm thử với JMeter**

## **1. Giới thiệu**
### **1.1. Giới thiệu chung**
Báo cáo kết quả kiểm thử hiệu suất của một trang web thương mại điện tử với công cụ Apache JMeter. Mục tiêu chính của kiểm thử là đánh giá thời gian phản hồi, thông lượng (Throughput) và tỷ lệ lỗi của hệ thống khi có nhiều người dùng truy cập đồng thời.

### **1.2. Nội dung kiểm thử**
Trang chủ của website thương mại điện tử: `https://demo.prestashop.com/#/en/front`

Quá trình kiểm thử bao gồm:
- **Kiểm thử tải (Load Testing):** Mô phỏng số lượng người dùng tăng dần từ 10 đến 50 và ghi nhận thời gian phản hồi trung bình cùng tỷ lệ lỗi.
- **Kiểm thử khả năng chịu tải (Stress Testing):** Tăng số người dùng lên 100 để xác định giới hạn chịu tải của hệ thống.
- **Phân tích kết quả:** Xuất báo cáo từ JMeter và đánh giá hiệu suất hệ thống.



## **2. Hướng dẫn cài đặt và thiết lập**
### **2.1. Cài đặt Apache JMeter**
- Tải JMeter từ [trang chủ](https://jmeter.apache.org/).
- Giải nén và chạy file `jmeter.bat` (Windows) hoặc `jmeter.sh` (Linux/Mac).

### **2.2. Thiết lập kiểm thử**
#### **Mô phỏng 50 người dùng trong 5 phút**
- Tạo **Thread Group**:
   - Number of Threads: **50**
   - Ramp-up Period: **60s**
   - Duration: **300s**
- Thêm **HTTP Request**:
   - Server Name: `demo.prestashop.com`
   - Method: **GET**
   - Path: `/#/en/front`
- Thêm **Listener**: Summary Report, View Results Tree, Aggregate Report.
- Chạy kiểm thử bằng cách nhấn **Start**.

#### **Kiểm thử tải (Load Testing)**
- Thiết lập Thread Group tăng dần từ **10 → 50** người dùng.
- Ghi nhận thời gian phản hồi trung bình và tỷ lệ lỗi.

#### **Kiểm thử khả năng chịu tải (Stress Testing)**
- Tăng số lượng người dùng lên **100** và xác định thời điểm hệ thống giảm hiệu suất.
- Theo dõi thông số Throughput và thời gian phản hồi.



## **3. Kết quả kiểm thử**
### **3.1. Phân tích thời gian phản hồi**
| Kiểm thử | Số mẫu | Thời gian phản hồi trung bình (ms) |  Min (ms) | Max (ms) | Độ lệch chuẩn (ms) |
|-|-|-|-|-|-|
| Load Testing 10 | 8420 | 356 | 294 | 2459 | 112.7 |
| Load Testing 20 | 16765 | 357 | 295 | 2673 | 116.1 |
| Load Testing 30 | 25244 | 356 | 295 | 2622 | 115.02 |
| Load Testing 40 | 33682 | 356 | 294 | 2650 | 115 |
| Load Testing 50 | 42059 | 356 | 294 | 2654 | 113.79 |
| Stress Testing 100 | 83990 | 357 | 295 | 10386 | 122.44 |

**Nhận xét:**
- Thời gian phản hồi trung bình rất ổn định (~356-357ms) dù số lượng người dùng tăng.
- Tuy nhiên, thời gian phản hồi tối đa (Max) trong Stress Testing (100 users) tăng mạnh lên 10,386ms, cho thấy hệ thống bị quá tải ở mức này.
- Độ lệch chuẩn (Std. Dev.) cũng tăng lên đáng kể khi kiểm thử 100 người dùng (122.44ms), phản ánh sự dao động lớn về hiệu suất.

### **3.2. Phân tích Throughput (Thông lượng)**
| Kiểm thử | Throughput (req/sec) |
|-|-|
| Load Testing 10 | 28.04 |
| Load Testing 20 | 55.80 |
| Load Testing 30 | 84.06 |
| Load Testing 40 | 112.16 |
| Load Testing 50 | 140.07 |
| Stress Testing 100 | 279.60 |

**Nhận xét:**
- Throughput tăng tuyến tính từ 10 → 50 users, cho thấy hệ thống vẫn hoạt động ổn.
- Ở 100 users, throughput tăng mạnh lên 279.6 req/sec nhưng có dấu hiệu bị giới hạn tài nguyên.
- Hệ thống vẫn có thể xử lý số lượng lớn request nhưng thời gian phản hồi bắt đầu dao động nhiều hơn.

### **3.3. Phân tích băng thông (Received/Sent KB/sec)**
| Kiểm thử | Received KB/sec | Sent KB/sec |
|-|-|-|
| Load Testing 10 | 103.6 | 6.68 |
| Load Testing 20 | 206.14 | 13.3 |
| Load Testing 30 | 310.56 | 20.03 |
| Load Testing 40 | 414.36 | 26.73 |
| Load Testing 50 | 517.48 | 33.38 |
| Stress Testing 100 | 1032.94| 66.62 |

**Nhận xét:**
- Lượng dữ liệu nhận/gửi tăng đều theo số lượng người dùng.
- Ở mức 100 users, Received KB/sec tăng gấp đôi so với 50 users, phản ánh việc tải nặng hơn lên hệ thống.

