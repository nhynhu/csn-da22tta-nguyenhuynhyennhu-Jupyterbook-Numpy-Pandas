# Định Hình Lại và Pivoting

Có một số thao tác cơ bản để sắp xếp lại dữ liệu bảng. Những thao tác này thay đổi được gọi là thao tác định hình lại hoặc pivot.

## Định Hình Lại Với Đánh Chỉ Mục Phân Cấp

Đánh chỉ mục phân cấp cung cấp một cách nhất quán để sắp xếp lại dữ liệu trong DataFrame. Có hai hành động chính:

- `stack`: “xoay” hoặc pivot từ các cột trong dữ liệu thành các hàng.
- `unstack`: pivot từ các hàng vào các cột.

Tôi sẽ minh họa các thao tác này qua một loạt các ví dụ. Hãy xem xét một DataFrame nhỏ với các mảng chuỗi làm chỉ mục hàng và cột:

```python
data = pd.DataFrame(np.arange(6).reshape((2, 3)),
                    index=pd.Index(['Ohio', 'Colorado'], name='state'),
                    columns=pd.Index(['one', 'two', 'three'], name='number'))
data
```
Kết quả:

```python
number    one  two  three
state
Ohio        0    1      2
Colorado    3    4      5
```
Sử dụng phương thức stack trên dữ liệu này pivot các cột vào hàng, tạo thành một Series:

```python
result = data.stack()
result
```
Kết quả:

```python
state     number
Ohio      one       0
          two       1
          three     2
Colorado  one       3
          two       4
          three     5
dtype: int64
```
Từ một Series được đánh chỉ mục phân cấp, bạn có thể sắp xếp lại dữ liệu thành một DataFrame với unstack:

```python
result.unstack()
```
Kết quả:

```python
number    one  two  three
state
Ohio        0    1      2
Colorado    3    4      5
```
Mặc định mức trong cùng được unstack (giống với stack). Bạn có thể unstack một mức khác bằng cách truyền một số hoặc tên mức:

```python
result.unstack(0)
result.unstack('state')
```
Kết quả lần lượt:

```python
state      Ohio  Colorado
number
one          0         3
two          1         4
three        2         5

state      Ohio  Colorado
number
one          0         3
two          1         4
three        2         5
```
Unstacking có thể giới thiệu dữ liệu thiếu nếu tất cả các giá trị trong mức không được tìm thấy trong mỗi nhóm con:

```python
s1 = pd.Series([0, 1, 2, 3], index=['a', 'b', 'c', 'd'])
s2 = pd.Series([4, 5, 6], index=['c', 'd', 'e'])
data2 = pd.concat([s1, s2], keys=['one', 'two'])
data2.unstack()
```
Kết quả:

```python
       a    b    c    d    e
one   0.0  1.0  2.0  3.0  NaN
two   NaN  NaN  4.0  5.0  6.0
```
Stacking loại bỏ dữ liệu thiếu mặc định, do đó thao tác này dễ dàng đảo ngược hơn:

```python
data2.unstack().stack()
data2.unstack().stack(dropna=False)
```
Kết quả lần lượt:

```python
one   a    0.0
      b    1.0
      c    2.0
      d    3.0
two   c    4.0
      d    5.0
      e    6.0
dtype: float64

one   a    0.0
      b    1.0
      c    2.0
      d    3.0
      e    NaN
two   a    NaN
      b    NaN
      c    4.0
      d    5.0
      e    6.0
dtype: float64
```
Khi bạn unstack trong một DataFrame, mức unstacked trở thành mức thấp nhất trong kết quả:

```python
df = pd.DataFrame({'left': result, 'right': result + 5},
                  columns=pd.Index(['left', 'right'], name='side'))
df.unstack('state')
```
Kết quả:

```python
side     left      right
state    Ohio  Colorado  Ohio  Colorado
number
one         0        3      5        8
two         1        4      6        9
three       2        5      7       10
```
Khi gọi stack, chúng ta có thể chỉ định tên của trục để stack:

```python
df.unstack('state').stack('side')
```
Kết quả:

```python
state    Colorado  Ohio
number  side
one      left       3     0
         right      8     5
two      left       4     1
         right      9     6
three    left       5     2
         right     10     7
dtype: int64
```

## Chuyển Đổi Định Dạng "Long" thành "Wide"

Một cách phổ biến để lưu trữ nhiều chuỗi thời gian trong cơ sở dữ liệu và CSV là ở định dạng dài hoặc xếp chồng. Hãy tải một số dữ liệu ví dụ và thực hiện một chút xử lý chuỗi thời gian và làm sạch dữ liệu khác:

```python
data = pd.read_csv('examples/macrodata.csv')
data.head()
```
Kết quả:

```python
    year  quarter   realgdp  realcons   realinv  realgovt  realdpi    cpi     m1  tbilrate  unemp      pop   infl  realint
0  1959.0      1.0  2710.349    1707.4   286.898   470.045   1886.9  28.98  139.7      2.82    5.8  177.146   0.00     0.00
1  1959.0      2.0  2778.801    1733.7   310.859   481.301   1919.7  29.15  141.7      3.08    5.1  177.830   2.34     0.74
2  1959.0      3.0  2775.488    1751.8   289.226   491.260   1916.4  29.35  140.5      3.82    5.3  178.657   2.74     1.09
3  1959.0      4.0  2785.204    1753.7   299.356   484.052   1931.3  29.37  140.0      4.33    5.6  179.386   0.27     4.06
4  1960.0      1.0  2847.699    1770.5   331.722   462.199   1955.5  29.54  139.6      3.50    5.2  180.007   2.31     1.19
```
```python
periods = pd.PeriodIndex(year=data.year, quarter=data.quarter, name='date')
columns = pd.Index(['realgdp', 'infl', 'unemp'], name='item')
data = data.reindex(columns=columns)
data.index = periods.to_timestamp('D', 'end')
ldata = data.stack().reset_index().rename(columns={0: 'value'})
```
Giờ đây, ldata trông như thế này:

```python
ldata[:10]
```
Kết quả:

```python
         date     item      value
0  1959-03-31  realgdp  2710.349
1  1959-03-31     infl     0.000
2  1959-03-31    unemp     5.800
3  1959-06-30  realgdp  2778.801
4  1959-06-30     infl     2.340
5  1959-06-30    unemp     5.100
6  1959-09-30  realgdp  2775.488
7  1959-09-30     infl     2.740
8  1959-09-30    unemp     5.300
9  1959-12-31  realgdp  2785.204
```
Đây là định dạng dài cho nhiều chuỗi thời gian, hoặc dữ liệu quan sát khác với hai hoặc nhiều khóa (ở đây, các khóa của chúng ta là date và item). Mỗi hàng trong bảng đại diện cho một quan sát duy nhất.

Dữ liệu thường được lưu trữ theo cách này trong các cơ sở dữ liệu quan hệ như MySQL, vì một lược đồ cố định (tên cột và loại dữ liệu) cho phép số lượng giá trị khác biệt trong cột item thay đổi khi dữ liệu được thêm vào bảng. Trong ví dụ trước, date và item thường là khóa chính (trong thuật ngữ cơ sở dữ liệu quan hệ), cung cấp cả toàn vẹn quan hệ và các phép nối dễ dàng hơn. Trong một số trường hợp, dữ liệu có thể khó làm việc hơn trong định dạng này; bạn có thể thích có một DataFrame chứa một cột cho mỗi giá trị item khác biệt được đánh chỉ mục bởi các dấu thời gian trong cột date. Phương thức pivot của DataFrame thực hiện chính xác chuyển đổi này:

```python
pivoted = ldata.pivot('date', 'item', 'value')
pivoted
```
Kết quả:

```python
item       infl   realgdp  unemp
date
1959-03-31  0.00  2710.349   5.8
1959-06-30  2.34  2778.801   5.1
1959-09-30  2.74  2775.488   5.3
1959-12-31  0.27  2785.204   5.6
1960-03-31  2.31  2847.699   5.2
...
2009-06-30  3.37  12901.504  9.2
2009-09-30  3.56  12990.341  9.6
```
Hai giá trị đầu tiên được truyền vào là các cột sẽ được sử dụng lần lượt làm chỉ mục hàng và cột, sau đó là một cột giá trị tùy chọn để điền vào DataFrame. Giả sử bạn có hai cột giá trị muốn định hình lại đồng thời:

```python
ldata['value2'] = np.random.randn(len(ldata))
pivoted = ldata.pivot('date', 'item')
pivoted[:5]
```
Kết quả:

```python
           value                      value2
item        infl   realgdp  unemp      infl   realgdp    unemp
date
1959-03-31  0.00  2710.349   5.8  0.000940  0.523772  1.343810
1959-06-30  2.34  2778.801   5.1 -0.831154 -0.713544 -2.370232
1959-09-30  2.74  2775.488   5.3 -0.860757 -1.860761  0.560145
1959-12-31  0.27  2785.204   5.6  0.119827 -1.265934 -1.063512
1960-03-31  2.31  2847.699   5.2 -2.359419  0.332883 -0.199543
```
Lưu ý rằng pivot tương đương với việc tạo một chỉ mục phân cấp bằng set_index sau đó gọi unstack:

```python
unstacked = ldata.set_index(['date', 'item']).unstack('item')
unstacked[:7]
```
Kết quả:

```python
           value                     value2
item        infl   realgdp  unemp      infl   realgdp    unemp
date
1959-03-31  0.00  2710.349   5.8  0.000940  0.523772  1.343810
1959-06-30  2.34  2778.801   5.1 -0.831154 -0.713544 -2.370232
1959-09-30  2.74  2775.488   5.3 -0.860757 -1.860761  0.560145
1959-12-31  0.27  2785.204   5.6  0.119827 -1.265934 -1.063512
1960-03-31  2.31  2847.699   5.2 -2.359419  0.332883 -0.199543
...
1960-09-30  2.70  2839.022   5.6  0.377984  0.286350 -0.753887
```
## Chuyển Đổi Định Dạng "Wide" thành "Long"

Một thao tác ngược lại với `pivot` cho DataFrame là `pandas.melt`. Thay vì chuyển đổi một cột thành nhiều cột trong một DataFrame mới, nó gộp nhiều cột thành một, tạo ra một DataFrame dài hơn so với đầu vào. Hãy xem một ví dụ:

```python
df = pd.DataFrame({'key': ['foo', 'bar', 'baz'],
                   'A': [1, 2, 3],
                   'B': [4, 5, 6],
                   'C': [7, 8, 9]})
df
```
Kết quả:

```python
   A  B  C  key
0  1  4  7  foo
1  2  5  8  bar
2  3  6  9  baz
```
Cột 'key' có thể là một chỉ báo nhóm, và các cột khác là giá trị dữ liệu. Khi sử dụng pandas.melt, chúng ta phải chỉ ra các cột nào (nếu có) là chỉ báo nhóm. Hãy sử dụng 'key' làm chỉ báo nhóm duy nhất ở đây:

```python
melted = pd.melt(df, ['key'])
melted
```
Kết quả:

```python
   key variable  value
0  foo        A      1
1  bar        A      2
2  baz        A      3
3  foo        B      4
4  bar        B      5
5  baz        B      6
6  foo        C      7
7  bar        C      8
8  baz        C      9
```
Sử dụng pivot, chúng ta có thể định hình lại về bố cục ban đầu:

```python
reshaped = melted.pivot('key', 'variable', 'value')
reshaped
```
Kết quả:

```python
variable  A  B  C
key
bar       2  5  8
baz       3  6  9
foo       1  4  7
```
Vì kết quả của pivot tạo ra một chỉ mục từ cột được sử dụng làm nhãn hàng, chúng ta có thể sử dụng reset_index để chuyển dữ liệu trở lại thành một cột:

```python
reshaped.reset_index()
```
Kết quả:

```python
variable  key  A  B  C
0          bar  2  5  8
1          baz  3  6  9
2          foo  1  4  7
```
Bạn cũng có thể chỉ định một tập hợp con các cột để sử dụng làm cột giá trị:

```python
pd.melt(df, id_vars=['key'], value_vars=['A', 'B'])
```
Kết quả:

```python
   key variable  value
0  foo        A      1
1  bar        A      2
2  baz        A      3
3  foo        B      4
4  bar        B      5
5  baz        B      6
```
pandas.melt cũng có thể được sử dụng mà không cần bất kỳ chỉ báo nhóm nào:

```python
pd.melt(df, value_vars=['A', 'B', 'C'])
```
Kết quả:

```python
  variable  value
0        A      1
1        A      2
2        A      3
3        B      4
4        B      5
5        B      6
6        C      7
7        C      8
8        C      
```
```python
pd.melt(df, value_vars=['key', 'A', 'B'])
````
Kết quả:

```python
  variable value
0      key   foo
1      key   bar
2      key   baz
3        A     1
4        A     2
5        A     3
6        B     4
7        B     5
8        B     6
```
