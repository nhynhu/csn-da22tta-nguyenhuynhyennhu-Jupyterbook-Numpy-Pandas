# Đọc và Ghi Dữ liệu ở Định dạng Văn bản

**pandas** cung cấp nhiều hàm để đọc dữ liệu dạng bảng vào đối tượng DataFrame. Trong số này, **read_csv** và **read_table** là hai hàm thường được sử dụng nhất. Dưới đây là một số hàm đọc dữ liệu trong pandas:

| **Hàm**            | **Mô tả**                                                                                  |
|---------------------|-------------------------------------------------------------------------------------------|
| **read_csv**        | Đọc dữ liệu được phân tách từ tệp, URL hoặc đối tượng tương tự tệp; mặc định sử dụng dấu phẩy.  |
| **read_table**      | Đọc dữ liệu được phân tách từ tệp, URL hoặc đối tượng tương tự tệp; mặc định sử dụng tab ('\t'). |
| **read_fwf**        | Đọc dữ liệu theo định dạng cột có độ rộng cố định (không có dấu phân tách).                 |
| **read_clipboard**  | Đọc dữ liệu từ clipboard, tiện dụng khi sao chép bảng từ trang web.                       |
| **read_excel**      | Đọc dữ liệu bảng từ tệp Excel (XLS hoặc XLSX).                                            |
| **read_hdf**        | Đọc tệp HDF5 được tạo bởi pandas.                                                         |
| **read_html**       | Đọc tất cả các bảng tìm thấy trong tài liệu HTML.                                          |
| **read_json**       | Đọc dữ liệu từ chuỗi JSON.                                                                |
| **read_pickle**     | Đọc đối tượng được lưu trữ ở định dạng pickle của Python.                                  |

## Tùy chọn và xử lý dữ liệu văn bản

Các hàm đọc dữ liệu của pandas cho phép tùy chỉnh qua các tham số như:

1. **Indexing**: Xác định cột làm chỉ mục hoặc tự động gán tên cột.
2. **Chuyển đổi dữ liệu**: Xác định cách xử lý dữ liệu bị thiếu và kiểu dữ liệu.
3. **Xử lý ngày và thời gian**: Gộp các cột ngày, giờ vào một cột duy nhất.
4. **Đọc tệp lớn**: Hỗ trợ đọc dữ liệu theo từng phần (*chunks*).
5. **Dữ liệu không sạch**: Bỏ qua dòng, nhận xét hoặc xử lý số có dấu phân cách hàng nghìn.

## Ví dụ: Đọc tệp CSV cơ bản

Giả sử bạn có một tệp CSV như sau:

```plaintext
a,b,c,d,message
1,2,3,4,hello
5,6,7,8,world
9,10,11,12,foo
```

Sử dụng **read_csv** để đọc dữ liệu:

```python
import pandas as pd
df = pd.read_csv('examples/ex1.csv')
print(df)
```

Kết quả:

```plaintext
   a   b   c   d message
0  1   2   3   4   hello
1  5   6   7   8   world
2  9  10  11  12     foo
```
Nếu tệp không có dòng tiêu đề, pandas sẽ tự động gán tên cột mặc định hoặc có thể chỉ định tên cột cụ thể:

```python
pd.read_csv('examples/ex2.csv', header=None)
```

Hoặc chỉ định tên cột:

```python
pd.read_csv('examples/ex2.csv', names=['a', 'b', 'c', 'd', 'message'])
```

Khi cần chỉ định một cột làm chỉ mục, sử dụng tham số `index_col`:

```python
names = ['a', 'b', 'c', 'd', 'message']
pd.read_csv('examples/ex2.csv', names=names, index_col='message')
```

## Xử lý dữ liệu phân cấp

Đối với dữ liệu có chỉ mục phân cấp, có thể truyền danh sách tên cột hoặc số cột vào `index_col`:

```python
pd.read_csv('examples/csv_mindex.csv', index_col=['key1', 'key2'])
```

## Đọc tệp với định dạng không cố định

Với tệp có dấu phân tách không cố định (như khoảng trắng), sử dụng biểu thức chính quy:

```python
pd.read_table('examples/ex3.txt', sep='\s+')
```

Trong trường hợp này, pandas sẽ tự động xử lý khi số lượng tên cột ít hơn số cột dữ liệu.

## Bỏ qua dòng không mong muốn

Có thể bỏ qua các dòng không cần thiết bằng cách sử dụng `skiprows`:

```python
pd.read_csv('examples/ex4.csv', skiprows=[0, 2, 3])
```

## Xử lý giá trị bị thiếu

Khi dữ liệu có giá trị bị thiếu, pandas tự động nhận diện các giá trị phổ biến như `NA`, `NULL` hoặc có thể chỉ định tùy chỉnh:

```python
pd.read_csv('examples/ex5.csv', na_values=['NULL'])
```

Đối với các cột cụ thể, sử dụng từ điển để gán giá trị thiếu:

```python
sentinels = {'message': ['foo', 'NA'], 'something': ['two']}
pd.read_csv('examples/ex5.csv', na_values=sentinels)
```

### Bảng 6-2. Một số tham số của hàm `read_csv`/`read_table` 
| Tham số         | Mô tả                                                                                                                                                           |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `path`          | Chuỗi chỉ định vị trí tệp trong hệ thống tệp, URL, hoặc đối tượng giống tệp.                                                                                  |
| `sep` hoặc `delimiter` | Chuỗi ký tự hoặc biểu thức chính quy để sử dụng để tách các trường trong mỗi hàng.                                                                          |
| `header`        | Số hàng để sử dụng làm tên cột; mặc định là 0 (hàng đầu tiên), nhưng nên là `None` nếu không có hàng tiêu đề.                                                  |
| `index_col`     | Số cột hoặc tên cột để sử dụng làm chỉ mục hàng trong kết quả; có thể là một tên/số đơn hoặc danh sách chúng cho chỉ mục phân cấp.                            |
| `names`         | Danh sách tên cột cho kết quả, kết hợp với `header=None`.                                                                                                       |
| `skiprows`      | Số hàng ở đầu tệp để bỏ qua hoặc danh sách các số hàng (bắt đầu từ 0) để bỏ qua.                                                                                |
| `na_values`     | Chuỗi các giá trị để thay thế bằng NA.                                                                                                                           |
| `comment`       | Ký tự(hoặc ký tự) để tách nhận xét khỏi cuối dòng.                                                                                                               |
| `parse_dates`   | Cố gắng phân tích dữ liệu thành ngày giờ; mặc định là False. Nếu True, sẽ cố gắng phân tích tất cả các cột. Nếu không, có thể chỉ định danh sách các số cột hoặc tên cột để phân tích. Nếu phần tử của danh sách là tuple hoặc danh sách, sẽ kết hợp nhiều cột lại với nhau và phân tích thành ngày (ví dụ: nếu ngày/giờ được chia thành hai cột). |
| `keep_date_col` | Nếu kết hợp các cột để phân tích ngày, giữ các cột đã kết hợp; mặc định là False.                                                                               |
| `converters`    | Dict chứa số cột hoặc tên cột ánh xạ đến các hàm (ví dụ: `{'foo': f}` sẽ áp dụng hàm `f` cho tất cả các giá trị trong cột 'foo').                              |
| `dayfirst`      | Khi phân tích các ngày có thể gây nhầm lẫn, coi đó là định dạng quốc tế (ví dụ: 7/6/2012 -> 7 tháng 6, 2012); mặc định là False.                                 |
| `date_parser`   | Hàm để sử dụng để phân tích ngày.                                                                                                                               |
| `nrows`         | Số hàng để đọc từ đầu tệp.                                                                                                                                      |
| `iterator`      | Trả về một đối tượng TextParser để đọc tệp theo phần.                                                                                                          |
| `chunksize`     | Kích thước các khối tệp cho việc lặp lại.                                                                                                                        |
| `skip_footer`   | Số dòng để bỏ qua ở cuối tệp.                                                                                                                                   |
| `verbose`       | In thông tin đầu ra của bộ phân tích cú pháp khác nhau, như số lượng giá trị bị thiếu được đặt vào các cột không phải số.                                    |
| `encoding`      | Mã hóa văn bản cho Unicode (ví dụ: 'utf-8' cho văn bản được mã hóa UTF-8).                                                                                     |
| `squeeze`       | Nếu dữ liệu được phân tích chỉ chứa một cột, trả về một Series.                                                                                               |
| `thousands`     | Dấu phân cách cho hàng nghìn (ví dụ: ',' hoặc '.').                                                                                                            |
### Đọc Tệp Văn Bản Từng Phần

Khi xử lý các tệp rất lớn hoặc tìm ra bộ tham số đúng để xử lý một tệp lớn, bạn có thể chỉ muốn đọc một phần nhỏ của tệp hoặc lặp qua các phần nhỏ hơn của tệp.

Trước khi chúng ta xem một tệp lớn, hãy làm cho các cài đặt hiển thị của pandas gọn gàng hơn:

```python
pd.options.display.max_rows = 10
```
Bây giờ chúng ta có:

```python
result = pd.read_csv('examples/ex6.csv')
print(result)
```
Kết quả:

```python
         one       two     three      four key
0   0.467976 -0.038649 -0.295344 -1.824726   L
1  -0.358893  1.404453  0.704965 -0.200638   B
2  -0.501840  0.659254 -0.421691 -0.057688   G
3   0.204886  1.074134  1.388361 -0.982404   R
4   0.354628 -0.133116  0.283763 -0.837063   Q
...       ...       ...       ...       ...  ..
9995  2.311896 -0.417070 -1.409599 -0.515821   L
9996 -0.479893 -0.650419  0.745152 -0.646038   E
9997  0.523331  0.787112  0.486066  1.093156   K
9998 -0.362559  0.598894 -1.843201  0.887292   G
9999 -0.096376 -1.012999 -0.657431 -0.573315   O
[10000 rows x 5 columns]
```
Nếu bạn chỉ muốn đọc một số lượng nhỏ các hàng (tránh đọc toàn bộ tệp), hãy chỉ định bằng nrows:

```python
result = pd.read_csv('examples/ex6.csv', nrows=5)
print(result)
```
Kết quả:

```python
        one       two     three      four key
0  0.467976 -0.038649 -0.295344 -1.824726   L
1 -0.358893  1.404453  0.704965 -0.200638   B
2 -0.501840  0.659254 -0.421691 -0.057688   G
3  0.204886  1.074134  1.388361 -0.982404   R
4  0.354628 -0.133116  0.283763 -0.837063   Q
```
Để đọc tệp theo từng phần, hãy chỉ định chunksize là một số hàng:

```python
chunker = pd.read_csv('examples/ex6.csv', chunksize=1000)
print(chunker)
```
Đối tượng TextParser được trả về bởi read_csv cho phép bạn lặp qua các phần của tệp theo chunksize. Ví dụ, chúng ta có thể lặp qua ex6.csv, tổng hợp số lần xuất hiện giá trị trong cột 'key' như sau:

```python
chunker = pd.read_csv('examples/ex6.csv', chunksize=1000)
tot = pd.Series([], dtype='float64')
for piece in chunker:
    tot = tot.add(piece['key'].value_counts(), fill_value=0)
tot = tot.sort_values(ascending=False)
print(tot[:10])
```
Kết quả:

```python
E    368.0
X    364.0
L    346.0
O    343.0
Q    340.0
M    338.0
J    337.0
F    335.0
K    334.0
H    330.0
dtype: float64
```
TextParser cũng được trang bị phương thức get_chunk cho phép bạn đọc các phần có kích thước tùy ý.

### Ghi Dữ Liệu ra Định Dạng Văn Bản

Dữ liệu cũng có thể được xuất ra định dạng phân cách. Hãy xem xét một trong các tệp CSV đã đọc trước đó:

```python
In [41]: data = pd.read_csv('examples/ex5.csv')
In [42]: data
Out[42]:
  something   a   b    c    d message
0       one   1   2  3.0    4     NaN
1       two   5   6  NaN    8   world
2     three   9  10 11.0   12     foo
```
Sử dụng phương thức to_csv của DataFrame, chúng ta có thể ghi dữ liệu ra tệp phân cách bằng dấu phẩy:

```python
In [43]: data.to_csv('examples/out.csv')
In [44]: !cat examples/out.csv
,something,a,b,c,d,message
0,one,1,2,3.0,4,
1,two,5,6,,8,world
2,three,9,10,11.0,12,foo
```
Các dấu phân cách khác cũng có thể được sử dụng. (Viết ra sys.stdout để in kết quả văn bản ra console):

```python
In [45]: import sys
In [46]: data.to_csv(sys.stdout, sep='|')
|something|a|b|c|d|message
0|one|1|2|3.0|4|
1|two|5|6||8|world
2|three|9|10|11.0|12|foo
```
Các giá trị bị thiếu xuất hiện dưới dạng chuỗi rỗng trong đầu ra. Bạn có thể muốn biểu thị chúng bằng một giá trị đặc biệt khác:

```python
In [47]: data.to_csv(sys.stdout, na_rep='NULL')
,something,a,b,c,d,message
0,one,1,2,3.0,4,NULL
1,two,5,6,NULL,8,world
2,three,9,10,11.0,12,foo
```
Với các tùy chọn khác không được chỉ định, cả nhãn hàng và cột đều được ghi. Cả hai đều có thể bị tắt:

```python
In [48]: data.to_csv(sys.stdout, index=False, header=False)
one,1,2,3.0,4,
two,5,6,,8,world
three,9,10,11.0,12,foo
```
Bạn cũng có thể chỉ ghi một phần tập hợp các cột, và theo thứ tự tùy chọn của bạn:

```python
In [49]: data.to_csv(sys.stdout, index=False, columns=['a', 'b', 'c'])
a,b,c
1,2,3.0
5,6,
9,10,11.0
```
Series cũng có phương thức to_csv:

```python
In [50]: dates = pd.date_range('1/1/2000', periods=7)
In [51]: ts = pd.Series(np.arange(7), index=dates)
In [52]: ts.to_csv('examples/tseries.csv')
In [53]: !cat examples/tseries.csv
2000-01-01,0
2000-01-02,1
2000-01-03,2
2000-01-04,3
2000-01-05,4
2000-01-06,5
2000-01-07,6
```
## Làm Việc Với Các Định Dạng Phân Cách

Bạn có thể tải hầu hết các dạng dữ liệu dạng bảng từ đĩa bằng cách sử dụng các hàm như `pandas.read_table`. Tuy nhiên, trong một số trường hợp, có thể cần phải xử lý thủ công. Không hiếm khi nhận được một tệp với một hoặc nhiều dòng bị lỗi gây ra sự cố khi sử dụng `read_table`. Để minh họa các công cụ cơ bản, hãy xem xét một tệp CSV nhỏ:

```python
In [54]: !cat examples/ex7.csv
"a","b","c"
"1","2","3"
"1","2","3"
```
Đối với bất kỳ tệp nào có dấu phân cách một ký tự, bạn có thể sử dụng module csv tích hợp của Python. Để sử dụng nó, hãy truyền bất kỳ tệp mở hoặc đối tượng giống tệp nào vào csv.reader:

```python
import csv
f = open('examples/ex7.csv')
reader = csv.reader(f)
```
Lặp qua reader như một tệp trả về các tuple giá trị với bất kỳ ký tự dấu ngoặc nào đã được loại bỏ:

```python
In [56]: for line in reader:
 ....: print(line)
['a', 'b', 'c']
['1', '2', '3']
['1', '2', '3']
```
Từ đó, bạn có thể thực hiện các thao tác cần thiết để đưa dữ liệu vào dạng bạn cần. Hãy làm theo từng bước. Đầu tiên, chúng ta đọc tệp vào một danh sách các dòng:

```python
In [57]: with open('examples/ex7.csv') as f:
 ....: lines = list(csv.reader(f))
 ```
Sau đó, chúng ta tách các dòng thành dòng tiêu đề và các dòng dữ liệu:

```python
In [58]: header, values = lines[0], lines[1:]
```
Sau đó, chúng ta có thể tạo một từ điển các cột dữ liệu bằng cách sử dụng một comprehension từ điển và biểu thức zip(*values), biểu thức này chuyển các hàng thành cột:

```python
In [59]: data_dict = {h: v for h, v in zip(header, zip(*values))}
In [60]: data_dict
Out[60]: {'a': ('1', '1'), 'b': ('2', '2'), 'c': ('3', '3')}
```
Các tệp CSV có nhiều định dạng khác nhau. Để định nghĩa một định dạng mới với dấu phân cách khác, quy ước trích dẫn chuỗi, hoặc dấu kết thúc dòng khác, chúng ta định nghĩa một lớp con đơn giản của csv.Dialect:

```python
class my_dialect(csv.Dialect):
    lineterminator = '\n'
    delimiter = ';'
    quotechar = '"'
    quoting = csv.QUOTE_MINIMAL
reader = csv.reader(f, dialect=my_dialect)
```
Chúng ta cũng có thể cung cấp các tham số dial của CSV riêng lẻ dưới dạng từ khóa cho csv.reader mà không cần định nghĩa một lớp con:

```python
reader = csv.reader(f, delimiter='|')
Các tùy chọn có thể có (thuộc tính của csv.Dialect) và công dụng của chúng có thể được tìm thấy trong Bảng 6-3.
```

### Bảng 6-3. Tùy chọn của dial CSV

| Tham số            | Mô tả                                                                                                                   |
|--------------------|-------------------------------------------------------------------------------------------------------------------------|
| `delimiter`        | Chuỗi một ký tự để tách các trường; mặc định là ','.                                                                     |
| `lineterminator`   | Dấu kết thúc dòng để ghi; mặc định là '\r\n'. Bộ đọc bỏ qua điều này và nhận ra các dấu kết thúc dòng đa nền tảng.        |
| `quotechar`        | Ký tự trích dẫn cho các trường có ký tự đặc biệt (như dấu phân cách); mặc định là '".                                    |
| `quoting`          | Quy ước trích dẫn. Các tùy chọn bao gồm `csv.QUOTE_ALL` (trích dẫn tất cả các trường), `csv.QUOTE_MINIMAL` (chỉ các trường có ký tự đặc biệt như dấu phân cách), `csv.QUOTE_NONNUMERIC`, và `csv.QUOTE_NONE` (không trích dẫn). Xem tài liệu Python để biết chi tiết đầy đủ. Mặc định là `QUOTE_MINIMAL`. |
| `skipinitialspace` | Bỏ qua khoảng trắng sau mỗi dấu phân cách; mặc định là False.                                                           |
| `doublequote`      | Cách xử lý ký tự trích dẫn bên trong trường; nếu True, nó được nhân đôi (xem tài liệu trực tuyến để biết chi tiết đầy đủ và hành vi). |
| `escapechar`       | Chuỗi để thoát khỏi dấu phân cách nếu trích dẫn được đặt thành `csv.QUOTE_NONE`; bị vô hiệu hóa theo mặc định.           |

Đối với các tệp có dấu phân cách phức tạp hơn hoặc cố định nhiều ký tự, bạn sẽ không thể sử dụng module csv. Trong các trường hợp đó, bạn sẽ phải chia nhỏ dòng và làm sạch bằng cách sử dụng phương thức split của chuỗi hoặc phương thức biểu thức chính quy re.split.

Để viết các tệp phân cách thủ công, bạn có thể sử dụng csv.writer. Nó chấp nhận một đối tượng tệp mở, có thể ghi và các tùy chọn định dạng và dial tương tự như csv.reader:

```python
with open('mydata.csv', 'w') as f:
    writer = csv.writer(f, dialect=my_dialect)
    writer.writerow(('one', 'two', 'three'))
    writer.writerow(('1', '2', '3'))
    writer.writerow(('4', '5', '6'))
    writer.writerow(('7', '8', '9'))
```
## Dữ liệu JSON

JSON (JavaScript Object Notation) đã trở thành một trong những định dạng tiêu chuẩn để gửi dữ liệu qua yêu cầu HTTP giữa các trình duyệt web và các ứng dụng khác. Đây là một định dạng dữ liệu tự do hơn nhiều so với dạng văn bản bảng như CSV. Dưới đây là một ví dụ:

```python
obj = """
{"name": "Wes",
 "places_lived": ["United States", "Spain", "Germany"],
 "pet": null,
 "siblings": [{"name": "Scott", "age": 30, "pets": ["Zeus", "Zuko"]},
 {"name": "Katie", "age": 38,
 "pets": ["Sixes", "Stache", "Cisco"]}]
}
"""
```
JSON rất gần với mã Python hợp lệ, ngoại trừ giá trị null của nó và một số khác biệt nhỏ khác (chẳng hạn như không cho phép dấu phẩy cuối trong danh sách). Các kiểu cơ bản bao gồm đối tượng (dict), mảng (list), chuỗi, số, boolean và null. Tất cả các khóa trong một đối tượng phải là chuỗi. Có một số thư viện Python để đọc và ghi dữ liệu JSON. Tôi sẽ sử dụng json ở đây, vì nó được tích hợp vào thư viện chuẩn của Python. Để chuyển đổi một chuỗi JSON sang dạng Python, hãy sử dụng json.loads:

```python
import json
result = json.loads(obj)
print(result)
```
Kết quả:

```python
{'name': 'Wes',
 'pet': None,
 'places_lived': ['United States', 'Spain', 'Germany'],
 'siblings': [{'age': 30, 'name': 'Scott', 'pets': ['Zeus', 'Zuko']},
              {'age': 38, 'name': 'Katie', 'pets': ['Sixes', 'Stache', 'Cisco']}]}
```
Ngược lại, json.dumps chuyển đổi một đối tượng Python trở lại thành JSON:

```python
asjson = json.dumps(result)
```
Cách bạn chuyển đổi một đối tượng JSON hoặc danh sách các đối tượng thành DataFrame hoặc một cấu trúc dữ liệu khác để phân tích sẽ phụ thuộc vào bạn. Thuận tiện, bạn có thể truyền một danh sách các dict (trước đây là các đối tượng JSON) vào hàm khởi tạo DataFrame và chọn một tập con của các trường dữ liệu:

```python
siblings = pd.DataFrame(result['siblings'], columns=['name', 'age'])
print(siblings)
```
Kết quả:

```python
    name  age
0  Scott   30
1  Katie   38
```
Hàm pandas.read_json có thể tự động chuyển đổi các bộ dữ liệu JSON trong các sắp xếp cụ thể thành Series hoặc DataFrame. Ví dụ:

```python
!cat examples/example.json
```
Nội dung tệp:

```json
[{"a": 1, "b": 2, "c": 3},
 {"a": 4, "b": 5, "c": 6},
 {"a": 7, "b": 8, "c": 9}]

```
Các tùy chọn mặc định cho pandas.read_json giả định rằng mỗi đối tượng trong mảng JSON là một hàng trong bảng:

```python
data = pd.read_json('examples/example.json')
print(data)
```
Kết quả:

```python
   a  b  c
0  1  2  3
1  4  5  6
2  7  8  9
```
Nếu bạn cần xuất dữ liệu từ pandas sang JSON, một cách là sử dụng các phương thức to_json trên Series và DataFrame:

```python
print(data.to_json())
print(data.to_json(orient='records'))
```
Kết quả:

```json
{"a":{"0":1,"1":4,"2":7},"b":{"0":2,"1":5,"2":8},"c":{"0":3,"1":6,"2":9}}
```
```json
[{"a":1,"b":2,"c":3},{"a":4,"b":5,"c":6},{"a":7,"b":8,"c":9}]
```
## XML và HTML: Web Scraping

Python có nhiều thư viện để đọc và ghi dữ liệu ở định dạng HTML và XML phổ biến. Các thư viện bao gồm lxml, Beautiful Soup và html5lib. Trong khi lxml thường nhanh hơn, các thư viện khác có thể xử lý tốt hơn các tệp HTML hoặc XML bị lỗi.

pandas có một hàm tích hợp, `read_html`, sử dụng các thư viện như lxml và Beautiful Soup để tự động phân tích các bảng từ các tệp HTML thành các đối tượng DataFrame. Để minh họa cách hoạt động của hàm này, tôi đã tải một tệp HTML từ cơ quan chính phủ FDIC Hoa Kỳ hiển thị danh sách các ngân hàng bị phá sản. Đầu tiên, bạn cần cài đặt một số thư viện bổ sung mà `read_html` sử dụng:

```bash
conda install lxml
pip install beautifulsoup4 html5lib
```
Nếu bạn không sử dụng conda, pip install lxml cũng có thể hoạt động.

Hàm pandas.read_html có nhiều tùy chọn, nhưng mặc định nó tìm kiếm và cố gắng phân tích tất cả dữ liệu dạng bảng chứa trong các thẻ <table>. Kết quả là một danh sách các đối tượng DataFrame:

```python
tables = pd.read_html('examples/fdic_failed_bank_list.html')
len(tables)
failures = tables[0]
failures.head()
```
Kết quả:

```plaintext
 Bank Name City ST CERT
0 Allied Bank Mulberry AR 91
1 The Woodbury Banking Company Woodbury GA 11297
2 First CornerStone Bank King of Prussia PA 35312
3 Trust Company Bank Memphis TN 9956
4 North Milwaukee State Bank Milwaukee WI 20364
 Acquiring Institution Closing Date Updated Date
0 Today's Bank September 23, 2016 November 17, 2016
1 United Bank August 19, 2016 November 17, 2016
2 First-Citizens Bank & Trust Company May 6, 2016 September 6, 2016
3 The Bank of Fayette County April 29, 2016 September 6, 2016
4 First-Citizens Bank & Trust Company March 11, 2016 June 16, 2016
```
Do failures có nhiều cột, pandas chèn một ký tự xuống dòng \. Từ đây, chúng ta có thể thực hiện một số thao tác làm sạch và phân tích dữ liệu, như tính số lượng ngân hàng bị phá sản theo năm:

```python
close_timestamps = pd.to_datetime(failures['Closing Date'])
close_timestamps.dt.year.value_counts()
```
Kết quả:

```plaintext
2010    157
2009    140
2011     92
2012     51
2008     25
...
2004      4
2001      4
2007      3
2003      3
2000      2
```
Name: Closing Date, Length: 15, dtype: int64
```
Phân tích cú pháp XML với lxml.objectify
XML (eXtensible Markup Language) là một định dạng dữ liệu cấu trúc phổ biến khác hỗ trợ dữ liệu phân cấp, lồng nhau với siêu dữ liệu. Cuốn sách bạn đang đọc được tạo ra từ một loạt các tài liệu XML lớn.

Hàm pandas.read_html, sử dụng lxml hoặc Beautiful Soup để phân tích dữ liệu từ HTML. XML và HTML có cấu trúc tương tự nhau, nhưng XML tổng quát hơn. Dưới đây là ví dụ về cách sử dụng lxml để phân tích dữ liệu từ định dạng XML tổng quát hơn.

Cơ quan Giao thông Vận tải Metropolitan New York (MTA) công bố một số loạt dữ liệu về dịch vụ xe buýt và tàu hỏa của mình. Chúng ta sẽ xem xét dữ liệu hiệu suất, được chứa trong một tập hợp các tệp XML. Mỗi dịch vụ tàu hỏa hoặc xe buýt có một tệp khác nhau (như Performance_MNR.xml cho Đường sắt Metro-North) chứa dữ liệu hàng tháng dưới dạng một loạt các bản ghi XML như sau:
```
xml
<INDICATOR>
    <INDICATOR_SEQ>373889</INDICATOR_SEQ>
    <PARENT_SEQ></PARENT_SEQ>
    <AGENCY_NAME>Metro-North Railroad</AGENCY_NAME>
    <INDICATOR_NAME>Escalator Availability</INDICATOR_NAME>
    <DESCRIPTION>Percent of the time that escalators are operational systemwide. The availability rate is based on physical observations performed the morning of regular business days only. This is a new indicator the agency began reporting in 2009.</DESCRIPTION>
    <PERIOD_YEAR>2011</PERIOD_YEAR>
    <PERIOD_MONTH>12</PERIOD_MONTH>
    <CATEGORY>Service Indicators</CATEGORY>
    <FREQUENCY>M</FREQUENCY>
    <DESIRED_CHANGE>U</DESIRED_CHANGE>
    <INDICATOR_UNIT>%</INDICATOR_UNIT>
    <DECIMAL_PLACES>1</DECIMAL_PLACES>
    <YTD_TARGET>97.00</YTD_TARGET>
    <YTD_ACTUAL></YTD_ACTUAL>
    <MONTHLY_TARGET>97.00</MONTHLY_TARGET>
    <MONTHLY_ACTUAL></MONTHLY_ACTUAL>
</INDICATOR>
```
Sử dụng lxml.objectify, chúng ta phân tích tệp và lấy tham chiếu đến nút gốc của tệp XML với getroot:

```python
from lxml import objectify
path = 'examples/mta_perf/Performance_MNR.xml'
parsed = objectify.parse(open(path))
root = parsed.getroot()
```
root.INDICATOR trả về một generator tạo ra mỗi phần tử XML <INDICATOR>. Đối với mỗi bản ghi, chúng ta có thể tạo một dict tên thẻ (như YTD_ACTUAL) với các giá trị dữ liệu (loại trừ một số thẻ):

```python
data = []
skip_fields = ['PARENT_SEQ', 'INDICATOR_SEQ', 'DESIRED_CHANGE', 'DECIMAL_PLACES']
for elt in root.INDICATOR:
    el_data = {}
    for child in elt.getchildren():
        if child.tag in skip_fields:
            continue
        el_data[child.tag] = child.pyval
    data.append(el_data)
```
Cuối cùng, chuyển đổi danh sách dict này thành một DataFrame:

```python
perf = pd.DataFrame(data)
perf.head()
```
Kết quả:

```plaintext
Empty DataFrame
Columns: []
Index: []
```
Dữ liệu XML có thể phức tạp hơn nhiều so với ví dụ này. Mỗi thẻ cũng có thể có siêu dữ liệu. Xem xét một thẻ liên kết HTML, cũng là XML hợp lệ:

```python
from io import StringIO
tag = '<a href="http://www.google.com">Google</a>'
root = objectify.parse(StringIO(tag)).getroot()
```
Bây giờ bạn có thể truy cập bất kỳ trường nào (như href) trong thẻ hoặc văn bản liên kết:

```python
root.get('href')
root.text
```
Kết quả:

```python
'http://www.google.com'
'Google'
```
