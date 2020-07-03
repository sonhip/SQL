# A. Store Procedure
  
store proceduce

## Store Procedure là gì?  

Stored procedure là tập hợp một hoặc nhiều câu lệnh T-SQL thành một nhóm đơn vị xử lý logic và được lưu trữ trên Database Server. Khi một câu lệnh gọi chạy stored procedure lần đầu tiên thì SQL Server sẽ chạy nó và lưu trữ vào bộ nhớ đệm, gọi là **plan cache**, những lần tiếp theo SQL Server sẽ sử dụng lại **plan cache** nên sẽ cho tốc độ xử lý tối ưu.  
  

## 2. Tạo Stored Procedure trong SQL Server  

```sql  
select   
	product_name,  
	list_price  
from  
	production.products  
order by  
	product_name;  
```  

Để tạo một stored procedure chứa câu truy vấn này thì mình sẽ viết như sau:  

```sql  
create procedure uspProductList  
as  
begin  
	select  
		product_name,  
		list_price  
	from  
		production.products  
	order by  
		product_name;  
end;  
```  

Như vậy cú pháp tạo stored procedure như sau:  

```sql  
create procedure store_name  
as   
begin  
	...  
end;  
```  

Đối với câu lệnh `CREATE PROCEDURE` thì bạn có thể rút ngắn thành `CREATE PROC`.  

## 3. Thay đổi Stored Procedure trong SQL Server  

Để thay đổi stored thì bạn sử dụng lệnh ALTER PROCEDURE và tên của stored sẽ nằm phía sau.  
  

Ví dụ giờ muốn muốn thay đổi cách sắp xếp kết quả trả về từ **product_name** thành **list_price** của `uspProductList` thì viết như sau:  

```sql  
alter procedure uspProductList  
as  
begin  
	select  
		product_name,  
		list_price  
	from  
		production.products  
	order by  
		list_price;  
end;  
```  

## 4. Xóa Stored Procedure trong SQL Server  

Để xóa stored procedure thì bạn sử dụng lệnh `DROP PROCEDURE` hoặc `DROP PROC`.  

```sql  
drop procedure sp_name  
Hoặc:  
drop proc sp_name  
```  

Trong đó sp_name là tên của stored muốn xóa.  
  

Ví dụ mình muốn xóa stored procedure có tên là **uspProductList** thì sẽ viết như sau:  

```sql  
drop procedure uspProductList;  
```  

## Store Procedure là gì?

Stored procedure là tập hợp một hoặc nhiều câu lệnh T-SQL thành một nhóm đơn vị xử lý logic và được lưu trữ trên Database Server. Khi một câu lệnh gọi chạy stored procedure lần đầu tiên thì SQL Server sẽ chạy nó và lưu trữ vào bộ nhớ đệm, gọi là  **plan cache**, những lần tiếp theo SQL Server sẽ sử dụng lại  **plan cache**  nên sẽ cho tốc độ xử lý tối ưu.

## 2. Tạo Stored Procedure trong SQL Server

```sql
select 
	product_name,
	list_price
from
	production.products
order by
	product_name;

```

Để tạo một stored procedure chứa câu truy vấn này thì mình sẽ viết như sau:

```sql
create procedure uspProductList
as
begin
	select
		product_name,
		list_price
	from
		production.products
	order by
		product_name;
end;

```

Như vậy cú pháp tạo stored procedure như sau:

```sql
create procedure store_name
as 
begin
	...
end;

```

Đối với câu lệnh  `CREATE PROCEDURE`  thì bạn có thể rút ngắn thành  `CREATE PROC`.

## 3. Thay đổi Stored Procedure trong SQL Server

Để thay đổi stored thì bạn sử dụng lệnh ALTER PROCEDURE và tên của stored sẽ nằm phía sau.

Ví dụ giờ muốn muốn thay đổi cách sắp xếp kết quả trả về từ  **product_name**  thành  **list_price**  của  `uspProductList`  thì viết như sau:

```sql
alter procedure uspProductList
as
begin
	select
		product_name,
		list_price
	from
		production.products
	order by
		list_price;
end;

```

## 4. Xóa Stored Procedure trong SQL Server

Để xóa stored procedure thì bạn sử dụng lệnh  `DROP PROCEDURE`  hoặc  `DROP PROC`.

```sql
drop procedure sp_name
Hoặc:
drop proc sp_name

```

Trong đó sp_name là tên của stored muốn xóa.

Ví dụ mình muốn xóa stored procedure có tên là  **uspProductList**  thì sẽ viết như sau:

```sql
drop procedure uspProductList;

```
# B. Cursor
  
cursor

## Cách sử dụng Cursor  

Corsor thường được dùng trong các Stored Procedure, Cursor mở và duyệt qua từng dòng dữ liệu mà nó chỉ tới.  

## Khai báo và sử dụng  

**Khai báo**  
 Trong SQL, tại các Stored Procedure, ta có thể dùng Cursor để duyệt qua dữ liệu. Ta hiểu Cursor là một tập hợp kết quả truy vấn (các hàng), với Corsor ta có thể duyệt qua từng hàng kết quả để thi hành những tác vụ phức tạp.  
 Ở một thời điểm, Cursor có thể truy cập bởi một con trỏ đến một hàng của nó, bạn chỉ có thể dịch chuyển con trỏ từ dòng này sang dòng khác.  

```sql  
declare @id int				  
declare @title nvarchar(200)	--Khai báo biến @id,  @title  để lưu nội dung đọc  
  
declare cursorProduct cursor for 	-- khai báo con trỏ cursorProduct  
select id, title from product	-- dữ liệu trỏ tới  
open cursorProduct	-- Mở con trỏ  
fetch next from cursorProduct	--  Đọc dòng đầu tiên  
	into @id, @title  
while @@fetch_status = 0	--vòng lặp WHILE khi đọc Cursor thành công  
begin  
	print 'ID: ' + cast(@id as nvarchar)  
	print 'TITLE: '+ @title  
	fetch next from cursorProduct 	--  Đọc dòng tiếp   
		into @id, @title  
end  
close cursorProduct		--  Đóng Cursor  
deallocate cursorProduct	--  Giải phóng tài nguyên  
```  
  

## Cách sử dụng Cursor

Corsor thường được dùng trong các Stored Procedure, Cursor mở và duyệt qua từng dòng dữ liệu mà nó chỉ tới.

## Khai báo và sử dụng

**Khai báo**  
Trong SQL, tại các Stored Procedure, ta có thể dùng Cursor để duyệt qua dữ liệu. Ta hiểu Cursor là một tập hợp kết quả truy vấn (các hàng), với Corsor ta có thể duyệt qua từng hàng kết quả để thi hành những tác vụ phức tạp.  
Ở một thời điểm, Cursor có thể truy cập bởi một con trỏ đến một hàng của nó, bạn chỉ có thể dịch chuyển con trỏ từ dòng này sang dòng khác.

```sql
declare @id int				
declare @title nvarchar(200)	--Khai báo biến @id,  @title  để lưu nội dung đọc

declare cursorProduct cursor for 	-- khai báo con trỏ cursorProduct
select id, title from product	-- dữ liệu trỏ tới
open cursorProduct	-- Mở con trỏ
fetch next from cursorProduct	--  Đọc dòng đầu tiên
	into @id, @title
while @@fetch_status = 0	--vòng lặp WHILE khi đọc Cursor thành công
begin
	print 'ID: ' + cast(@id as nvarchar)
	print 'TITLE: '+ @title
	fetch next from cursorProduct 	--  Đọc dòng tiếp	
		into @id, @title
end
close cursorProduct		--  Đóng Cursor
deallocate cursorProduct	--  Giải phóng tài nguyên

```

# C. Trigger và Instead Trigger
  
trigger

## Khái niệm  

Hiểu đơn giản thì Trigger là một stored procedure không có tham số. Trigger thực thi một cách tự động khi một trong ba câu lệnh Insert, Update, Delete làm thay đổi dữ liệu trên bảng có chứa trigger.  

## Trigger dùng làm gì?  

-   Trigger thường được sử dụng để kiểm tra ràng buộc (check constraints) trên nhiều quan hệ (nhiều bảng/table) hoặc trên nhiều dòng (nhiều record) của bảng.  
-   Ngoài ra việc sử dụng Trigger để chương trình có những hàm chạy ngầm nhằm phục vụ nhưng trường hợp hữu hạn và thường không sử dụng cho mục đích kinh doanh hoặc giao dịch.  

---  

-   Sử dụng Trigger để kiểm tra tính toàn vẹn của cơ sở dữ liệu.  
-   Trigger có thể bắt lỗi logic ở mức cơ sở dữ liệu.  
-   Có thể dùng trigger là một cách khác để thay thế việc thực hiện những công việc hẹn giờ theo lịch.  
-   Trigger rất hiệu quả khi sử dụng để kiểm soát những thay đổi của dữ liệu trong bảng.  

## Cú pháp Trigger  

```sql  
create trigger trigger_name trigger_time trigger_event  
on table_name  
for each row  
begin  
	...  
end;  
```  

Trong đó:  
  

- Một Trigger được khởi tạo sau câu lệnh `CREATE TRIGGER`. Quy tắc đặt tên nên tuân theo nguyên tắc: `[trigger time]_[table name]_[trigger event]`, ví dụ `before_employees_update`.  
- Thời gian kích hoạt : `BEFORE` hoặc `AFTER`. Cần phải chỉ định thời gian kích hoạc khi bạn xác định được tiến trình kích hoạt của nó. Sử dụng từ khóa `BEFORE` nếu bạn muốn xử lý hành động trước khi thực hiện thay đổi trên bản và `AFTER` nếu bạn cần phải xử lý hành động sau khi thay đổi được thực hiện xong.  
- Sự kiện gây ra có thể là `INSERT`, `UPDATE`, `DELETE`.  
- Trình kích hoạt phải được liên kết với một bảng cụ thể, sử dụng từ khóa `ON` để xác định.  
- Câu lệnh SQL phải được đặt giữa từ khóa `BEGIN` và `END`.  
  

## Instead of Trigger  

INSTEAD OF trigger là một loại trigger đặc biệt, nó cho phép bạn bỏ qua câu lệnh INSERT, UPDATE hoặc DELETE trên một table hoặc view. Ví dụ bạn muốn khi có hành động delete trên table product thì bạn không xóa sản phẩm mà sẽ chạy một câu lệnh UPDATE status của product đó sang chế độ ẩn. Mặc dù bạn có thể không sử dung lệnh DELETE mà UPDATE trực tiếp cũng được, tuy nhiên để đảm bảo mọi câu truy vấn đều đúng thì nên sử dụng trigger.  
  

Cú pháp của nó cũng không khá gì trigger:  

```sql  
create trigger [schema_name.] trigger_name  
on {table_name|view_name}  
instead of {insert/update,delete}  
as   
begin  
	...  
end;  
```  

## Khái niệm

Hiểu đơn giản thì Trigger là một stored procedure không có tham số. Trigger thực thi một cách tự động khi một trong ba câu lệnh Insert, Update, Delete làm thay đổi dữ liệu trên bảng có chứa trigger.

## Trigger dùng làm gì?

-   Trigger thường được sử dụng để kiểm tra ràng buộc (check constraints) trên nhiều quan hệ (nhiều bảng/table) hoặc trên nhiều dòng (nhiều record) của bảng.
-   Ngoài ra việc sử dụng Trigger để chương trình có những hàm chạy ngầm nhằm phục vụ nhưng trường hợp hữu hạn và thường không sử dụng cho mục đích kinh doanh hoặc giao dịch.

----------

-   Sử dụng Trigger để kiểm tra tính toàn vẹn của cơ sở dữ liệu.
-   Trigger có thể bắt lỗi logic ở mức cơ sở dữ liệu.
-   Có thể dùng trigger là một cách khác để thay thế việc thực hiện những công việc hẹn giờ theo lịch.
-   Trigger rất hiệu quả khi sử dụng để kiểm soát những thay đổi của dữ liệu trong bảng.

## Cú pháp Trigger

```sql
create trigger trigger_name trigger_time trigger_event
on table_name
for each row
begin
	...
end;

```

Trong đó:

-   Một Trigger được khởi tạo sau câu lệnh  `CREATE TRIGGER`. Quy tắc đặt tên nên tuân theo nguyên tắc:  `[trigger time]_[table name]_[trigger event]`, ví dụ  `before_employees_update`.
-   Thời gian kích hoạt :  `BEFORE`  hoặc  `AFTER`. Cần phải chỉ định thời gian kích hoạc khi bạn xác định được tiến trình kích hoạt của nó. Sử dụng từ khóa  `BEFORE`  nếu bạn muốn xử lý hành động trước khi thực hiện thay đổi trên bản và  `AFTER`  nếu bạn cần phải xử lý hành động sau khi thay đổi được thực hiện xong.
-   Sự kiện gây ra có thể là  `INSERT`,  `UPDATE`,  `DELETE`.
-   Trình kích hoạt phải được liên kết với một bảng cụ thể, sử dụng từ khóa  `ON`  để xác định.
-   Câu lệnh SQL phải được đặt giữa từ khóa  `BEGIN`  và  `END`.

## Instead of Trigger

INSTEAD OF trigger là một loại trigger đặc biệt, nó cho phép bạn bỏ qua câu lệnh INSERT, UPDATE hoặc DELETE trên một table hoặc view. Ví dụ bạn muốn khi có hành động delete trên table product thì bạn không xóa sản phẩm mà sẽ chạy một câu lệnh UPDATE status của product đó sang chế độ ẩn. Mặc dù bạn có thể không sử dung lệnh DELETE mà UPDATE trực tiếp cũng được, tuy nhiên để đảm bảo mọi câu truy vấn đều đúng thì nên sử dụng trigger.

Cú pháp của nó cũng không khá gì trigger:

```sql
create trigger [schema_name.] trigger_name
on {table_name|view_name}
instead of {insert/update,delete}
as 
begin
	...
end;

```
