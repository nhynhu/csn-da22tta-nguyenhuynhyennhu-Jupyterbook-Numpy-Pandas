# Xử Lý Chuỗi

Python từ lâu đã là một ngôn ngữ phổ biến để thao tác dữ liệu thô, một phần nhờ vào sự dễ dàng sử dụng cho xử lý chuỗi và văn bản. Hầu hết các thao tác văn bản được thực hiện đơn giản với các phương thức tích hợp của đối tượng chuỗi. Đối với việc khớp mẫu và thao tác văn bản phức tạp hơn, có thể cần sử dụng các biểu thức chính quy. pandas bổ sung vào hỗn hợp này bằng cách cho phép áp dụng các biểu thức chuỗi và chính quy một cách ngắn gọn trên toàn bộ mảng dữ liệu, ngoài ra còn xử lý sự khó chịu của dữ liệu thiếu.

## Các Phương Thức Đối Tượng Chuỗi

Trong nhiều ứng dụng xử lý chuỗi và lập trình kịch bản, các phương thức chuỗi tích hợp là đủ. Ví dụ, một chuỗi phân tách bằng dấu phẩy có thể được tách thành các phần với `split`:

```python
val = 'a,b, guido'
val.split(',')
``
Kết quả:

```python
['a', 'b', ' guido']
```
split thường được kết hợp với strip để cắt bỏ khoảng trắng (bao gồm cả dòng trống):

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
Nhưng đây không phải là một phương pháp tổng quát thực tế. Một cách nhanh hơn và Pythonic hơn là truyền một danh sách hoặc tuple cho phương thức join trên chuỗi '::':

```python
'::'.join(pieces)
```
Kết quả:

```python
'a::b::guido'
```
Các phương thức khác liên quan đến việc xác định vị trí chuỗi con. Sử dụng từ khóa in của Python là cách tốt nhất để phát hiện một chuỗi con, mặc dù index và find cũng có thể được sử dụng:

```python
'guido' in val
val.index(',')
val.find(':')
```
Kết quả:

```python
True
1
-1
```
Lưu ý sự khác biệt giữa find và index là index sẽ ném ra một ngoại lệ nếu chuỗi không được tìm thấy (trái ngược với việc trả về –1):

```python
val.index(':')
```
Kết quả:

```python
ValueError: substring not found
```
Liên quan, count trả về số lần xuất hiện của một chuỗi con cụ thể:

```python
val.count(',')
```
Kết quả:

```python
2
```
replace sẽ thay thế các lần xuất hiện của một mẫu bằng một mẫu khác. Nó cũng thường được sử dụng để xóa các mẫu bằng cách truyền một chuỗi rỗng:

```python
val.replace(',', '::')
val.replace(',', '')
```
Kết quả:

```python
'a::b:: guido'
'ab guido'
```
### Bảng 7-3. Các phương thức chuỗi tích hợp của Python

| Tham số   | Mô tả                                                                                                    |
|-----------|----------------------------------------------------------------------------------------------------------|
| `count`   | Trả về số lần xuất hiện không trùng lặp của chuỗi con trong chuỗi.                                       |
| `endswith`| Trả về True nếu chuỗi kết thúc bằng hậu tố.                                                              |
| `startswith`| Trả về True nếu chuỗi bắt đầu bằng tiền tố.                                                            |
| `join`    | Sử dụng chuỗi làm dấu phân cách để nối một chuỗi các chuỗi khác.                                         |
| `index`   | Trả về vị trí của ký tự đầu tiên trong chuỗi con nếu tìm thấy trong chuỗi; ném ra ValueError nếu không tìm thấy. |
| `find`    | Trả về vị trí của ký tự đầu tiên của lần xuất hiện đầu tiên của chuỗi con trong chuỗi; giống như `index`, nhưng trả về –1 nếu không tìm thấy. |
| `rfind`   | Trả về vị trí của ký tự đầu tiên của lần xuất hiện cuối cùng của chuỗi con trong chuỗi; trả về –1 nếu không tìm thấy. |
| `replace` | Thay thế các lần xuất hiện của chuỗi bằng một chuỗi khác.                                                |
| `strip`, `rstrip`, `lstrip` | Cắt bỏ khoảng trắng, bao gồm cả dòng mới; tương đương với `x.strip()` (và `rstrip`, `lstrip` tương ứng) cho mỗi phần tử. |
| `split`   | Phá vỡ chuỗi thành danh sách các chuỗi con bằng dấu phân cách được truyền vào.                          |
| `lower`   | Chuyển đổi các ký tự chữ cái thành chữ thường.                                                           |
| `upper`   | Chuyển đổi các ký tự chữ cái thành chữ hoa.                                                              |
| `casefold`| Chuyển đổi các ký tự thành chữ thường và chuyển đổi bất kỳ kết hợp ký tự biến đổi cụ thể vùng nào thành một dạng có thể so sánh chung. |
| `ljust`, `rjust` | Căn trái hoặc căn phải tương ứng; điền bên đối diện của chuỗi bằng khoảng trắng (hoặc một ký tự lấp đầy khác) để trả về một chuỗi với chiều rộng tối thiểu. |

Các biểu thức chính quy cũng có thể được sử dụng với nhiều thao tác này như đã thấy.

## Biểu Thức Chính Quy

Biểu thức chính quy cung cấp một cách linh hoạt để tìm kiếm hoặc khớp (thường phức tạp hơn) các mẫu chuỗi trong văn bản. Một biểu thức đơn lẻ, thường gọi là regex, là một chuỗi được tạo theo ngôn ngữ biểu thức chính quy. Module `re` tích hợp của Python chịu trách nhiệm áp dụng biểu thức chính quy cho chuỗi; dưới đây là một số ví dụ về việc sử dụng nó.

## Các Hàm trong Module re

Các hàm trong module re thuộc ba loại: khớp mẫu, thay thế và chia tách. Tất cả đều liên quan đến nhau; một regex mô tả một mẫu để xác định vị trí trong văn bản, sau đó có thể được sử dụng cho nhiều mục đích. Hãy xem xét một ví dụ đơn giản: giả sử bạn muốn chia một chuỗi với số lượng biến đổi của các ký tự khoảng trắng (tab, khoảng trắng và dòng mới). Regex mô tả một hoặc nhiều ký tự khoảng trắng là `\s+`:

```python
import re
text = "foo bar\t baz \tqux"
re.split('\s+', text)
```
Kết quả:

```python
['foo', 'bar', 'baz', 'qux']
```
Khi gọi re.split('\s+', text), biểu thức chính quy được biên dịch đầu tiên, và sau đó phương thức split của nó được gọi trên văn bản được truyền vào. Bạn có thể tự biên dịch regex với re.compile, tạo thành một đối tượng regex tái sử dụng:

```python
regex = re.compile('\s+')
regex.split(text)
```
Kết quả:

```python
['foo', 'bar', 'baz', 'qux']
```
Nếu muốn có một danh sách tất cả các mẫu khớp với regex, bạn có thể sử dụng phương thức findall:

```python
regex.findall(text)
```
Kết quả:

```python
[' ', '\t ', ' \t']
```
Để tránh việc thoát không mong muốn với \ trong biểu thức chính quy, sử dụng chuỗi thô như r'C:\x' thay vì tương đương C:\\x.

Tạo một đối tượng regex với re.compile rất được khuyến khích nếu bạn dự định áp dụng cùng một biểu thức cho nhiều chuỗi; làm như vậy sẽ tiết kiệm chu kỳ CPU.

match và search liên quan chặt chẽ đến findall. Trong khi findall trả về tất cả các khớp trong một chuỗi, search chỉ trả về khớp đầu tiên. Rigid hơn, match chỉ khớp ở đầu chuỗi. Ví dụ, hãy xem xét một đoạn văn bản và một biểu thức chính quy có thể xác định hầu hết các địa chỉ email:

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
search trả về một đối tượng khớp đặc biệt cho địa chỉ email đầu tiên trong văn bản. Đối với regex trước đó, đối tượng khớp chỉ có thể cho biết vị trí bắt đầu và kết thúc của mẫu trong chuỗi:

```python
m = regex.search(text)
m
text[m.start():m.end()]
```
Kết quả:

```python
<_sre.SRE_Match object; span=(5, 20), match='dave@google.com'>
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
Liên quan, sub sẽ trả về một chuỗi mới với các lần xuất hiện của mẫu được thay thế bằng chuỗi mới:

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
Giả sử bạn muốn tìm các địa chỉ email và đồng thời phân đoạn mỗi địa chỉ thành ba thành phần: tên người dùng, tên miền và hậu tố miền. Để làm điều này, đặt dấu ngoặc đơn xung quanh các phần của mẫu để phân đoạn:

```python
pattern = r'([A-Z0-9._%+-]+)@([A-Z0-9.-]+)\.([A-Z]{2,4})'
regex = re.compile(pattern, flags=re.IGNORECASE)
```
Một đối tượng khớp được tạo ra bởi regex đã sửa đổi này trả về một tuple của các thành phần mẫu với phương thức groups của nó:

```python
m = regex.match('wesm@bright.net')
m.groups()
```
Kết quả:

```python
('wesm', 'bright', 'net')
```
findall trả về một danh sách các tuple khi mẫu có các nhóm:

```python
regex.findall(text)
```
Kết quả:

```python
[('dave', 'google', 'com'), ('steve', 'gmail', 'com'), ('rob', 'gmail', 'com'), ('ryan', 'yahoo', 'com')]
```
sub cũng có quyền truy cập vào các nhóm trong mỗi lần khớp sử dụng các ký hiệu đặc biệt như \1 và \2. Ký hiệu \1 tương ứng với nhóm khớp đầu tiên, \2 tương ứng với nhóm thứ hai, và như vậy:

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
### Bảng 7-4. Các phương thức biểu thức chính quy

| Tham số    | Mô tả                                                                                                                  |
|------------|------------------------------------------------------------------------------------------------------------------------|
| `findall`  | Trả về tất cả các mẫu khớp không trùng lặp trong chuỗi dưới dạng danh sách.                                            |
| `finditer` | Giống như `findall`, nhưng trả về một iterator.                                                                        |
| `match`    | Khớp mẫu tại đầu chuỗi và tùy chọn phân đoạn các thành phần mẫu thành các nhóm; nếu mẫu khớp, trả về một đối tượng khớp, ngược lại None. |
| `search`   | Quét chuỗi để tìm khớp với mẫu; trả về một đối tượng khớp nếu có; khác với `match`, khớp có thể ở bất kỳ đâu trong chuỗi chứ không chỉ ở đầu. |
| `split`    | Chia chuỗi thành các phần tại mỗi lần xuất hiện của mẫu.                                                               |
| `sub`, `subn` | Thay thế tất cả (sub) hoặc n lần xuất hiện đầu tiên (subn) của mẫu trong chuỗi bằng biểu thức thay thế; sử dụng các ký hiệu \1, \2, ... để tham chiếu đến các phần tử nhóm khớp trong chuỗi thay thế. |

## Các Hàm Chuỗi Đa Chiều trong pandas

Làm sạch một tập dữ liệu lộn xộn để phân tích thường đòi hỏi nhiều công việc xử lý và chuẩn hóa chuỗi. Để làm phức tạp thêm, một cột chứa chuỗi đôi khi có thể có dữ liệu bị thiếu:

```python
data = {'Dave': 'dave@google.com', 'Steve': 'steve@gmail.com',
        'Rob': 'rob@gmail.com', 'Wes': np.nan}
data = pd.Series(data)
data
```
Kết quả:

```python
Dave     dave@google.com
Rob       rob@gmail.com
Steve    steve@gmail.com
Wes                  NaN
dtype: object
```
Có thể áp dụng các phương thức chuỗi và biểu thức chính quy bằng cách truyền một lambda hoặc hàm khác cho từng giá trị bằng cách sử dụng data.map, nhưng nó sẽ thất bại trên các giá trị NA (null). Để đối phó với điều này, Series có các phương thức hướng mảng cho các thao tác chuỗi mà bỏ qua các giá trị NA. Các phương thức này được truy cập thông qua thuộc tính str của Series; ví dụ, có thể kiểm tra xem mỗi địa chỉ email có 'gmail' trong đó hay không với str.contains:

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
Cũng có thể sử dụng các biểu thức chính quy, cùng với bất kỳ tùy chọn re như IGNORECASE:

```python
pattern = '([A-Z0-9._%+-]+)@([A-Z0-9.-]+)\\.([A-Z]{2,4})'
data.str.findall(pattern, flags=re.IGNORECASE)
```
Kết quả:

```python
Dave     [(dave, google, com)]
Rob       [(rob, gmail, com)]
Steve    [(steve, gmail, com)]
Wes                         NaN
dtype: object
```
Có một số cách để truy xuất phần tử đa chiều. Hoặc sử dụng str.get hoặc chỉ mục vào thuộc tính str:

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
Để truy cập các phần tử trong các danh sách nhúng, có thể truyền một chỉ số cho bất kỳ hàm nào trong số này:

```python
matches.str.get(1)
matches.str[0]
```
Kết quả:

```python
Dave     NaN
Rob      NaN
Steve    NaN
Wes      NaN
dtype: float64
```
Có thể cắt chuỗi theo cú pháp này:

```python
data.str[:5]
```
Kết quả:

```python
Dave dave@
Rob rob@g
Steve steve
Wes NaN
dtype: object
```
### Bảng 7-5. Các phương thức chuỗi vector hóa

| Phương thức | Mô tả |
|-------------|--------------------------------------------------------------|
| `cat`       | Nối chuỗi theo từng phần tử với dấu phân cách tùy chọn. |
| `contains`  | Trả về mảng boolean nếu mỗi chuỗi chứa mẫu/regex. |
| `count`     | Đếm số lần xuất hiện của mẫu. |
| `extract`   | Sử dụng một biểu thức chính quy với các nhóm để trích xuất một hoặc nhiều chuỗi từ một Series chuỗi; kết quả sẽ là một DataFrame với một cột cho mỗi nhóm. |
| `endswith`  | Tương đương với x.endswith(mẫu) cho mỗi phần tử. |
| `startswith`| Tương đương với x.startswith(mẫu) cho mỗi phần tử. |
| `findall`   | Tính toán danh sách tất cả các lần xuất hiện của mẫu/regex cho mỗi chuỗi. |
| `get`       | Truy cập vào từng phần tử (lấy phần tử thứ i). |
| `isalnum`   | Tương đương với `str.alnum` tích hợp. |
| `isalpha`   | Tương đương với `str.isalpha` tích hợp. |
| `isdecimal` | Tương đương với `str.isdecimal` tích hợp. |
| `isdigit`   | Tương đương với `str.isdigit` tích hợp. |
| `islower`   | Tương đương với `str.islower` tích hợp. |
| `isnumeric` | Tương đương với `str.isnumeric` tích hợp. |
| `isupper`   | Tương đương với `str.isupper` tích hợp. |
| `join`      | Nối các chuỗi trong mỗi phần tử của Series với dấu phân cách được truyền vào. |
| `len`       | Tính độ dài của mỗi chuỗi. |
| `lower`, `upper` | Chuyển đổi chữ thường, chữ hoa; tương đương với `x.lower()` hoặc `x.upper()` cho mỗi phần tử. |
| `match`     | Sử dụng `re.match` với biểu thức chính quy được truyền vào trên mỗi phần tử, trả về các nhóm khớp như danh sách. |
| `pad`       | Thêm khoảng trắng vào bên trái, phải hoặc cả hai bên của chuỗi. |
| `center`    | Tương đương với `pad(side='both')`. |
| `repeat`    | Nhân đôi các giá trị (ví dụ, `s.str.repeat(3)` tương đương với `x * 3` cho mỗi chuỗi). |
| `replace`   | Thay thế các lần xuất hiện của mẫu/regex bằng một chuỗi khác. |
| `slice`     | Cắt mỗi chuỗi trong Series. |
| `split`     | Chia các chuỗi theo dấu phân cách hoặc biểu thức chính quy. |
| `strip`     | Cắt bỏ khoảng trắng từ cả hai bên, bao gồm cả dòng mới. |
| `rstrip`    | Cắt bỏ khoảng trắng ở bên phải. |
| `lstrip`    | Cắt bỏ khoảng trắng ở bên trái. |

Các biểu thức chính quy cũng có thể được sử dụng với nhiều thao tác này như đã thấy.

