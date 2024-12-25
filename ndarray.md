## Đối Tượng Mảng N Chiều NumPy: Một Đối Tượng Mảng Đa Chiều

Một trong những tính năng chính của **NumPy** là đối tượng mảng N chiều, hay **ndarray**, là một container nhanh chóng và linh hoạt cho các tập dữ liệu lớn trong Python. Các mảng cho phép bạn thực hiện các phép toán toán học trên toàn bộ khối dữ liệu bằng cách sử dụng cú pháp tương tự như các phép toán tương đương giữa các phần tử vô hướng.

### Ví Dụ Cơ Bản Về Mảng NumPy

Để cung cấp cho bạn một cái nhìn tổng quan về cách **NumPy** cho phép thực hiện các phép toán theo lô với cú pháp tương tự như các giá trị vô hướng trên các đối tượng tích hợp sẵn của Python, tôi sẽ nhập **NumPy** và tạo ra một mảng nhỏ dữ liệu ngẫu nhiên:

```python
import numpy as np

# Tạo một số dữ liệu ngẫu nhiên
data = np.random.randn(2, 3)
print(data)
```
```lua
array([[-0.2047,  0.4789, -0.5194],
       [-0.5557,  1.9658,  1.3934]])
```
```python
# Nhân tất cả các phần tử với 10
print(data * 10)

# Cộng các phần tử tương ứng trong mỗi hàng
print(data + data)
```
```lua
array([[-2.0471,  4.7894, -5.1944],
       [-5.5573, 19.6578, 13.9341]])

array([[-0.4094,  0.9579, -1.0389],
       [-1.1115,  3.9316,  2.7868]])
```
Trong ví dụ đầu tiên, tất cả các phần tử trong mảng đã được nhân với 10. Trong ví dụ thứ hai, các giá trị tương ứng trong mỗi "ô" trong mảng đã được cộng lại với nhau.

Một ndarray là một container đa chiều tổng quát cho dữ liệu đồng nhất, tức là tất cả các phần tử phải cùng loại. Mỗi mảng có:

**shape**: Một tuple chỉ ra kích thước của mỗi chiều.

**dtype**: Một đối tượng mô tả kiểu dữ liệu của mảng.

```python
print(data.shape)  # (2, 3)
print(data.dtype)  # dtype('float64')
```
## Tạo ndarrays

Cách dễ nhất để tạo một mảng là sử dụng hàm `array`. Hàm này chấp nhận bất kỳ đối tượng giống như chuỗi nào (bao gồm cả các mảng khác) và tạo ra một mảng **NumPy** mới chứa dữ liệu đã được truyền vào. 

```python
data1 = [6, 7.5, 8, 0, 1]
arr1 = np.array(data1)
print(arr1)
```
```lua
array([ 6. , 7.5, 8. , 0. , 1. ])
```
Các chuỗi lồng nhau, như một danh sách các danh sách có chiều dài bằng nhau, sẽ được chuyển đổi thành một mảng đa chiều:

```python
data2 = [[1, 2, 3, 4], [5, 6, 7, 8]]
arr2 = np.array(data2)
print(arr2)
```
Output:

```lua
array([[1, 2, 3, 4],
       [5, 6, 7, 8]])
```
Vì data2 là một danh sách các danh sách, mảng NumPy arr2 có hai chiều với hình dạng được suy diễn từ dữ liệu. Chúng ta có thể xác nhận điều này bằng cách kiểm tra các thuộc tính ndim và shape:

```python
print(arr2.ndim)  # Số chiều của mảng
print(arr2.shape)  # Hình dạng của mảng
```
Output:

```scss
2
(2, 4)
```
Trừ khi được chỉ định rõ ràng (sẽ được đề cập sau), np.array cố gắng suy diễn một kiểu dữ liệu tốt cho mảng mà nó tạo ra. Kiểu dữ liệu được lưu trữ trong một đối tượng metadata dtype đặc biệt; ví dụ, trong hai ví dụ trên, chúng ta có:

```python
print(arr1.dtype)  # dtype('float64')
print(arr2.dtype)  # dtype('int64')
```
Ngoài np.array, còn có một số hàm khác để tạo ra các mảng mới. Ví dụ, zeros và ones tạo ra các mảng chứa toàn số 0 hoặc 1, tương ứng, với một chiều dài hoặc hình dạng nhất định. empty tạo ra một mảng mà không khởi tạo các giá trị của nó thành bất kỳ giá trị cụ thể nào. Để tạo ra một mảng có chiều cao hơn với các phương pháp này, hãy truyền một tuple cho hình dạng:

```python
np.zeros(10)
# Output: array([ 0., 0., 0., 0., 0., 0., 0., 0., 0., 0.])

np.zeros((3, 6))
# Output: array([[ 0., 0., 0., 0., 0., 0.],
#                  [ 0., 0., 0., 0., 0., 0.],
#                  [ 0., 0., 0., 0., 0., 0.]])
                  
np.empty((2, 3, 2))
# Output: array([[[ 0., 0.],
#                   [ 0., 0.],
#                   [ 0., 0.]],
#                  [[ 0., 0.],
#                   [ 0., 0.],
#                   [ 0., 0.]]])
```
Lưu ý: Không an toàn khi giả định rằng np.empty sẽ trả về một mảng toàn số 0. Trong một số trường hợp, nó có thể trả về các giá trị "rác" chưa được khởi tạo.

arange là một phiên bản mảng của hàm range tích hợp sẵn trong Python:

```python
np.arange(15)
# Output: array([ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14])
```
### Các Hàm Tạo Mảng

| Hàm               | Mô tả                                                                 |
|-------------------|-----------------------------------------------------------------------|
| `array`           | Chuyển đổi dữ liệu đầu vào (danh sách, tuple, mảng hoặc loại chuỗi khác) thành một `ndarray` bằng cách suy diễn một `dtype` hoặc chỉ định rõ ràng một `dtype`; sao chép dữ liệu đầu vào theo mặc định. |
| `asarray`         | Chuyển đổi đầu vào thành `ndarray`, nhưng không sao chép nếu đầu vào đã là một `ndarray`. |
| `arange`          | Giống như hàm `range` tích hợp nhưng trả về một `ndarray` thay vì một danh sách. |
| `ones`, `ones_like` | Tạo ra một mảng chứa toàn số 1 với hình dạng và `dtype` đã cho; `ones_like` nhận một mảng khác và tạo ra một mảng chứa toàn số 1 có cùng hình dạng và `dtype`. |
| `zeros`, `zeros_like` | Giống như `ones` và `ones_like` nhưng tạo ra các mảng chứa toàn số 0. |
| `empty`, `empty_like` | Tạo ra các mảng mới bằng cách cấp phát bộ nhớ mới, nhưng không điền vào bất kỳ giá trị nào như `ones` và `zeros`. |
| `full`, `full_like` | Tạo ra một mảng có hình dạng và `dtype` đã cho với tất cả các giá trị được đặt thành "giá trị điền" đã chỉ định; `full_like` nhận một mảng khác và tạo ra một mảng đã điền có cùng hình dạng và `dtype`. |
| `eye`, `identity` | Tạo ra một ma trận đơn vị vuông N × N (1s trên đường chéo và 0s ở những nơi khác). |

dịch theo phong cách học thuật để tôi có thể viết vào tài liệu
Chắc chắn rồi! Dưới đây là bản dịch theo phong cách học thuật:

### Các Kiểu Dữ Liệu cho ndarrays
Kiểu dữ liệu (hay dtype) là một đối tượng đặc biệt trong NumPy chứa thông tin cần thiết để diễn giải một khối bộ nhớ dưới dạng một loại dữ liệu cụ thể. Ví dụ:

```python
In [33]: arr1 = np.array([1, 2, 3], dtype=np.float64)
In [34]: arr2 = np.array([1, 2, 3], dtype=np.int32)
In [35]: arr1.dtype
Out[35]: dtype('float64')
In [36]: arr2.dtype
Out[36]: dtype('int32')
```
dtypes là yếu tố quan trọng cho sự linh hoạt của NumPy trong việc tương tác với dữ liệu từ các hệ thống khác nhau. Chúng cung cấp một ánh xạ trực tiếp đến đại diện đĩa hoặc bộ nhớ cơ bản, giúp dễ dàng đọc và ghi các luồng dữ liệu nhị phân vào đĩa và kết nối với mã viết bằng các ngôn ngữ cấp thấp như C hoặc Fortran.

Các kiểu dữ liệu số được đặt tên theo cách bao gồm tên kiểu như float hoặc int, tiếp theo là số bit mỗi phần tử chiếm dụng. Ví dụ, giá trị số thực với độ chính xác kép chuẩn (được sử dụng trong đối tượng float của Python) chiếm 8 byte hoặc 64 bit, được biết đến trong NumPy là float64.

### Bảng 4-2. Các Kiểu Dữ Liệu của NumPy

| Kiểu            | Mã Kiểu  | Mô Tả                                                                          |
|-----------------|----------|----------------------------------------------------------------------------------|
| `int8`, `uint8`  | `i1`, `u1`  | Số nguyên 8-bit có dấu và không dấu (1 byte)                           |
| `int16`, `uint16`| `i2`, `u2`  | Số nguyên 16-bit có dấu và không dấu                                  |
| `int32`, `uint32`| `i4`, `u4`  | Số nguyên 32-bit có dấu và không dấu                                  |
| `int64`, `uint64`| `i8`, `u8`  | Số nguyên 64-bit có dấu và không dấu                                  |
| `float16`        | `f2`       | Số thực độ chính xác nửa                                              |
| `float32`        | `f4`, `f`   | Số thực độ chính xác đơn (tương thích với `float` trong C)            |
| `float64`        | `f8`, `d`   | Số thực độ chính xác gấp đôi (tương thích với `double` trong C) và `float` trong Python |
| `float128`       | `f16`, `g`  | Số thực độ chính xác mở rộng                                          |
| `complex64`, `complex128`, `complex256` | `c8`, `c16`, `c32` | Số phức với hai số thực 32, 64, hoặc 128 bit                      |
| `bool`           | `?`        | Kiểu boolean, lưu trữ giá trị `True` và `False`                      |
| `object`         | `O`        | Kiểu đối tượng Python, có thể lưu bất kỳ đối tượng Python nào         |
| `string_`        | `S`        | Kiểu chuỗi ASCII cố định (1 byte mỗi ký tự); ví dụ, để tạo một kiểu chuỗi với độ dài 10, sử dụng `'S10'` |
| `unicode_`       | `U`        | Kiểu Unicode cố định, số byte phụ thuộc vào nền tảng; cách xác định giống như `string_` (ví dụ, `'U10'`) |

### Chuyển Đổi Kiểu Dữ Liệu

Có thể chuyển đổi hoặc ép kiểu một mảng từ kiểu này sang kiểu khác bằng cách sử dụng phương thức `astype` của `ndarray`:

```python
In [37]: arr = np.array([1, 2, 3, 4, 5])
In [38]: arr.dtype
Out[38]: dtype('int64')
In [39]: float_arr = arr.astype(np.float64)
In [40]: float_arr.dtype
Out[40]: dtype('float64')

Trong ví dụ trên, các số nguyên được chuyển đổi thành số thực. Nếu chuyển đổi số thực thành số nguyên, phần thập phân sẽ bị cắt:

```python
In [41]: arr = np.array([3.7, -1.2, -2.6, 0.5, 12.9, 10.1])
In [42]: arr
Out[42]: array([ 3.7, -1.2, -2.6, 0.5, 12.9, 10.1])
In [43]: arr.astype(np.int32)
Out[43]: array([ 3, -1, -2, 0, 12, 10], dtype=int32)
```
Nếu có một mảng chuỗi đại diện cho các số, có thể dùng astype để chuyển đổi chúng thành dạng số:

```python
In [44]: numeric_strings = np.array(['1.25', '-9.6', '42'], dtype=np.string_)
In [45]: numeric_strings.astype(float)
Out[45]: array([ 1.25, -9.6 , 42. ])
```
Quan trọng cần lưu ý khi sử dụng kiểu numpy.string_, vì dữ liệu chuỗi trong NumPy có kích thước cố định và có thể bị cắt mà không có cảnh báo. pandas có hành vi trực quan hơn khi xử lý dữ liệu không phải số.

Nếu việc ép kiểu thất bại (ví dụ, chuỗi không thể chuyển đổi thành float64), một lỗi ValueError sẽ được ném ra.
