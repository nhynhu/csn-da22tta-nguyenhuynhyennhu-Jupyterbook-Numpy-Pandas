# Giới thiệu về các cấu trúc dữ liệu trong pandas

Để làm việc với pandas, hai cấu trúc dữ liệu cơ bản cần phải làm quen là Series và DataFrame. Chúng là nền tảng mạnh mẽ, dễ sử dụng cho hầu hết các ứng dụng trong pandas.

## Series
Series là một đối tượng một chiều chứa một chuỗi các giá trị có kiểu dữ liệu giống như kiểu của NumPy, và một mảng các nhãn dữ liệu liên kết, được gọi là chỉ mục.

Series đơn giản nhất có thể được tạo từ một mảng dữ liệu:

```python
In [11]: obj = pd.Series([4, 7, -5, 3])
In [12]: obj
Out[12]:
0    4
1    7
2   -5
3    3
dtype: int64
```
Trong đó, chỉ mục mặc định là một dãy số nguyên từ 0 đến N-1 (N là độ dài của dữ liệu). Mảng giá trị và đối tượng chỉ mục có thể truy xuất thông qua các thuộc tính values và index của Series:

```python
In [13]: obj.values
Out[13]: array([ 4, 7, -5, 3])

In [14]: obj.index
Out[14]: RangeIndex(start=0, stop=4, step=1)
```
Series có thể được tạo với một chỉ mục xác định, với các nhãn thay thế cho số nguyên mặc định:

```python
In [15]: obj2 = pd.Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
In [16]: obj2
Out[16]:
d    4
b    7
a   -5
c    3
dtype: int64
```
Chỉ mục có thể được sử dụng để chọn các giá trị đơn lẻ hoặc một tập hợp giá trị:

```python
In [18]: obj2['a']
Out[18]: -5

In [19]: obj2['d'] = 6
In [20]: obj2[['c', 'a', 'd']]
Out[20]:
c    3
a   -5
d    6
dtype: int64
```
Các phép toán với Series giữ nguyên liên kết giữa chỉ mục và giá trị:

```python
In [21]: obj2[obj2 > 0]
Out[21]:
d    6
b    7
c    3
dtype: int64

In [22]: obj2 * 2
Out[22]:
d    12
b    14
a   -10
c     6
dtype: int64

In [23]: np.exp(obj2)
Out[23]:
d     403.428793
b     1096.633158
a       0.006738
c      20.085537
dtype: float64
```
Series có thể được coi như một từ điển có thứ tự, vì nó ánh xạ chỉ mục đến các giá trị. Trong các tình huống, Series có thể thay thế cho một từ điển:

```python
In [24]: 'b' in obj2
Out[24]: True

In [25]: 'e' in obj2
Out[25]: False
```
Khi dữ liệu được chứa trong một từ điển Python, Series có thể được tạo ra từ nó:

```python
In [26]: sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
In [27]: obj3 = pd.Series(sdata)
In [28]: obj3
Out[28]:
Ohio        35000
Oregon      16000
Texas       71000
Utah         5000
dtype: int64
```
Khi chỉ truyền vào một từ điển, chỉ mục trong Series kết quả sẽ là các khóa của từ điển, được sắp xếp theo thứ tự. Nếu muốn thay đổi thứ tự này, có thể chỉ định một chỉ mục khác:

```python
In [29]: states = ['California', 'Ohio', 'Oregon', 'Texas']
In [30]: obj4 = pd.Series(sdata, index=states)
In [31]: obj4
Out[31]:
California    NaN
Ohio        35000.0
Oregon      16000.0
Texas       71000.0
dtype: float64
```
Ở đây, các giá trị từ sdata được chèn vào đúng vị trí, nhưng giá trị cho 'California' không có sẵn, nên nó xuất hiện là NaN (Not a Number), đại diện cho giá trị bị thiếu.

Các hàm isnull và notnull trong pandas giúp phát hiện giá trị bị thiếu:

```python
In [32]: pd.isnull(obj4)
Out[32]:
California    True
Ohio        False
Oregon      False
Texas       False
dtype: bool

In [33]: pd.notnull(obj4)
Out[33]:
California    False
Ohio        True
Oregon      True
Texas       True
dtype: bool
```
Series cũng có các phương thức để kiểm tra giá trị bị thiếu:

```python
In [34]: obj4.isnull()
Out[34]:
California    True
Ohio        False
Oregon      False
Texas       False
dtype: bool
```
Một tính năng hữu ích của Series là tự động căn chỉnh dữ liệu theo chỉ mục khi thực hiện các phép toán số học:

```python
In [35]: obj3
Out[35]:
Ohio        35000
Oregon      16000
Texas       71000
Utah         5000
dtype: int64

In [36]: obj4
Out[36]:
California    NaN
Ohio        35000.0
Oregon      16000.0
Texas       71000.0
dtype: float64

In [37]: obj3 + obj4
Out[37]:
California    NaN
Ohio        70000.0
Oregon      32000.0
Texas      142000.0
Utah         NaN
dtype: float64
```
Các tính năng căn chỉnh dữ liệu sẽ được giải thích chi tiết hơn ở các phần sau. Cả Series và chỉ mục của nó đều có thuộc tính name, giúp tích hợp với các tính năng khác trong pandas:

```python
In [38]: obj4.name = 'population'
In [39]: obj4.index.name = 'state'
In [40]: obj4
Out[40]:
state
California    NaN
Ohio        35000.0
Oregon      16000.0
Texas       71000.0
Name: population, dtype: float64
```
Chỉ mục của một Series có thể được thay đổi bằng cách gán lại giá trị mới:

```python
In [41]: obj
Out[41]:
0    4
1    7
2   -5
3    3
dtype: int64

In [42]: obj.index = ['Bob', 'Steve', 'Jeff', 'Ryan']
In [43]: obj
Out[43]:
Bob      4
Steve    7
Jeff    -5
Ryan     3
dtype: int64
```
### DataFrame

Một `DataFrame` đại diện cho một bảng dữ liệu hình chữ nhật và chứa một tập hợp có thứ tự các cột, mỗi cột có thể là một kiểu giá trị khác nhau (số, chuỗi, boolean, v.v.). `DataFrame` có cả chỉ số hàng và cột; nó có thể được xem như một từ điển của các `Series` cùng chia sẻ chung một chỉ số. Về mặt nội bộ, dữ liệu được lưu trữ dưới dạng một hoặc nhiều khối hai chiều thay vì một danh sách, từ điển hoặc một số tập hợp khác của các mảng một chiều. Chi tiết cụ thể về nội bộ của `DataFrame` nằm ngoài phạm vi của tài liệu này.

Mặc dù `DataFrame` về mặt vật lý là hai chiều, nó có thể được sử dụng để biểu diễn dữ liệu đa chiều ở dạng bảng thông qua chỉ số phân cấp. Đây là một chủ đề sẽ được thảo luận chi tiết trong Chương 8 và là thành phần trong một số tính năng xử lý dữ liệu tiên tiến hơn trong pandas.

Có nhiều cách để tạo một `DataFrame`, nhưng một trong những cách phổ biến nhất là từ một từ điển chứa các danh sách hoặc mảng NumPy có độ dài bằng nhau:

```python
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002, 2003],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}
frame = pd.DataFrame(data)
```
DataFrame kết quả sẽ có chỉ số tự động như với Series, và các cột được sắp xếp theo thứ tự:

```python
In [45]: frame
Out[45]:
    pop   state  year
0   1.5    Ohio  2000
1   1.7    Ohio  2001
2   3.6    Ohio  2002
3   2.4  Nevada  2001
4   2.9  Nevada  2002
5   3.2  Nevada  2003
```
Nếu đang sử dụng Jupyter notebook, các đối tượng DataFrame của pandas sẽ được hiển thị dưới dạng bảng HTML thân thiện với trình duyệt.

Đối với các DataFrame lớn, phương thức head chỉ chọn năm hàng đầu tiên:

```python
In [46]: frame.head()
Out[46]:
    pop   state  year
0   1.5    Ohio  2000
1   1.7    Ohio  2001
2   3.6    Ohio  2002
3   2.4  Nevada  2001
4   2.9  Nevada  2002
```
Nếu chỉ định một chuỗi các cột, các cột của DataFrame sẽ được sắp xếp theo thứ tự đó:

```python
In [47]: pd.DataFrame(data, columns=['year', 'state', 'pop'])
Out[47]:
   year   state  pop
0  2000    Ohio  1.5
1  2001    Ohio  1.7
2  2002    Ohio  3.6
3  2001  Nevada  2.4
4  2002  Nevada  2.9
5  2003  Nevada  3.2
```
Nếu truyền một cột không có trong từ điển, nó sẽ xuất hiện với các giá trị bị thiếu trong kết quả:

```python
In [48]: frame2 = pd.DataFrame(data, columns=['year', 'state', 'pop', 'debt'], 
                               index=['one', 'two', 'three', 'four', 'five', 'six'])
In [49]: frame2
Out[49]:
       year   state  pop  debt
one    2000    Ohio  1.5   NaN
two    2001    Ohio  1.7   NaN
three  2002    Ohio  3.6   NaN
four   2001  Nevada  2.4   NaN
five   2002  Nevada  2.9   NaN
six    2003  Nevada  3.2   NaN
```

Một cột trong DataFrame có thể được truy xuất dưới dạng Series bằng cách sử dụng ký hiệu giống từ điển hoặc bằng thuộc tính:

```python
In [51]: frame2['state']
Out[51]:
one        Ohio
two        Ohio
three      Ohio
four     Nevada
five     Nevada
six      Nevada
Name: state, dtype: object

In [52]: frame2.year
Out[52]:
one      2000
two      2001
three    2002
four     2001
five     2002
six      2003
Name: year, dtype: int64
```
Lưu ý rằng các Series trả về có cùng chỉ số với DataFrame, và thuộc tính name của chúng đã được đặt phù hợp.

Các hàng cũng có thể được truy xuất theo vị trí hoặc tên bằng thuộc tính đặc biệt loc:

```python
In [53]: frame2.loc['three']
Out[53]:
year     2002
state    Ohio
pop       3.6
debt      NaN
Name: three, dtype: object
```
Các cột có thể được sửa đổi bằng cách gán giá trị. Ví dụ, cột 'debt' trống có thể được gán một giá trị vô hướng hoặc một mảng giá trị:

```python
In [54]: frame2['debt'] = 16.5
In [55]: frame2
Out[55]:
       year   state  pop  debt
one    2000    Ohio  1.5  16.5
two    2001    Ohio  1.7  16.5
three  2002    Ohio  3.6  16.5
four   2001  Nevada  2.4  16.5
five   2002  Nevada  2.9  16.5
six    2003  Nevada  3.2  16.5

In [56]: frame2['debt'] = np.arange(6.)
In [57]: frame2
Out[57]:
       year   state  pop  debt
one    2000    Ohio  1.5   0.0
two    2001    Ohio  1.7   1.0
three  2002    Ohio  3.6   2.0
four   2001  Nevada  2.4   3.0
five   2002  Nevada  2.9   4.0
six    2003  Nevada  3.2   5.0
```
Khi gán các danh sách hoặc mảng cho một cột, độ dài của giá trị phải khớp với độ dài của DataFrame. Nếu gán một Series, các nhãn của nó sẽ được căn chỉnh chính xác với chỉ số của DataFrame, chèn các giá trị bị thiếu vào bất kỳ khoảng trống nào:

```python
In [58]: val = pd.Series([-1.2, -1.5, -1.7], index=['two', 'four', 'five'])
In [59]: frame2['debt'] = val
In [60]: frame2
Out[60]:
       year   state  pop  debt
one    2000    Ohio  1.5   NaN
two    2001    Ohio  1.7  -1.2
three  2002    Ohio  3.6   NaN
four   2001  Nevada  2.4  -1.5
five   2002  Nevada  2.9  -1.7
six    2003  Nevada  3.2   NaN
```
Gán một cột không tồn tại sẽ tạo ra một cột mới. Từ khóa del sẽ xóa các cột giống như với từ điển.

Là một ví dụ về del, thêm một cột mới chứa các giá trị boolean nơi cột state bằng 'Ohio':

```python
In [61]: frame2['eastern'] = frame2.state == 'Ohio'
In [62]: frame2
Out[62]:
       year   state  pop  debt  eastern
one    2000    Ohio  1.5   NaN     True
two    2001    Ohio  1.7  -1.2     True
three  2002    Ohio  3.6   NaN     True
four   2001  Nevada  2.4  -1.5    False
five   2002  Nevada  2.9  -1.7    False
six    2003  Nevada 3.2
```
### Các Kiểu Dữ Liệu Đầu Vào Có Thể Sử Dụng Cho Constructor của DataFrame

| Type                    | Notes                                                                                  |
|-------------------------|----------------------------------------------------------------------------------------|
| 2D ndarray              | Một ma trận dữ liệu, truyền tùy chọn nhãn hàng và cột                                   |
| dict của arrays, lists, hoặc tuples | Mỗi chuỗi trở thành một cột trong DataFrame; tất cả các chuỗi phải có cùng độ dài |
| NumPy structured/record array | Được xử lý như trường hợp “dict của arrays”                                        |
| dict của Series         | Mỗi giá trị trở thành một cột; chỉ mục từ mỗi Series được hợp nhất để tạo chỉ mục hàng của kết quả nếu không có chỉ mục rõ ràng |
| dict của dicts          | Mỗi dict bên trong trở thành một cột; các khóa được hợp nhất để tạo chỉ mục hàng như trong trường hợp “dict của Series” |
| List của dicts hoặc Series | Mỗi mục trở thành một hàng trong DataFrame; hợp nhất các khóa dict hoặc chỉ mục Series trở thành nhãn cột của DataFrame |
| List của lists hoặc tuples | Được xử lý như trường hợp “2D ndarray”                                               |
| Another DataFrame       | Chỉ mục của DataFrame được sử dụng trừ khi chỉ mục khác được truyền vào                 |
| NumPy MaskedArray       | Giống như trường hợp “2D ndarray” ngoại trừ các giá trị bị che trở thành NA/mất trong kết quả DataFrame |

### Đối Tượng Index

Các đối tượng `Index` trong pandas chịu trách nhiệm lưu giữ các nhãn trục và các siêu dữ liệu khác (như tên trục). Bất kỳ mảng hoặc chuỗi nhãn nào bạn sử dụng khi tạo `Series` hoặc `DataFrame` đều được chuyển đổi nội bộ thành một `Index`:

```python
In [76]: obj = pd.Series(range(3), index=['a', 'b', 'c'])
In [77]: index = obj.index
In [78]: index
Out[78]: Index(['a', 'b', 'c'], dtype='object')
In [79]: index[1:]
Out[79]: Index(['b', 'c'], dtype='object')
```
Các đối tượng Index là bất biến và do đó không thể được sửa đổi bởi người dùng:

```python
index[1] = 'd' # TypeError
```
Tính bất biến giúp chia sẻ các đối tượng Index giữa các cấu trúc dữ liệu an toàn hơn:

```python
In [80]: labels = pd.Index(np.arange(3))
In [81]: labels
Out[81]: Int64Index([0, 1, 2], dtype='int64')
In [82]: obj2 = pd.Series([1.5, -2.5, 0], index=labels)
In [83]: obj2
Out[83]:
0    1.5
1   -2.5
2    0.0
dtype: float64
In [84]: obj2.index is labels
Out[84]: True
```
Một số người dùng sẽ không thường xuyên tận dụng các khả năng do Index cung cấp, nhưng vì một số thao tác sẽ trả về kết quả chứa dữ liệu được lập chỉ mục, nên điều quan trọng là phải hiểu cách chúng hoạt động.

Ngoài việc có tính chất giống mảng, một Index còn hoạt động giống như một tập hợp có kích thước cố định:

```python
In [85]: frame3
Out[85]:
state   Nevada  Ohio
year                  
2000       NaN   1.5
2001       2.4   1.7
2002       2.9   3.6

In [86]: frame3.columns
Out[86]: Index(['Nevada', 'Ohio'], dtype='object', name='state')

In [87]: 'Ohio' in frame3.columns
Out[87]: True

In [88]: 2003 in frame3.index
Out[88]: False
```
Không giống như các tập hợp trong Python, một Index của pandas có thể chứa các nhãn trùng lặp:

```python
In [89]: dup_labels = pd.Index(['foo', 'foo', 'bar', 'bar'])
In [90]: dup_labels
Out[90]: Index(['foo', 'foo', 'bar', 'bar'], dtype='object')
```
Việc chọn các nhãn trùng lặp sẽ chọn tất cả các lần xuất hiện của nhãn đó.

Mỗi Index có một số phương thức và thuộc tính để thực hiện logic tập hợp, trả lời các câu hỏi phổ biến khác về dữ liệu mà nó chứa. Một số phương thức hữu ích được tóm tắt trong Bảng 5-2.

### Bảng 5-2. Một Số Phương Thức và Thuộc Tính của Index

| Method         | Description                                                                        |
|----------------|------------------------------------------------------------------------------------|
| append         | Nối thêm với các đối tượng `Index` khác, tạo ra một `Index` mới                     |
| difference     | Tính toán hiệu tập hợp và trả về một `Index`                                        |
| intersection   | Tính toán giao tập hợp                                                              |
| union          | Tính toán hợp tập hợp                                                               |
| isin           | Tính toán mảng boolean chỉ ra liệu mỗi giá trị có trong tập hợp đã truyền hay không |
| delete         | Tính toán `Index` mới với phần tử tại chỉ số i đã bị xóa                            |
| drop           | Tính toán `Index` mới bằng cách xóa các giá trị đã truyền                           |
| insert         | Tính toán `Index` mới bằng cách chèn phần tử tại chỉ số i                           |
| is_monotonic   | Trả về `True` nếu mỗi phần tử lớn hơn hoặc bằng phần tử trước đó                    |
| is_unique      | Trả về `True` nếu `Index` không có giá trị trùng lặp                                |
| unique         | Tính toán mảng các giá trị duy nhất trong `Index`                                   |

