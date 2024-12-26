
#  Xử Lý Dữ Liệu Thiếu

Dữ liệu thiếu xuất hiện phổ biến trong nhiều ứng dụng phân tích dữ liệu. Một trong những mục tiêu của pandas là làm cho việc xử lý dữ liệu thiếu trở nên dễ dàng nhất có thể. Ví dụ, tất cả các thống kê mô tả trên các đối tượng pandas mặc định loại trừ dữ liệu thiếu.

Cách mà dữ liệu thiếu được biểu diễn trong các đối tượng pandas có phần không hoàn hảo, nhưng nó vẫn có tác dụng đối với nhiều người dùng. Đối với dữ liệu số, pandas sử dụng giá trị dấu phẩy động NaN (Not a Number) để biểu diễn dữ liệu thiếu. Đây được gọi là một giá trị sentinel có thể dễ dàng phát hiện:

```python
In [10]: string_data = pd.Series(['aardvark', 'artichoke', np.nan, 'avocado'])
In [11]: string_data
Out[11]:
0    aardvark
1   artichoke
2         NaN
3     avocado
dtype: object
In [12]: string_data.isnull()
Out[12]:
0    False
1    False
2     True
3    False
dtype: bool
```
Trong pandas, quy ước của ngôn ngữ lập trình R được sử dụng bằng cách gọi dữ liệu thiếu là NA, viết tắt của not available (không có sẵn). Trong các ứng dụng thống kê, dữ liệu NA có thể là dữ liệu không tồn tại hoặc tồn tại nhưng không được quan sát (do các vấn đề với việc thu thập dữ liệu, ví dụ). Khi làm sạch dữ liệu để phân tích, thường rất quan trọng để phân tích dữ liệu thiếu để xác định các vấn đề thu thập dữ liệu hoặc các sai lệch tiềm tàng trong dữ liệu gây ra bởi dữ liệu thiếu.

Giá trị None tích hợp của Python cũng được xử lý như NA trong các mảng đối tượng:

```python
In [13]: string_data[0] = None
In [14]: string_data.isnull()
Out[14]:
0     True
1    False
2     True
3    False
dtype: bool
```
Có công việc đang diễn ra trong dự án pandas để cải thiện các chi tiết nội bộ của cách dữ liệu thiếu được xử lý, nhưng các hàm API người dùng, như pandas.isnull, trừu tượng hóa nhiều chi tiết phiền phức. Xem Bảng 7-1 để biết danh sách một số hàm liên quan đến xử lý dữ liệu thiếu.

## Bảng 5-1. Các phương thức xử lý NA

| Tham số  | Mô tả                                                                                                         |
|----------|---------------------------------------------------------------------------------------------------------------|
| `dropna` | Lọc nhãn trục dựa trên việc các giá trị cho mỗi nhãn có dữ liệu thiếu hay không, với các ngưỡng thay đổi về mức độ dữ liệu thiếu chấp nhận được. |
| `fillna` | Điền vào dữ liệu thiếu bằng một giá trị nào đó hoặc sử dụng một phương pháp nội suy như 'ffill' hoặc 'bfill'. |
| `isnull` | Trả về các giá trị boolean chỉ ra giá trị nào là thiếu/NA.                                                    |
| `notnull`| Phủ định của isnull.                                                                                          |

## Lọc Dữ Liệu Thiếu

Có một số cách để lọc dữ liệu thiếu. Trong khi bạn luôn có tùy chọn làm điều đó thủ công bằng cách sử dụng pandas.isnull và chỉ số boolean, hàm dropna có thể hữu ích. Trên một Series, hàm này trả về Series chỉ chứa dữ liệu không bị thiếu và các giá trị chỉ mục:

```python
from numpy import nan as NA
data = pd.Series([1, NA, 3.5, NA, 7])
data.dropna()
```
Kết quả:

```python
0    1.0
2    3.5
4    7.0
dtype: float64
```
Điều này tương đương với:

```python
data[data.notnull()]
```
Kết quả:

```python
0    1.0
2    3.5
4    7.0
dtype: float64
```
Với các đối tượng DataFrame, mọi thứ phức tạp hơn một chút. Bạn có thể muốn loại bỏ các hàng hoặc cột chứa toàn bộ NA hoặc chỉ chứa bất kỳ giá trị NA nào. Hàm dropna mặc định sẽ loại bỏ bất kỳ hàng nào chứa giá trị thiếu:

```python
data = pd.DataFrame([[1., 6.5, 3.], [1., NA, NA], [NA, NA, NA], [NA, 6.5, 3.]])
cleaned = data.dropna()
data
cleaned
```
Kết quả:

```python
     0    1    2
0  1.0  6.5  3.0
1  1.0  NaN  NaN
2  NaN  NaN  NaN
3  NaN  6.5  3.0

     0    1    2
0  1.0  6.5  3.0
```
Truyền how='all' sẽ chỉ loại bỏ các hàng chứa toàn bộ giá trị NA:

```python
data.dropna(how='all')
```
Kết quả:

```python
     0    1    2
0  1.0  6.5  3.0
1  1.0  NaN  NaN
3  NaN  6.5  3.0
```
Để loại bỏ cột theo cách tương tự, truyền axis=1:

```python
data[4] = NA
data.dropna(axis=1, how='all')
```
Kết quả:

```python
     0    1    2
0  1.0  6.5  3.0
1  1.0  NaN  NaN
2  NaN  NaN  NaN
3  NaN  6.5  3.0
```
Một cách liên quan để lọc hàng DataFrame thường liên quan đến dữ liệu chuỗi thời gian. Giả sử bạn muốn giữ lại các hàng chứa một số lượng nhất định các quan sát. Bạn có thể chỉ định điều này với tham số thresh:

```python
df = pd.DataFrame(np.random.randn(7, 3))
df.iloc[:4, 1] = NA
df.iloc[:2, 2] = NA
df.dropna(thresh=2)
```
Kết quả:

```python
          0         1         2
2  0.092908       NaN  0.769023
3  1.246435       NaN -1.296221
4  0.274992  0.228913  1.352917
5  0.886429 -2.001637 -0.371843
6  1.669025 -0.438570 -0.539741
```
## Điền Dữ Liệu Thiếu

Thay vì lọc dữ liệu thiếu (và có thể loại bỏ dữ liệu khác cùng với nó), bạn có thể muốn điền vào các "khoảng trống" theo nhiều cách khác nhau. Đối với hầu hết các mục đích, hàm fillna là công cụ chính để sử dụng. Gọi fillna với một hằng số thay thế các giá trị thiếu bằng giá trị đó:

```python
df.fillna(0)
```
Kết quả:

```python
          0         1         2
0 -0.204708  0.000000  0.000000
1 -0.555730  0.000000  0.000000
2  0.092908  0.000000  0.769023
3  1.246435  0.000000 -1.296221
4  0.274992  0.228913  1.352917
5  0.886429 -2.001637 -0.371843
6  1.669025 -0.438570 -0.539741
```
Gọi fillna với một dict, bạn có thể sử dụng một giá trị điền khác nhau cho mỗi cột:

```python
df.fillna({1: 0.5, 2: 0})
```
Kết quả:

```python
          0         1         2
0 -0.204708  0.500000  0.000000
1 -0.555730  0.500000  0.000000
2  0.092908  0.500000  0.769023
3  1.246435  0.500000 -1.296221
4  0.274992  0.228913  1.352917
5  0.886429 -2.001637 -0.371843
6  1.669025 -0.438570 -0.539741
```
fillna trả về một đối tượng mới, nhưng bạn có thể chỉnh sửa đối tượng hiện tại:

```python
_ = df.fillna(0, inplace=True)
df
```
Kết quả:

```python
          0         1         2
0 -0.204708  0.000000  0.000000
1 -0.555730  0.000000  0.000000
2  0.092908  0.000000  0.769023
3  1.246435  0.000000 -1.296221
4  0.274992  0.228913  1.352917
5  0.886429 -2.001637 -0.371843
6  1.669025 -0.438570 -0.539741
```
Các phương pháp nội suy có sẵn cho việc tái chỉ mục cũng có thể được sử dụng với fillna:

```python
df = pd.DataFrame(np.random.randn(6, 3))
df.iloc[2:, 1] = NA
df.iloc[4:, 2] = NA
df.fillna(method='ffill')
```
Kết quả:

```python
          0         1         2
0  0.476985  3.248944 -1.021228
1 -0.577087  0.124121  0.302614
2  0.523772  0.124121  1.343810
3 -0.713544  0.124121 -2.370232
4 -1.860761  0.124121 -2.370232
5 -1.265934  0.124121 -2.370232
```
Với fillna, bạn có thể thực hiện nhiều điều khác với một chút sáng tạo. Ví dụ, bạn có thể truyền giá trị trung bình hoặc giá trị trung vị của một Series:

```python
data = pd.Series([1., NA, 3.5, NA, 7])
data.fillna(data.mean())
```
Kết quả:

```python
0    1.000000
1    3.833333
2    3.500000
3    3.833333
4    7.000000
dtype: float64
```

## Bảng 5-2. Các tham số của hàm fillna

| Tham số   | Mô tả                                                                                                 |
|-----------|-------------------------------------------------------------------------------------------------------|
| `value`   | Giá trị vô hướng hoặc đối tượng giống dict để sử dụng để điền vào các giá trị thiếu.                   |
| `method`  | Phương pháp nội suy; mặc định là 'ffill' nếu hàm được gọi mà không có tham số khác.                    |
| `axis`    | Trục để điền vào; mặc định là axis=0.                                                                 |
| `inplace` | Chỉnh sửa đối tượng gọi hàm mà không tạo ra một bản sao.                                               |
| `limit`   | Đối với việc điền tiến và điền lùi, số lượng tối đa các giai đoạn liên tiếp để điền vào.              |

