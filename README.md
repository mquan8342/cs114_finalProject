<p align="center">
  <a href="https://www.uit.edu.vn/" border="none">
    <img src="https://i.imgur.com/WmMnSRt.png" border="none">
  </a>
</p>

<h1 align="center">
  <b>
	BÁO CÁO ĐỒ ÁN <br> MÁY HỌC
  </b>
</h>

## GIỚI THIỆU MÔN HỌC
* **Tên môn học:** Máy Học
* **Mã môn học:** CS114
* **Mã lớp:** CS114.N11.KHCL
* **Năm học:** HK1 (2022-2023)
* **Giảng viên:**
  + **Lý thuyết:** PGS-TS. Lê Đình Duy - duyld@uit.edu.vn
  + **Thực hành:** Ths. Phạm Nguyễn Trường An - truonganpn@uit.edu.vn

---

## GIỚI THIỆU NHÓM

| STT | MSSV | Họ và Tên | Email |
| :-: |:----:| :-------: | :----:|
| 1 | 20520707 | Huỳnh Minh Quân | 20520707@gm.uit.edu.vn |
| 2 | 21521973 | Trương Minh Đức | 21521973@gm.uit.edu.vn |

---

## **Đề tài: Nhận diện và đếm số lượng phương tiện di chuyển trên một đoạn đường trong video.**

**1. Mô tả bài toán:**
+ **Input**: Video có các phương tiện giao thông đang lưu thông trên đường.
+ **Output**: Video với từng frame đã được vẽ thêm bounding box trên các phương tiện nhận diện được và số lượng phương tiện ra/vào qua mốc được định sẵn từ trước theo 2 chiều ***in*** (di chuyển từ trên xuống), ***out*** (di chuyển từ dưới lên).
+ **Ngữ cảnh ứng dụng**:
  Có thể ứng dụng vào một số tác vụ như:
  + Hỗ trợ tự động phát hiện ùn tắc giao thông.
  + Đo lưu lượng xe tại một thời điểm, lưu lượng xe trung bình trong một khoảng thời gian.
  + Thống kê số lượng xe đi ngược chiều.

**2. Mô tả về bộ dữ liệu:**
+ [Dataset](https://github.com/mquan8342/cs114_finalProject/tree/main/yolo_data)
+ Cách thức xây dựng bộ dữ liệu:
  + Tự xây dựng dữ liệu với video được quay trực tiếp từ cầu đi bộ ở các địa điểm: khu vực lân cận hầm Thủ Thiêm; bến xe Miền Tây; đường Võ Văn Kiệt.
  + Chất lượng video: độ phân giải HD, 30fps.
  + Ảnh lấy được từ data:
![example](/yolo_data/images/train/VID_20230115_170408_1.png)
+ Số lượng ảnh: **1092 ảnh**
+ Các thao tác tiền xử lý dữ liệu: 
  + Tách ảnh từ video, mỗi 90 frames lưu 1 ảnh.
  + Sử dụng tool [labelimg](https://github.com/heartexlabs/labelImg) để tạo label cho các đối tượng trong ảnh.
  + Ứng với mỗi ảnh là một file `.txt` có cùng tên với ảnh, chứa thông tin của label theo format của YOLO.
  + Nội dung file `.txt` bao gồm: `class_id` `center_x` `center_y` `width` `height`
    + Ví dụ: `0 0.446875 0.288889 0.015625 0.072222`

  + Label format:
    + class_id: Nhãn của đối tượng trong Bounding Box: [0: motorbike, 1: car, 2: truck, 3: bus]
    + center_x, center_y: Tọa độ trung tâm của Bounding Box
    + width: Chiều rộng của Bounding Box
    + height: Chiều cao của Bounding Box

 + Phân chia train/valid theo tỉ lệ 9/1:
   + Training set: **983 ảnh**
   + Validation set: **109 ảnh**
 + Số lượng đối tượng mỗi class:
    + Motorbike: 9222
    + Car: 3728
    + Truck: 182
    + Bus: 394

  + Training config:
    + YOLO model image size: 640x640
    + Batch size: 16
    + Epochs: 150
  
**3. Mô tả về thuật toán:** Sử dụng mô hình [YOLOv8](https://github.com/ultralytics/ultralytics) để nhận dạng các phương tiện đang lưu thông, kết hợp với thư viện [ByteTrack](https://github.com/ifzhang/ByteTrack.git) để đếm số lượng phương tiện di chuyển qua một đoạn đường.
     
**4. Đánh giá kết quả:**
***Thông tin các video được dùng để so sánh kết quả:***
| Dataset | Name | Resolution | Ground truth | Description |
| :-----: | :--: | :--------: | :----------: | :---------: |
| GRAM-RTM | M-30 | 480x800 | 77 | Sunny day |
| GRAM-RTM | M-30-HD | 720x1200 | 42 | Cloudy day |

***Kết quả khi so sánh với Yang et al. , Abdelwahab và Adson M. Santos***
| Author | Video name | Ground truth | Counted | Precision (%) |
| :----: | :--------: | :----------: | :-----: | :-----------: |
| Yang et al. | M-30 | 77 | 71 | 92.21 |
| Abdelwahab | M-30 | 77 | 72 | 93.51 |
| Adson M. Santos | M-30 | 77 | 78 | 98.7 |
| Ours | M-30 | 77 | 75 | 97.4 |
| Yang et al. | M-30-HD | 42 | 37 | 90.03 |
| Abdelwahab | M-30-HD | 42 | 42 | 100 |
| Adson M. Santos | M-30-HD | 42 | 42 | 100 |
| Ours | M-30-HD | 42 |  41| 97.62 |

$Precision = (1 - \frac{|Ground truth - Counted|}{Ground truth}) \times 100$

**5. Kết luận**
  + Nhận diện và đếm xe tự động bằng máy học có thể hỗ trợ con người ở một số tác vụ liên quan đến giao thông.  Việc thu thập dữ liệu không quá khó khăn, có thể thu thập bằng nhiều cách khác nhau. Số lượng xe máy lớn và giao thông không trật tự ở Việt Nam khiến việc gán nhãn xe máy là một thử thách không dễ. Bên cạnh đó, vẫn còn một số hạn chế khác trong quá trình hoàn thiện đồ án này.

  + Hạn chế
    + Kết quả dự đoán dễ bị ảnh hưởng bởi các yếu tố bên ngoài như ánh sáng, thời tiết.
    + Một số loại phương tiện ít xuất hiện, gây khó khăn cho việc lấy mẫu và tạo thêm class.
    + Mất cân bằng dữ liệu do xe máy là phương tiện chính để đi lại của người dân Việt Nam.
    + Xe máy khó gán nhãn chính xác, dễ bị nhiễu nhất nhưng chiếm số lượng nhiều nhất.
    + Phần cứng không đáp ứng được các cài đặt cao hơn trong quá trình training.

  + Hướng phát triển
    + Thu thập thêm của các phương tiện khác ngoài xe máy như xe tải, xe bus để giảm mất cân bằng dữ liệu.
    + Thêm các class mới như xe đạp, xe ba bánh và một số phương tiện ít phổ biến.
    + Thêm chức năng thống kê theo từng class để có thêm thông tin về mật độ lưu thông của các loại phương tiện khác nhau.

**6. Phân công**
  + **Thu thập dữ liệu:**
    + Huỳnh Minh Quân: 9 video (3.31GB), tổng thời lượng 00:39:32
    + Trương Minh Đức: 7 video (380MB, tổng thời lượng 00:27:34
    
    [Videos](https://drive.google.com/file/d/1VZj-VXvslpbcJLhzN8Kv7rLtNmAoMs5a/view?usp=share_link)
  
  + **Gán nhãn dữ liệu:**
    + Huỳnh Minh Quân: 1027 ảnh
      + Motorbike:  8062 instance
      + Car: 3251 instance
      + Truck: 182 instance
      + Bus: 374 instance
      
    + Trương Minh Đức: 65 ảnh
      + Motorbike:  620 instance
      + Car: 447 instance
      + Truck: 0 instance
      + Bus: 20 instance
    
  + **Code:** Huỳnh Minh Quân, Trương Minh Đức
  
  + **Viết báo cáo:** Huỳnh Minh Quân 

**7. Tham khảo**
  + Counting Vehicle with High-Precision in Brazilian Roads Using YOLOv3 and Deep SORT - 2020 33rd SIBGRAPI Conference on Graphics, Patterns and Images (SIBGRAPI)
  + [YOLOv8](https://github.com/ultralytics/ultralytics/)
  + [ByteTrack](https://github.com/ifzhang/ByteTrack.git) Multi-Object Tracking by Associating Every Detection Box – ECCV2022
  
