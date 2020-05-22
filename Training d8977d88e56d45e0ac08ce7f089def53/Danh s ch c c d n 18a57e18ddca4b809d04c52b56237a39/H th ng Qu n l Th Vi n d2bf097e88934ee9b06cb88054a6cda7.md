# Hệ thống Quản lý Thư Viện

# 1. Giới thiệu

- Là một hệ thống gồm nhiều thư viện được xây dựng với mục đích phi lợi nhuận.

Quy trình xây dựng một thư viện như sau:

- Thư viện CodeGym khảo sát tại cơ sở
- Thư viện CodeGym cung cấp hạ tầng, sách, máy tính... tại cơ sở
- Các cơ sở (thư viện) chịu trách nhiệm quản lý tại cơ sở một cách độc lập

Bài toán Thư viện CodeGym đang gặp: **Không biết hiệu quả việc triển khai tại cơ sở**

- Không biết số lượng, tình trạng sách còn lại;
- Không biết sách nào được mượn nhiều;
- Không biết tần suất sách được mượn / trả;
- ...

# 2. Yêu cầu

- Sử dụng template: [https://adminlte.io/](https://adminlte.io/) (hoặc tự chọn một template tương tự)
- Sử dụng Trello để quản lý công việc
- Báo cáo công việc hằng ngày qua Slack

# 3. Định nghĩa hoàn thành

Một chức năng chỉ được coi là Done khi đảm bảo quy chuẩn thiết kế:

- Sử dụng các thiết kế của template
- Mọi tác vụ nhập/chỉnh sửa phải có thông báo thành công hoặc lỗi khi dữ liệu được lưu dưới dạng popup (modal hoặc dialog)

# 4. Chức năng chính

## Quản lý người dùng

Có 2 đối tượng người dùng chính:

- Quản lý hệ thống: tạo ra thư viện , tạo thủ thư , xem các báo cáo tổng thể
- Thủ thư: Phụ trách công việc từng thư viện

## Quản lý thư viện

Mỗi thư viện được quản lý bởi 1 thủ thư.

## Quản lý sách

Sách được quản lý tại các thư viện theo:

- Danh mục
- Tình trạng
- ISBN
Sử dụng chính mã vạch mặc định của NXB như định danh sách.

## Quản lý mượn / trả

Việc mượn trả cần được ghi nhận:

- Học sinh mượn: Họ tên, mã HS, trường, lớp
- Ngày mượn / ngày trả
- Tình trạng sách khi mượn / trả (Mới, Cũ, Hỏng)

## Thống kê / báo cáo

Báo cáo dành cho quản trị viên của toàn hệ thống thư viện CodeGym:

- Những đầu sách được mượn nhiều
- Thư viện nào đang hoạt động hiệu quả