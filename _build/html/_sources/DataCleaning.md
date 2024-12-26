# Chương 5 Làm sạch và Chuẩn bị Dữ liệu

Làm sạch và chuẩn bị dữ liệu là một bước quan trọng trong quá trình phân tích dữ liệu và khoa học dữ liệu. Quá trình này bao gồm việc xử lý dữ liệu thiếu, biến đổi dữ liệu, và xử lý chuỗi ký tự để đảm bảo dữ liệu đủ chất lượng cho các phân tích và mô hình hóa tiếp theo. Dưới đây là một số khía cạnh chính của việc làm sạch và chuẩn bị dữ liệu:

## Xử Lý Dữ Liệu Thiếu
Dữ liệu thiếu là một vấn đề phổ biến trong các tập dữ liệu. Pandas cung cấp nhiều công cụ để xử lý dữ liệu thiếu:

- Xác Định Dữ Liệu Thiếu: Sử dụng các phương thức như isnull() và notnull() để xác định các giá trị thiếu trong DataFrame.

- Loại Bỏ Dữ Liệu Thiếu: Sử dụng dropna() để loại bỏ các hàng hoặc cột chứa giá trị thiếu.

- Điền Giá Trị Thiếu: Sử dụng fillna() để điền các giá trị thiếu bằng cách sử dụng giá trị trung bình, giá trị trước đó, hoặc bất kỳ giá trị nào khác.

## Biến Đổi Dữ Liệu
Biến đổi dữ liệu là quá trình chuyển đổi dữ liệu từ dạng này sang dạng khác để phục vụ cho mục đích phân tích. Một số phương thức biến đổi dữ liệu phổ biến bao gồm:

- Đổi Tên Cột: Sử dụng rename() để đổi tên các cột trong DataFrame.

- Thay Đổi Kiểu Dữ Liệu: Sử dụng astype() để thay đổi kiểu dữ liệu của các cột.

-  Tạo Cột Mới: Tạo các cột mới bằng cách thao tác trên các cột hiện có.

- Ánh Xạ Giá Trị: Sử dụng map() hoặc apply() để thay đổi giá trị của các phần tử trong cột dựa trên một hàm hoặc từ điển.

## Xử Lý Chuỗi Ký Tự
Dữ liệu dạng chuỗi ký tự cần được xử lý để loại bỏ các ký tự không mong muốn, chuẩn hóa định dạng, và trích xuất thông tin hữu ích. Pandas cung cấp nhiều phương thức để xử lý chuỗi ký tự:

- Chuyển Đổi Chuỗi: Sử dụng các phương thức như str.lower(), str.upper(), và str.strip() để chuyển đổi và làm sạch chuỗi.

- Trích Xuất Thông Tin: Sử dụng str.extract() và các biểu thức chính quy để trích xuất thông tin từ chuỗi.

- Thay Thế Giá Trị: Sử dụng str.replace() để thay thế các giá trị trong chuỗi.

- Phân Tách và Ghép Nối Chuỗi: Sử dụng str.split() để phân tách chuỗi thành nhiều phần và str.cat() để ghép nối các chuỗi lại với nhau.

Làm sạch và chuẩn bị dữ liệu là bước đầu tiên và rất quan trọng trong quá trình phân tích dữ liệu. Các công cụ mạnh mẽ của Pandas giúp thực hiện các nhiệm vụ này một cách hiệu quả và linh hoạt, giúp đảm bảo dữ liệu đủ chất lượng để phục vụ cho các bước phân tích và mô hình hóa tiếp theo.