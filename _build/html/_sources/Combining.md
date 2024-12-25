# Kết Hợp và Ghép Nối Các Tập Dữ Liệu

Dữ liệu chứa trong các đối tượng pandas có thể được kết hợp theo nhiều cách khác nhau:
- `pandas.merge` kết nối các hàng trong DataFrame dựa trên một hoặc nhiều khóa. Điều này sẽ quen thuộc với những người dùng SQL hoặc các cơ sở dữ liệu quan hệ khác, vì nó thực hiện các thao tác nối cơ sở dữ liệu.
- `pandas.concat` nối hoặc "xếp chồng" các đối tượng cùng một trục.
- Phương thức `combine_first` cho phép ghép nối dữ liệu chồng chéo để điền vào các giá trị thiếu trong một đối tượng bằng các giá trị từ một đối tượng khác.

## Ghép Nối DataFrame Theo Kiểu Cơ Sở Dữ Liệu

Các thao tác ghép nối hoặc nối dữ liệu kết hợp các tập dữ liệu bằng cách liên kết các hàng sử dụng một hoặc nhiều khóa. Các thao tác này là trung tâm của các cơ sở dữ liệu quan hệ (ví dụ, dựa trên SQL). Hàm `merge` trong pandas là điểm nhập chính để sử dụng các thuật toán này trên dữ liệu của bạn. Hãy bắt đầu với một ví dụ đơn giản:

```python
df1 = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'], 'data1': range(7)})
df2 = pd.DataFrame({'key': ['a', 'b', 'd'], 'data2': range(3)})
df1
```
Kết quả:

```python
   data1 key
0      0   b
1      1   b
2      2   a
3      3   c
4      4   a
5      5   a
6      6   b
```
```python
df2
```
Kết quả:

```python
   data2 key
0      0   a
1      1   b
2      2   d
```
Đây là một ví dụ về ghép nối nhiều-đến-một; dữ liệu trong df1 có nhiều hàng được gán nhãn a và b, trong khi df2 chỉ có một hàng cho mỗi giá trị trong cột khóa. Gọi merge với các đối tượng này, chúng ta thu được:

```python
pd.merge(df1, df2)
```
Kết quả:

```python
   data1 key  data2
0      0   b      1
1      1   b      1
2      6   b      1
3      2   a      0
4      4   a      0
5      5   a      0
```
Lưu ý rằng không chỉ định cột nào để nối. Nếu thông tin đó không được chỉ định, merge sử dụng các tên cột trùng lặp làm khóa. Tốt hơn là chỉ định rõ ràng:

```python
pd.merge(df1, df2, on='key')
```
Kết quả:

```python
   data1 key  data2
0      0   b      1
1      1   b      1
2      6   b      1
3      2   a      0
4      4   a      0
5      5   a      0
```
Nếu các tên cột khác nhau trong từng đối tượng, có thể chỉ định chúng riêng biệt:

```python
df3 = pd.DataFrame({'lkey': ['b', 'b', 'a', 'c', 'a', 'a', 'b'], 'data1': range(7)})
df4 = pd.DataFrame({'rkey': ['a', 'b', 'd'], 'data2': range(3)})
pd.merge(df3, df4, left_on='lkey', right_on='rkey')
```
Kết quả:

```python
   data1 lkey  data2 rkey
0      0    b      1    b
1      1    b      1    b
2      6    b      1    b
3      2    a      0    a
4      4    a      0    a
5      5    a      0    a
```
Bạn có thể nhận thấy rằng các giá trị 'c' và 'd' và dữ liệu liên quan bị thiếu trong kết quả. Mặc định merge thực hiện một ghép nối 'inner'; các khóa trong kết quả là giao, hoặc tập hợp chung được tìm thấy trong cả hai bảng. Các tùy chọn khác có thể là 'left', 'right' và 'outer'. Ghép nối 'outer' lấy hợp của các khóa, kết hợp hiệu ứng của việc áp dụng cả hai ghép nối trái và phải:

```python
pd.merge(df1, df2, how='outer')
```
Kết quả:

```python
   data1 key  data2
0    0.0   b    1.0
1    1.0   b    1.0
2    6.0   b    1.0
3    2.0   a    0.0
4    4.0   a    0.0
5    5.0   a    0.0
6    3.0   c    NaN
7    NaN   d    2.0
```
### Bảng 8-1. Các loại ghép nối khác nhau với đối số `how`

| Tùy chọn  | Hành vi                                                       |
|-----------|---------------------------------------------------------------|
| `inner`   | Sử dụng chỉ các kết hợp khóa quan sát được trong cả hai bảng. |
| `left`    | Sử dụng tất cả các kết hợp khóa tìm thấy trong bảng trái.      |
| `right`   | Sử dụng tất cả các kết hợp khóa tìm thấy trong bảng phải.      |
| `outer`   | Sử dụng tất cả các kết hợp khóa quan sát được trong cả hai bảng cùng nhau. |

## Ghép Nối Nhiều-Đến-Nhiều

Các ghép nối nhiều-đến-nhiều có hành vi được xác định rõ ràng, mặc dù không nhất thiết phải trực quan. Dưới đây là một ví dụ:

```python
df1 = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'b'], 'data1': range(6)})
df2 = pd.DataFrame({'key': ['a', 'b', 'a', 'b', 'd'], 'data2': range(5)})
```
Kết quả:

```python
df1
```
```python
   data1 key
0      0   b
1      1   b
2      2   a
3      3   c
4      4   a
5      5   b
```
```python
df2
```
```python
   data2 key
0      0   a
1      1   b
2      2   a
3      3   b
4      4   d
```
Ghép nối nhiều-đến-nhiều tạo ra tích Descartes của các hàng. Vì có ba hàng 'b' trong DataFrame bên trái và hai trong DataFrame bên phải, nên có sáu hàng 'b' trong kết quả. Phương thức nối chỉ ảnh hưởng đến các giá trị khóa khác nhau xuất hiện trong kết quả:

```python
pd.merge(df1, df2, on='key', how='left')
```
Kết quả:

```python
   data1 key  data2
0      0   b    1.0
1      0   b    3.0
2      1   b    1.0
3      1   b    3.0
4      2   a    0.0
5      2   a    2.0
6      3   c    NaN
7      4   a    0.0
8      4   a    2.0
9      5   b    1.0
10     5   b    3.0
```
```python
pd.merge(df1, df2, how='inner')
```
Kết quả:

```python
   data1 key  data2
0      0   b      1
1      0   b      3
2      1   b      1
3      1   b      3
4      5   b      1
5      5   b      3
6      2   a      0
7      2   a      2
8      4   a      0
9      4   a      2
```
Để ghép nối với nhiều khóa, hãy truyền một danh sách các tên cột:

```python
left = pd.DataFrame({'key1': ['foo', 'foo', 'bar'], 'key2': ['one', 'two', 'one'], 'lval': [1, 2, 3]})
right = pd.DataFrame({'key1': ['foo', 'foo', 'bar', 'bar'], 'key2': ['one', 'one', 'one', 'two'], 'rval': [4, 5, 6, 7]})
pd.merge(left, right, on=['key1', 'key2'], how='outer')
```
Kết quả:

```python
  key1 key2  lval  rval
0  foo  one   1.0   4.0
1  foo  one   1.0   5.0
2  foo  two   2.0   NaN
3  bar  one   3.0   6.0
4  bar  two   NaN   7.0
```
Khi ghép nối các cột với các cột, các chỉ mục trên các đối tượng DataFrame được truyền vào sẽ bị loại bỏ.

Một vấn đề cuối cùng cần xem xét trong các thao tác ghép nối là việc xử lý các tên cột trùng lặp. Trong khi bạn có thể giải quyết sự trùng lặp thủ công (xem phần trước về đổi tên nhãn trục), hàm merge có tùy chọn suffixes để chỉ định các chuỗi để thêm vào các tên trùng lặp trong các đối tượng DataFrame bên trái và bên phải:

```python
pd.merge(left, right, on='key1')
```
Kết quả:

```python
  key1 key2_x  lval key2_y  rval
0  foo    one     1    one     4
1  foo    one     1    one     5
2  foo    two     2    one     4
3  foo    two     2    one     5
4  bar    one     3    one     6
5  bar    one     3    two     7
```
```python
pd.merge(left, right, on='key1', suffixes=('_left', '_right'))
```
Kết quả:

```python
  key1 key2_left  lval key2_right  rval
0  foo       one     1        one     4
1  foo       one     1        one     5
2  foo       two     2        one     4
3  foo       two     2        one     5
4  bar       one     3        one     6
5  bar       one     3        two     7
```
### Bảng 8-2. Các tham số hàm `merge`

| Tham số    | Mô tả                                                                                                                                                             |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `left`     | DataFrame được ghép nối ở phía trái.                                                                                                                             |
| `right`    | DataFrame được ghép nối ở phía phải.                                                                                                                             |
| `how`      | Một trong các giá trị 'inner', 'outer', 'left', hoặc 'right'; mặc định là 'inner'.                                                                                |
| `on`       | Tên các cột để ghép nối. Phải được tìm thấy trong cả hai đối tượng DataFrame. Nếu không chỉ định và không có khóa nối khác, sẽ sử dụng giao của tên các cột trong `left` và `right` làm khóa nối. |
| `left_on`  | Các cột trong DataFrame bên trái để sử dụng làm khóa nối.                                                                                                         |
| `right_on` | Tương tự như `left_on` cho DataFrame bên phải.                                                                                                                    |
| `left_index` | Sử dụng chỉ mục hàng trong `left` làm khóa nối của nó (hoặc các khóa, nếu là một `MultiIndex`).                                                                             |
| `right_index` | Tương tự như `left_index`.                                                                                                                                                    |
| `sort`     | Sắp xếp dữ liệu đã ghép nối theo thứ tự từ điển học bằng các khóa nối; True theo mặc định (vô hiệu hóa để có hiệu suất tốt hơn trong một số trường hợp trên các tập dữ liệu lớn).                  |
| `suffixes` | Tuple các giá trị chuỗi để thêm vào tên cột trong trường hợp trùng lặp; mặc định là ('_x', '_y') (ví dụ, nếu 'data' trong cả hai đối tượng DataFrame, sẽ xuất hiện dưới dạng 'data_x' và 'data_y' trong kết quả). |
| `copy`     | Nếu False, tránh sao chép dữ liệu vào cấu trúc dữ liệu kết quả trong một số trường hợp ngoại lệ; mặc định luôn sao chép.                                                                                |
| `indicator` | Thêm một cột đặc biệt `_merge` chỉ ra nguồn của mỗi hàng; các giá trị sẽ là 'left_only', 'right_only', hoặc 'both' dựa trên nguồn gốc của dữ liệu đã ghép nối trong mỗi hàng.                         |


## Ghép Nối Trên Chỉ Mục

Trong một số trường hợp, khóa ghép nối trong một DataFrame sẽ nằm trong chỉ mục của nó. Trong trường hợp này, bạn có thể truyền `left_index=True` hoặc `right_index=True` (hoặc cả hai) để chỉ ra rằng chỉ mục nên được sử dụng làm khóa ghép nối:

```python
left1 = pd.DataFrame({'key': ['a', 'b', 'a', 'a', 'b', 'c'], 'value': range(6)})
right1 = pd.DataFrame({'group_val': [3.5, 7]}, index=['a', 'b'])
pd.merge(left1, right1, left_on='key', right_index=True)
```
Kết quả:

```python
  key  value  group_val
0   a      0        3.5
2   a      2        3.5
3   a      3        3.5
1   b      1        7.0
4   b      4        7.0
```
Vì phương thức ghép nối mặc định là giao của các khóa nối, bạn có thể thay vào đó tạo hợp của chúng với một ghép nối ngoài:

```python
pd.merge(left1, right1, left_on='key', right_index=True, how='outer')
```
Kết quả:

```python
  key  value  group_val
0   a      0        3.5
2   a      2        3.5
3   a      3        3.5
1   b      1        7.0
4   b      4        7.0
5   c      5        NaN
```
Với dữ liệu được đánh chỉ mục phân cấp, mọi thứ phức tạp hơn, vì ghép nối trên chỉ mục ngầm định là một ghép nối nhiều khóa:

```python
lefth = pd.DataFrame({'key1': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'],
                      'key2': [2000, 2001, 2002, 2001, 2002],
                      'data': np.arange(5.)})
righth = pd.DataFrame(np.arange(12).reshape((6, 2)),
                      index=[['Nevada', 'Nevada', 'Ohio', 'Ohio', 'Ohio', 'Ohio'],
                             [2001, 2000, 2000, 2000, 2001, 2002]],
                      columns=['event1', 'event2'])
pd.merge(lefth, righth, left_on=['key1', 'key2'], right_index=True)
```
Kết quả:

```python
   data    key1  key2  event1  event2
0   0.0    Ohio  2000     4.0     5.0
0   0.0    Ohio  2000     6.0     7.0
1   1.0    Ohio  2001     8.0     9.0
2   2.0    Ohio  2002    10.0    11.0
3   3.0  Nevada  2001     0.0     1.0
```
```python
pd.merge(lefth, righth, left_on=['key1', 'key2'], right_index=True, how='outer')
```
Kết quả:

```python
   data    key1  key2  event1  event2
0   0.0    Ohio  2000     4.0     5.0
0   0.0    Ohio  2000     6.0     7.0
1   1.0    Ohio  2001     8.0     9.0
2   2.0    Ohio  2002    10.0    11.0
3   3.0  Nevada  2001     0.0     1.0
4   4.0  Nevada  2002     NaN     NaN
4   NaN  Nevada  2000     2.0     3.0
```
Sử dụng chỉ mục của cả hai phía của ghép nối cũng có thể thực hiện được:

```python
left2 = pd.DataFrame([[1., 2.], [3., 4.], [5., 6.]], index=['a', 'c', 'e'], columns=['Ohio', 'Nevada'])
right2 = pd.DataFrame([[7., 8.], [9., 10.], [11., 12.], [13, 14]], index=['b', 'c', 'd', 'e'], columns=['Missouri', 'Alabama'])
pd.merge(left2, right2, how='outer', left_index=True, right_index=True)
```
Kết quả:

```python
   Ohio  Nevada  Missouri  Alabama
a   1.0     2.0       NaN      NaN
b   NaN     NaN       7.0      8.0
c   3.0     4.0       9.0     10.0
d   NaN     NaN      11.0     12.0
e   5.0     6.0      13.0     14.0
```
DataFrame có một phương thức join tiện lợi để ghép nối theo chỉ mục. Nó cũng có thể được sử dụng để kết hợp nhiều đối tượng DataFrame có cùng hoặc tương tự chỉ mục nhưng các cột không trùng lặp. Trong ví dụ trước, chúng ta có thể viết:

```python
left2.join(right2, how='outer')
```
Kết quả:

```python
   Ohio  Nevada  Missouri  Alabama
a   1.0     2.0       NaN      NaN
b   NaN     NaN       7.0      8.0
c   3.0     4.0       9.0     10.0
d   NaN     NaN      11.0     12.0
e   5.0     6.0      13.0     14.0
```
Phần nào vì lý do kế thừa (tức là các phiên bản pandas cũ hơn nhiều), phương thức join của DataFrame thực hiện một ghép nối trái trên các khóa nối, giữ nguyên chỉ mục hàng của DataFrame bên trái. Nó cũng hỗ trợ ghép nối chỉ mục của DataFrame được truyền vào trên một trong các cột của DataFrame gọi:

```python
left1.join(right1, on='key')
```
Kết quả:

```python
  key  value  group_val
0   a      0        3.5
1   b      1        7.0
2   a      2        3.5
3   a      3        3.5
4   b      4        7.0
5   c      5        NaN
```
Cuối cùng, đối với các ghép nối chỉ mục-đến-chỉ mục đơn giản, bạn có thể truyền một danh sách các DataFrame để ghép nối như một thay thế cho việc sử dụng hàm concat tổng quát hơn được mô tả trong phần tiếp theo:

```python
another = pd.DataFrame([[7., 8.], [9., 10.], [11., 12.], [16., 17.]], index=['a', 'c', 'e', 'f'], columns=['New York', 'Oregon'])
left2.join([right2, another])
```
Kết quả:

```python
   Ohio  Nevada  Missouri  Alabama  New York  Oregon
a   1.0     2.0       NaN      NaN       7.0     8.0
c   3.0     4.0       9.0     10.0       9.0    10.0
e   5.0     6.0      13.0     14.0      11.0    12.0
```
```python
left2.join([right2, another], how='outer')
```
Kết quả:

```python
   Ohio  Nevada  Missouri  Alabama  New York  Oregon
a   1.0     2.0       NaN      NaN       7.0     8.0
b   NaN     NaN       7.0      8.0       NaN     NaN
c   3.0     4.0       9.0     10.0       9.0    10.0
d   NaN     NaN      11.0     12.0       NaN     NaN
e   5.0     6.0      13.0     14.0      11.0    12.0
f   NaN     NaN       NaN      NaN      16.0    17.0
```
## Ghép Nối Dọc Theo Một Trục

Một loại thao tác kết hợp dữ liệu khác được gọi là ghép nối, liên kết, hoặc xếp chồng. Hàm `concatenate` của NumPy có thể làm điều này với các mảng NumPy:

```python
arr = np.arange(12).reshape((3, 4))
np.concatenate([arr, arr], axis=1)
```
Kết quả:

```python
array([[ 0, 1, 2, 3, 0, 1, 2, 3],
       [ 4, 5, 6, 7, 4, 5, 6, 7],
       [ 8, 9, 10, 11, 8, 9, 10, 11]])
```
Trong ngữ cảnh của các đối tượng pandas như Series và DataFrame, có các trục được gán nhãn cho phép bạn tổng quát hóa thêm việc ghép nối mảng. Đặc biệt, bạn có một số điều cần suy nghĩ thêm:

Nếu các đối tượng được đánh chỉ mục khác nhau trên các trục khác, liệu chúng ta nên kết hợp các phần tử khác biệt trong các trục này hay chỉ sử dụng các giá trị chung (giao)?

Các khối dữ liệu ghép nối có cần phải được nhận dạng trong đối tượng kết quả không?

Trục "ghép nối" có chứa dữ liệu cần được bảo toàn không? Trong nhiều trường hợp, các nhãn số nguyên mặc định trong DataFrame tốt nhất là bỏ qua trong quá trình ghép nối.

Hàm concat trong pandas cung cấp một cách nhất quán để giải quyết từng vấn đề này. Dưới đây là một số ví dụ minh họa cách nó hoạt động. Giả sử chúng ta có ba Series không có sự trùng lặp chỉ mục:

```python
s1 = pd.Series([0, 1], index=['a', 'b'])
s2 = pd.Series([2, 3, 4], index=['c', 'd', 'e'])
s3 = pd.Series([5, 6], index=['f', 'g'])
pd.concat([s1, s2, s3])
```
Kết quả:

```python
a    0
b    1
c    2
d    3
e    4
f    5
g    6
dtype: int64
```
Mặc định, concat hoạt động dọc theo axis=0, tạo ra một Series khác. Nếu bạn truyền axis=1, kết quả sẽ là một DataFrame (trục axis=1 là các cột):

```python
pd.concat([s1, s2, s3], axis=1)
```
Kết quả:

```python
     0    1    2
a  0.0  NaN  NaN
b  1.0  NaN  NaN
c  NaN  2.0  NaN
d  NaN  3.0  NaN
e  NaN  4.0  NaN
f  NaN  NaN  5.0
g  NaN  NaN  6.0
```
Trong trường hợp này, không có sự trùng lặp trên trục khác, như bạn có thể thấy là hợp sắp xếp (ghép nối 'outer') của các chỉ mục. Bạn có thể thay vào đó giao nhau bằng cách truyền join='inner':

```python
s4 = pd.concat([s1, s3])
pd.concat([s1, s4], axis=1, join='inner')
```
Kết quả:

```python
     0  1
a  0  0
b  1  1
```
Bạn cũng có thể chỉ định các trục được sử dụng trên các trục khác với join_axes:

```python
pd.concat([s1, s4], axis=1, join_axes=[['a', 'c', 'b', 'e']])
```
Kết quả:

```python
     0    1
a  0.0  0.0
c  NaN  NaN
b  1.0  1.0
e  NaN  NaN
```
Một vấn đề tiềm năng là các phần ghép nối không được nhận dạng trong kết quả. Giả sử bạn muốn tạo một chỉ mục phân cấp trên trục ghép nối. Để làm điều này, sử dụng đối số keys:

```python
result = pd.concat([s1, s1, s3], keys=['one', 'two', 'three'])
result.unstack()
```
Kết quả:

```python
        a    b    f    g
one   0.0  1.0  NaN  NaN
two   0.0  1.0  NaN  NaN
three NaN  NaN  5.0  6.0
```
Trong trường hợp kết hợp các Series dọc theo axis=1, các keys trở thành các tiêu đề cột của DataFrame:

```python
pd.concat([s1, s2, s3], axis=1, keys=['one', 'two', 'three'])
```
Kết quả:

```python
    one  two  three
a  0.0  NaN  NaN
b  1.0  NaN  NaN
c  NaN  2.0  NaN
d  NaN  3.0  NaN
e  NaN  4.0  NaN
f  NaN  NaN  5.0
g  NaN  NaN  6.0
```
Logic tương tự cũng áp dụng cho các đối tượng DataFrame:

```python
df1 = pd.DataFrame(np.arange(6).reshape(3, 2), index=['a', 'b', 'c'], columns=['one', 'two'])
df2 = pd.DataFrame(5 + np.arange(4).reshape(2, 2), index=['a', 'c'], columns=['three', 'four'])
pd.concat([df1, df2], axis=1, keys=['level1', 'level2'])
```
Kết quả:

```python
      level1      level2
       one two three four
a     0   1   5.0   6.0
b     2   3   NaN   NaN
c     4   5   7.0   8.0
```
Nếu bạn truyền một dict các đối tượng thay vì một danh sách, các khóa của dict sẽ được sử dụng cho tùy chọn keys:

```python
pd.concat({'level1': df1, 'level2': df2}, axis=1)
```
Kết quả:

```python
      level1      level2
       one two three four
a     0   1   5.0   6.0
b     2   3   NaN   NaN
c     4   5   7.0   8.0
```
Có các đối số bổ sung điều chỉnh cách tạo chỉ mục phân cấp. Ví dụ, chúng ta có thể đặt tên cho các mức trục được tạo bằng đối số names:

```python
pd.concat([df1, df2], axis=1, keys=['level1', 'level2'], names=['upper', 'lower'])
```
Kết quả:

```python
upper  level1       level2
lower   one two three four
a     0   1   5.0   6.0
b     2   3   NaN   NaN
c     4   5   7.0   8.0
```
Cuối cùng, đối với các DataFrame mà chỉ mục hàng không chứa bất kỳ dữ liệu liên quan nào:

```python
df1 = pd.DataFrame(np.random.randn(3, 4), columns=['a', 'b', 'c', 'd'])
df2 = pd.DataFrame(np.random.randn(2, 3), columns=['b', 'd', 'a'])
pd.concat([df1, df2], ignore_index=True)
```
Kết quả:

```python
          a        b         c         d
0  1.246435  1.007189 -1.296221  0.274992
1  0.228913  1.352917  0.886429 -2.001637
2 -0.371843  1.669025 -0.438570 -0.539741
3 -1.021228  0.476985       NaN  3.248944
4  0.302614 -0.577087       NaN  0.124121
```
### Bảng 8-3. Các tham số hàm `concat`

| Tham số           | Mô tả                                                                                                                                                                                      |
|-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `objs`            | Danh sách hoặc dict các đối tượng pandas để ghép nối; đây là tham số bắt buộc duy nhất.                                                                                                     |
| `axis`            | Trục để ghép nối dọc theo; mặc định là 0 (dọc theo các hàng).                                                                                                                               |
| `join`            | 'inner' hoặc 'outer' ('outer' theo mặc định); có ghép nối giao (inner) hoặc hợp (outer) các chỉ mục dọc theo các trục khác.                                                                  |
| `join_axes`       | Các chỉ mục cụ thể để sử dụng cho các trục khác thay vì thực hiện logic giao/hợp.                                                                                                           |
| `keys`            | Các giá trị liên kết với các đối tượng đang được ghép nối, tạo thành một chỉ mục phân cấp dọc theo trục ghép nối; có thể là một danh sách hoặc mảng các giá trị bất kỳ, một mảng các tuple, hoặc một danh sách các mảng (nếu nhiều mức mảng được truyền vào). |
| `levels`          | Các chỉ mục cụ thể để sử dụng làm mức chỉ mục phân cấp nếu `keys` được truyền vào.                                                                                                           |
| `names`           | Tên cho các mức phân cấp được tạo ra nếu `keys` và/hoặc `levels` được truyền vào.                                                                                                          |
| `verify_integrity`| Kiểm tra trục mới trong đối tượng ghép nối để phát hiện các trùng lặp và ném ra ngoại lệ nếu có; theo mặc định (False) cho phép trùng lặp.                                                  |
| `ignore_index`    | Không bảo toàn các chỉ mục dọc theo trục ghép nối, thay vào đó tạo ra một chỉ mục `range(total_length)` mới.                                                                                |

## Kết Hợp Dữ Liệu Với Sự Chồng Lấn

Có một tình huống kết hợp dữ liệu khác không thể biểu diễn dưới dạng một thao tác ghép nối hoặc ghép nối liên kết. Bạn có thể có hai tập dữ liệu mà chỉ mục của chúng chồng lấn một phần hoặc toàn bộ. Ví dụ điển hình là hàm `where` của NumPy, thực hiện tương đương với một biểu thức `if-else` theo mảng:

```python
a = pd.Series([np.nan, 2.5, np.nan, 3.5, 4.5, np.nan], index=['f', 'e', 'd', 'c', 'b', 'a'])
b = pd.Series(np.arange(len(a), dtype=np.float64), index=['f', 'e', 'd', 'c', 'b', 'a'])
b[-1] = np.nan
```
```python
np.where(pd.isnull(a), b, a)
```
Kết quả:

```python
array([ 0. , 2.5, 2. , 3.5, 4.5, nan])
```
Series có một phương thức combine_first, thực hiện tương đương với thao tác này cùng với logic căn chỉnh dữ liệu thông thường của pandas:

```python
b[:-2].combine_first(a[2:])
```
Kết quả:

```python
a    NaN
b    4.5
c    3.0
d    2.0
e    1.0
f    0.0
dtype: float64
```
Với DataFrame, combine_first làm điều tương tự từng cột một, vì vậy bạn có thể nghĩ nó như là "vá" dữ liệu thiếu trong đối tượng gọi bằng dữ liệu từ đối tượng bạn truyền vào:

```python
df1 = pd.DataFrame({'a': [1., np.nan, 5., np.nan], 'b': [np.nan, 2., np.nan, 6.], 'c': range(2, 18, 4)})
df2 = pd.DataFrame({'a': [5., 4., np.nan, 3., 7.], 'b': [np.nan, 3., 4., 6., 8.]})
df1.combine_first(df2)
```
Kết quả:

```python
     a    b     c
0  1.0  NaN   2.0
1  4.0  2.0   6.0
2  5.0  4.0  10.0
3  3.0  6.0  14.0
4  7.0  8.0   NaN 
```

