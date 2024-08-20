# Cơ sở dữ liệu
## CSDL, CSDL quan hệ
### CSDL
**Cơ sở dữ liệu (Database)** là một tập hợp các dữ liệu có tổ chức liên quan đến nhau, thường được lưu trữ và truy cập điện tử từ hệ thống máy tính.

**Hệ quản trị cơ sở dữ liệu (DBMS -Database Management System)** là một hệ thống phần mềm giúp doanh nghiệp tổ chức, quản lý và sử dụng dữ liệu hiệu quả. DBMS cung cấp giao diện giữa cơ sở dữ liệu và người dùng hoặc các ứng dụng để thực hiện các thao tác như tạo, truy vấn, sửa đổi và xóa dữ liệu.
### CSDL quan hệ
**Cơ sở dữ liệu quan hệ** là một loại cơ sở dữ liệu lưu trữ cung cấp quyền truy cập vào các điểm dữ liệu có liên quan đến nhau. Cơ sở dữ liệu quan hệ dựa trên mô hình dữ liệu quan hệ, một cách trực quan, đơn giản để biểu diễn dữ liệu trong bảng.

Trong cơ sở dữ liệu quan hệ, mỗi hàng trong bảng là một bản ghi với một ID duy nhất được gọi là khóa. Các cột của bảng chứa các thuộc tính của dữ liệu và mỗi bản ghi thường có một giá trị cho mỗi thuộc tính, giúp dễ dàng thiết lập mối quan hệ giữa các điểm dữ liệu.
#### Table
Bảng (Table) là thành phần chính trong CSDL quan hệ. Trong bảng sẽ chứa những thông tin như:
![](imgs/table.jpg)

- Field (Cột/Trường): là trường dữ liệu thể hiện các thuộc tính của bảng. Chẳng hạn như: tên, địa chỉ…vv.
- Row (dòng): là dòng dữ liệu gồm các thông tin dữ liệu liên quan với nhau gọi là bảng record ( bảng ghi).
- Cell (ô): là các ô giao giữa các dòng và cột là nơi để chứa các dữ liệu.
- Primary Key (Khóa chính): là một hoặc nhiều trường được gộp lại để định nghĩa bảng ghi. Không được trùng và cũng không được để trống. Lấy ví dụ đơn giản để bạn hình dung giá trị 1 của trường customer ID thể hiện cho tất cả dữ liệu của dòng đầu tiên. Hay nói gọn là tất cả các giá trị của dòng đầu tiên là thuộc trường customer ID = 1.

Khóa chính có thể có hoặc không trong bảng nhưng để thuận tiện và dễ dàng quản lý thường người ta sẽ đinh nghĩa khóa chính cho bảng.
## SQL
SQL (Structured Query Language) là ngôn ngữ truy vấn có cấu trúc. Nó là một ngôn ngữ, là tập hợp các lệnh để tương tác với cơ sở dữ liệu. Dùng để lưu trữ, thao tác và truy xuất dữ liệu được lưu trữ trong một cơ sở dữ liệu quan hệ. Trong thực tế, SQL là ngôn ngữ chuẩn được sử dụng hầu hết cho hệ cơ sở dữ liệu quan hệ. Tất cả các hệ thống quản lý cơ sở dữ liệu quan hệ (RDMS) như MySQL, MS Access, Oracle, Postgres và SQL Server… đều sử dụng SQL làm ngôn ngữ cơ sở dữ liệu chuẩn.

### Các câu lệnh trong SQL
#### Tạo một database
```java
CREATE DATABASE <database_name>;
```
#### Xóa một database
```
DROP DATABASE <database_name>;
```
#### Tạo table
Tạo bảng mới:
```
CREATE TABLE <table_name>(
    column1 datatype,
    column2 datatype,
    ...
);
```
Tạo bảng từ một bảng khác:
```java
CREATE TABLE new_table_name AS
    SELECT column1, column2,...
    FROM existing_table_name
    WHERE ....;
```
#### Xóa table
```java
DROP TABLE <table_name>;
```
#### Insert
```java
INSERT INTO <table_name> VALUES('column1data','column2data','column3data',...);
```
#### Delete
```java
DELETE FROM <table_name> WHERE column_name = "data";
```
#### Update
```java
UPDATE mytable SET column1="mydata" WHERE column2="mydata";
```
#### Câu lệnh Select
SỬ dụng để chọn dữ liệu từ trong database.

Kết quả trả về được lưu trong một bảng kết quả, gọi là 'result-set'.

- Cú pháp cơ bản:
```java
SELECT column1, column2, ...
FROM <table_name>;
```
- Nếu muốn chọn toàn bộ các trường, sử dụng:
```java
SELECT * FROM <table_name>;
```
- Để chọn chỉ những giá trị riêng biệt, sử dụng:
```java
SELECT DISTINCT column1, column2, ...
FROM table_name;
```
#### Điều kiện WHERE
Mệnh đề WHERE được sử dụng để lọc các bản ghi.

Nó được sử dụng để chỉ trích xuất những bản ghi đáp ứng một điều kiện cụ thể.
```java
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```
Ví dụ:
```java
SELECT * FROM Customers
WHERE Country = 'Mexico';
```
Các toán tử dùng trong điều kiện WHERE:
![](imgs/where condition.png)

- `WHERE` cũng có thể kết hợp với `AND, OR, NOT`

`AND`:
```java
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND condition2 AND condition3 ...;
```
`OR`:
```java
SELECT column1, column2, ...
FROM table_name
WHERE condition1 OR condition2 OR condition3 ...;
```

`NOT`:
```java
SELECT column1, column2, ...
FROM table_name
WHERE NOT condition;
```
#### Từ khóa ORDER BY
Từ khóa `ORDER BY` được sử dụng để sắp xếp tập kết quả theo thứ tự tăng dần hoặc giảm dần.

Từ khóa này mặc định sẽ sắp xếp các bản ghi theo thứ tự tăng dần, nếu muốn sắp xếp giảm dần, sử dụng thêm từ khóa `DESC`.

```java
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;
```
Nếu các hàng có giá trị column1 bằng nhau thì sẽ sắp xếp theo giá trị ở column2,...

#### Mệnh đề LIMIT
Mệnh đề LIMIT được sử dụng để chỉ định số lượng bản ghi cần trả về.

Mệnh đề LIMIT hữu ích trên các bảng lớn có hàng nghìn bản ghi. Trả lại một số lượng lớn hồ sơ có thể ảnh hưởng đến hiệu suất.

```java
SELECT column_name(s)
FROM table_name
LIMIT number;
```
Nếu muốn bắt đầu lấy các bản ghi từ một thứ tự nào đó, sử dụng từ khóa `OFFSET`.

Ví dụ:
```java
SELECT * FROM Customers
LIMIT 3 OFFSET 3; //trả về 3 bản ghi bắt đầu từ bản ghi thứ 4
```
#### Các loại JOIN
Mệnh đề `JOIN` được sử dụng để kết hợp các hàng từ 2 hoặc nhiều bảng, dựa trên 1 cột có liên quan giữa chúng.

Các loại `JOIN`:
- `INNER JOIN`: Trả về các bản ghi có giá trị khớp trong cả hai bảng.

    Ví dụ: Có 2 bảng Orders và Customers
    ```java
    SELECT column_name(s)
    FROM table1
    INNER JOIN table2
    ON table1.column_name = table2.column_name;
    ```
- `LEFT JOIN`: Trả về tất cả các bản ghi từ bảng bên trái (table1), và các bản ghi khớp từ bảng bên phải (table2).
    ```java
    SELECT column_name(s)
    FROM table1
    LEFT JOIN table2
    ON table1.column_name = table2.column_name;
    ```
- `RIGHT JOIN`: Trả về tất cả các bản ghi từ bảng bên phải (table2), và các bản ghi khớp từ bảng bên trái (table1).
    ```java
    SELECT column_name(s)
    FROM table1
    RIGHT JOIN table2
    ON table1.column_name = table2.column_name;
    ```
- `CROSS JOIN`: Trả về tất cả các kết quả chứa tất cả các tổ hợp có thể có giữa các hàng từ cả hai bảng (kết hợp Cartesian).
  ```java
  SELECT column_name(s)
  FROM table1
  CROSS JOIN table2;
  ```
  Nếu sử dụng cùng điều kiện WHERE, kết quả trả về sẽ giống như khi sử dụng mệnh đề INNER JOIN. 
  
  Ví dụ:
  ```java
  SELECT Customers.CustomerName, Orders.OrderID
  FROM Customers
  CROSS JOIN Orders
  WHERE Customers.CustomerID=Orders.CustomerID;
  ```
- `SELF JOIN`: là một phép JOIN đặc biệt trong SQL, trong đó một bảng được kết hợp với chính nó. 
  ```java
  SELECT column_name(s)
  FROM table1 T1, table1 T2
  WHERE condition;
  ```
  Ví dụ:
  ```java
  SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
  FROM Customers A, Customers B
  WHERE A.CustomerID <> B.CustomerID
  AND A.City = B.City
  ORDER BY A.City;
  ```
  
#### GROUP BY
`GROUP BY` là một câu lệnh trong SQL được sử dụng để nhóm các hàng có cùng giá trị trong một hoặc nhiều cột lại với nhau. Thường thì GROUP BY được kết hợp với các hàm tổng hợp như `COUNT, SUM, AVG, MAX, và MIN` để thực hiện các phép tính trên từng nhóm.

Ví dụ: Giả sử có một bảng `sales` lưu trữ thông tin về các giao dịch bán hàng:

| sale_id | product_name | amount |
|---------|--------------|--------|
| 1       | Apple        | 10     |
| 2       | Orange       | 20     |
| 3       | Apple        | 15     |
| 4       | Banana       | 5      |
| 5       | Orange       | 10     |
Bạn muốn tính tổng số lượng bán hàng cho mỗi sản phẩm.
```java
SELECT product_name, SUM(amount) AS total_sales
FROM sales
GROUP BY product_name;
```
Kết quả trả về:

| product_name | total_sales |
|--------------|-------------|
| Apple        | 25          |
| Orange       | 30          |
| Banana       | 5           |
#### HAVING

`HAVING` là một mệnh đề trong SQL được sử dụng để lọc kết quả sau khi đã áp dụng `GROUP BY`. Trong khi mệnh đề `WHERE` được sử dụng để lọc các hàng trước khi nhóm, `HAVING` được sử dụng để lọc các nhóm kết quả sau khi nhóm.

Ví dụ: Giả sử bạn có một bảng `sales` lưu trữ thông tin về các giao dịch bán hàng:

| sale_id | product_name | amount |
|---------|--------------|--------|
| 1       | Apple        | 10     |
| 2       | Orange       | 20     |
| 3       | Apple        | 15     |
| 4       | Banana       | 5      |
| 5       | Orange       | 10     |
Bạn muốn tính tổng số lượng bán hàng cho mỗi sản phẩm, nhưng chỉ hiển thị những sản phẩm có tổng số lượng bán hàng lớn hơn 20.
```java
SELECT product_name, SUM(amount) AS total_sales
FROM sales
GROUP BY product_name
HAVING SUM(amount) > 20;
```
Kết quả:

| product_name | total_sales |
|--------------|-------------|
| Apple        | 25          |
| Orange       | 30          |

> Lưu ý:
`HAVING` thường được sử dụng với các hàm tổng hợp `(SUM, COUNT, AVG, MAX, MIN)`, trong khi `WHERE` được sử dụng để lọc các hàng trước khi nhóm.
