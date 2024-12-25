# Hàm Toàn Cục: Các Hàm Mảng Nhanh Theo Phần Tử

Hàm toàn cục, hay còn gọi là ufunc, là một hàm thực hiện các phép toán theo phần tử trên dữ liệu trong các ndarray. Các hàm này có thể được xem như là các lớp bọc nhanh cho các phép toán vector hóa trên các giá trị số và tạo ra kết quả theo từng phần tử. Nhiều ufunc là các phép biến đổi đơn giản theo phần tử, chẳng hạn như `sqrt` hoặc `exp`.

## Ufunc Đơn (Unary Ufuncs)

Những hàm này thực hiện phép toán trên một mảng duy nhất. Ví dụ, các phép toán đơn giản như tính căn bậc hai (`sqrt`) hoặc tính lũy thừa của e (`exp`) cho mỗi phần tử có thể được thực hiện bằng ufunc đơn.

### Ví Dụ về Ufunc Đơn

Tạo mảng và tính toán căn bậc hai và lũy thừa của e cho mỗi phần tử:

```python
import numpy as np

arr = np.arange(10)
print(np.sqrt(arr))  # Căn bậc hai của các phần tử
print(np.exp(arr))    # Lũy thừa e của các phần tử
```
Kết quả:

```python
array([ 0. , 1. , 1.4142, 1.7321, 2. , 2.2361, 2.4495, 2.6458, 2.8284, 3. ])
array([ 1. , 2.7183, 7.3891, 20.0855, 54.5982, 148.4132, 403.4288, 1096.6332, 2980.958 , 8103.0839])
```
Các ufunc đơn như sqrt và exp thực hiện phép toán theo từng phần tử trong mảng.

## Ufunc Nhị Phân
Các ufunc nhị phân nhận hai mảng làm đầu vào và thực hiện phép toán trên các phần tử tương ứng của chúng. Một ví dụ đơn giản là maximum, hàm này trả về mảng có các giá trị lớn hơn giữa hai mảng.

### Ví Dụ về Ufunc Nhị Phân
```python
x = np.random.randn(8)
y = np.random.randn(8)

print(np.maximum(x, y))  # Giá trị lớn hơn giữa x và y
```
Kết quả:

```python
array([ 0.8626, 1.0048, 1.3272, 0.6702, 0.853 , 0.0222, 0.7584, -0.6605])
```
Hàm np.maximum tính toán giá trị lớn hơn giữa từng phần tử của x và y.

## Ufunc Trả Về Nhiều Mảng
Một số ufunc có thể trả về nhiều mảng. Ví dụ là hàm modf, trả về phần thập phân và phần nguyên của một mảng số thực.

### Ví Dụ về Ufunc Trả Về Nhiều Mảng
```python
arr = np.random.randn(7) * 5

remainder, whole_part = np.modf(arr)
print(remainder)
print(whole_part)
```
Kết quả:

```python
array([-0.2623, -0.0915, -0.663 , 0.3731, 0.6182, 0.45 , 0.0077])
array([-3., -6., -6., 5., 3., 3., 5.])
```
Hàm np.modf trả về hai mảng: phần thập phân và phần nguyên.

## Thực Hiện Trực Tiếp Trên Mảng với Tham Số out
Một tính năng hữu ích của ufunc là khả năng thực hiện trực tiếp trên mảng bằng cách sử dụng tham số out. Điều này cho phép thực hiện phép toán trực tiếp trên mảng mà không cần sao chép dữ liệu.

### Ví Dụ về Thực Hiện Trực Tiếp Trên Mảng
```python
arr = np.random.randn(7) * 5

np.sqrt(arr, arr)  # Tính căn bậc hai và gán kết quả vào chính mảng arr
print(arr)
```
Kết quả:

```python
array([   nan,    nan,    nan,    2.318 ,   1.9022,   1.8574,   2.2378])
```
Ở đây, np.sqrt tính toán căn bậc hai của các phần tử trong arr và gán trực tiếp vào chính mảng đó.

# Bảng 4-3. Ufunc đơn

| **Hàm**        | **Mô tả**                                                                 |
|----------------|---------------------------------------------------------------------------|
| `abs`, `fabs`  | Tính giá trị tuyệt đối theo phần tử cho các giá trị số nguyên, số thực hoặc số phức |
| `sqrt`         | Tính căn bậc hai của mỗi phần tử (tương đương với `arr ** 0.5`)         |
| `square`       | Tính bình phương của mỗi phần tử (tương đương với `arr ** 2`)           |
| `exp`          | Tính lũy thừa của `e` cho mỗi phần tử                                    |
| `log`, `log10`, `log2`, `log1p` | Log tự nhiên (cơ sở `e`), log cơ sở 10, log cơ sở 2 và `log(1 + x)`, tương ứng |
| `sign`         | Tính dấu của mỗi phần tử: 1 (dương), 0 (không) hoặc -1 (âm)              |
| `ceil`         | Tính trần của mỗi phần tử (tức là, số nguyên nhỏ nhất lớn hơn hoặc bằng số đó) |
| `floor`        | Tính sàn của mỗi phần tử (tức là, số nguyên lớn nhất nhỏ hơn hoặc bằng mỗi phần tử) |
| `rint`         | Làm tròn các phần tử về số nguyên gần nhất, giữ nguyên kiểu dữ liệu      |
| `modf`         | Trả về các phần thập phân và nguyên của mảng dưới dạng một mảng riêng biệt |
| `isnan`        | Trả về mảng boolean chỉ ra xem mỗi giá trị có phải là NaN (Not a Number) hay không |
| `isfinite`, `isinf` | Trả về mảng boolean chỉ ra xem mỗi phần tử có hữu hạn (không phải vô cùng, không phải NaN) hoặc vô cùng, tương ứng |
| `cos`, `cosh`, `sin`, `sinh`, `tan`, `tanh` | Các công thức lượng giác thường và hyperbolic |
| `arccos`, `arccosh`, `arcsin`, `arcsinh`, `arctan`, `arctanh` | Các công thức lượng giác ngược |
| `logical_not`  | Tính giá trị sự thật của `not x` theo phần tử (tương đương với `~arr`)   |


# Bảng 4-4. Hàm toàn cầu nhị phân

| **Hàm**        | **Mô tả**                                                                 |
|----------------|---------------------------------------------------------------------------|
| `add`          | Cộng các phần tử tương ứng trong các mảng                                 |
| `subtract`     | Trừ các phần tử trong mảng thứ hai từ mảng đầu tiên                       |
| `multiply`     | Nhân các phần tử trong mảng                                              |
| `divide`, `floor_divide` | Chia hoặc chia nguyên (cắt bỏ phần dư)                                   |
| `power`        | Nâng các phần tử trong mảng đầu tiên lên các lũy thừa được chỉ định trong mảng thứ hai |
| `maximum`, `fmax` | Giá trị lớn nhất theo phần tử; `fmax` bỏ qua NaN                        |
| `minimum`, `fmin` | Giá trị nhỏ nhất theo phần tử; `fmin` bỏ qua NaN                        |
| `mod`          | Phép chia theo phần tử (số dư của phép chia)                              |
| `copysign`     | Sao chép dấu của các giá trị trong đối số thứ hai cho các giá trị trong đối số đầu tiên |
| `greater`, `greater_equal`, `less`, `less_equal`, `equal`, `not_equal` | Thực hiện so sánh theo phần tử, tạo ra mảng boolean (tương đương với các toán tử infix `>`, `>=`, `<`, `<=`, `==`, `!=`) |
| `logical_and`, `logical_or`, `logical_xor` | Tính giá trị sự thật theo phần tử của phép toán logic (tương đương với các toán tử infix `&`, `|`, `^`) |
# Bảng 4-3. Ufunc đơn

| **Hàm**        | **Mô tả**                                                                 |
|----------------|---------------------------------------------------------------------------|
| `abs`, `fabs`  | Tính giá trị tuyệt đối theo phần tử cho các giá trị số nguyên, số thực hoặc số phức |
| `sqrt`         | Tính căn bậc hai của mỗi phần tử (tương đương với `arr ** 0.5`)         |
| `square`       | Tính bình phương của mỗi phần tử (tương đương với `arr ** 2`)           |
| `exp`          | Tính lũy thừa của `e` cho mỗi phần tử                                    |
| `log`, `log10`, `log2`, `log1p` | Log tự nhiên (cơ sở `e`), log cơ sở 10, log cơ sở 2 và `log(1 + x)`, tương ứng |
| `sign`         | Tính dấu của mỗi phần tử: 1 (dương), 0 (không) hoặc -1 (âm)              |
| `ceil`         | Tính trần của mỗi phần tử (tức là, số nguyên nhỏ nhất lớn hơn hoặc bằng số đó) |
| `floor`        | Tính sàn của mỗi phần tử (tức là, số nguyên lớn nhất nhỏ hơn hoặc bằng mỗi phần tử) |
| `rint`         | Làm tròn các phần tử về số nguyên gần nhất, giữ nguyên kiểu dữ liệu      |
| `modf`         | Trả về các phần thập phân và nguyên của mảng dưới dạng một mảng riêng biệt |
| `isnan`        | Trả về mảng boolean chỉ ra xem mỗi giá trị có phải là NaN (Not a Number) hay không |
| `isfinite`, `isinf` | Trả về mảng boolean chỉ ra xem mỗi phần tử có hữu hạn (không phải vô cùng, không phải NaN) hoặc vô cùng, tương ứng |
| `cos`, `cosh`, `sin`, `sinh`, `tan`, `tanh` | Các công thức lượng giác thường và hyperbolic |
| `arccos`, `arccosh`, `arcsin`, `arcsinh`, `arctan`, `arctanh` | Các công thức lượng giác ngược |
| `logical_not`  | Tính giá trị sự thật của `not x` theo phần tử (tương đương với `~arr`)   |

# Bảng 4-4. Hàm toàn cầu nhị phân

| **Hàm**        | **Mô tả**                                                                 |
|----------------|---------------------------------------------------------------------------|
| `add`          | Cộng các phần tử tương ứng trong các mảng                                 |
| `subtract`     | Trừ các phần tử trong mảng thứ hai từ mảng đầu tiên                       |
| `multiply`     | Nhân các phần tử trong mảng                                              |
| `divide`, `floor_divide` | Chia hoặc chia nguyên (cắt bỏ phần dư)                                   |
| `power`        | Nâng các phần tử trong mảng đầu tiên lên các lũy thừa được chỉ định trong mảng thứ hai |
| `maximum`, `fmax` | Giá trị lớn nhất theo phần tử; `fmax` bỏ qua NaN                        |
| `minimum`, `fmin` | Giá trị nhỏ nhất theo phần tử; `fmin` bỏ qua NaN                        |
| `mod`          | Phép chia theo phần tử (số dư của phép chia)                              |
| `copysign`     | Sao chép dấu của các giá trị trong đối số thứ hai cho các giá trị trong đối số đầu tiên |
| `greater`, `greater_equal`, `less`, `less_equal`, `equal`, `not_equal` | Thực hiện so sánh theo phần tử, tạo ra mảng boolean (tương đương với các toán tử infix `>`, `>=`, `<`, `<=`, `==`, `!=`) |
| `logical_and`, `logical_or`, `logical_xor` | Tính giá trị sự thật theo phần tử của phép toán logic (tương đương với các toán tử infix `&`, `|`, `^`) |
