# Essential Functionality

Phần này sẽ hướng dẫn các cơ chế cơ bản để tương tác với dữ liệu chứa trong một `Series` hoặc `DataFrame`. Trong các chương tiếp theo, chúng ta sẽ đi sâu hơn vào các chủ đề phân tích và thao tác dữ liệu sử dụng pandas. Cuốn sách này không nhằm mục đích làm tài liệu đầy đủ cho thư viện pandas; thay vào đó, chúng ta sẽ tập trung vào các tính năng quan trọng nhất, để bạn tự khám phá các tính năng ít phổ biến hơn.

## Reindexing

Một phương thức quan trọng trên các đối tượng pandas là `reindex`, tức là tạo một đối tượng mới với dữ liệu phù hợp với một chỉ số mới. Xem xét ví dụ sau:

```python
In [91]: obj = pd.Series([4.5, 7.2, -5.3, 3.6], index=['d', 'b', 'a', 'c'])
In [92]: obj
Out[92]:
d    4.5
b    7.2
a   -5.3
c    3.6
dtype: float64
```
Gọi reindex trên Series này sẽ sắp xếp lại dữ liệu theo chỉ số mới, và nếu có bất kỳ giá trị nào của chỉ số không có sẵn, các giá trị bị thiếu sẽ được thêm vào:

```python
In [93]: obj2 = obj.reindex(['a', 'b', 'c', 'd', 'e'])
In [94]: obj2
Out[94]:
a   -5.3
b    7.2
c    3.6
d    4.5
e    NaN
dtype: float64
```
Đối với dữ liệu có thứ tự như chuỗi thời gian, có thể mong muốn thực hiện một số nội suy hoặc điền giá trị khi reindexing. Tùy chọn method cho phép thực hiện điều này, sử dụng một phương thức như ffill, sẽ điền giá trị từ phía trước:

```python
In [95]: obj3 = pd.Series(['blue', 'purple', 'yellow'], index=[0, 2, 4])
In [96]: obj3
Out[96]:
0      blue
2    purple
4    yellow
dtype: object

In [97]: obj3.reindex(range(6), method='ffill')
Out[97]:
0      blue
1      blue
2    purple
3    purple
4    yellow
5    yellow
dtype: object
```
Với DataFrame, reindex có thể thay đổi chỉ số hàng (row index), cột (columns), hoặc cả hai. Khi chỉ truyền một chuỗi, nó sẽ reindex các hàng trong kết quả:

```python
In [98]: frame = pd.DataFrame(np.arange(9).reshape((3, 3)),
                              index=['a', 'c', 'd'],
                              columns=['Ohio', 'Texas', 'California'])
In [99]: frame
Out[99]:
   Ohio  Texas  California
a     0      1           2
c     3      4           5
d     6      7           8

In [100]: frame2 = frame.reindex(['a', 'b', 'c', 'd'])
In [101]: frame2
Out[101]:
   Ohio  Texas  California
a   0.0    1.0         2.0
b   NaN    NaN         NaN
c   3.0    4.0         5.0
d   6.0    7.0         8.0
```
Các cột có thể được reindex với từ khóa columns:

```python
In [102]: states = ['Texas', 'Utah', 'California']
In [103]: frame.reindex(columns=states)
Out[103]:
   Texas  Utah  California
a      1   NaN           2
c      4   NaN           5
d      7   NaN           8
```

Như sẽ được khám phá chi tiết hơn, có thể reindex ngắn gọn hơn bằng cách lập chỉ mục nhãn với loc, và nhiều người dùng thích sử dụng cách này:

```python
In [104]: frame.loc[['a', 'b', 'c', 'd'], states]
Out[104]:
   Texas  Utah  California
a    1.0   NaN         2.0
b    NaN   NaN         NaN
c    4.0   NaN         5.0
d    7.0   NaN         8.0
```

### Bảng 3-3. Các Tham Số của Hàm reindex

| Argument    | Description                                                                                                                   |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| index       | Chuỗi mới để sử dụng làm chỉ số. Có thể là một đối tượng `Index` hoặc bất kỳ cấu trúc dữ liệu dạng chuỗi nào của Python. Một `Index` sẽ được sử dụng chính xác như vậy mà không cần sao chép. |
| method      | Phương thức nội suy (điền giá trị); 'ffill' điền từ phía trước, trong khi 'bfill' điền từ phía sau.                             |
| fill_value  | Giá trị thay thế để sử dụng khi giới thiệu dữ liệu bị thiếu bằng cách `reindex`.                                               |
| limit       | Khi điền tiến hoặc điền lùi, khoảng cách tối đa (tính theo số phần tử) để điền.                                                |
| tolerance   | Khi điền tiến hoặc điền lùi, khoảng cách tối đa (tính theo khoảng cách số tuyệt đối) để điền cho các kết quả không chính xác.   |
| level       | Khớp chỉ số đơn giản trên mức của `MultiIndex`; nếu không, chọn tập hợp con của.                                               |
| copy        | Nếu `True`, luôn sao chép dữ liệu cơ bản ngay cả khi chỉ số mới tương đương với chỉ số cũ; nếu `False`, không sao chép dữ liệu khi các chỉ số tương đương. |

## Dropping Entries from an Axis

Việc loại bỏ một hoặc nhiều mục từ một trục trở nên dễ dàng nếu đã có một mảng chỉ số hoặc danh sách mà không có những mục đó. Vì điều này có thể yêu cầu một chút xử lý và logic tập hợp, phương thức `drop` sẽ trả về một đối tượng mới với giá trị hoặc các giá trị được chỉ định bị xóa khỏi một trục:

```python
In [105]: obj = pd.Series(np.arange(5.), index=['a', 'b', 'c', 'd', 'e'])
In [106]: obj
Out[106]:
a    0.0
b    1.0
c    2.0
d    3.0
e    4.0
dtype: float64

In [107]: new_obj = obj.drop('c')
In [108]: new_obj
Out[108]:
a    0.0
b    1.0
d    3.0
e    4.0
dtype: float64

In [109]: obj.drop(['d', 'c'])
Out[109]:
a    0.0
b    1.0
e    4.0
dtype: float64
```
Với DataFrame, các giá trị chỉ số có thể được xóa từ bất kỳ trục nào. Để minh họa điều này, trước tiên chúng ta tạo một DataFrame 

Ví dụ:

```python
In [110]: data = pd.DataFrame(np.arange(16).reshape((4, 4)),
                              index=['Ohio', 'Colorado', 'Utah', 'New York'],
                              columns=['one', 'two', 'three', 'four'])
In [111]: data
Out[111]:
            one  two  three  four
Ohio          0    1      2     3
Colorado      4    5      6     7
Utah          8    9     10    11
New York     12   13     14    15
```
Gọi drop với một chuỗi các nhãn sẽ loại bỏ các giá trị từ nhãn hàng (trục 0):

```python
In [112]: data.drop(['Colorado', 'Ohio'])
Out[112]:
            one  two  three  four
Utah          8    9     10    11
New York     12   13     14    15
```
Có thể loại bỏ các giá trị từ các cột bằng cách truyền axis=1 hoặc axis='columns':

```python
In [113]: data.drop('two', axis=1)
Out[113]:
            one  three  four
Ohio          0      2     3
Colorado      4      6     7
Utah          8     10    11
New York     12     14    15

In [114]: data.drop(['two', 'four'], axis='columns')
Out[114]:
            one  three
Ohio          0      2
Colorado      4      6
Utah          8     10
New York     12     14
```
Nhiều hàm, như drop, thay đổi kích thước hoặc hình dạng của một Series hoặc DataFrame, có thể thao tác một đối tượng tại chỗ mà không trả về một đối tượng mới:

```python
In [115]: obj.drop('c', inplace=True)
In [116]: obj
Out[116]:
a    0.0
b    1.0
d    3.0
e    4.0
dtype: float64
```
## Indexing, Selection, and Filtering

Việc lập chỉ mục (`indexing`) của `Series` (obj[...]) hoạt động tương tự như lập chỉ mục mảng NumPy, ngoại trừ việc bạn có thể sử dụng các giá trị chỉ mục của `Series` thay vì chỉ sử dụng các số nguyên. Dưới đây là một số ví dụ:

```python
In [117]: obj = pd.Series(np.arange(4.), index=['a', 'b', 'c', 'd'])
In [118]: obj
Out[118]:
a    0.0
b    1.0
c    2.0
d    3.0
dtype: float64

In [119]: obj['b']
Out[119]: 1.0

In [120]: obj[1]
Out[120]: 1.0

In [121]: obj[2:4]
Out[121]:
c    2.0
d    3.0
dtype: float64

In [122]: obj[['b', 'a', 'd']]
Out[122]:
b    1.0
a    0.0
d    3.0
dtype: float64

In [123]: obj[[1, 3]]
Out[123]:
b    1.0
d    3.0
dtype: float64

In [124]: obj[obj < 2]
Out[124]:
a    0.0
b    1.0
dtype: float64
```
Cắt lát (slicing) với nhãn hoạt động khác với cắt lát thông thường trong Python ở chỗ điểm kết thúc được bao gồm:

```python
In [125]: obj['b':'c']
Out[125]:
b    1.0
c    2.0
dtype: float64
```
Thiết lập bằng cách sử dụng các phương thức này sẽ thay đổi phần tương ứng của Series:

```python
In [126]: obj['b':'c'] = 5
In [127]: obj
Out[127]:
a    0.0
b    5.0
c    5.0
d    3.0
dtype: float64
```
Lập chỉ mục vào một DataFrame là để truy xuất một hoặc nhiều cột bằng một giá trị đơn hoặc một chuỗi:

```python
In [128]: data = pd.DataFrame(np.arange(16).reshape((4, 4)),
                              index=['Ohio', 'Colorado', 'Utah', 'New York'],
                              columns=['one', 'two', 'three', 'four'])
In [129]: data
Out[129]:
            one  two  three  four
Ohio          0    1      2     3
Colorado      4    5      6     7
Utah          8    9     10    11
New York     12   13     14    15

In [130]: data['two']
Out[130]:
Ohio         1
Colorado     5
Utah         9
New York    13
Name: two, dtype: int64

In [131]: data[['three', 'one']]
Out[131]:
            three  one
Ohio            2    0
Colorado        6    4
Utah           10    8
New York       14   12
```
Lập chỉ mục như thế này có một vài trường hợp đặc biệt. Đầu tiên, cắt lát hoặc chọn dữ liệu với một mảng boolean:

```python
In [132]: data[:2]
Out[132]:
            one  two  three  four
Ohio          0    1      2     3
Colorado      4    5      6     7

In [133]: data[data['three'] > 5]
Out[133]:
            one  two  three  four
Colorado      4    5      6     7
Utah          8    9     10    11
New York     12   13     14    15
```
Cú pháp chọn hàng data[:2] được cung cấp để thuận tiện. Việc truyền một phần tử đơn hoặc một danh sách cho toán tử [] sẽ chọn các cột.

Một trường hợp sử dụng khác là lập chỉ mục với một DataFrame boolean, chẳng hạn như một cái được tạo ra bởi một so sánh số học:

```python
In [134]: data < 5
Out[134]:
            one    two  three   four
Ohio       True   True   True   True
Colorado   True  False  False  False
Utah      False  False  False  False
New York  False  False  False  False

In [135]: data[data < 5] = 0
In [136]: data
Out[136]:
            one  two  three  four
Ohio          0    0      0     0
Colorado      0    5      6     7
Utah          8    9     10    11
New York     12   13     14    15
```
Điều này làm cho DataFrame cú pháp hóa giống như một mảng NumPy hai chiều trong trường hợp cụ thể này.

## Selection with loc and iloc

Đối với việc lập chỉ mục theo nhãn trong DataFrame, pandas cung cấp các toán tử lập chỉ mục đặc biệt `loc` và `iloc`. Chúng cho phép bạn chọn một tập hợp các hàng và cột từ một DataFrame với cú pháp tương tự NumPy, sử dụng nhãn trục (`loc`) hoặc số nguyên (`iloc`).

### Ví dụ sơ bộ, chọn một hàng và nhiều cột bằng nhãn:

```python
In [137]: data.loc['Colorado', ['two', 'three']]
Out[137]:
two      5
three    6
Name: Colorado, dtype: int64
```
Thực hiện một số lựa chọn tương tự bằng các số nguyên sử dụng iloc:
```python
In [138]: data.iloc[2, [3, 0, 1]]
Out[138]:
four    11
one      8
two      9
Name: Utah, dtype: int64

In [139]: data.iloc[2]
Out[139]:
one       8
two       9
three    10
four     11
Name: Utah, dtype: int64
```
Cả hai hàm lập chỉ mục làm việc với các lát cắt (slices) ngoài các nhãn đơn lẻ hoặc danh sách nhãn:

```python
In [140]: data.iloc[[1, 2], [3, 0, 1]]
Out[140]:
            four  one  two
Colorado      7    4    5
Utah         11    8    9

In [141]: data.loc[:'Utah', 'two']
Out[141]:
Ohio         0
Colorado     5
Utah         9
Name: two, dtype: int64

In [142]: data.iloc[:, :3][data.three > 5]
Out[142]:
            one  two  three
Colorado      4    5      6
Utah          8    9     10
New York     12   13     14
```

Như vậy, có rất nhiều cách để chọn và sắp xếp lại dữ liệu chứa trong một đối tượng pandas.

Bảng 5-4 cung cấp một tóm tắt ngắn về nhiều phương pháp này. Như sẽ thấy sau, có nhiều tùy chọn bổ sung để làm việc với các chỉ số phân cấp (hierarchical indexes).

Khi thiết kế ban đầu pandas, việc phải gõ frame[:, col] để chọn một cột được cho là quá dài dòng và dễ sai sót, vì việc chọn cột là một trong những thao tác phổ biến nhất. Quyết định đã được đưa ra để chuyển tất cả hành vi lập chỉ mục phức tạp (cả nhãn và số nguyên) vào toán tử ix. Trong thực tế, điều này dẫn đến nhiều trường hợp ngoại lệ trong dữ liệu có nhãn trục là số nguyên, vì vậy đội ngũ pandas đã quyết định tạo ra các toán tử loc và iloc để xử lý lần lượt lập chỉ mục dựa trên nhãn và số nguyên. Toán tử lập chỉ mục ix vẫn tồn tại, nhưng đã bị loại bỏ và không được khuyến nghị sử dụng.

### Bảng 3-4. Các Tùy Chọn Lập Chỉ Mục với DataFrame

| Type                        | Notes                                                                                                                   |
|-----------------------------|-------------------------------------------------------------------------------------------------------------------------|
| df[val]                     | Chọn một cột đơn hoặc một chuỗi các cột từ DataFrame; các trường hợp đặc biệt tiện lợi: mảng boolean (lọc hàng), lát cắt (cắt lát hàng), hoặc DataFrame boolean (thiết lập giá trị dựa trên một tiêu chí nào đó) |
| df.loc[val]                 | Chọn một hàng đơn hoặc một tập hợp các hàng từ DataFrame theo nhãn                                                       |
| df.loc[:, val]              | Chọn một cột đơn hoặc một tập hợp các cột theo nhãn                                                                      |
| df.loc[val1, val2]          | Chọn cả hàng và cột theo nhãn                                                                                            |
| df.iloc[where]              | Chọn một hàng đơn hoặc một tập hợp các hàng từ DataFrame theo vị trí số nguyên                                           |
| df.iloc[:, where]           | Chọn một cột đơn hoặc một tập hợp các cột theo vị trí số nguyên                                                          |
| df.iloc[where_i, where_j]   | Chọn cả hàng và cột theo vị trí số nguyên                                                                                |
| df.at[label_i, label_j]     | Chọn một giá trị vô hướng đơn bằng nhãn hàng và nhãn cột                                                                 |
| df.iat[i, j]                | Chọn một giá trị vô hướng đơn bằng vị trí hàng và vị trí cột (số nguyên)                                                 |
| reindex method              | Chọn hàng hoặc cột theo nhãn                                                                                             |
| get_value, set_value methods| Chọn một giá trị đơn theo nhãn hàng và nhãn cột                                                                          |

## Integer Indexes

Làm việc với các đối tượng pandas có chỉ mục là số nguyên là điều thường gây khó khăn cho người mới bắt đầu do một số khác biệt với ngữ nghĩa lập chỉ mục trên các cấu trúc dữ liệu Python tích hợp như danh sách và tuple. Ví dụ, bạn có thể không mong đợi đoạn mã sau tạo ra lỗi:

```python
ser = pd.Series(np.arange(3.))
ser
ser[-1]
```
Trong trường hợp này, pandas có thể "quay lại" lập chỉ mục bằng số nguyên, nhưng rất khó để làm điều này mà không gây ra các lỗi tinh tế. Ở đây chúng ta có một chỉ mục chứa 0, 1, 2, nhưng suy luận điều mà người dùng muốn (lập chỉ mục dựa trên nhãn hoặc dựa trên vị trí) là khó khăn:

```python
In [144]: ser
Out[144]:
0    0.0
1    1.0
2    2.0
dtype: float64
```
Mặt khác, với một chỉ mục không phải là số nguyên, không có khả năng gây nhầm lẫn:

```python
In [145]: ser2 = pd.Series(np.arange(3.), index=['a', 'b', 'c'])
In [146]: ser2[-1]
Out[146]: 2.0
```
Để giữ cho mọi thứ nhất quán, nếu có một chỉ mục trục chứa các số nguyên, việc lựa chọn dữ liệu sẽ luôn dựa trên nhãn. Để xử lý chính xác hơn, sử dụng loc (cho nhãn) hoặc iloc (cho số nguyên):

```python
In [147]: ser[:1]
Out[147]:
0    0.0
dtype: float64

In [148]: ser.loc[:1]
Out[148]:
0    0.0
1    1.0
dtype: float64

In [149]: ser.iloc[:1]
Out[149]:
0    0.0
dtype: float64
```
## Arithmetic and Data Alignment

Một tính năng quan trọng của pandas cho một số ứng dụng là cách thức thực hiện các phép toán giữa các đối tượng có chỉ số khác nhau. Khi cộng các đối tượng lại với nhau, nếu bất kỳ cặp chỉ số nào không giống nhau, chỉ số tương ứng trong kết quả sẽ là hợp của các cặp chỉ số. Đối với người dùng có kinh nghiệm về cơ sở dữ liệu, điều này tương tự như một phép nối ngoài tự động trên các nhãn chỉ số. Hãy xem một ví dụ:

```python
In [150]: s1 = pd.Series([7.3, -2.5, 3.4, 1.5], index=['a', 'c', 'd', 'e'])
In [151]: s2 = pd.Series([-2.1, 3.6, -1.5, 4, 3.1], index=['a', 'c', 'e', 'f', 'g'])
In [152]: s1
Out[152]:
a    7.3
c   -2.5
d    3.4
e    1.5
dtype: float64

In [153]: s2
Out[153]:
a   -2.1
c    3.6
e   -1.5
f    4.0
g    3.1
dtype: float64
Cộng hai đối tượng này lại với nhau cho kết quả:

python
In [154]: s1 + s2
Out[154]:
a    5.2
c    1.1
d    NaN
e    0.0
f    NaN
g    NaN
dtype: float64
```
Sự căn chỉnh dữ liệu bên trong tạo ra các giá trị bị thiếu tại các vị trí nhãn không trùng lặp. Các giá trị bị thiếu sau đó sẽ lan truyền trong các phép toán số học tiếp theo.

Trong trường hợp của DataFrame, việc căn chỉnh được thực hiện trên cả các hàng và các cột:

```python
In [155]: df1 = pd.DataFrame(np.arange(9.).reshape((3, 3)), columns=list('bcd'), index=['Ohio', 'Texas', 'Colorado'])
In [156]: df2 = pd.DataFrame(np.arange(12.).reshape((4, 3)), columns=list('bde'), index=['Utah', 'Ohio', 'Texas', 'Oregon'])
In [157]: df1
Out[157]:
            b    c    d
Ohio       0.0  1.0  2.0
Texas      3.0  4.0  5.0
Colorado   6.0  7.0  8.0

In [158]: df2
Out[158]:
            b    d    e
Utah       0.0  1.0  2.0
Ohio       3.0  4.0  5.0
Texas      6.0  7.0  8.0
Oregon     9.0 10.0 11.0
```
Cộng hai đối tượng này lại với nhau trả về một DataFrame với chỉ số hàng và cột là hợp của các chỉ số trong mỗi DataFrame:

```python
In [159]: df1 + df2
Out[159]:
            b    c     d     e
Colorado   NaN  NaN  NaN   NaN
Ohio       3.0  NaN  6.0   NaN
Oregon     NaN  NaN  NaN   NaN
Texas      9.0  NaN 12.0   NaN
Utah       NaN  NaN  NaN   NaN
```
Vì cột 'c' và 'e' không được tìm thấy trong cả hai đối tượng DataFrame, chúng xuất hiện dưới dạng tất cả các giá trị bị thiếu trong kết quả. Điều tương tự cũng xảy ra với các hàng có nhãn không trùng lặp trong cả hai đối tượng.

Nếu cộng các đối tượng DataFrame mà không có cột hoặc hàng nào có nhãn trùng, kết quả sẽ chứa toàn bộ giá trị null:

```python
In [160]: df1 = pd.DataFrame({'A': [1, 2]})
In [161]: df2 = pd.DataFrame({'B': [3, 4]})
In [162]: df1
Out[162]:
   A
0  1
1  2

In [163]: df2
Out[163]:
   B
0  3
1  4

In [164]: df1 - df2
Out[164]:
   A   B
0 NaN NaN
1 NaN NaN
```
### Arithmetic Methods with Fill Values

Trong các phép toán số học giữa các đối tượng có chỉ số khác nhau, có thể cần điền một giá trị đặc biệt, như 0, khi một nhãn trục xuất hiện trong một đối tượng nhưng không xuất hiện trong đối tượng kia:

```python
In [165]: df1 = pd.DataFrame(np.arange(12.).reshape((3, 4)), columns=list('abcd'))
In [166]: df2 = pd.DataFrame(np.arange(20.).reshape((4, 5)), columns=list('abcde'))
In [167]: df2.loc[1, 'b'] = np.nan
In [168]: df1
Out[168]:
     a    b     c     d
0  0.0  1.0  2.0  3.0
1  4.0  5.0  6.0  7.0
2  8.0  9.0  10.0  11.0

In [169]: df2
Out[169]:
     a     b     c     d     e
0  0.0  1.0  2.0  3.0  4.0
1  5.0  NaN  7.0  8.0  9.0
2  10.0  11.0  12.0  13.0  14.0
3  15.0  16.0  17.0  18.0  19.0
```
Cộng hai đối tượng này lại với nhau dẫn đến các giá trị NA ở các vị trí không trùng lặp:

```python
In [170]: df1 + df2
Out[170]:
     a     b     c     d     e
0  0.0  2.0  4.0  6.0  NaN
1  9.0  NaN  13.0  15.0  NaN
2  18.0  20.0  22.0  24.0  NaN
3  NaN  NaN  NaN  NaN  NaN
```
Sử dụng phương thức add trên df1, truyền vào df2 và một tham số fill_value:

```python
In [171]: df1.add(df2, fill_value=0)
Out[171]:
     a     b     c     d     e
0  0.0  2.0  4.0  6.0  4.0
1  9.0  5.0  13.0  15.0  9.0
2  18.0  20.0  22.0  24.0  14.0
3  15.0  16.0  17.0  18.0  19.0
```
Xem Bảng 5-5 để biết danh sách các phương thức số học cho Series và DataFrame. Mỗi phương thức đều có một đối tác bắt đầu bằng chữ "r", với các tham số được đảo ngược. Do đó, hai câu lệnh sau là tương đương:

```python
In [172]: 1 / df1
Out[172]:
     a       b          c          d
0  inf  1.000000  0.500000  0.333333
1  0.250000  0.200000  0.166667  0.142857
2  0.125000  0.111111  0.100000  0.090909

In [173]: df1.rdiv(1)
Out[173]:
     a       b          c          d
0  inf  1.000000  0.500000  0.333333
1  0.250000  0.200000  0.166667  0.142857
2  0.125000  0.111111  0.100000  0.090909
```
Liên quan đến điều này, khi reindex một Series hoặc DataFrame, bạn cũng có thể chỉ định một giá trị điền khác:

```python
In [174]: df1.reindex(columns=df2.columns, fill_value=0)
Out[174]:
     a    b    c    d    e
0  0.0  1.0  2.0  3.0  0
1  4.0  5.0  6.0  7.0  0
2  8.0  9.0  10.0 11.0  0
```
### Bảng 3-5. Các Phương Thức Số Học Linh Hoạt

| Method       | Description                                      |
|--------------|--------------------------------------------------|
| add, radd    | Phương thức cho phép cộng (+)                    |
| sub, rsub    | Phương thức cho phép trừ (-)                     |
| div, rdiv    | Phương thức cho phép chia (/)                    |
| floordiv, rfloordiv | Phương thức cho phép chia lấy phần nguyên (//) |
| mul, rmul    | Phương thức cho phép nhân (*)                    |
| pow, rpow    | Phương thức cho phép lũy thừa (**)               |

### Operations between DataFrame and Series

Cũng giống như các mảng NumPy có các kích thước khác nhau, các phép toán số học giữa `DataFrame` và `Series` cũng được định nghĩa. Trước tiên, hãy xem xét sự khác biệt giữa một mảng hai chiều và một trong các hàng của nó:

```python
In [175]: arr = np.arange(12.).reshape((3, 4))
In [176]: arr
Out[176]:
array([[ 0., 1., 2., 3.],
       [ 4., 5., 6., 7.],
       [ 8., 9., 10., 11.]])

In [177]: arr[0]
Out[177]: array([ 0., 1., 2., 3.])

In [178]: arr - arr[0]
Out[178]:
array([[ 0., 0., 0., 0.],
       [ 4., 4., 4., 4.],
       [ 8., 8., 8., 8.]])
```
Khi trừ arr[0] khỏi arr, phép trừ được thực hiện một lần cho mỗi hàng. Điều này được gọi là phát sóng (broadcasting) và được giải thích chi tiết hơn liên quan đến các mảng NumPy tổng quát trong Phụ lục A. Các phép toán giữa DataFrame và Series cũng tương tự:

```python
In [179]: frame = pd.DataFrame(np.arange(12.).reshape((4, 3)), columns=list('bde'), index=['Utah', 'Ohio', 'Texas', 'Oregon'])
In [180]: series = frame.iloc[0]
In [181]: frame
Out[181]:
         b    d     e
Utah    0.0  1.0  2.0
Ohio    3.0  4.0  5.0
Texas   6.0  7.0  8.0
Oregon  9.0  10.0 11.0

In [182]: series
Out[182]:
b    0.0
d    1.0
e    2.0
Name: Utah, dtype: float64
```
Theo mặc định, các phép toán số học giữa DataFrame và Series sẽ ghép chỉ số của Series vào các cột của DataFrame, phát sóng xuống các hàng:

```python
In [183]: frame - series
Out[183]:
         b    d    e
Utah    0.0  0.0  0.0
Ohio    3.0  3.0  3.0
Texas   6.0  6.0  6.0
Oregon  9.0  9.0  9.0
```
Nếu một giá trị chỉ số không được tìm thấy trong cột của DataFrame hoặc chỉ số của Series, các đối tượng sẽ được tái lập chỉ số để tạo thành hợp:

```python
In [184]: series2 = pd.Series(range(3), index=['b', 'e', 'f'])
In [185]: frame + series2
Out[185]:
         b    d    e    f
Utah    0.0  NaN  3.0  NaN
Ohio    3.0  NaN  6.0  NaN
Texas   6.0  NaN  9.0  NaN
Oregon  9.0  NaN  12.0 NaN
```
### Function Application and Mapping

Các hàm NumPy ufuncs (phương thức mảng theo phần tử) cũng hoạt động với các đối tượng pandas:

```python
In [190]: frame = pd.DataFrame(np.random.randn(4, 3), columns=list('bde'), index=['Utah', 'Ohio', 'Texas', 'Oregon'])
In [191]: frame
Out[191]:
               b         d         e
Utah   -0.204708  0.478943 -0.519439
Ohio   -0.555730  1.965781  1.393406
Texas   0.092908  0.281746  0.769023
Oregon  1.246435  1.007189 -1.296221

In [192]: np.abs(frame)
Out[192]:
               b         d         e
Utah    0.204708  0.478943  0.519439
Ohio    0.555730  1.965781  1.393406
Texas   0.092908  0.281746  0.769023
Oregon  1.246435  1.007189  1.296221
```
Một hoạt động thường xuyên khác là áp dụng một hàm trên các mảng một chiều cho mỗi cột hoặc hàng. Phương thức apply của DataFrame thực hiện chính xác điều này:

```python
In [193]: f = lambda x: x.max() - x.min()
In [194]: frame.apply(f)
Out[194]:
b    1.802165
d    1.684034
e    2.689627
dtype: float64
```
Ở đây hàm f, tính toán sự chênh lệch giữa giá trị lớn nhất và nhỏ nhất của một Series, được gọi một lần trên mỗi cột trong frame. Kết quả là một Series có các cột của frame làm chỉ số.

Nếu bạn truyền axis='columns' cho apply, hàm sẽ được gọi một lần cho mỗi hàng:

```python
In [195]: frame.apply(f, axis='columns')
Out[195]:
Utah       0.998382
Ohio       2.521511
Texas      0.676115
Oregon     2.542656
dtype: float64
```
Nhiều thống kê mảng phổ biến nhất (như sum và mean) là các phương thức của DataFrame, vì vậy không cần thiết phải sử dụng apply.

Hàm truyền vào apply không nhất thiết phải trả về một giá trị vô hướng; nó cũng có thể trả về một Series với nhiều giá trị:

```python
In [196]: def f(x):
     ...:     return pd.Series([x.min(), x.max()], index=['min', 'max'])
In [197]: frame.apply(f)
Out[197]:
              b         d         e
min   -0.555730  0.281746 -1.296221
max    1.246435  1.965781  1.393406
```
Các hàm Python theo phần tử cũng có thể được sử dụng. Giả sử bạn muốn tính một chuỗi định dạng từ mỗi giá trị dấu phẩy động trong frame. Bạn có thể làm điều này với applymap:

```python
In [198]: format = lambda x: '%.2f' % x
In [199]: frame.applymap(format)
Out[199]:
             b     d     e
Utah     -0.20  0.48  -0.52
Ohio     -0.56  1.97   1.39
Texas     0.09  0.28   0.77
Oregon    1.25  1.01  -1.30
```
Lý do cho tên gọi applymap là vì Series có một phương thức map để áp dụng một hàm theo phần tử:

```python
In [200]: frame['e'].map(format)
Out[200]:
Utah      -0.52
Ohio       1.39
Texas      0.77
Oregon    -1.30
Name: e, dtype: object
```
### Sorting and Ranking

Sắp xếp dữ liệu theo một tiêu chí nào đó là một thao tác tích hợp quan trọng khác. Để sắp xếp theo thứ tự từ điển theo chỉ mục hàng hoặc cột, sử dụng phương thức `sort_index`, phương thức này trả về một đối tượng mới đã được sắp xếp:

```python
In [201]: obj = pd.Series(range(4), index=['d', 'a', 'b', 'c'])
In [202]: obj.sort_index()
Out[202]:
a    1
b    2
c    3
d    0
dtype: int64
```
Với DataFrame, có thể sắp xếp theo chỉ mục trên bất kỳ trục nào:

```python
In [203]: frame = pd.DataFrame(np.arange(8).reshape((2, 4)), index=['three', 'one'], columns=['d', 'a', 'b', 'c'])
In [204]: frame.sort_index()
Out[204]:
        d  a  b  c
one     4  5  6  7
three   0  1  2  3

In [205]: frame.sort_index(axis=1)
Out[205]:
        a  b  c  d
three   1  2  3  0
one     5  6  7  4
```
Dữ liệu được sắp xếp theo thứ tự tăng dần theo mặc định, nhưng cũng có thể sắp xếp theo thứ tự giảm dần:

```python
In [206]: frame.sort_index(axis=1, ascending=False)
Out[206]:
        d  c  b  a
three   0  3  2  1
one     4  7  6  5
```
Để sắp xếp một Series theo các giá trị của nó, sử dụng phương thức sort_values:

```python
In [207]: obj = pd.Series([4, 7, -3, 2])
In [208]: obj.sort_values()
Out[208]:
2   -3
3    2
0    4
1    7
dtype: int64
```
Bất kỳ giá trị thiếu nào đều được sắp xếp đến cuối của Series theo mặc định:

```python
In [209]: obj = pd.Series([4, np.nan, 7, np.nan, -3, 2])
In [210]: obj.sort_values()
Out[210]:
4   -3.0
5    2.0
0    4.0
2    7.0
1    NaN
3    NaN
dtype: float64
```
Khi sắp xếp một DataFrame, có thể sử dụng dữ liệu trong một hoặc nhiều cột làm khóa sắp xếp. Để làm điều này, truyền tên một hoặc nhiều cột vào tùy chọn by của sort_values:

```python
In [211]: frame = pd.DataFrame({'b': [4, 7, -3, 2], 'a': [0, 1, 0, 1]})
In [212]: frame
Out[212]:
    a   b
0   0   4
1   1   7
2   0  -3
3   1   2

In [213]: frame.sort_values(by='b')
Out[213]:
    a   b
2   0  -3
3   1   2
0   0   4
1   1   7
```
Để sắp xếp theo nhiều cột, truyền một danh sách các tên:

```python
In [214]: frame.sort_values(by=['a', 'b'])
Out[214]:
    a   b
2   0  -3
0   0   4
3   1   2
1   1   7
```
Việc xếp hạng (ranking) gán các xếp hạng từ một đến số lượng điểm dữ liệu hợp lệ trong một mảng. Phương thức rank cho Series và DataFrame là nơi cần xem; theo mặc định, rank phá vỡ các ràng buộc bằng cách gán cho mỗi nhóm xếp hạng trung bình:

```python
In [215]: obj = pd.Series([7, -5, 7, 4, 2, 0, 4])
In [216]: obj.rank()
Out[216]:
0    6.5
1    1.0
2    6.5
3    4.5
4    3.0
5    2.0
6    4.5
dtype: float64
```
Xếp hạng cũng có thể được gán theo thứ tự quan sát trong dữ liệu:

```python
In [217]: obj.rank(method='first')
Out[217]:
0    6.0
1    1.0
2    7.0
3    4.0
4    3.0
5    2.0
6    5.0
dtype: float64
```
Ở đây, thay vì sử dụng xếp hạng trung bình 6.5 cho các mục 0 và 2, chúng được đặt thành 6 và 7 vì nhãn 0 đứng trước nhãn 2 trong dữ liệu.

Có thể xếp hạng theo thứ tự giảm dần:

```python
In [218]: obj.rank(ascending=False, method='max')
Out[218]:
0    2.0
1    7.0
2    2.0
3    4.0
4    5.0
5    6.0
6    4.0
dtype: float64
```
Xem Bảng 5-6 để biết danh sách các phương pháp phá vỡ ràng buộc có sẵn.

DataFrame có thể tính toán các xếp hạng qua các hàng hoặc các cột:

```python
In [219]: frame = pd.DataFrame({'b': [4.3, 7, -3, 2], 'a': [0, 1, 0, 1], 'c': [-2, 5, 8, -2.5]})
In [220]: frame
Out[220]:
    a    b    c
0   0  4.3  -2.0
1   1  7.0   5.0
2   0 -3.0   8.0
3   1  2.0  -2.5

In [221]: frame.rank(axis='columns')
Out[221]:
      a    b    c
0   2.0  3.0  1.0
1   1.0  3.0  2.0
2   2.0  1.0  3.0
3   2.0  3.0  1.0
```
### Bảng 3-6. Các Phương Pháp Phá Vỡ Ràng Buộc Với Rank

| Method   | Description                                                                                                           |
|----------|-----------------------------------------------------------------------------------------------------------------------|
| 'average'| Mặc định: gán xếp hạng trung bình cho mỗi mục trong nhóm bằng nhau                                                   |
| 'min'    | Sử dụng xếp hạng tối thiểu cho toàn bộ nhóm                                                                          |
| 'max'    | Sử dụng xếp hạng tối đa cho toàn bộ nhóm                                                                             |
| 'first'  | Gán xếp hạng theo thứ tự xuất hiện của các giá trị trong dữ liệu                                                     |
| 'dense'  | Giống phương pháp 'min', nhưng xếp hạng luôn tăng thêm 1 giữa các nhóm thay vì số lượng phần tử bằng nhau trong một nhóm |


