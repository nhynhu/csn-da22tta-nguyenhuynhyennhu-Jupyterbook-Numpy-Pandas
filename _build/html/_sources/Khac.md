# Đại Số Tuyến Tính với NumPy

Đại số tuyến tính, bao gồm các phép toán như nhân ma trận, phân tích ma trận, định thức và các phép toán trên ma trận vuông, là một phần quan trọng của bất kỳ thư viện mảng nào. Không giống như một số ngôn ngữ như MATLAB, phép nhân giữa hai mảng hai chiều với dấu `*` là phép nhân phần tử, thay vì nhân ma trận chấm. Vì vậy, NumPy cung cấp hàm `dot`, là một phương thức của mảng và cũng là một hàm trong không gian tên của NumPy, để thực hiện phép nhân ma trận.

## Nhân Ma Trận với `dot`

Ví dụ về nhân ma trận với `dot`:

```python
import numpy as np

x = np.array([[1., 2., 3.], [4., 5., 6.]])
y = np.array([[6., 23.], [-1, 7], [8, 9]])

# Nhân ma trận
result = x.dot(y)
print(result)
```
Kết quả:

```lua
array([[ 28., 64.],
       [ 67., 181.]])
x.dot(y) tương đương với np.dot(x, y):
```
```python
np.dot(x, y)
```
Kết quả cũng sẽ là:

```lua
array([[ 28., 64.],
       [ 67., 181.]])
```
Khi nhân ma trận giữa một mảng hai chiều và một mảng một chiều với kích thước phù hợp, kết quả là một mảng một chiều:

```python
np.dot(x, np.ones(3))
```
Kết quả:

```scss
array([ 6., 15.])
```
Từ Python 3.5 trở đi, có thể sử dụng toán tử @ để thực hiện phép nhân ma trận:

```python
x @ np.ones(3)
```
Kết quả:

```scss
array([ 6., 15.])
```
## Các Hàm Đại Số Tuyến Tính trong numpy.linalg
numpy.linalg cung cấp các hàm chuẩn cho phân tích ma trận và các phép toán như nghịch đảo và định thức. Các hàm này được triển khai thông qua các thư viện tiêu chuẩn ngành như BLAS, LAPACK hoặc có thể là thư viện Intel MKL (Math Kernel Library), tùy vào cách bạn cài đặt NumPy.

### Ví dụ về nghịch đảo ma trận và phân tích QR:

```python
from numpy.linalg import inv, qr

X = np.random.randn(5, 5)
mat = X.T.dot(X)

# Nghịch đảo ma trận
inv_mat = inv(mat)
print(inv_mat)

# Kiểm tra nghịch đảo
print(mat.dot(inv_mat))

# Phân tích QR
q, r = qr(mat)
print(r)
```
Kết quả:

```yaml
array([[ 933.1189, 871.8258, -1417.6902, -1460.4005, 1782.1391],
       [ 871.8258, 815.3929, -1325.9965, -1365.9242, 1666.9347],
       [-1417.6902, -1325.9965, 2158.4424, 2222.0191, -2711.6822],
       [-1460.4005, -1365.9242, 2222.0191, 2289.0575, -2793.422 ],
       [ 1782.1391, 1666.9347, -2711.6822, -2793.422 , 3409.5128]])
```
Ma trận chuyển vị của X nhân với chính nó cho ra ma trận trên.

### Các Hàm `numpy.linalg` Thường Dùng

| Hàm    | Mô Tả                                                                                                                       |
|--------|-----------------------------------------------------------------------------------------------------------------------------|
| `diag` | Trả về các phần tử chéo (hoặc ngoài chéo) của ma trận vuông dưới dạng mảng 1D, hoặc chuyển đổi mảng 1D thành ma trận vuông với phần tử ngoài chéo là 0. |
| `dot`  | Nhân ma trận.                                                                                                               |
| `trace`| Tính tổng các phần tử trên đường chéo của ma trận.                                                                          |
| `det`  | Tính định thức của ma trận.                                                                                                 |
| `eig`  | Tính các giá trị và vectơ riêng của ma trận vuông.                                                                          |
| `inv`  | Tính nghịch đảo của ma trận vuông.                                                                                          |
| `pinv` | Tính nghịch đảo Moore-Penrose của ma trận.                                                                                  |
| `qr`   | Tính phân tích QR.                                                                                                          |
| `svd`  | Tính phân tích giá trị kỳ dị (SVD).                                                                                         |
| `solve`| Giải hệ phương trình tuyến tính \( Ax = b \) cho \( x \), với \( A \) là ma trận vuông.                                      |
| `lstsq`| Tính nghiệm ít bình phương cho \( Ax = b \).                                                                                |

# Sinh Số Ngẫu Nhiên Giả (Pseudorandom Number Generation)

Module `numpy.random` bổ sung cho module `random` tích hợp của Python với các hàm sinh mảng số ngẫu nhiên từ nhiều phân phối xác suất khác nhau. Ví dụ, bạn có thể tạo một mảng 4 × 4 mẫu từ phân phối chuẩn chuẩn (standard normal distribution) bằng cách sử dụng hàm `normal`:

```python
import numpy as np

samples = np.random.normal(size=(4, 4))
print(samples)
```
Kết quả:

```lua
array([[ 0.5732,  0.1933,  0.4429,  1.2796],
       [ 0.575 ,  0.4339, -0.7658, -1.237 ],
       [-0.5367,  1.8545, -0.92  , -0.1082],
       [ 0.1525,  0.9435, -1.0953, -0.144 ]])
```
Trong khi đó, module random tích hợp của Python chỉ sinh một giá trị tại một thời điểm. Như bạn có thể thấy từ bài kiểm tra tốc độ dưới đây, numpy.random nhanh hơn rất nhiều khi sinh các mẫu số lượng lớn:

```python
from random import normalvariate

N = 1000000

# Dùng random module
%timeit samples = [normalvariate(0, 1) for _ in range(N)]

# Dùng numpy.random
%timeit np.random.normal(size=N)
```
Kết quả cho thấy:

```pythonpython
Sao chép mã
1.77 s ± 126 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
61.7 ms ± 1.32 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)
```

Chúng ta gọi đây là số ngẫu nhiên giả (pseudorandom numbers) vì chúng được sinh ra bởi một thuật toán với hành vi xác định dựa trên hạt giống (seed) của bộ sinh số ngẫu nhiên. Bạn có thể thay đổi hạt giống số ngẫu nhiên của NumPy bằng cách sử dụng np.random.seed:

```python
np.random.seed(1234)
```
Các hàm sinh dữ liệu trong numpy.random sử dụng một hạt giống toàn cục. Để tránh trạng thái toàn cục, bạn có thể sử dụng numpy.random.RandomState để tạo ra một bộ sinh số ngẫu nhiên cách ly với các bộ sinh khác:

```python
rng = np.random.RandomState(1234)
print(rng.randn(10))
```
Kết quả:

```scss
array([ 0.4714, -1.191 ,  1.4327, -0.3127, -0.7206,  0.8872,  0.8596,
       -0.6365,  0.0157, -2.2427])
```
### Các Hàm Trong `numpy.random`

| Hàm         | Mô Tả                                                                                                              |
|-------------|--------------------------------------------------------------------------------------------------------------------|
| `seed`      | Đặt hạt giống cho bộ sinh số ngẫu nhiên.                                                                           |
| `permutation` | Trả về một hoán vị ngẫu nhiên của một dãy số hoặc trả về một dãy số đã hoán vị.                                   |
| `shuffle`   | Hoán vị ngẫu nhiên một dãy số tại chỗ.                                                                              |
| `rand`      | Lấy mẫu từ phân phối đều.                                                                                          |
| `randint`   | Lấy các số nguyên ngẫu nhiên từ một phạm vi cho trước.                                                             |
| `randn`     | Lấy mẫu từ phân phối chuẩn (Gaussian) với trung bình 0 và độ lệch chuẩn 1 (giao diện giống MATLAB).                |
| `binomial`  | Lấy mẫu từ phân phối nhị thức.                                                                                     |
| `normal`    | Lấy mẫu từ phân phối chuẩn (Gaussian).                                                                             |
| `beta`      | Lấy mẫu từ phân phối Beta.                                                                                         |
| `chisquare` | Lấy mẫu từ phân phối chi-square.                                                                                   |
| `gamma`     | Lấy mẫu từ phân phối Gamma.                                                                                        |
| `uniform`   | Lấy mẫu từ phân phối đều trên 

# Ví Dụ: Các Bước Ngẫu Nhiên (Random Walks)

Mô phỏng các bước ngẫu nhiên là một ứng dụng minh họa việc sử dụng các phép toán mảng. Cùng xem xét một bước đi ngẫu nhiên đơn giản bắt đầu từ 0 với các bước 1 và -1 xảy ra với xác suất bằng nhau.

Dưới đây là cách thực hiện một bước đi ngẫu nhiên đơn giản với 1.000 bước bằng Python thuần (pure Python) sử dụng module `random` tích hợp:

```python
import random

position = 0
walk = [position]
steps = 1000

for i in range(steps):
    step = 1 if random.randint(0, 1) else -1
    position += step
    walk.append(position)
```
Hình 4-4 dưới đây là một ví dụ về đồ thị của 100 giá trị đầu tiên trong một bước đi ngẫu nhiên như thế:

```python
import matplotlib.pyplot as plt
plt.plot(walk[:100])
```
Hình 4-4. Một bước đi ngẫu nhiên đơn giản

CCó thể nhận thấy rằng walk thực chất là tổng cộng dồn (cumulative sum) của các bước ngẫu nhiên và có thể được tính bằng cách sử dụng một biểu thức mảng. Do đó, ta sẽ sử dụng module np.random để rút ra 1.000 lần lật đồng xu cùng một lúc, gán chúng thành 1 và -1, và tính tổng cộng dồn:

```python
import numpy as np

nsteps = 1000
draws = np.random.randint(0, 2, size=nsteps)
steps = np.where(draws > 0, 1, -1)
walk = steps.cumsum()
```
Từ đây, ta có thể bắt đầu tính toán các thống kê như giá trị nhỏ nhất và lớn nhất trong hành trình bước đi:

```python
walk.min()  # Giá trị nhỏ nhất
```
Kết quả:

```diff
-3
```
```python
walk.max()  # Giá trị lớn nhất
```
Kết quả:

```
31
```
Một thống kê phức tạp hơn là thời gian vượt qua đầu tiên, tức là bước đi mà bước đi ngẫu nhiên đạt đến một giá trị nhất định. Ở đây, ta muốn biết mất bao lâu để bước đi ngẫu nhiên cách gốc (0) ít nhất 10 bước về một phía.

Câu lệnh np.abs(walk) >= 10 sẽ cho ra một mảng boolean chỉ ra những vị trí mà bước đi đã đạt hoặc vượt qua 10, nhưng ta muốn chỉ lấy chỉ số của lần đầu tiên đạt giá trị 10 hoặc -10. Hóa ra, ta có thể tính điều này bằng cách sử dụng argmax, hàm này trả về chỉ số đầu tiên của giá trị lớn nhất trong mảng boolean (True là giá trị lớn nhất):

```python
(walk.abs() >= 10).argmax()
```
Kết quả:

```
37
```
Lưu ý rằng việc sử dụng argmax ở đây không phải lúc nào cũng hiệu quả, vì nó luôn quét toàn bộ mảng. Trong trường hợp đặc biệt này, khi một giá trị True được phát hiện, ta biết đó là giá trị tối đa.

# Mô Phỏng Nhiều Bước Ngẫu Nhiên Cùng Lúc

Nếu mục tiêu là mô phỏng nhiều bước ngẫu nhiên, chẳng hạn 5.000 bước, bạn có thể tạo ra tất cả các bước ngẫu nhiên này với một số sửa đổi nhỏ trong mã trước. Nếu truyền vào một bộ 2 phần tử, các hàm của `numpy.random` sẽ tạo ra một mảng hai chiều các giá trị ngẫu nhiên, và ta có thể tính tổng cộng dồn trên các hàng để tính tất cả 5.000 bước ngẫu nhiên trong một lần:

```python
nwalks = 5000
nsteps = 1000

# Tạo một mảng 2 chiều với giá trị ngẫu nhiên 0 hoặc 1
draws = np.random.randint(0, 2, size=(nwalks, nsteps))

# Gán các giá trị 1 và -1 cho các bước
steps = np.where(draws > 0, 1, -1)

# Tính tổng cộng dồn trên các hàng
walks = steps.cumsum(1)
```
Kết quả là một mảng hai chiều, trong đó mỗi hàng là một bước đi ngẫu nhiên:

```python
walks
```
Kết quả:

```css
array([[ 1, 0, 1, ..., 8, 7, 8],
       [ 1, 0, -1, ..., 34, 33, 32],
       [ 1, 0, -1, ..., 4, 5, 4],
       ...,
       [ 1, 2, 1, ..., 24, 25, 26],
       [ 1, 2, 3, ..., 14, 13, 14],
       [ -1, -2, -3, ..., -24, -23, -22]])
```

Giờ đây, ta có thể tính các giá trị tối đa và tối thiểu có được trong tất cả các bước đi:

```python
walks.max()  # Giá trị lớn nhất
```
Kết quả:
```
138
```
```python
walks.min()  # Giá trị nhỏ nhất
```
Kết quả:

```diff
-133
```
Tiếp theo, ta sẽ tính thời gian vượt qua mức 30 hoặc -30 lần đầu tiên. Điều này hơi phức tạp vì không phải tất cả 5.000 bước đi đều đạt đến mức 30. Ta có thể kiểm tra điều này bằng cách sử dụng phương thức any:

```python
hits30 = (np.abs(walks) >= 30).any(1)
hits30
```
Kết quả:

```python
array([False, True, False, ..., False, True, False], dtype=bool)
```
Đếm số lượng bước đi đạt 30 hoặc -30:

```python
hits30.sum()  # Số bước đi đạt 30 hoặc -30
```
Kết quả:

```yaml
3410
```
Ta có thể sử dụng mảng boolean này để chọn ra các hàng của bước đi thực sự vượt qua mức 30 và gọi argmax dọc theo trục 1 để lấy thời gian vượt qua:

```python
crossing_times = (np.abs(walks[hits30]) >= 30).argmax(1)
crossing_times.mean()  # Tính trung bình thời gian vượt qua
```
Kết quả:
```
498.89
```
Có thể thử nghiệm với các phân phối khác cho các bước đi thay vì chỉ sử dụng đồng xu với xác suất bằng nhau. CChỉ cần sử dụng một hàm sinh số ngẫu nhiên khác, chẳng hạn như normal để tạo các bước phân phối chuẩn với một giá trị trung bình và độ lệch chuẩn nhất định:

```python
steps = np.random.normal(loc=0, scale=0.25, size=(nwalks, nsteps))
```