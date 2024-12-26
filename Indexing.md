# Đánh Chỉ Mục Phân Cấp

Đánh chỉ mục phân cấp là một tính năng quan trọng của pandas cho phép bạn có nhiều (hai hoặc nhiều hơn) mức chỉ mục trên một trục. Về mặt trừu tượng, nó cung cấp một cách để làm việc với dữ liệu có chiều cao hơn dưới dạng có chiều thấp hơn. Hãy bắt đầu với một ví dụ đơn giản; tạo một Series với danh sách các danh sách (hoặc mảng) làm chỉ mục:

```python
data = pd.Series(np.random.randn(9),
                 index=[['a', 'a', 'a', 'b', 'b', 'c', 'c', 'd', 'd'],
                        [1, 2, 3, 1, 3, 1, 2, 2, 3]])
data
```
Kết quả:

```python
a  1   -0.204708
   2    0.478943
   3   -0.519439
b  1   -0.555730
   3    1.965781
c  1    1.393406
   2    0.092908
d  2    0.281746
   3    0.769023
dtype: float64
```
Đây là một cái nhìn đẹp mắt của một Series với một MultiIndex làm chỉ mục của nó. Các "khoảng trống" trong hiển thị chỉ mục có nghĩa là "sử dụng nhãn ngay bên trên":

```python
data.index
```
Kết quả:

```python
MultiIndex(levels=[['a', 'b', 'c', 'd'], [1, 2, 3]],
           labels=[[0, 0, 0, 1, 1, 2, 2, 3, 3],
                   [0, 1, 2, 0, 2, 0, 1, 1, 2]])
```
Với một đối tượng có chỉ mục phân cấp, cái gọi là chỉ mục một phần là khả thi, cho phép bạn chọn các tập hợp con của dữ liệu một cách ngắn gọn:

```python
data['b']
data['b':'c']
data.loc[['b', 'd']]
data.loc[:, 2]
```
Kết quả lần lượt:

```python
1   -0.555730
3    1.965781
dtype: float64

b  1   -0.555730
   3    1.965781
c  1    1.393406
   2    0.092908
dtype: float64

b  1   -0.555730
   3    1.965781
d  2    0.281746
   3    0.769023
dtype: float64

a    0.478943
c    0.092908
d    0.281746
dtype: float64
```
Đánh chỉ mục phân cấp đóng vai trò quan trọng trong việc định hình lại dữ liệu và các hoạt động dựa trên nhóm như hình thành bảng pivot. Ví dụ, bạn có thể sắp xếp lại dữ liệu thành một DataFrame sử dụng phương thức unstack của nó:

```python
data.unstack()
```
Kết quả:

```python
          1         2         3
a -0.204708  0.478943 -0.519439
b -0.555730       NaN  1.965781
c  1.393406  0.092908       NaN
d       NaN  0.281746  0.769023
```
Hoạt động ngược lại của unstack là stack:

```python
data.unstack().stack()
```
Kết quả:

```python
a  1   -0.204708
   2    0.478943
   3   -0.519439
b  1   -0.555730
   3    1.965781
c  1    1.393406
   2    0.092908
d  2    0.281746
   3    0.769023
dtype: float64
```
Với một DataFrame, bất kỳ trục nào cũng có thể có chỉ mục phân cấp:

```python
frame = pd.DataFrame(np.arange(12).reshape((4, 3)),
                     index=[['a', 'a', 'b', 'b'], [1, 2, 1, 2]],
                     columns=[['Ohio', 'Ohio', 'Colorado'],
                              ['Green', 'Red', 'Green']])
frame
```
Kết quả:

```python
       Ohio     Colorado
       Green Red Green
a 1  0  1  2
  2  3  4  5
b 1  6  7  8
  2  9 10 11
```
Các mức phân cấp có thể có tên (như chuỗi hoặc bất kỳ đối tượng Python nào). Nếu có, chúng sẽ xuất hiện trong kết quả console:

```python
frame.index.names = ['key1', 'key2']
frame.columns.names = ['state', 'color']
frame
```
Kết quả:

```python
state         Ohio            Colorado
color         Green    Red      Green
key1 key2
a    1             0        1           2
     2             3        4           5
b    1             6        7           8
     2             9       10         11
```
Với chỉ mục cột một phần, bạn có thể chọn các nhóm cột tương tự:

```python
frame['Ohio']
```
Kết quả:

```python
color Green Red
key1 key2
a    1         0       1
     2         3       4
b    1         6       7
     2         9      10
```
Một MultiIndex có thể được tạo ra riêng và sau đó tái sử dụng; các cột trong DataFrame trước đó với tên mức có thể được tạo như sau:

```python
MultiIndex.from_arrays([['Ohio', 'Ohio', 'Colorado'], ['Green', 'Red', 'Green']],
                       names=['state', 'color'])
```
## Sắp Xếp Lại và Sắp Xếp Các Mức

Đôi khi bạn sẽ cần sắp xếp lại thứ tự của các mức trên một trục hoặc sắp xếp dữ liệu theo các giá trị trong một mức cụ thể. Hàm `swaplevel` nhận hai số mức hoặc tên mức và trả về một đối tượng mới với các mức được hoán đổi (nhưng dữ liệu không thay đổi):

```python
frame.swaplevel('key1', 'key2')
```
Kết quả:

```python
state  Ohio     Colorado
color  Green Red Green
key2  key1
1     a     0    1    2
2     a     3    4    5
1     b     6    7    8
2     b     9   10   11
```
Ngược lại, sort_index sắp xếp dữ liệu chỉ sử dụng các giá trị trong một mức duy nhất. Khi hoán đổi các mức, không hiếm khi sử dụng sort_index để kết quả được sắp xếp theo thứ tự từ điển học bởi mức chỉ định:

```python
frame.sort_index(level=1)
```
Kết quả:

```python
state  Ohio     Colorado
color  Green Red Green
key1  key2
a     1     0    1    2
b     1     6    7    8
a     2     3    4    5
b     2     9   10   11
```
```python
frame.swaplevel(0, 1).sort_index(level=0)
```
Kết quả:

```python
state  Ohio     Colorado
color  Green Red Green
key2  key1
1     a     0    1    2
1     b     6    7    8
2     a     3    4    5
2     b     9   10   11
```
Hiệu suất lựa chọn dữ liệu tốt hơn nhiều trên các đối tượng có chỉ mục phân cấp nếu chỉ mục được sắp xếp theo thứ tự từ điển học bắt đầu từ mức ngoài cùng—đó là kết quả của việc gọi sort_index(level=0) hoặc sort_index().

## Thống Kê Tổng Hợp Theo Mức

Nhiều thống kê mô tả và tổng hợp trên DataFrame và Series có tùy chọn `level` trong đó bạn có thể chỉ định mức bạn muốn tổng hợp theo trên một trục cụ thể. Hãy xem xét DataFrame ở trên; chúng ta có thể tổng hợp theo mức trên cả hàng hoặc cột như sau:

```python
frame.sum(level='key2')
```
Kết quả:

```python
state  Ohio     Colorado
color  Green Red Green
key2
1     6    8   10
2    12   14   16
```
```python
frame.sum(level='color', axis=1)
```
Kết quả:

```python
color      Green Red
key1 key2
a      1          2    1
       2          8    4
b      1         14    7
       2         20   10
```
## Lập Chỉ Mục Bằng Các Cột Của DataFrame

Không hiếm khi bạn muốn sử dụng một hoặc nhiều cột từ một DataFrame làm chỉ mục hàng; hoặc ngược lại, bạn có thể muốn di chuyển chỉ mục hàng vào các cột của DataFrame. Dưới đây là một ví dụ về DataFrame:

```python
frame = pd.DataFrame({'a': range(7), 'b': range(7, 0, -1),
                      'c': ['one', 'one', 'one', 'two', 'two', 'two', 'two'],
                      'd': [0, 1, 2, 0, 1, 2, 3]})
frame
```
Kết quả:

```python
   a  b    c  d
0  0  7  one  0
1  1  6  one  1
2  2  5  one  2
3  3  4  two  0
4  4  3  two  1
5  5  2  two  2
6  6  1  two  3
```
Hàm set_index của DataFrame sẽ tạo một DataFrame mới sử dụng một hoặc nhiều cột của nó làm chỉ mục:

```python
frame2 = frame.set_index(['c', 'd'])
frame2
```
Kết quả:

```python
         a  b
c   d        
one 0  0  7
     1  1  6
     2  2  5
two 0  3  4
     1  4  3
     2  5  2
     3  6  1
```
Theo mặc định, các cột bị loại bỏ khỏi DataFrame, mặc dù bạn có thể giữ chúng lại:

```python
frame.set_index(['c', 'd'], drop=False)
```
Kết quả:

```python
         a  b    c  d
c   d               
one 0  0  7  one  0
     1  1  6  one  1
     2  2  5  one  2
two 0  3  4  two  0
     1  4  3  two  1
     2  5  2  two  2
     3  6  1  two  3
```
Ngược lại, reset_index làm điều ngược lại của set_index; các mức chỉ mục phân cấp được di chuyển vào các cột:

```python
frame2.reset_index()
```
Kết quả:

```python
     c  d  a  b
0  one  0  0  7
1  one  1  1  6
2  one  2  2  5
3  two  0  3  4
4  two  1  4  3
5  two  2  5  2
6  two  3  6  1
```
