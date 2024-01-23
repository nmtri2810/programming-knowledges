# Database

## Index

### Khái niệm index

- Là một **cấu trúc dữ liệu** dùng để **định vị và truy cập nhanh nhất** vào dữ liệu trong các bảng trong DB
- **Tối ưu hiệu suất** bằng việc giảm lượng truy cập vào bộ nhớ khi thực hiện truy vấn
- Giống như **mục lục** của 1 quyển sách
- VD: Giả sử có truy vấn : `SELECT * FROM User WHERE Name = 'Tri';`
  - Nếu không có `index`, query sẽ phải chạy qua tất cả row trong bảng để so sánh và lấy ra những row thỏa mãn
  - Nếu có `index`, nó sẽ trỏ đến địa chỉ dữ liệu trong bảng => nhanh hơn

### Cấu trúc

Gồm 2 cột:

- Cột `Search Key`: chứa bản sao các giá trị của cột được tạo index
- Cột `Data Reference`: Chứa con trỏ trỏ đến địa chỉ của bản ghi có giá trị cột index tương ứng

![image](https://media.geeksforgeeks.org/wp-content/uploads/20230620131119/Structure-of-an-Index-in-Database_1-(1).webp)

### Các loại index

#### B-Tree Index

- `index` trong B-Tree được tổ chức và lưu trữ theo dạng cây (khá giống cây nhị phân)
- Khác ở điểm:
  - 1 node có thể có nhiều value
  - Value của node cx sắp xếp theo thứ tự tăng dần
- Truy vấn trong B-Tree là quá trình đệ quy từ `Root node`, tìm tới `Child node` và `Leaf node` đến khi tìm được dữ liệu thỏa mãn. Child nodes sẽ nằm bên trái value `X` nào đó khi value của node nhỏ hơn X, ngược lại sẽ nằm bên phải nếu có values lớn hơn.
- Cơ chế:...
- Tăng tốc truy vấn với các toán tử `= , >, <, LIKE, IN, ORDER BY`

#### Hash Index

- Tổ chức dưới dạng key-value với:
  - `key`: kết quả hash value của column được đánh `index`
  - `value`: con trỏ đến chính xác row tương ứng
- Hash index rất mạnh mẽ khi thực hiện các phép truy vấn `=, <> (IN, NOT IN)`, ngược lại với các phép truy vấn `>, <, LIKE, ORDER BY`

### Ưu nhược điểm của index

#### Ưu điểm

- Tăng tốc độ truy vấn, tối ưu hiệu suất

#### Nhược điểm

- Tốn thêm bộ nhớ lưu trữ
- Giảm tốc độ insert, update, delete

### Quy định chung khi đánh index

- Chỉ nên sử dụng `index` với những bảng có dữ liệu vừa và lớn + thường xuyên query trên đó
- Nên đánh `index` cho các column thương xuyên sử dụng trong mệnh đề `WHERE` hoặc `ORDER BY`
- Không nên đánh `index` cho column nhiều value trùng lặp hoặc NULL. VD: column `gender`
- Trường hợp sử dụng nhiều columns trong một `index` thì cần chú ý đến thứ tự các column

## Transaction

### Khái niệm trasaction

- Là một tập hợp các thao tác mà ở đó hoặc là tất cả các thao tác đó được thực hiện thành công, hoặc là không một thao tác nào được thực hiện thành công cả
- VD: transaction chuyển tiền từ A sang B

### Tính chất của transaction

- Atomicity - Tính đơn vị: Nguyên tắc "All or nothing"
- Consistency - Tính nhất quán: Dữ liệu từ thời điểm start transaction với lúc kết thúc phải nhất quán
- Isolation - Tính độc lập: Nếu có nhiều transaction cùng một lúc thì phải đảm bảo là các transaction này xảy ra độc lập, không tác động lẫn nhau trên dữ liệu
- Durability - Tính bền vững: Dữ liệu thay đổi đã được cố định, không có chuyện có thể chuyển lại trạng thái dữ liệu lúc trước khi thực hiện transaction

### Deadlock

- `Deadlock` là một tình trạng xảy ra khi hai hoặc nhiều `transactions` đang cố gắng truy cập các tài nguyên theo một thứ tự không đồng nhất, dẫn đến tình trạng đứng im vĩnh viễn và không thể tiếp tục
- VD:
  - Transaction A giữ tài nguyên X và yêu cầu tài nguyên Y
  - Transaction B giữ tài nguyên Y và yêu cầu tài nguyên X

### Table lock và row lock

## Stored Procedure và Function

### Cơ bản

- Cả 2 đều là những đối tượng CSDL chứa một tập lệnh SQL để hoàn thành một tác vụ nào đó
- Đều có thể sử dụng lại nhiều lần

### Phân biệt

- **Giá trị trả về**: `Stored Procedure` không nhất thiết phải có giá trị trả về hoặc trả về 1 hoặc nhiều gtri, trong khi `function` phải trả về 1 gtri duy nhất
- **Tham số**: `Stored Procedure` có thể chứa cả tham số input và output, `function` thì chỉ có tham số input
- **Gọi nhau**: `Stored Procedure` có thể gọi `function`, ngược lại thì không
- **Sử dụng `try-catch, stored procedure`**: `Stored Procedure` có thể sd, `function` thì không
- **Sử dụng trong `SELECT, WHERE, HAVING`**: `Stored Procedure` không được sd vì có thể có nhiều gtri trả về, `function` thì có thể

## Trigger

### Khái niệm Trigger

- Trigger là một khối mã SQL được kích hoạt tự động khi một sự kiện đã được xác định xảy ra. Các sự kiện này có thể bao gồm việc `INSERT, UPDATE, DELETE` bản ghi trong một bảng
-Trigger thường được sử dụng để thực hiện các hành động như kiểm tra tính toàn vẹn dữ liệu, ghi nhật ký (audit), hoặc thực hiện logic kinh doanh phức tạp khi một điều kiện cụ thể được đáp ứng

### Các loại Trigger

- Trigger trước, trigger sau

## Chuẩn hóa dữ liệu

### 1NF

- Một bảng ở dạng `1NF` khi giá trị ở các cột là duy nhất (loại bỏ các thuộc tính đa trị)

### 2NF

- Đã ở dạng 1NF
- Các thuộc tính không khoá phải phụ thuộc hàm đầy đủ vào khoá chính

### 3NF

- Đã ở dạng 2NF
- Các thuộc tính không khoá phải phụ thuộc trực tiếp vào khoá chính

### BCNF

- Đã ở dạng 3NF
- Không có thuộc tính khóa mà phụ thuộc hàm vào thuộc tính không khóa

## Các loại quan hệ

- Quan hệ 1-1, 1-n, n-n

## Các loại join

- JOIN (INNER JOIN), LEFT JOIN, RIGHT JOIN, CROSS JOIN

![join](https://i.stack.imgur.com/1Tfy0.jpg)

## UNION và UNION ALL

- Cả 2 đều được sử dụng để kết hợp 2 hoặc nhiều câu lệnh `SELECT`
- Yêu cầu: nói đơn giản là cột của 2 bảng phải trùng nhau về số lượng, thứ tự, data types
- `UNION`: loại bỏ các bản ghi trùng lặp
- `UNION ALL`: bao gồm tất cả bản ghi, nhanh hơn `UNION`

![union](https://i.stack.imgur.com/phA2q.jpg)

## Delete và truncate

- `TRUNCATE` không thể chạy được khi bảng bạn định xóa có `foreign_key`

### DELETE

- Là `Data Manipulation Language`
- Có thể có điều kiện hoặc xóa toàn bộ bảng
- Khi chạy lệnh `DELETE` thì SQL sẽ log lại từng dòng đã xóa vào `transaction log`, vì thế nên khi bạn tạo 1 record mới, giá trị của id sẽ không bắt đầu từ `1` mà sẽ có giá trị `n+1` với `n` là giá trị của record cuối cùng được tạo => chậm hơn, restored được

### TRUNCATE

- Là `Data Definition Language`
- Xóa toàn bộ dữ liệu của bảng
- Khi chạy lệnh `TRUNCATE` thì SQL sẽ xóa hết dữ liệu của bảng và reset `transaction log`, vì thế khi tạo 1 record mới, giá trị của id sẽ bắt đầu từ `1` => nhanh hơn, không restored được

![sql command](https://media.geeksforgeeks.org/wp-content/uploads/20210920153429/new.png)
