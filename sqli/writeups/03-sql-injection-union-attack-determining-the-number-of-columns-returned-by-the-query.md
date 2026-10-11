- Dựa trên đề bài -> biết đây là lỗ hổng sqli
- Trong web xuất hiện 2 truy vấn database
  - truy vấn về catagory
  - Truy vấn đăng nhập tài khoản

- Sau đi vào đăng nhập tài khoản cho thấy các lỗi trả về hoàn toàn bình thường , không có dấu hiệu xuất hiện sqli
- Thử truy vấn catagory cho thấy những payload cơ bản như ' OR '1'='1' thì nó có xuất hiện ở trên thành tìm kiếm -> đây là một lỗ hồng sqli -> tiếp tục thử các payload về UNION colume bằng ' UNION SELECT NULL,NULL,NULL-- trả về kết quả cho thấy bảng đang được selct 3 cột
  -> đây là lời giải của lab
