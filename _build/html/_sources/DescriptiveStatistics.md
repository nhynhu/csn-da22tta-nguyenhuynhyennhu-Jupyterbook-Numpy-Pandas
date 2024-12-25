# Summarizing and Computing Descriptive Statistics

Các đối tượng pandas được trang bị một bộ các phương pháp toán học và thống kê phổ biến. Hầu hết trong số này thuộc loại phương pháp giảm hoặc thống kê tóm tắt, các phương pháp này trích xuất một giá trị duy nhất (như tổng hoặc trung bình) từ một `Series` hoặc một chuỗi các giá trị từ các hàng hoặc cột của một `DataFrame`. So với các phương pháp tương tự tìm thấy trên các mảng NumPy, chúng có xử lý tích hợp cho dữ liệu bị thiếu. Hãy xem xét một `DataFrame` nhỏ:

```python
In [230]: df = pd.DataFrame([[1.4, np.nan], [7.1, -4.5], [np.nan, np.nan], [0.75, -1.3]],
                            index=['a', 'b', 'c', 'd'],
                            columns=['one', 'two'])
In [231]: df
Out[231]:
    one  two
a  1.40   NaN
b  7.10  -4.5
c   NaN   NaN
d  0.75  -1.3
```
Gọi phương thức sum của DataFrame trả về một Series chứa tổng của các cột:

```python
In [232]: df.sum()
Out[232]:
one    9.25
two   -5.80
dtype: float64
```
Truyền axis='columns' hoặc axis=1 để tổng hợp theo các cột thay vì:

```python
In [233]: df.sum(axis='columns')
Out[233]:
a    1.40
b    2.60
c     NaN
d   -0.55
dtype: float64
```
Các giá trị NA bị loại trừ trừ khi toàn bộ lát cắt (hàng hoặc cột trong trường hợp này) là NA. Điều này có thể được vô hiệu hóa với tùy chọn skipna:

```python
In [234]: df.mean(axis='columns', skipna=False)
Out[234]:
a       NaN
b    1.300
c       NaN
d   -0.275
dtype: float64
```
### Bảng 5-7. Các Tùy Chọn cho Phương Thức Giảm

| Method   | Description                                                                      |
|----------|----------------------------------------------------------------------------------|
| axis     | Axis to reduce over; 0 for DataFrame’s rows and 1 for columns                    |
| skipna   | Exclude missing values; True by default                                          |
| level    | Reduce grouped by level if the axis is hierarchically indexed (MultiIndex)       |


Một số phương thức, như idxmin và idxmax, trả về các thống kê gián tiếp như giá trị chỉ mục nơi các giá trị tối thiểu hoặc tối đa đạt được:

```python
In [235]: df.idxmax()
Out[235]:
one    b
two    d
dtype: object
```
Các phương thức khác là các tích lũy:

```python
In [236]: df.cumsum()
Out[236]:
     one  two
a   1.40   NaN
b   8.50  -4.5
c    NaN   NaN
d   9.25  -5.8
```
Một loại phương thức khác không phải là giảm cũng không phải là tích lũy. describe là một ví dụ như vậy, tạo ra nhiều thống kê tóm tắt chỉ trong một lần gọi:

```python
In [237]: df.describe()
Out[237]:
            one        two
count  3.000000   2.000000
mean   3.083333  -2.900000
std    3.493685   2.262742
min    0.750000  -4.500000
25%    1.075000  -3.700000
50%    1.400000  -2.900000
75%    4.250000  -2.100000
max    7.100000  -1.300000
```
Trên dữ liệu không phải số, describe tạo ra các thống kê tóm tắt thay thế:

```python
In [238]: obj = pd.Series(['a', 'a', 'b', 'c'] * 4)
In [239]: obj.describe()
Out[239]:
count       16
unique       3
top          a
freq         8
dtype:  object
```

### Bảng 5-8. Các Phương Pháp Thống Kê Tóm Tắt và Liên Quan

| Phương pháp        | Mô tả                                                                                 |
|--------------------|---------------------------------------------------------------------------------------|
| count              | Số lượng các giá trị không phải NA                                                    |
| describe           | Tính toán bộ thống kê tóm tắt cho Series hoặc mỗi cột của DataFrame                   |
| min, max           | Tính giá trị nhỏ nhất và lớn nhất                                                     |
| argmin, argmax     | Tính vị trí chỉ số (số nguyên) tại đó giá trị nhỏ nhất hoặc lớn nhất đạt được          |
| idxmin, idxmax     | Tính nhãn chỉ số tại đó giá trị nhỏ nhất hoặc lớn nhất đạt được                        |
| quantile           | Tính phần trăm mẫu dao động từ 0 đến 1                                                |
| sum                | Tổng các giá trị                                                                      |
| mean               | Trung bình các giá trị                                                                |
| median             | Trung vị số học (phần trăm thứ 50) của các giá trị                                    |
| mad                | Độ lệch tuyệt đối trung bình từ giá trị trung bình                                    |
| prod               | Tích của tất cả các giá trị                                                           |
| var                | Phương sai mẫu của các giá trị                                                        |
| std                | Độ lệch chuẩn mẫu của các giá trị                                                     |
| skew               | Độ xiên mẫu (khoảnh khắc thứ ba) của các giá trị                                      |
| kurt               | Độ nhọn mẫu (khoảnh khắc thứ tư) của các giá trị                                      |
| cumsum             | Tổng tích lũy của các giá trị                                                         |
| cummin, cummax     | Giá trị tối thiểu hoặc tối đa tích lũy của các giá trị                                 |
| cumprod            | Tích tích lũy của các giá trị                                                         |
| diff               | Tính toán chênh lệch số học đầu tiên (hữu ích cho chuỗi thời gian)                    |
| pct_change         | Tính phần trăm thay đổi                                                               |

### Correlation and Covariance

Một số thống kê tóm tắt, như tương quan (correlation) và hiệp phương sai (covariance), được tính toán từ các cặp tham số. Hãy xem xét một số `DataFrame` về giá cổ phiếu và khối lượng giao dịch lấy từ Yahoo! Finance sử dụng gói pandas-datareader bổ sung. Nếu bạn chưa cài đặt, có thể cài qua conda hoặc pip:

```bash
conda install pandas-datareader
```
Sử dụng mô-đun pandas_datareader để tải dữ liệu cho một số mã cổ phiếu:

```python
import pandas_datareader.data as web

all_data = {ticker: web.get_data_yahoo(ticker) for ticker in ['AAPL', 'IBM', 'MSFT', 'GOOG']}
price = pd.DataFrame({ticker: data['Adj Close'] for ticker, data in all_data.items()})
volume = pd.DataFrame({ticker: data['Volume'] for ticker, data in all_data.items()})
```
Lưu ý: Đến thời điểm bạn đọc tài liệu này, Yahoo! Finance có thể không còn tồn tại do Yahoo! đã được Verizon mua lại vào năm 2017. Tham khảo tài liệu pandas-datareader trực tuyến để biết các chức năng mới nhất.

Bây giờ tính phần trăm thay đổi của giá, một thao tác chuỗi thời gian sẽ được khám phá thêm ở Chương 11:

```python
In [242]: returns = price.pct_change()
In [243]: returns.tail()
Out[243]:
                AAPL       GOOG       IBM      MSFT
Date                                              
2016-10-17 -0.000680  0.001837  0.002072 -0.003483
2016-10-18 -0.000681  0.019616 -0.026168  0.007690
2016-10-19 -0.002979  0.007846  0.003583 -0.002255
2016-10-20 -0.000512 -0.005652  0.001719 -0.004867
2016-10-21 -0.003930  0.003011 -0.012474  0.042096
```
Phương thức corr của Series tính toán tương quan của các giá trị chồng chéo, không phải NA, căn chỉnh theo chỉ mục trong hai Series. Liên quan, cov tính toán hiệp phương sai:

```python
In [244]: returns['MSFT'].corr(returns['IBM'])
Out[244]: 0.49976361144151144

In [245]: returns['MSFT'].cov(returns['IBM'])
Out[245]: 8.8706554797035462e-05
```
Vì MSFT là một thuộc tính Python hợp lệ, có thể chọn các cột này sử dụng cú pháp ngắn gọn hơn:

```python
In [246]: returns.MSFT.corr(returns.IBM)
Out[246]: 0.49976361144151144
```
Các phương thức corr và cov của DataFrame, mặt khác, trả về một ma trận tương quan hoặc hiệp phương sai đầy đủ dưới dạng một DataFrame:

```python
In [247]: returns.corr()
Out[247]:
           AAPL      GOOG       IBM      MSFT
AAPL   1.000000  0.407919  0.386817  0.389695
GOOG   0.407919  1.000000  0.405099  0.465919
IBM    0.386817  0.405099  1.000000  0.499764
MSFT   0.389695  0.465919  0.499764  1.000000

In [248]: returns.cov()
Out[248]:
            AAPL      GOOG       IBM      MSFT
AAPL   0.000277  0.000107  0.000078  0.000095
GOOG   0.000107  0.000251  0.000078  0.000108
IBM    0.000078  0.000078  0.000146  0.000089
MSFT   0.000095  0.000108  0.000089  0.000215
```
Sử dụng phương thức corrwith của DataFrame, có thể tính toán tương quan cặp giữa các cột hoặc hàng của DataFrame với một Series hoặc DataFrame khác. Truyền vào một Series trả về một Series với giá trị tương quan được tính toán cho mỗi cột:

```python
In [249]: returns.corrwith(returns.IBM)
Out[249]:
AAPL    0.386817
GOOG    0.405099
IBM     1.000000
MSFT    0.499764
dtype: float64
```
Truyền vào một DataFrame tính toán các tương quan của tên cột tương ứng. Ở đây tính toán các tương quan của phần trăm thay đổi với khối lượng:

```python
In [250]: returns.corrwith(volume)
Out[250]:
AAPL   -0.075565
GOOG   -0.007067
IBM    -0.204849
MSFT   -0.092950
dtype: float64
```
Truyền axis='columns' thực hiện thao tác theo từng hàng. Trong mọi trường hợp, các điểm dữ liệu được căn chỉnh theo nhãn trước khi tính toán tương quan.

### Unique Values, Value Counts, and Membership

Một lớp phương pháp liên quan khác là trích xuất thông tin về các giá trị chứa trong một `Series` một chiều. Để minh họa điều này, hãy xem xét ví dụ sau:

```python
In [251]: obj = pd.Series(['c', 'a', 'd', 'a', 'a', 'b', 'b', 'c', 'c'])
```
Phương thức đầu tiên là unique, cung cấp cho bạn một mảng các giá trị duy nhất trong một Series:

```python
In [252]: uniques = obj.unique()
In [253]: uniques
Out[253]: array(['c', 'a', 'd', 'b'], dtype=object)
```
Các giá trị duy nhất không nhất thiết được trả về theo thứ tự sắp xếp, nhưng có thể được sắp xếp sau nếu cần (uniques.sort()). Liên quan, value_counts tính toán một Series chứa tần số giá trị:

```python
In [254]: obj.value_counts()
Out[254]:
c    3
a    3
b    2
d    1
dtype: int64
```
Series được sắp xếp theo giá trị giảm dần để tiện lợi. value_counts cũng có sẵn dưới dạng một phương thức cấp cao nhất của pandas có thể được sử dụng với bất kỳ mảng hoặc chuỗi nào:

```python
In [255]: pd.value_counts(obj.values, sort=False)
Out[255]:
a    3
b    2
c    3
d    1
dtype: int64
```
isin thực hiện kiểm tra thành viên tập hợp theo vector hóa và có thể hữu ích trong việc lọc dữ liệu xuống một tập hợp các giá trị trong một Series hoặc cột trong một DataFrame:

```python
In [256]: obj
Out[256]:
0    c
1    a
2    d
3    a
4    a
5    b
6    b
7    c
8    c
dtype: object

In [257]: mask = obj.isin(['b', 'c'])
In [258]: mask
Out[258]:
0     True
1    False
2    False
3    False
4    False
5     True
6     True
7     True
8     True
dtype: bool

In [259]: obj[mask]
Out[259]:
0    c
5    b
6    b
7    c
8    c
dtype: object
```
Liên quan đến isin là phương thức Index.get_indexer, cung cấp cho bạn một mảng chỉ số từ một mảng có thể có giá trị không duy nhất vào một mảng giá trị duy nhất khác:

```python
In [260]: to_match = pd.Series(['c', 'a', 'b', 'b', 'c', 'a'])
In [261]: unique_vals = pd.Series(['c', 'b', 'a'])
In [262]: pd.Index(unique_vals).get_indexer(to_match)
Out[262]: array([0, 2, 1, 1, 0, 2])
```
### Bảng 5-9. Các Phương Pháp Giá Trị Duy Nhất, Tần Số Giá Trị, và Kiểm Tra Thành Viên

| Phương pháp      | Mô tả                                                                                           |
|------------------|-------------------------------------------------------------------------------------------------|
| isin             | Tính mảng boolean chỉ ra liệu mỗi giá trị trong Series có nằm trong chuỗi giá trị đã truyền vào hay không |
| match            | Tính các chỉ số số nguyên cho mỗi giá trị trong một mảng vào một mảng các giá trị duy nhất khác; hữu ích cho căn chỉnh dữ liệu và các phép nối |
| unique           | Tính mảng các giá trị duy nhất trong Series, trả về theo thứ tự quan sát được                    |
| value_counts     | Trả về một Series chứa các giá trị duy nhất làm chỉ mục và tần số làm giá trị, được sắp xếp theo thứ tự giảm dần của tần số |

Trong một số trường hợp, bạn có thể muốn tính toán biểu đồ histogram trên nhiều cột liên quan trong một `DataFrame`. Dưới đây là một ví dụ:

```python
In [263]: data = pd.DataFrame({'Qu1': [1, 3, 4, 3, 4],
                              'Qu2': [2, 3, 1, 2, 3],
                              'Qu3': [1, 5, 2, 4, 4]})
In [264]: data
Out[264]:
   Qu1  Qu2  Qu3
0    1    2    1
1    3    3    5
2    4    1    2
3    3    2    4
4    4    3    4
```
Truyền pandas.value_counts vào hàm apply của DataFrame:

```python
In [265]: result = data.apply(pd.value_counts).fillna(0)
In [266]: result
Out[266]:
     Qu1  Qu2  Qu3
1    1.0  1.0  1.0
2    0.0  2.0  1.0
3    2.0  2.0  0.0
4    2.0  0.0  2.0
5    0.0  0.0  1.0
```
Ở đây, các nhãn hàng trong kết quả là các giá trị khác nhau xuất hiện trong tất cả các cột. Các giá trị là số lượng tương ứng của các giá trị này trong mỗi cột.

