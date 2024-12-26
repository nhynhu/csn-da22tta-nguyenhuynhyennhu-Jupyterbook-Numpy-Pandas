# String Manipulation
Python từ lâu đã là một ngôn ngữ phổ biến cho việc thao tác dữ liệu thô, phần nào nhờ vào tính dễ sử dụng trong việc xử lý chuỗi và văn bản. Hầu hết các thao tác văn bản được thực hiện dễ dàng với các phương thức có sẵn của đối tượng chuỗi. Đối với các thao tác phức tạp hơn về mẫu và thao tác văn bản, có thể cần sử dụng biểu thức chính quy (regular expressions). Pandas bổ sung vào danh sách này bằng cách cho phép áp dụng chuỗi và biểu thức chính quy một cách ngắn gọn trên toàn bộ mảng dữ liệu, đồng thời xử lý vấn đề dữ liệu bị thiếu.

## String Object Methods
Trong nhiều ứng dụng xâu chuỗi và kịch bản, các phương thức chuỗi tích hợp đã đủ. Ví dụ, một chuỗi phân tách bằng dấu phẩy có thể được chia thành các phần với split:

```python
val = 'a,b, guido'
val.split(',')
```
Kết quả:

```python
['a', 'b', ' guido']
```
split thường được kết hợp với strip để loại bỏ khoảng trắng (bao gồm cả dòng mới):

```python
pieces = [x.strip() for x in val.split(',')]
pieces
```
Kết quả:

```python
['a', 'b', 'guido']
```
Các chuỗi con này có thể được nối lại với nhau bằng một dấu hai chấm bằng cách sử dụng phép cộng:

```python
first, second, third = pieces
first + '::' + second + '::' + third
```
Kết quả:

```python
'a::b::guido'
```
Nhưng đây không phải là một phương pháp chung thực tế. Một cách nhanh hơn và Pythonic hơn là truyền một danh sách hoặc tuple vào phương thức join trên chuỗi '::':

```python
'::'.join(pieces)
```
Kết quả:

```python
'a::b::guido'
```
Các phương thức khác liên quan đến việc xác định vị trí chuỗi con. Sử dụng từ khóa in của Python là cách tốt nhất để phát hiện chuỗi con, mặc dù index và find cũng có thể được sử dụng:

```python
'guido' in val
val.index(',')
val.find(':')
```
Kết quả lần lượt:

```python
True
1
-1
```
Lưu ý sự khác biệt giữa find và index là index sẽ đưa ra ngoại lệ nếu chuỗi không được tìm thấy (trong khi find trả về -1):

```python
val.index(':')
```
Kết quả:

```python
ValueError: substring not found
```
Tương tự, count trả về số lần xuất hiện của một chuỗi con cụ thể:

```python
val.count(',')
```
Kết quả:

```python
2
```
replace sẽ thay thế các lần xuất hiện của một mẫu bằng một mẫu khác. Nó thường được sử dụng để xóa các mẫu, bằng cách truyền vào một chuỗi rỗng:

```python
val.replace(',', '::')
val.replace(',', '')
```
Kết quả lần lượt:

```python
'a::b:: guido'
'ab guido'
```
Xem bảng dưới đây để biết danh sách một số phương thức chuỗi tích hợp của Python:
## Bảng 5-3: Các phương thức chuỗi tích hợp của Python

| **Phương thức**       | **Mô tả**                                                                                     |
|-----------------------|---------------------------------------------------------------------------------------------|
| `count`              | Trả về số lần xuất hiện không trùng lặp của chuỗi con trong chuỗi.                          |
| `endswith`           | Trả về `True` nếu chuỗi kết thúc bằng hậu tố.                                               |
| `startswith`         | Trả về `True` nếu chuỗi bắt đầu bằng tiền tố.                                               |
| `join`               | Sử dụng chuỗi làm dấu phân cách để nối một dãy các chuỗi khác.                              |
| `index`              | Trả về vị trí của ký tự đầu tiên trong chuỗi con nếu tìm thấy trong chuỗi; đưa ra `ValueError` nếu không tìm thấy. |
| `find`               | Trả về vị trí của ký tự đầu tiên của lần xuất hiện đầu tiên của chuỗi con trong chuỗi; giống như `index`, nhưng trả về `-1` nếu không tìm thấy. |
| `rfind`              | Trả về vị trí của ký tự đầu tiên của lần xuất hiện cuối cùng của chuỗi con trong chuỗi; trả về `-1` nếu không tìm thấy. |
| `replace`            | Thay thế các lần xuất hiện của chuỗi này bằng chuỗi khác.                                   |
| `strip`, `rstrip`, `lstrip` | Loại bỏ khoảng trắng, bao gồm cả dòng mới; tương đương với `x.strip()` (và `rstrip`, `lstrip`, tương ứng). |
| `split`              | Chia chuỗi thành danh sách các chuỗi con sử dụng dấu phân cách được truyền vào.             |
| `lower`              | Chuyển đổi các ký tự chữ cái thành chữ thường.                                              |
| `upper`              | Chuyển đổi các ký tự chữ cái thành chữ hoa.                                                 |
| `casefold`           | Chuyển đổi các ký tự thành chữ thường và chuẩn hóa các ký tự khu vực để so sánh chung.       |
| `ljust`, `rjust`     | Canh trái hoặc canh phải, điền thêm khoảng trắng (hoặc ký tự khác) để đạt chiều rộng tối thiểu. |

## Biểu Thức Chính Quy (Regular Expressions)
Biểu thức chính quy cung cấp một cách linh hoạt để tìm kiếm hoặc khớp các mẫu chuỗi phức tạp trong văn bản. Một biểu thức duy nhất, thường được gọi là regex, là một chuỗi được hình thành theo ngôn ngữ biểu thức chính quy. Mô-đun re tích hợp sẵn của Python chịu trách nhiệm áp dụng các biểu thức chính quy cho các chuỗi; dưới đây là một số ví dụ về cách sử dụng của nó.

Các chức năng của mô-đun re chia thành ba loại: khớp mẫu, thay thế và phân tách. Tự nhiên, chúng đều có liên quan; một regex mô tả một mẫu để xác định trong văn bản, sau đó có thể được sử dụng cho nhiều mục đích khác nhau. Hãy xem một ví dụ đơn giản:

Giả sử chúng ta muốn phân tách một chuỗi với một số lượng biến đổi của các ký tự khoảng trắng (tab, khoảng trắng và dòng mới). Regex mô tả một hoặc nhiều ký tự khoảng trắng là \s+:

```python
import re
text = "foo bar\t baz \tqux"
re.split('\s+', text)
```
Kết quả:

```python
['foo', 'bar', 'baz', 'qux']
```
Khi gọi re.split('\s+', text), biểu thức chính quy đầu tiên được biên dịch, và sau đó phương thức split của nó được gọi trên văn bản đã truyền vào. Bạn có thể tự biên dịch regex bằng re.compile, tạo thành một đối tượng regex có thể tái sử dụng:

```python
regex = re.compile('\s+')
regex.split(text)
```
Kết quả:

```python
['foo', 'bar', 'baz', 'qux']
```
Nếu bạn muốn lấy danh sách tất cả các mẫu khớp với regex, bạn có thể sử dụng phương thức findall:

```python
regex.findall(text)
```
Kết quả:

```python
[' ', '\t ', ' \t']
```
Để tránh việc thoát ký tự không mong muốn với \ trong biểu thức chính quy, hãy sử dụng chuỗi thô như r'C:\x' thay vì tương đương 'C:\\x'.

Tạo một đối tượng regex với re.compile được khuyến khích cao nếu bạn dự định áp dụng cùng một biểu thức cho nhiều chuỗi; làm như vậy sẽ tiết kiệm chu kỳ CPU.

match và search có liên quan chặt chẽ đến findall. Trong khi findall trả về tất cả các khớp trong một chuỗi, search chỉ trả về khớp đầu tiên. Chặt chẽ hơn, match chỉ khớp ở đầu chuỗi. Như một ví dụ ít tầm thường hơn, hãy xem xét một khối văn bản và một biểu thức chính quy có thể xác định hầu hết các địa chỉ email:

```python
text = """Dave dave@google.com
Steve steve@gmail.com
Rob rob@gmail.com
Ryan ryan@yahoo.com
"""
pattern = r'[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}'
# re.IGNORECASE làm cho regex không phân biệt chữ hoa chữ thường
regex = re.compile(pattern, flags=re.IGNORECASE)
```
Sử dụng findall trên văn bản tạo ra một danh sách các địa chỉ email:

```python
regex.findall(text)
```
Kết quả:

```python
['dave@google.com', 'steve@gmail.com', 'rob@gmail.com', 'ryan@yahoo.com']

```
search trả về một đối tượng khớp đặc biệt cho địa chỉ email đầu tiên trong văn bản. Đối với regex trước đó, đối tượng khớp chỉ có thể cho chúng ta biết vị trí bắt đầu và kết thúc của mẫu trong chuỗi:

```python
m = regex.search(text)
m
```
Kết quả:

```python
<_sre.SRE_Match object; span=(5, 20), match='dave@google.com'>
```
```python
text[m.start():m.end()]
```
Kết quả:

```python
'dave@google.com'
```
regex.match trả về None, vì nó chỉ khớp nếu mẫu xuất hiện ở đầu chuỗi:

```python
print(regex.match(text))
```
Kết quả:

```python
None
```
Liên quan, sub sẽ trả về một chuỗi mới với các lần xuất hiện của mẫu được thay thế bằng một chuỗi mới:

```python
print(regex.sub('REDACTED', text))
```
Kết quả:

```python
Dave REDACTED
Steve REDACTED
Rob REDACTED
Ryan REDACTED
```
Giả sử bạn muốn tìm địa chỉ email và đồng thời phân đoạn mỗi địa chỉ thành ba thành phần của nó: tên người dùng, tên miền và hậu tố miền. Để làm điều này, hãy đặt dấu ngoặc đơn xung quanh các phần của mẫu để phân đoạn:

```python
pattern = r'([A-Z0-9._%+-]+)@([A-Z0-9.-]+)\.([A-Z]{2,4})'
regex = re.compile(pattern, flags=re.IGNORECASE)
```
Một đối tượng khớp được tạo ra bởi regex đã chỉnh sửa này trả về một bộ của các thành phần mẫu với phương thức groups của nó:

```python
m = regex.match('wesm@bright.net')
m.groups()
```
Kết quả:

```python
('wesm', 'bright', 'net')
```
findall trả về một danh sách các bộ khi mẫu có nhóm:

```python
regex.findall(text)
```
Kết quả:

```python
[('dave', 'google', 'com'), ('steve', 'gmail', 'com'), ('rob', 'gmail', 'com'), ('ryan', 'yahoo', 'com')]
```
sub cũng có quyền truy cập vào các nhóm trong mỗi khớp sử dụng các ký hiệu đặc biệt như \1 và \2. Ký hiệu \1 tương ứng với nhóm khớp đầu tiên, \2 tương ứng với nhóm thứ hai, và cứ như vậy:

```python
print(regex.sub(r'Username: \1, Domain: \2, Suffix: \3', text))
```
Kết quả:

```python
Dave Username: dave, Domain: google, Suffix: com
Steve Username: steve, Domain: gmail, Suffix: com
Rob Username: rob, Domain: gmail, Suffix: com
Ryan Username: ryan, Domain: yahoo, Suffix: com
```
Có nhiều hơn nữa về biểu thức chính quy trong Python, phần lớn trong đó nằm ngoài phạm vi của cuốn sách. Bảng 7-4 cung cấp một tóm tắt ngắn gọn.
## Bảng 5-4: Các phương thức Regular Expression (Regex) trong Python

| **Phương thức**         | **Mô tả**                                                                                                  |
|-------------------------|----------------------------------------------------------------------------------------------------------|
| `findall`              | Trả về tất cả các mẫu khớp không trùng lặp trong một chuỗi dưới dạng danh sách.                          |
| `finditer`             | Giống như `findall`, nhưng trả về một iterator.                                                          |
| `match`                | Khớp mẫu ở đầu chuỗi và tùy chọn phân đoạn các thành phần mẫu thành các nhóm. Nếu mẫu khớp, trả về một đối tượng khớp; nếu không, trả về `None`. |
| `search`               | Quét chuỗi để tìm khớp với mẫu. Trả về một đối tượng khớp nếu có; không giống như `match`, mẫu khớp có thể ở bất kỳ đâu trong chuỗi, không chỉ ở đầu. |
| `split`                | Phân tách chuỗi thành các phần tại mỗi lần xuất hiện của mẫu.                                             |
| `sub`, `subn`          | Thay thế tất cả (`sub`) hoặc `n` lần xuất hiện đầu tiên (`subn`) của mẫu trong chuỗi bằng biểu thức thay thế. Sử dụng các ký hiệu `\1`, `\2`, ... để tham chiếu các phần tử nhóm khớp trong chuỗi thay thế. |

## Các Hàm Chuỗi Vector hóa trong pandas
Việc làm sạch một tập dữ liệu lộn xộn để phân tích thường đòi hỏi nhiều thao tác và chuẩn hóa chuỗi. Để làm phức tạp hơn, một cột chứa các chuỗi đôi khi sẽ có dữ liệu thiếu:

```python
data = {'Dave': 'dave@google.com', 'Steve': 'steve@gmail.com',
        'Rob': 'rob@gmail.com', 'Wes': np.nan}
data = pd.Series(data)
data
```
Kết quả:

```python
Dave     dave@google.com
Rob      rob@gmail.com
Steve    steve@gmail.com
Wes      NaN
dtype: object
```
Kiểm tra các giá trị thiếu:

```python
data.isnull()
```
Kết quả:

```python
Dave     False
Rob      False
Steve    False
Wes      True
dtype: bool
```
Có thể áp dụng các phương thức chuỗi và biểu thức chính quy (bằng cách truyền vào một lambda hoặc hàm khác) cho từng giá trị bằng cách sử dụng data.map, nhưng nó sẽ thất bại trên các giá trị NA (null). Để đối phó với điều này, Series có các phương thức hướng mảng cho các thao tác chuỗi bỏ qua các giá trị NA. Các phương thức này được truy cập thông qua thuộc tính str của Series; ví dụ, có thể kiểm tra xem mỗi địa chỉ email có chứa 'gmail' trong đó hay không với str.contains:

```python
data.str.contains('gmail')
```
Kết quả:

```python
Dave     False
Rob       True
Steve     True
Wes        NaN
dtype: object
```
Cũng có thể sử dụng các biểu thức chính quy cùng với bất kỳ tùy chọn re nào như IGNORECASE:

```python
pattern = '([A-Z0-9._%+-]+)@([A-Z0-9.-]+)\\.([A-Z]{2,4})'
data.str.findall(pattern, flags=re.IGNORECASE)
```
Kết quả:

```python
Dave     [(dave, google, com)]
Rob       [(rob, gmail, com)]
Steve    [(steve, gmail, com)]
Wes                      NaN
dtype: object
```
Có một số cách để truy xuất phần tử theo dạng vector hóa. Hoặc sử dụng str.get hoặc truy cập vào thuộc tính str:

```python
matches = data.str.match(pattern, flags=re.IGNORECASE)
matches
```
Kết quả:

```python
Dave     True
Rob      True
Steve    True
Wes       NaN
dtype: object
```
Để truy cập các phần tử trong danh sách nhúng, có thể truyền một chỉ mục vào một trong các hàm này:

```python
matches.str.get(1)
```
Kết quả:

```python
Dave    NaN
Rob     NaN
Steve   NaN
Wes     NaN
dtype: float64
python
matches.str[0]
```
Kết quả:

```python
Dave    NaN
Rob     NaN
Steve   NaN
Wes     NaN
dtype: float64
```
Tương tự, có thể cắt chuỗi bằng cách sử dụng cú pháp này:

```python
data.str[:5]
```
Kết quả:

```python
Dave     dave@
Rob      rob@g
Steve    steve
Wes       NaN
dtype: object
```
Xem Bảng 5-5 để biết thêm các phương thức chuỗi trong pandas.
## Bảng 5-5: Một số phương thức chuỗi vector hóa trong Python

| **Phương thức**      | **Mô tả**                                                                                                  |
|----------------------|----------------------------------------------------------------------------------------------------------|
| `cat`               | Nối chuỗi theo phần tử với dấu phân cách tùy chọn.                                                       |
| `contains`          | Trả về mảng boolean nếu mỗi chuỗi chứa mẫu/regex.                                                        |
| `count`             | Đếm số lần xuất hiện của mẫu.                                                                            |
| `extract`           | Sử dụng biểu thức chính quy với các nhóm để trích xuất một hoặc nhiều chuỗi từ một `Series` của chuỗi; kết quả là một `DataFrame` với một cột cho mỗi nhóm. |
| `endswith`          | Tương đương với `x.endswith(pattern)` cho mỗi phần tử.                                                  |
| `startswith`        | Tương đương với `x.startswith(pattern)` cho mỗi phần tử.                                                |
| `findall`           | Tính toán danh sách tất cả các lần xuất hiện của mẫu/regex cho mỗi chuỗi.                                |
| `get`               | Truy xuất vào mỗi phần tử (lấy phần tử thứ `i`).                                                         |
| `isalnum`           | Tương đương với `str.isalnum` tích hợp sẵn.                                                             |
| `isalpha`           | Tương đương với `str.isalpha` tích hợp sẵn.                                                             |
| `isdecimal`         | Tương đương với `str.isdecimal` tích hợp sẵn.                                                           |
| `isdigit`           | Tương đương với `str.isdigit` tích hợp sẵn.                                                             |
| `islower`           | Tương đương với `str.islower` tích hợp sẵn.                                                             |
| `isnumeric`         | Tương đương với `str.isnumeric` tích hợp sẵn.                                                           |
| `isupper`           | Tương đương với `str.isupper` tích hợp sẵn.                                                             |
| `join`              | Nối chuỗi trong mỗi phần tử của `Series` với dấu phân cách đã truyền.                                   |
| `len`               | Tính toán độ dài của mỗi chuỗi.                                                                         |
| `lower`, `upper`    | Chuyển đổi chữ cái; tương đương với `x.lower()` hoặc `x.upper()` cho mỗi phần tử.                       |
| `match`             | Sử dụng `re.match` với biểu thức chính quy đã truyền trên mỗi phần tử; trả về các nhóm đã khớp dưới dạng danh sách. |
| `pad`               | Thêm khoảng trắng vào bên trái, bên phải, hoặc cả hai bên của chuỗi.                                    |
| `center`            | Tương đương với `pad(side='both')`.                                                                     |
| `repeat`            | Nhân giá trị (ví dụ: `s.str.repeat(3)` tương đương với `x * 3` cho mỗi chuỗi).                          |
| `replace`           | Thay thế các lần xuất hiện của mẫu/regex bằng chuỗi khác.                                               |
| `slice`             | Cắt mỗi chuỗi trong `Series`.                                                                           |
| `split`             | Tách chuỗi theo dấu phân cách hoặc biểu thức chính quy.                                                 |
| `strip`             | Loại bỏ khoảng trắng từ cả hai bên, bao gồm cả dòng mới.                                                |
| `rstrip`            | Loại bỏ khoảng trắng bên phải.                                                                          |
| `lstrip`            | Loại bỏ khoảng trắng bên trái.                                                                          |

