# Biến Đổi Dữ Liệu

Cho đến nay trong chương này, chúng ta đã quan tâm đến việc sắp xếp lại dữ liệu. Lọc, làm sạch và các biến đổi khác cũng là một loại thao tác quan trọng.

## Loại Bỏ Dữ Liệu Trùng Lặp

Các hàng trùng lặp có thể được tìm thấy trong một DataFrame vì nhiều lý do khác nhau. Đây là một ví dụ:

```python
data = pd.DataFrame({'k1': ['one', 'two'] * 3 + ['two'],
                     'k2': [1, 1, 2, 3, 3, 4, 4]})
data
```
Kết quả:

```python
    k1  k2
0  one   1
1  two   1
2  one   2
3  two   3
4  one   3
5  two   4
6  two   4
```
Phương thức duplicated của DataFrame trả về một Series boolean chỉ ra liệu mỗi hàng có bị trùng lặp (đã được quan sát trong một hàng trước đó) hay không:

```python
data.duplicated()
```
Kết quả:

```python
0    False
1    False
2    False
3    False
4    False
5    False
6     True
dtype: bool
```
Tương tự, drop_duplicates trả về một DataFrame nơi mảng trùng lặp là False:

```python
data.drop_duplicates()
```
Kết quả:

```python
    k1  k2
0  one   1
1  two   1
2  one   2
3  two   3
4  one   3
5  two   4
```
Cả hai phương thức này mặc định xem xét tất cả các cột; ngoài ra, bạn có thể chỉ định bất kỳ tập hợp con nào của chúng để phát hiện trùng lặp. Giả sử có một cột giá trị bổ sung và muốn lọc trùng lặp chỉ dựa trên cột 'k1':

```python
data['v1'] = range(7)
data.drop_duplicates(['k1'])
```
Kết quả:

```python
    k1  k2  v1
0  one   1   0
1  two   1   1
```
duplicated và drop_duplicates mặc định giữ giá trị kết hợp đầu tiên được quan sát. Truyền keep='last' sẽ trả về giá trị cuối cùng:

```python
data.drop_duplicates(['k1', 'k2'], keep='last')
```
Kết quả:

```python
    k1  k2  v1
0  one   1   0
1  two   1   1
2  one   2   2
3  two   3   3
4  one   3   4
6  two   4   6
```
## Biến Đổi Dữ Liệu Sử Dụng Hàm hoặc Ánh Xạ

Đối với nhiều tập dữ liệu, bạn có thể muốn thực hiện một số biến đổi dựa trên các giá trị trong một mảng, Series hoặc cột trong DataFrame. Hãy xem xét dữ liệu giả định sau đây được thu thập về các loại thịt khác nhau:

```python
data = pd.DataFrame({'food': ['bacon', 'pulled pork', 'bacon',
                              'Pastrami', 'corned beef', 'Bacon',
                              'pastrami', 'honey ham', 'nova lox'],
                     'ounces': [4, 3, 12, 6, 7.5, 8, 3, 5, 6]})
data
```
Kết quả:

```python
          food  ounces
0        bacon     4.0
1  pulled pork     3.0
2        bacon    12.0
3     Pastrami     6.0
4  corned beef     7.5
5        Bacon     8.0
6     pastrami     3.0
7    honey ham     5.0
8     nova lox     6.0
```
Giả sử muốn thêm một cột chỉ ra loại động vật mà mỗi loại thịt đến từ. Hãy viết một ánh xạ của từng loại thịt cụ thể đến loại động vật:

```python
meat_to_animal = {
    'bacon': 'pig',
    'pulled pork': 'pig',
    'pastrami': 'cow',
    'corned beef': 'cow',
    'honey ham': 'pig',
    'nova lox': 'salmon'
}
```
Phương thức map trên một Series chấp nhận một hàm hoặc đối tượng giống dict chứa một ánh xạ, nhưng ở đây có một vấn đề nhỏ là một số loại thịt viết hoa và một số loại thì không. Vì vậy, cần chuyển đổi mỗi giá trị thành chữ thường bằng cách sử dụng phương thức str.lower của Series:

```python
lowercased = data['food'].str.lower()
lowercased
```
Kết quả:

```python
0          bacon
1    pulled pork
2          bacon
3       pastrami
4    corned beef
5          bacon
6       pastrami
7      honey ham
8       nova lox
Name: food, dtype: object
```
Thêm cột animal vào DataFrame bằng cách sử dụng ánh xạ:

```python
data['animal'] = lowercased.map(meat_to_animal)
data
```
Kết quả:

```python
          food  ounces  animal
0        bacon     4.0     pig
1  pulled pork     3.0     pig
2        bacon    12.0     pig
3     Pastrami     6.0     cow
4  corned beef     7.5     cow
5        Bacon     8.0     pig
6     pastrami     3.0     cow
7    honey ham     5.0     pig
8     nova lox     6.0  salmon
```
Cũng có thể truyền một hàm thực hiện tất cả công việc:

```python
data['food'].map(lambda x: meat_to_animal[x.lower()])
```
Kết quả:

```python
0       pig
1       pig
2       pig
3       cow
4       cow
5       pig
6       cow
7       pig
8    salmon
Name: food, dtype: object
```
Sử dụng map là một cách thuận tiện để thực hiện các biến đổi theo từng phần tử và các thao tác làm sạch dữ liệu khác.

## Thay Thế Giá Trị

Điền vào dữ liệu thiếu bằng phương thức `fillna` là một trường hợp đặc biệt của việc thay thế giá trị tổng quát hơn. Như đã thấy, `map` có thể được sử dụng để thay đổi một tập hợp con các giá trị trong một đối tượng, nhưng `replace` cung cấp một cách đơn giản và linh hoạt hơn để làm điều đó. Hãy xem xét Series sau:

```python
data = pd.Series([1., -999., 2., -999., -1000., 3.])
data
```
Kết quả:

```python
0       1.0
1    -999.0
2       2.0
3    -999.0
4   -1000.0
5       3.0
dtype: float64
```
Các giá trị -999 có thể là các giá trị sentinel cho dữ liệu thiếu. Để thay thế chúng bằng các giá trị NA mà pandas hiểu, có thể sử dụng replace, tạo ra một Series mới (trừ khi truyền inplace=True):

```python
data.replace(-999, np.nan)
```
Kết quả:

```python
0       1.0
1       NaN
2       2.0
3       NaN
4   -1000.0
5       3.0
dtype: float64
```
Nếu muốn thay thế nhiều giá trị cùng một lúc, thay vì đó truyền một danh sách và sau đó là giá trị thay thế:

```python
data.replace([-999, -1000], np.nan)
```
Kết quả:

```python
0    1.0
1    NaN
2    2.0
3    NaN
4    NaN
5    3.0
dtype: float64
```
Để sử dụng một giá trị thay thế khác cho từng giá trị, truyền một danh sách các giá trị thay thế:

```python
data.replace([-999, -1000], [np.nan, 0])
```
Kết quả:

```python
0    1.0
1    NaN
2    2.0
3    NaN
4    0.0
5    3.0
dtype: float64
```
Tham số truyền vào cũng có thể là một dict:

```python
data.replace({-999: np.nan, -1000: 0})
```
Kết quả:

```python
0    1.0
1    NaN
2    2.0
3    NaN
4    0.0
5    3.0
dtype: float64
```
Phương thức data.replace khác với data.str.replace, phương thức thực hiện thay thế chuỗi theo từng phần tử. Các phương thức chuỗi trên Series sẽ được xem xét sau trong chương.

Đổi Tên Chỉ Mục Trục
Như các giá trị trong một Series, các nhãn trục cũng có thể được biến đổi tương tự bằng một hàm hoặc ánh xạ của một số dạng để tạo ra các đối tượng mới có nhãn khác. Bạn cũng có thể chỉnh sửa các trục tại chỗ mà không cần tạo ra một cấu trúc dữ liệu mới. Đây là một ví dụ đơn giản:

```python
data = pd.DataFrame(np.arange(12).reshape((3, 4)),
                    index=['Ohio', 'Colorado', 'New York'],
                    columns=['one', 'two', 'three', 'four'])
```
Giống như một Series, các chỉ mục trục có phương thức map:

```python
transform = lambda x: x[:4].upper()
data.index.map(transform)
```
Kết quả:

```python
Index(['OHIO', 'COLO', 'NEW '], dtype='object')
```
Có thể gán cho chỉ mục, chỉnh sửa DataFrame tại chỗ:

```python
data.index = data.index.map(transform)
data
```
Kết quả:

```python
       one  two  three  four
OHIO      0    1      2     3
COLO      4    5      6     7
NEW       8    9     10    11
```
Nếu muốn tạo một phiên bản biến đổi của một tập dữ liệu mà không chỉnh sửa bản gốc, một phương thức hữu ích là rename:

```python
data.rename(index=str.title, columns=str.upper)
```
Kết quả:

```python
       ONE  TWO  THREE  FOUR
Ohio      0    1      2     3
Colo      4    5      6     7
New       8    9     10    11
```
Đáng chú ý, rename có thể được sử dụng kết hợp với một đối tượng giống dict cung cấp các giá trị mới cho một tập hợp con của các nhãn trục:

```python
data.rename(index={'OHIO': 'INDIANA'}, columns={'three': 'peekaboo'})
```
Kết quả:

```python
         one  two  peekaboo  four
INDIANA     0    1         2     3
COLO        4    5         6     7
NEW         8    9        10    11
```
rename giúp tiết kiệm công việc sao chép DataFrame thủ công và gán cho các thuộc tính chỉ mục và cột của nó. Nếu muốn chỉnh sửa tập dữ liệu tại chỗ, truyền inplace=True:

```python
data.rename(index={'OHIO': 'INDIANA'}, inplace=True)
data
```
Kết quả:

```python
         one  two  three  four
INDIANA     0    1     2     3
COLO        4    5     6     7
NEW         8    9    10    11
```
## Phân Loại và Đưa Dữ Liệu Vào Các Khoảng

Dữ liệu liên tục thường được phân loại hoặc chia thành các "khoảng" để phân tích. Giả sử bạn có dữ liệu về một nhóm người trong một nghiên cứu và muốn chia họ thành các nhóm tuổi rời rạc:

```python
ages = [20, 22, 25, 27, 21, 23, 37, 31, 61, 45, 41, 32]
```
Hãy chia chúng thành các khoảng từ 18 đến 25, 26 đến 35, 36 đến 60, và cuối cùng là 61 trở lên. Để làm điều đó, sử dụng hàm cut trong pandas:

```python
bins = [18, 25, 35, 60, 100]
cats = pd.cut(ages, bins)
cats
```
Kết quả:

```python
[(18, 25], (18, 25], (18, 25], (25, 35], (18, 25], ..., (25, 35], (60, 100], (35, 60], (35, 60], (25, 35]]
Length: 12
Categories (4, interval[int64]): [(18, 25] < (25, 35] < (35, 60] < (60, 100]]
```
Đối tượng pandas trả về là một đối tượng Categorical đặc biệt. Đầu ra mô tả các khoảng được tính toán bởi pandas.cut. Có thể xử lý nó giống như một mảng chuỗi chỉ ra tên khoảng; bên trong nó chứa một mảng categories chỉ định các tên danh mục khác biệt cùng với việc gán nhãn cho dữ liệu tuổi trong thuộc tính codes:

```python
cats.codes
cats.categories
pd.value_counts(cats)
```
Kết quả:

```python
array([0, 0, 0, 1, 0, 0, 2, 1, 3, 2, 2, 1], dtype=int8)
IntervalIndex([(18, 25], (25, 35], (35, 60], (60, 100]], closed='right', dtype='interval[int64]')
(18, 25]     5
(35, 60]     3
(25, 35]     3
(60, 100]    1
dtype: int64
```
Lưu ý rằng pd.value_counts(cats) là số lượng bin cho kết quả của pandas.cut. Theo quy ước toán học cho các khoảng, dấu ngoặc đơn có nghĩa là phía đó mở, trong khi dấu ngoặc vuông có nghĩa là đóng (bao gồm). Có thể thay đổi phía đóng bằng cách truyền right=False:

```python
pd.cut(ages, [18, 26, 36, 61, 100], right=False)
```
Kết quả:

```python
[[18, 26), [18, 26), [18, 26), [26, 36), [18, 26), ..., [26, 36), [61, 100), [36, 61), [36, 61), [26, 36)]
Length: 12
Categories (4, interval[int64]): [[18, 26) < [26, 36) < [36, 61) < [61, 100)]
```
Có thể truyền tên bin riêng bằng cách truyền một danh sách hoặc mảng cho tùy chọn labels:

```python
group_names = ['Youth', 'YoungAdult', 'MiddleAged', 'Senior']
pd.cut(ages, bins, labels=group_names)
```
Kết quả:

```python
[Youth, Youth, Youth, YoungAdult, Youth, ..., YoungAdult, Senior, MiddleAged, MiddleAged, YoungAdult]
Length: 12
Categories (4, object): [Youth < YoungAdult < MiddleAged < Senior]
```
Nếu truyền một số nguyên số bin cho cut thay vì các cạnh bin cụ thể, nó sẽ tính các bin có độ dài bằng nhau dựa trên các giá trị tối thiểu và tối đa trong dữ liệu. Xem xét trường hợp dữ liệu phân phối đều được chia thành bốn phần:

```python
data = np.random.rand(20)
pd.cut(data, 4, precision=2)
```
Kết quả:

```python
[(0.34, 0.55], (0.34, 0.55], (0.76, 0.97], (0.76, 0.97], (0.34, 0.55], ..., (0.34, 0.55], (0.34, 0.55], (0.55, 0.76], (0.34, 0.55], (0.12, 0.34]]
Length: 20
Categories (4, interval[float64]): [(0.12, 0.34] < (0.34, 0.55] < (0.55, 0.76] < (0.76, 0.97]]
```
Tùy chọn precision=2 giới hạn độ chính xác của số thập phân thành hai chữ số.

Một hàm liên quan chặt chẽ, qcut, đưa dữ liệu vào các bin dựa trên các phân vị mẫu. Tùy thuộc vào phân phối của dữ liệu, sử dụng cut thường sẽ không dẫn đến mỗi bin có cùng số điểm dữ liệu. Vì qcut sử dụng các phân vị mẫu, theo định nghĩa sẽ thu được các bin có kích thước xấp xỉ bằng nhau:

```python
data = np.random.randn(1000) # Phân phối chuẩn
cats = pd.qcut(data, 4) # Chia thành bốn phần
cats
pd.value_counts(cats)
```
Kết quả:

```python
[(-0.0265, 0.62], (0.62, 3.928], (-0.68, -0.0265], (0.62, 3.928], (-0.0265, 0.62], ..., (-0.68, -0.0265], (-0.68, -0.0265], (-2.95, -0.68], (0.62, 3.928], (-0.68, -0.0265]]
Length: 1000
Categories (4, interval[float64]): [(-2.95, -0.68] < (-0.68, -0.0265] < (-0.0265, 0.62] < (0.62, 3.928]]

(0.62, 3.928]        250
(-0.0265, 0.62]      250
(-0.68, -0.0265]     250
(-2.95, -0.68]       250
dtype: int64
```
Tương tự như cut, có thể truyền các phân vị riêng (các số từ 0 đến 1, bao gồm):

```python
pd.qcut(data, [0, 0.1, 0.5, 0.9, 1.])
```
Kết quả:

```python
[(-0.0265, 1.286], (-0.0265, 1.286], (-1.187, -0.0265], (-0.0265, 1.286], (-0.0265, 1.286], ..., (-1.187, -0.0265], (-1.187, -0.0265], (-2.95, -1.187], (-0.0265, 1.286], (-1.187, -0.0265]]
Length: 1000
Categories (4, interval[float64]): [(-2.95, -1.187] < (-1.187, -0.0265] < (-0.0265, 1.286] < (1.286, 3.928]]
```
Sẽ quay lại với cut và qcut sau trong chương khi thảo luận về tổng hợp và các thao tác nhóm, vì các hàm phân loại này đặc biệt hữu ích cho phân tích phân vị và nhóm.

## Phát Hiện và Lọc Ngoại Lệ

Lọc hoặc biến đổi ngoại lệ chủ yếu là việc áp dụng các thao tác trên mảng. Hãy xem xét một DataFrame với một số dữ liệu phân phối chuẩn:

```python
data = pd.DataFrame(np.random.randn(1000, 4))
data.describe()
```
Kết quả:

```python
              0            1            2            3
count  1000.000000  1000.000000  1000.000000  1000.000000
mean      0.049091     0.026112    -0.002544    -0.051827
std       0.996947     1.007458     0.995232     0.998311
min      -3.645860    -3.184377    -3.745356    -3.428254
25%      -0.599807    -0.612162    -0.687373    -0.747478
50%       0.047101    -0.013609    -0.022158    -0.088274
75%       0.756646     0.695298     0.699046     0.623331
max       2.653656     3.525865     2.735527     3.366626
```
Giả sử muốn tìm các giá trị trong một trong các cột vượt quá 3 theo giá trị tuyệt đối:

```python
col = data[2]
col[np.abs(col) > 3]
```
Kết quả:

```python
41    -3.399312
136   -3.745356
Name: 2, dtype: float64
```
Để chọn tất cả các hàng có giá trị vượt quá 3 hoặc –3, có thể sử dụng phương thức any trên một DataFrame boolean:

```python
data[(np.abs(data) > 3).any(1)]
```
Kết quả:

```python
            0         1         2         3
41   0.457246 -0.025907 -3.399312 -0.974657
60   1.951312  3.260383  0.963301  1.201206
136  0.508391 -0.196713 -3.745356 -1.520113
235 -0.242459 -3.056990  1.918403 -0.578828
258  0.682841  0.326045  0.425384 -3.428254
322  1.179227 -3.184377  1.369891 -1.074833
544 -3.548824  1.553205 -2.186301  1.277104
635 -0.578093  0.193299  1.397822  3.366626
782 -0.207434  3.525865  0.283070  0.544635
803 -3.645860  0.255475 -0.549574 -1.907459
```
Các giá trị có thể được đặt dựa trên các tiêu chí này. Dưới đây là mã để giới hạn các giá trị ngoài khoảng –3 đến 3:

```python
data[np.abs(data) > 3] = np.sign(data) * 3
data.describe()
```
Kết quả:

```python
              0            1            2            3
count  1000.000000  1000.000000  1000.000000  1000.000000
mean      0.050286     0.025567    -0.001399    -0.051765
std       0.992920     1.004214     0.991414     0.995761
min      -3.000000    -3.000000    -3.000000    -3.000000
25%      -0.599807    -0.612162    -0.687373    -0.747478
50%       0.047101    -0.013609    -0.022158    -0.088274
75%       0.756646     0.695298     0.699046     0.623331
max       2.653656     3.000000     2.735527     3.000000
```
Câu lệnh np.sign(data) tạo ra các giá trị 1 và –1 dựa trên việc các giá trị trong data là dương hay âm:

```python
np.sign(data).head()
```
Kết quả:

```python
     0    1    2    3
0 -1.0  1.0 -1.0  1.0
1  1.0 -1.0  1.0 -1.0
2  1.0  1.0  1.0 -1.0
3 -1.0 -1.0  1.0 -1.0
4 -1.0  1.0 -1.0 -1.0
```
## Hoán Vị và Lấy Mẫu Ngẫu Nhiên

Hoán vị (xáo trộn ngẫu nhiên) một Series hoặc các hàng trong DataFrame rất dễ dàng bằng cách sử dụng hàm `numpy.random.permutation`. Gọi hàm permutation với chiều dài của trục bạn muốn hoán vị sẽ tạo ra một mảng số nguyên chỉ định thứ tự mới:

```python
df = pd.DataFrame(np.arange(5 * 4).reshape((5, 4)))
sampler = np.random.permutation(5)
sampler
```
Kết quả:

```python
array([3, 1, 4, 2, 0])
```
Mảng đó sau đó có thể được sử dụng trong lập chỉ mục dựa trên iloc hoặc hàm take tương đương:

```python
df
df.take(sampler)
```
Kết quả:

```python
     0  1   2   3
0    0  1   2   3
1    4  5   6   7
2    8  9  10  11
3   12 13  14  15
4   16 17  18  19

     0   1   2   3
3   12  13  14  15
1    4   5   6   7
4   16  17  18  19
2    8   9  10  11
0    0   1   2   3
```
Để chọn một tập hợp con ngẫu nhiên không lặp lại, có thể sử dụng phương thức sample trên Series và DataFrame:

```python
df.sample(n=3)
```
Kết quả:

```python
     0   1   2   3
3   12  13  14  15
4   16  17  18  19
2    8   9  10  11
```
Để tạo một mẫu với lặp lại (cho phép chọn lặp lại), truyền replace=True vào sample:

```python
choices = pd.Series([5, 7, -1, 6, 4])
draws = choices.sample(n=10, replace=True)
draws
```
Kết quả:

```python
4    4
1    7
4    4
2   -1
0    5
3    6
1    7
4    4
0    5
4    4
dtype: int64
```
## Tính Toán Biến Chỉ Thị/Biến Giả

Một loại biến đổi khác cho các ứng dụng mô hình hóa thống kê hoặc học máy là chuyển đổi biến phân loại thành ma trận "giả" hoặc "chỉ thị". Nếu một cột trong DataFrame có k giá trị khác nhau, bạn sẽ tạo ra một ma trận hoặc DataFrame với k cột chứa toàn bộ các giá trị 1 và 0. pandas có hàm `get_dummies` để làm điều này, mặc dù việc tự viết một hàm không khó. Hãy quay lại một ví dụ DataFrame trước đây:

```python
df = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'b'],
                   'data1': range(6)})
pd.get_dummies(df['key'])
```
Kết quả:

```python
   a  b  c
0  0  1  0
1  0  1  0
2  1  0  0
3  0  0  1
4  1  0  0
5  0  1  0
```
Trong một số trường hợp, bạn có thể muốn thêm tiền tố vào các cột trong DataFrame chỉ thị, sau đó có thể được hợp nhất với dữ liệu khác. get_dummies có một đối số prefix để làm điều này:

```python
dummies = pd.get_dummies(df['key'], prefix='key')
df_with_dummy = df[['data1']].join(dummies)
df_with_dummy
```
Kết quả:

```python
   data1  key_a  key_b  key_c
0      0      0      1      0
1      1      0      1      0
2      2      1      0      0
3      3      0      0      1
4      4      1      0      0
5      5      0      1      0
```
Nếu một hàng trong DataFrame thuộc về nhiều danh mục, mọi thứ sẽ phức tạp hơn. Hãy xem xét tập dữ liệu MovieLens 1M, được nghiên cứu chi tiết hơn trong Chương 14:

```python
mnames = ['movie_id', 'title', 'genres']
movies = pd.read_table('datasets/movielens/movies.dat', sep='::', header=None, names=mnames)
movies[:10]
```
Kết quả:

```python
   movie_id                                        title                         genres
0         1                             Toy Story (1995)     Animation|Children's|Comedy
1         2                               Jumanji (1995)         Adventure|Children's|Fantasy
2         3                     Grumpier Old Men (1995)                          Comedy|Romance
3         4                    Waiting to Exhale (1995)                         Comedy|Drama
4         5      Father of the Bride Part II (1995)                        Comedy
5         6                                    Heat (1995)                    Action|Crime|Thriller
6         7                                 Sabrina (1995)                       Comedy|Romance
7         8                               Tom and Huck (1995)                    Adventure|Children's
8         9                               Sudden Death (1995)                    Action
9        10                               GoldenEye (1995)                 Action|Adventure|Thriller
```
Thêm các biến chỉ thị cho mỗi thể loại yêu cầu một số xử lý. Đầu tiên, hãy trích xuất danh sách các thể loại duy nhất trong tập dữ liệu:

```python
all_genres = []
for x in movies.genres:
    all_genres.extend(x.split('|'))
genres = pd.unique(all_genres)
genres
```
Kết quả:

```python
array(['Animation', "Children's", 'Comedy', 'Adventure', 'Fantasy',
       'Romance', 'Drama', 'Action', 'Crime', 'Thriller', 'Horror',
       'Sci-Fi', 'Documentary', 'War', 'Musical', 'Mystery', 'Film-Noir',
       'Western'], dtype=object)
```
Một cách để tạo DataFrame chỉ thị là bắt đầu với một DataFrame toàn bộ là số 0:

```python
zero_matrix = np.zeros((len(movies), len(genres)))
dummies = pd.DataFrame(zero_matrix, columns=genres)
```
Bây giờ, lặp qua từng bộ phim và đặt các giá trị trong mỗi hàng của dummies thành 1. Để làm điều này, sử dụng dummies.columns để tính toán các chỉ mục cột cho từng thể loại:

```python
gen = movies.genres[0]
gen.split('|')
dummies.columns.get_indexer(gen.split('|'))
```
Kết quả:

```python
array([0, 1, 2])
```
Sau đó, có thể sử dụng .iloc để đặt các giá trị dựa trên các chỉ mục này:

```python
for i, gen in enumerate(movies.genres):
    indices = dummies.columns.get_indexer(gen.split('|'))
    dummies.iloc[i, indices] = 1
```
Sau đó, như trước đây, có thể kết hợp điều này với movies:

```python
movies_windic = movies.join(dummies.add_prefix('Genre_'))
movies_windic.iloc[0]
```
Kết quả:

```python
movie_id                                 1
title                   Toy Story (1995)
genres        Animation|Children's|Comedy
Genre_Animation                          1
Genre_Children's                         1
Genre_Comedy                             1
Genre_Adventure                          0
Genre_Fantasy                            0
Genre_Romance                            0
Genre_Drama                              0
...
Genre_Crime                              0
Genre_Thriller                           0
Genre_Horror                             0
Genre_Sci-Fi                             0
Genre_Documentary                        0
Genre_War                                0
Genre_Musical                            0
Genre_Mystery                            0
Genre_Film-Noir                          0
Genre_Western                            0
Name: 0, Length: 21, dtype: object
```
Với dữ liệu lớn hơn nhiều, phương pháp này không đặc biệt nhanh chóng. Tốt hơn là viết một hàm cấp thấp hơn viết trực tiếp vào một mảng NumPy và sau đó bao quanh kết quả trong một DataFrame.

Một công thức hữu ích cho các ứng dụng thống kê là kết hợp get_dummies với một hàm phân loại như cut:

```python
np.random.seed(12345)
values = np.random.rand(10)
values
bins = [0, 0.2, 0.4, 0.6, 0.8, 1]
pd.get_dummies(pd.cut(values, bins))
```
Kết quả:

```python
   (0.0, 0.2]  (0.2, 0.4]  (0.4, 0.6]  (0.6, 0.8]  (0.8, 1.0]
0           0          0          0          0          1
1           0          1          0          0          0
2           1          0          0          0          0
3           0          1          0          0          0
4           0          0          1          0          0
5           0          0          1          0          0
6           0          0          0          0          1
7           0          0          0          1          0
8           0          0          0          1          0
9           0          0          0          1          0
```

