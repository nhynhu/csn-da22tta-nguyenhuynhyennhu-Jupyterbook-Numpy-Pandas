## Phép toán với Mảng NumPy

Mảng NumPy đóng vai trò quan trọng bởi chúng cho phép thực hiện các phép toán theo lô trên dữ liệu mà không cần viết bất kỳ vòng lặp nào. Người dùng NumPy thường gọi quá trình này là vector hóa. Các phép toán số học giữa các mảng có kích thước tương đương sẽ áp dụng phép toán đó lên từng phần tử:

```python
arr = np.array([[1., 2., 3.], [4., 5., 6.]])
arr
```
```python
#Output:
array([[ 1., 2., 3.],
       [ 4., 5., 6.]])
```
```python
arr * arr
```
```python
#Output:
array([[ 1.,  4.,  9.],
       [16., 25., 36.]])
```
```python
arr - arr
```
```python
#Output:
array([[0., 0., 0.],
       [0., 0., 0.]])
```
Các phép toán số học với các số vô hướng sẽ truyền đối số số vô hướng đến từng phần tử trong mảng:

```python
1 / arr
```
```python
#Output:
array([[1.    , 0.5   , 0.3333],
       [0.25  , 0.2   , 0.1667]])
```
```python
arr ** 0.5
```
```python
#Output:
array([[1.    , 1.4142, 1.7321],
       [2.    , 2.2361, 2.4495]])
```
Các phép so sánh giữa các mảng có cùng kích thước sẽ tạo ra các mảng boolean:

```python
arr2 = np.array([[0., 4., 1.], [7., 2., 12.]])
arr2
```
```python
#Output:
array([[ 0.,  4.,  1.],
       [ 7.,  2., 12.]])
```
```python
arr2 > arr
```
```python
#Output:
array([[False,  True, False],
       [ True, False,  True]], dtype=bool)
```
### Chỉ mục cơ bản và Cắt lát

Chỉ mục mảng NumPy là một chủ đề phong phú, vì có nhiều cách để bạn chọn một tập con của dữ liệu hoặc các phần tử riêng lẻ. Mảng một chiều rất đơn giản; bề ngoài, chúng hoạt động tương tự như danh sách Python:

```python
arr = np.arange(10)
arr
```
```python
#output
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```
Chỉ mục đơn:
```python
arr[5]
```
```python
#output
5
```
Cắt lát (slicing):

```python
arr[5:8]
```
Kết quả:

```python
array([5, 6, 7])
```
Gán giá trị cho lát cắt:

```python
arr[5:8] = 12
arr
```
Kết quả:

```python
array([ 0, 1, 2, 3, 4, 12, 12, 12, 8, 9])
```
Lát cắt và Dạng xem (View)
Trong NumPy, các lát cắt mảng là dạng xem của mảng gốc, không sao chép dữ liệu. Điều này có nghĩa là thay đổi lát cắt cũng thay đổi dữ liệu trong mảng nguồn:

```python
arr_slice = arr[5:8]
arr_slice
```
Kết quả:

```python
array([12, 12, 12])
```
Thay đổi giá trị trong lát cắt:

```python
arr_slice[1] = 12345
arr
```
Kết quả:

```python
array([ 0, 1, 2, 3, 4, 12, 12345, 12, 8, 9])
Gán giá trị toàn bộ lát cắt:
```
```python
arr_slice[:] = 64
arr
```
Kết quả:

```python
array([ 0, 1, 2, 3, 4, 64, 64, 64, 8, 9])
```
Nếu bạn cần sao chép dữ liệu thay vì tạo dạng xem, sử dụng copy():
```python
arr_copy = arr[5:8].copy()
```
Chỉ mục trong Mảng Đa Chiều
Mảng hai chiều có thể được chỉ mục tương tự:

```python
arr2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
arr2d[2]
```
Kết quả:

```python
array([7, 8, 9])
```
```python
arr2d[0][2]
```
Kết quả:

```python
3
```
Hoặc viết gọn:

```python
arr2d[0, 2]
```
Kết quả:

```python
3
```
Chỉ mục Mảng Ba Chiều
Ví dụ với mảng 3 chiều:

```python
arr3d = np.array([[[1, 2, 3], [4, 5, 6]], [[7, 8, 9], [10, 11, 12]]])
arr3d[0]
```
Kết quả:

```python
array([[1, 2, 3],
       [4, 5, 6]])
```
Gán giá trị mới:
```python
old_values = arr3d[0].copy()
arr3d[0] = 42
arr3d
```
Kết quả:

```python
array([[[42, 42, 42],
        [42, 42, 42]],
       [[ 7,  8,  9],
        [10, 11, 12]]])
```
Khôi phục giá trị:

```python
arr3d[0] = old_values
arr3d
```
Kết quả:

```python
array([[[ 1,  2,  3],
        [ 4,  5,  6]],
       [[ 7,  8,  9],
        [10, 11, 12]]])
```
Chỉ mục phức hợp:

```python
arr3d[1, 0]
```
Kết quả:

```python
array([7, 8, 9])
```
Lưu ý, trong tất cả các trường hợp trên, các lát cắt đều là dạng xem, không phải bản sao.
### Lập chỉ mục bằng cách cắt
NumPy cung cấp khả năng thao tác mạnh mẽ với dữ liệu nhờ tính năng lập chỉ mục và cắt lát (slicing). Phương pháp này tương tự như thao tác với danh sách Python, nhưng mạnh mẽ hơn vì hỗ trợ mảng nhiều chiều.

#### Lập chỉ mục trên mảng một chiều
Với mảng một chiều, việc lập chỉ mục và cắt lát tương tự như trong danh sách Python. Phép cắt lát cho phép truy xuất một dải phần tử từ mảng.

Ví dụ:

```python
arr = np.array([0, 1, 2, 3, 4, 64, 64, 64, 8, 9])
print(arr[1:6])
```
Kết quả:

```python
array([ 1, 2, 3, 4, 64])
```
Trong ví dụ trên, cú pháp arr[1:6] chọn các phần tử từ chỉ số 1 đến chỉ số 5.

#### Lập chỉ mục trên mảng hai chiều
Mảng hai chiều (2D) yêu cầu cách lập chỉ mục khác, vì cần chỉ định cả trục hàng (trục 0) và trục cột (trục 1). Ví dụ:

```python
arr2d = np.array([[1, 2, 3],
                  [4, 5, 6],
                  [7, 8, 9]])
print(arr2d[:2])
```
Kết quả:

```python
array([[1, 2, 3],
       [4, 5, 6]])
       
```
Phép cắt arr2d[:2] chọn hai hàng đầu tiên, trong đó mỗi hàng chứa toàn bộ cột.

Kết hợp nhiều phép cắt
Có thể sử dụng nhiều lát cắt để chỉ định vùng dữ liệu cần lấy. Ví dụ:

```python
print(arr2d[:2, 1:])
```
Kết quả:

```python
array([[2, 3],
       [5, 6]])
```
Trong ví dụ này, hai hàng đầu tiên (hàng 0 và 1) được chọn, nhưng chỉ các cột từ cột thứ hai trở đi.

Trộn chỉ số và phép cắt
Có thể kết hợp chỉ số nguyên và phép cắt để truy xuất các lát cắt có số chiều thấp hơn. Ví dụ:

Lấy hàng thứ hai nhưng chỉ hai cột đầu tiên:

```python
print(arr2d[1, :2])
```
Kết quả:

```python
array([4, 5])
```
Lấy cột thứ ba nhưng chỉ hai hàng đầu tiên:

```python
print(arr2d[:2, 2])
```
Kết quả:

```python
array([3, 6])
```
Sử dụng dấu hai chấm (:)
Dấu hai chấm (:) được sử dụng để chọn toàn bộ trục hoặc một phần của mảng. Ví dụ:

Chọn toàn bộ cột đầu tiên từ tất cả các hàng:
```python
print(arr2d[:, :1])
```
Kết quả:

```python
array([[1],
       [4],
       [7]])
```
Trong ví dụ này, toàn bộ cột đầu tiên được chọn từ tất cả các hàng.

Gán giá trị cho lát cắt

Có thể gán giá trị mới cho một lát cắt, áp dụng cho toàn bộ vùng được chọn. Ví dụ, thay thế tất cả phần tử trong hai hàng đầu tiên (hàng 0 và 1) từ cột thứ hai trở đi bằng giá trị 0:

```python
arr2d[:2, 1:] = 0
print(arr2d)
```
Kết quả:

```python
array([[1, 0, 0],
       [4, 0, 0],
       [7, 8, 9]])
```
### Lập Chỉ Mục Boolean trong NumPy

Lập chỉ mục Boolean là một kỹ thuật mạnh mẽ trong NumPy, cho phép chọn và thao tác với dữ liệu trong mảng dựa trên các điều kiện cụ thể. Trong chương này, sẽ tìm hiểu cách sử dụng mảng Boolean để lọc và thay đổi dữ liệu trong mảng.

#### Tạo Dữ Liệu Ngẫu Nhiên với NumPy

Giả sử có một mảng tên và một mảng dữ liệu với các giá trị ngẫu nhiên. Các mảng này sẽ được tạo ra từ hàm `randn` trong `numpy.random`.

```python
import numpy as np

# Tạo mảng tên
names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])

# Tạo mảng dữ liệu ngẫu nhiên với phân phối chuẩn
data = np.random.randn(7, 4)

print(names)
print(data)
```
Kết quả:

```python 
array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'], dtype='<U4')
array([[ 0.0929,  0.2817,  0.769 ,  1.2464],
       [ 1.0072, -1.2962,  0.275 ,  0.2289],
       [ 1.3529,  0.8864, -2.0016, -0.3718],
       [ 1.669 , -0.4386, -0.5397,  0.477 ],
       [ 3.2489, -1.0212, -0.5771,  0.1241],
       [ 0.3026,  0.5238,  0.0009,  1.3438],
       [-0.7135, -0.8312, -2.3702, -1.8608]])
```
Trong ví dụ trên, mảng names chứa các tên và mảng data chứa các giá trị ngẫu nhiên.

#### Sử Dụng Mảng Boolean để Lập Chỉ Mục Mảng
Để chọn tất cả các hàng có tên là "Bob", có thể so sánh mảng names với chuỗi 'Bob'. Kết quả sẽ là một mảng boolean.

```python
print(names == 'Bob')
```
Kết quả:

```python
array([ True, False, False,  True, False, False, False], dtype=bool)
```
Mảng boolean này có giá trị True cho những phần tử mà tên là "Bob" và False cho các phần tử còn lại.

Lập chỉ mục với mảng Boolean:
Có thể sử dụng mảng boolean để chọn các hàng trong mảng data.

```python
print(data[names == 'Bob'])
```
Kết quả:

```python
array([[ 0.0929,  0.2817,  0.769 ,  1.2464],
       [ 1.669 , -0.4386, -0.5397,  0.477 ]])
```
#### Kết Hợp Nhiều Điều Kiện Boolean
Nhiều điều kiện boolean có thể được kết hợp bằng cách sử dụng toán tử & (và) hoặc | (hoặc). Ví dụ, để chọn các hàng có tên là "Bob" hoặc "Will", có thể làm như sau:

```python
mask = (names == 'Bob') | (names == 'Will')
print(data[mask])
```
Kết quả:

```python
array([[ 0.0929,  0.2817,  0.769 ,  1.2464],
       [ 1.3529,  0.8864, -2.0016, -0.3718],
       [ 1.669 , -0.4386, -0.5397,  0.477 ],
       [ 3.2489, -1.0212, -0.5771,  0.1241]])
```
#### Sử Dụng Toán Tử ~ để Phủ Định Điều Kiện
Toán tử ~ có thể được sử dụng để phủ định điều kiện. Ví dụ, để chọn tất cả các phần tử không phải là "Bob", có thể làm như sau:

```python
print(data[~(names == 'Bob')])
```
Kết quả:

```python
array([[ 1.0072, -1.2962,  0.275 ,  0.2289],
       [ 1.3529,  0.8864, -2.0016, -0.3718],
       [ 3.2489, -1.0212, -0.5771,  0.1241],
       [ 0.3026,  0.5238,  0.0009,  1.3438],
       [-0.7135, -0.8312, -2.3702, -1.8608]])
```
#### Gán Giá Trị với Mảng Boolean
Một trong những ứng dụng mạnh mẽ của lập chỉ mục boolean là việc gán giá trị cho các phần tử trong mảng thỏa mãn điều kiện. Ví dụ, để thay thế tất cả các giá trị âm trong mảng data thành 0, có thể làm như sau:

```python
data[data < 0] = 0
print(data)
```
Kết quả:

```python
array([[ 0.0929,  0.2817,  0.769 ,  1.2464],
       [ 1.0072,  0.    ,  0.275 ,  0.2289],
       [ 1.3529,  0.8864,  0.    ,  0.    ],
       [ 1.669 ,  0.    ,  0.    ,  0.477 ],
       [ 3.2489,  0.    ,  0.    ,  0.1241],
       [ 0.3026,  0.5238,  0.0009,  1.3438],
       [ 0.    ,  0.    ,  0.    ,  0.    ]])
```
#### Gán Giá Trị Cho Hàng hoặc Cột với Boolean
Có thể dễ dàng gán giá trị cho toàn bộ hàng hoặc cột bằng cách sử dụng một mảng boolean. Ví dụ, để thay thế tất cả các giá trị trong mảng data của những người không tên là "Joe" thành 7, có thể làm như sau:

```python
data[names != 'Joe'] = 7
print(data)
```
Kết quả:

```python
array([[ 7. ,  7. ,  7. ,  7. ],
       [ 1.0072,  0. ,  0.275 ,  0.2289],
       [ 7. ,  7. ,  7. ,  7. ],
       [ 7. ,  7. ,  7. ,  7. ],
       [ 7. ,  7. ,  7. ,  7. ],
       [ 0.3026,  0.5238,  0.0009,  1.3438],
       [ 0. ,  0. ,  0. ,  0. ]])

```
### Fancy Indexing trong NumPy

Fancy indexing là một thuật ngữ được NumPy sử dụng để mô tả quá trình lập chỉ mục bằng cách sử dụng các mảng số nguyên. Điều này cho phép người dùng truy cập và thao tác với các phần tử của mảng thông qua chỉ số được xác định trước, thay vì phải sử dụng các phép toán hay chỉ mục theo kiểu mặc định.

#### Tạo Mảng 8 × 4

Giả sử có một mảng 8 × 4. Mảng này có thể được tạo ra và điền các giá trị theo cách sau:

```python
import numpy as np

arr = np.empty((8, 4))
for i in range(8):
    arr[i] = i

print(arr)
```
Kết quả:

```python
array([[ 0., 0., 0., 0.],
       [ 1., 1., 1., 1.],
       [ 2., 2., 2., 2.],
       [ 3., 3., 3., 3.],
       [ 4., 4., 4., 4.],
       [ 5., 5., 5., 5.],
       [ 6., 6., 6., 6.],
       [ 7., 7., 7., 7.]])
```
#### Lập Chỉ Mục Một Tập Con Các Hàng
Để chọn một tập hợp con của các hàng theo một thứ tự cụ thể, có thể truyền một danh sách hoặc ndarray các số nguyên để chỉ định thứ tự mong muốn. Ví dụ:

```python
print(arr[[4, 3, 0, 6]])
```
Kết quả:

```python
array([[ 4., 4., 4., 4.],
       [ 3., 3., 3., 3.],
       [ 0., 0., 0., 0.],
       [ 6., 6., 6., 6.]])
```
Sử Dụng Chỉ Số Âm
Sử dụng chỉ số âm sẽ chọn các hàng từ cuối mảng. Ví dụ:

```python
print(arr[[-3, -5, -7]])
```
Kết quả:

```python
array([[ 5., 5., 5., 5.],
       [ 3., 3., 3., 3.],
       [ 1., 1., 1., 1.]])
```
#### Lập Chỉ Mục Với Nhiều Mảng Chỉ Số
Khi truyền nhiều mảng chỉ số, quá trình sẽ chọn một mảng một chiều của các phần tử tương ứng với từng bộ chỉ số:

```python
arr = np.arange(32).reshape((8, 4))
print(arr)
```
Kết quả:

```python
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15],
       [16, 17, 18, 19],
       [20, 21, 22, 23],
       [24, 25, 26, 27],
       [28, 29, 30, 31]])
```
Tiếp theo, lập chỉ mục với nhiều mảng chỉ số:

```python
print(arr[[1, 5, 7, 2], [0, 3, 1, 2]])
```
Kết quả:

```python
array([ 4, 23, 29, 10])
```
Các phần tử (1, 0), (5, 3), (7, 1), và (2, 2) đã được chọn.

#### Lập Chỉ Mục Fancy với Lát Cắt
Hành vi của lập chỉ mục fancy có thể khác với những gì một số người dùng mong đợi. Để chọn một tập hợp con của các hàng và cột trong ma trận, có thể làm như sau:

```python
print(arr[[1, 5, 7, 2]][:, [0, 3, 1, 2]])
```
Kết quả:

```python
array([[ 4,  7,  5,  6],
       [20, 23, 21, 22],
       [28, 31, 29, 30],
       [ 8, 11,  9, 10]])
```
#### Lưu Ý về Fancy Indexing

Cần lưu ý rằng, khác với việc cắt (slicing), fancy indexing luôn sao chép dữ liệu vào một mảng mới. Điều này có nghĩa là kết quả của việc lập chỉ mục fancy không ảnh hưởng đến mảng gốc và sẽ được lưu trữ như một bản sao.

### Chuyển Vị Mảng và Hoán Đổi Trục trong NumPy

Chuyển vị mảng và hoán đổi trục là hai thao tác quan trọng trong NumPy giúp thay đổi cấu trúc mảng mà không cần phải sao chép dữ liệu. Chúng hỗ trợ việc thao tác với mảng đa chiều một cách hiệu quả.

#### Chuyển Vị Mảng

Chuyển vị mảng là phép thay đổi cấu trúc của mảng mà không sao chép dữ liệu, giúp tiết kiệm bộ nhớ và tăng hiệu suất. NumPy cung cấp phương thức `transpose()` và thuộc tính `T` để thực hiện chuyển vị.

Ví Dụ về Chuyển Vị:

Tạo một mảng 3x5 và chuyển vị nó:

```python
import numpy as np

# Tạo mảng 3x5
arr = np.arange(15).reshape((3, 5))
print(arr)
```
Kết quả:

```python
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])
```
Chuyển vị mảng bằng thuộc tính T:

```python
print(arr.T)
```
Kết quả:

```python
array([[ 0,  5, 10],
       [ 1,  6, 11],
       [ 2,  7, 12],
       [ 3,  8, 13],
       [ 4,  9, 14]])
```
#### Tính Toán Ma Trận
Chuyển vị rất hữu ích khi thực hiện các phép toán ma trận, chẳng hạn như tính toán tích ma trận. Ví dụ, sử dụng np.dot() để tính tích của mảng và chuyển vị của mảng:

```python
arr = np.random.randn(6, 3)
print(np.dot(arr.T, arr))
```
Kết quả

```python
array([[ 9.2291,  0.9394,  4.948 ],
       [ 0.9394,  3.7662, -1.3622],
       [ 4.948 , -1.3622,  4.3437]])
```
#### Hoán Đổi Trục
Đối với các mảng có nhiều chiều hơn, NumPy cho phép hoán đổi các trục của mảng bằng phương thức transpose(). Phương thức này nhận một bộ số trục và thay đổi thứ tự các trục.

Ví Dụ về Hoán Đổi Trục:
Tạo một mảng 2x2x4 và hoán đổi các trục:

```python
arr = np.arange(16).reshape((2, 2, 4))
print(arr)
```
Kết quả:

```python
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7]],

       [[ 8,  9, 10, 11],
        [12, 13, 14, 15]]])
```
Hoán đổi trục của mảng:

```python
print(arr.transpose((1, 0, 2)))
```
Kết quả:

```python
array([[[ 0,  1,  2,  3],
        [ 8,  9, 10, 11]],

       [[ 4,  5,  6,  7],
        [12, 13, 14, 15]]])
```
Trong ví dụ này, các trục đã được thay đổi thứ tự: trục thứ hai (axis 1) chuyển lên vị trí đầu tiên, trục đầu tiên (axis 0) xuống vị trí thứ hai, và trục cuối cùng (axis 2) không thay đổi.

#### Hoán Đổi Trục với swapaxes()
Phương thức swapaxes() cho phép hoán đổi hai trục trong mảng mà không sao chép dữ liệu. Đây là một cách hiệu quả để thay đổi trục mà không tốn thêm bộ nhớ.

Ví Dụ về swapaxes():

Hoán đổi trục 1 và 2:

```python
print(arr.swapaxes(1, 2))
```
Kết quả:

```python
array([[[ 0,  4],
        [ 1,  5],
        [ 2,  6],
        [ 3,  7]],

       [[ 8, 12],
        [ 9, 13],
        [10, 14],
        [11, 15]]])
```
Phương thức swapaxes() trả về một chế độ xem trên dữ liệu, tức là nó không tạo ra một bản sao mới mà thay đổi thứ tự các trục trực tiếp.

### Bảng 4-3. Ufunc đơn

| **Hàm**                       | **Mô tả**                                                                 |
|-------------------------------|---------------------------------------------------------------------------|
| `abs`, `fabs`                 | Tính giá trị tuyệt đối theo phần tử cho các giá trị số nguyên, số thực hoặc số phức |
| `sqrt`                        | Tính căn bậc hai của mỗi phần tử (tương đương với `arr ** 0.5`)         |
| `square`                      | Tính bình phương của mỗi phần tử (tương đương với `arr ** 2`)           |
| `exp`                         | Tính lũy thừa của `e` cho mỗi phần tử                                    |
| `log`, `log10`, `log2`, `log1p`| Log tự nhiên (cơ sở `e`), log cơ sở 10, log cơ sở 2 và `log(1 + x)`, tương ứng |
| `sign`                        | Tính dấu của mỗi phần tử: 1 (dương), 0 (không) hoặc -1 (âm)              |
| `ceil`                        | Tính trần của mỗi phần tử (tức là, số nguyên nhỏ nhất lớn hơn hoặc bằng số đó) |
| `floor`                       | Tính sàn của mỗi phần tử (tức là, số nguyên lớn nhất nhỏ hơn hoặc bằng mỗi phần tử) |
| `rint`                        | Làm tròn các phần tử về số nguyên gần nhất, giữ nguyên kiểu dữ liệu      |
| `modf`                        | Trả về các phần thập phân và nguyên của mảng dưới dạng một mảng riêng biệt |
| `isnan`                       | Trả về mảng boolean chỉ ra xem mỗi giá trị có phải là NaN (Not a Number) hay không |
| `isfinite`, `isinf`           | Trả về mảng boolean chỉ ra xem mỗi phần tử có hữu hạn (không phải vô cùng, không phải NaN) hoặc vô cùng, tương ứng |
| `cos`, `cosh`, `sin`, `sinh`, `tan`, `tanh` | Các công thức lượng giác thường và hyperbolic |
| `arccos`, `arccosh`, `arcsin`, `arcsinh`, `arctan`, `arctanh` | Các công thức lượng giác ngược |
| `logical_not`                 | Tính giá trị sự thật của `not x` theo phần tử (tương đương với `~arr`)   |

### Bảng 4-4. Hàm toàn cầu nhị phân

| **Hàm**                       | **Mô tả**                                                                 |
|-------------------------------|---------------------------------------------------------------------------|
| `add`                          | Cộng các phần tử tương ứng trong các mảng                                 |
| `subtract`                     | Trừ các phần tử trong mảng thứ hai từ mảng đầu tiên                       |
| `multiply`                     | Nhân các phần tử trong mảng                                              |
| `divide`, `floor_divide`       | Chia hoặc chia nguyên (cắt bỏ phần dư)                                   |
| `power`                        | Nâng các phần tử trong mảng đầu tiên lên các lũy thừa được chỉ định trong mảng thứ hai |
| `maximum`, `fmax`              | Giá trị lớn nhất theo phần tử; `fmax` bỏ qua NaN                        |
| `minimum`, `fmin`              | Giá trị nhỏ nhất theo phần tử; `fmin` bỏ qua NaN                        |
| `mod`                          | Phép chia theo phần tử (số dư của phép chia)                              |
| `copysign`                     | Sao chép dấu của các giá trị trong đối số thứ hai cho các giá trị trong đối số đầu tiên |
| `greater`, `greater_equal`, `less`, `less_equal`, `equal`, `not_equal` | Thực hiện so sánh theo phần tử, tạo ra mảng boolean (tương đương với các toán tử infix `>`, `>=`, `<`, `<=`, `==`, `!=`) |
| `logical_and`, `logical_or`, `logical_xor` | Tính giá trị sự thật theo phần tử của phép toán logic (tương đương với các toán tử infix `&`, `|`, `^`) |
### 4.3 Lập Trình Hướng Mảng với các Mảng

Việc sử dụng các mảng NumPy giúp biểu diễn nhiều tác vụ xử lý dữ liệu một cách ngắn gọn thông qua các biểu thức mảng, thay vì phải viết các vòng lặp phức tạp. Thực hành thay thế vòng lặp rõ ràng bằng các biểu thức mảng thường được gọi là **vector hóa**. Các phép toán mảng vector hóa thường nhanh hơn so với các phiên bản thuần Python, đặc biệt trong các phép toán số học, và có thể nhanh hơn nhiều lần.

#### Ví dụ về Vector hóa

Giả sử, ta muốn đánh giá hàm \(\sqrt{x^2 + y^2}\) trên một lưới giá trị đều. Để thực hiện điều này, ta có thể sử dụng hàm `np.meshgrid`, một hàm trong NumPy giúp tạo ra hai ma trận 2D từ hai mảng 1D, tương ứng với tất cả các cặp giá trị (x, y).

```python
import numpy as np

# Tạo một mảng giá trị đều từ -5 đến 5, bước nhảy 0.01
points = np.arange(-5, 5, 0.01)  # 1000 điểm cách đều

# Sử dụng meshgrid để tạo lưới (xs, ys)
xs, ys = np.meshgrid(points, points)

# In ra mảng ys
print(ys)
```
Kết quả của ys sẽ là một ma trận 2D, với tất cả các giá trị trong mỗi cột đều bằng giá trị của x tại cột đó.

```python
array([[-5. , -5. , -5. , ..., -5. , -5. , -5. ],
       [-4.99, -4.99, -4.99, ..., -4.99, -4.99, -4.99],
       [-4.98, -4.98, -4.98, ..., -4.98, -4.98, -4.98], 
       ..., 
       [ 4.97,  4.97,  4.97, ...,  4.97,  4.97,  4.97], 
       [ 4.98,  4.98,  4.98, ...,  4.98,  4.98,  4.98], 
       [ 4.99,  4.99,  4.99, ...,  4.99,  4.99,  4.99]])
```
Sau khi có lưới các điểm, ta có thể đánh giá hàm \(\sqrt{x^2 + y^2}\) bằng cách sử dụng biểu thức mảng vector hóa:

```python
# Đánh giá hàm sqrt(x^2 + y^2) trên lưới các điểm
z = np.sqrt(xs ** 2 + ys ** 2)

# In ra kết quả
print(z)
```
Kết quả sẽ là một ma trận 2D với giá trị của hàm 
\(\sqrt{x^2 + y^2}\)
  tại mỗi điểm trong lưới.

```python
array([[ 7.0711, 7.064 , 7.0569, ..., 7.0499, 7.0569, 7.064 ],
       [ 7.064 , 7.0569, 7.0499, ..., 7.0428, 7.0499, 7.0569], 
       [ 7.0569, 7.0499, 7.0428, ..., 7.0357, 7.0428, 7.0499], 
       ..., 
       [ 7.0499, 7.0428, 7.0357, ..., 7.0286, 7.0357, 7.0428], 
       [ 7.0569, 7.0499, 7.0428, ..., 7.0357, 7.0428, 7.0499], 
       [ 7.064 , 7.0569, 7.0499, ..., 7.0428, 7.0499, 7.0569]])
```
Minh họa với matplotlib
Để trực quan hóa kết quả trên, có thể sử dụng matplotlib để tạo biểu đồ hình ảnh từ mảng hai chiều z. Hàm imshow trong matplotlib giúp hiển thị mảng dưới dạng hình ảnh, với màu sắc thể hiện giá trị của các phần tử trong mảng.

```python
import matplotlib.pyplot as plt

# Hiển thị hình ảnh từ mảng z
plt.imshow(z, cmap=plt.cm.gray)
plt.colorbar()

# Thêm tiêu đề cho biểu đồ
plt.title("Biểu đồ hình ảnh của $\sqrt{x^2 + y^2}$ cho một lưới giá trị")
plt.show()
```
Kết quả sẽ là một hình ảnh biểu thị giá trị của 
\(\sqrt{x^2 + y^2}\) tại mỗi điểm trong lưới. Hình ảnh này giúp hình dung rõ ràng sự thay đổi giá trị của hàm theo không gian 2D.

### Biểu diễn Logic Điều kiện dưới dạng Các Phép Toán Mảng

Hàm `numpy.where` là phiên bản vector hóa của biểu thức ba ngôi, có cấu trúc `x if condition else y`. Giả sử ta có một mảng boolean và hai mảng giá trị sau:

```python
import numpy as np

xarr = np.array([1.1, 1.2, 1.3, 1.4, 1.5])
yarr = np.array([2.1, 2.2, 2.3, 2.4, 2.5])
cond = np.array([True, False, True, True, False])
```
Giả sử ta muốn lấy giá trị từ xarr nếu giá trị tương ứng trong cond là True, và lấy giá trị từ yarr nếu cond là False. Một cách làm thông thường sẽ như sau:

```python
result = [(x if c else y) for x, y, c in zip(xarr, yarr, cond)]
print(result)
```
Kết quả

```python
[1.1000000000000001, 2.2000000000000002, 1.3, 1.3999999999999999, 2.5]
```
Tuy nhiên, cách làm này có một số vấn đề:

Nó không nhanh chóng đối với các mảng lớn vì tất cả công việc đều được thực hiện trong mã Python thông dịch.
Nó không hoạt động với các mảng đa chiều.
Với np.where, ta có thể viết lại một cách ngắn gọn như sau:

```python
result = np.where(cond, xarr, yarr)
print(result)
```
Kết quả:

```python
array([ 1.1, 2.2, 1.3, 1.4, 2.5])
```
Các đối số thứ hai và thứ ba của np.where không nhất thiết phải là mảng; một trong số đó có thể là hằng số. Một ví dụ điển hình khi sử dụng np.where trong phân tích dữ liệu là để thay thế giá trị trong mảng. Ví dụ, giả sử bạn có một ma trận dữ liệu ngẫu nhiên và muốn thay thế tất cả các giá trị dương bằng 2 và tất cả các giá trị âm bằng -2:

```python
arr = np.random.randn(4, 4)
print(arr)
```
Kết quả:

```python
array([[-0.5031, -0.6223, -0.9212, -0.7262],
       [ 0.2229,  0.0513, -1.1577,  0.8167],
       [ 0.4336,  1.0107,  1.8249, -0.9975],
       [ 0.8506, -0.1316,  0.9124,  0.1882]])
```
Để xác định các giá trị dương, ta có thể kiểm tra điều kiện với arr > 0:

```python
arr > 0
```
Kết quả:

```python
array([[False, False, False, False],
       [ True,  True, False,  True],
       [ True,  True,  True, False],
       [ True, False,  True,  True]], dtype=bool)
```
Sau đó, ta có thể sử dụng np.where để thay thế các giá trị:

```python
np.where(arr > 0, 2, -2)
```
Kết quả:

```python
array([[-2, -2, -2, -2],
       [ 2,  2, -2,  2],
       [ 2,  2,  2, -2],
       [ 2, -2,  2,  2]])
```
Ngoài ra, ta cũng có thể thay thế tất cả các giá trị dương trong arr bằng một hằng số, ví dụ như 2:

```python
np.where(arr > 0, 2, arr)
```
Kết quả:

```python
array([[-0.5031, -0.6223, -0.9212, -0.7262],
       [ 2. ,    2. ,    -1.1577, 2. ],
       [ 2. ,    2. ,    2. ,   -0.9975],
       [ 2. ,   -0.1316, 2. ,    2. ]])
```
Các mảng được truyền vào np.where có thể không chỉ là các mảng có kích thước bằng nhau hoặc hằng số.
### Phương pháp Toán học và Thống kê

Một tập hợp các hàm toán học tính toán thống kê cho toàn bộ mảng hoặc cho dữ liệu theo một trục có sẵn, như các phương pháp của lớp mảng. Các phép toán tổng hợp (thường được gọi là giảm thiểu) như tổng, trung bình và độ lệch chuẩn có thể được tính toán bằng cách gọi phương thức của thể hiện mảng hoặc sử dụng hàm NumPy cấp cao.

Dưới đây là ví dụ về việc tạo dữ liệu ngẫu nhiên phân phối chuẩn và tính toán một số thống kê tổng hợp:

```python
import numpy as np

arr = np.random.randn(5, 4)
print(arr)
```
Kết quả:

```python
array([[ 2.1695, -0.1149,  2.0037,  0.0296],
       [ 0.7953,  0.1181, -0.7485,  0.585 ],
       [ 0.1527, -1.5657, -0.5625, -0.0327],
       [-0.929 , -0.4826, -0.0363,  1.0954],
       [ 0.9809, -0.5895,  1.5817, -0.5287]])
```
Tiếp theo, tính toán các giá trị thống kê như trung bình và tổng:

```python
arr.mean()
```
Kết quả:

```python
0.19607051119998253
```
Hoặc sử dụng hàm NumPy tương ứng:

```python
np.mean(arr)
```
Kết quả:

```python
0.19607051119998253
```
Tương tự với phép tính tổng:

```python
arr.sum()
```
Kết quả:

```python
3.9214102239996507
```
Các hàm như mean và sum có tham số trục tùy chọn cho phép tính toán thống kê theo trục nhất định, dẫn đến một mảng có một chiều giảm đi. Ví dụ:

```python
arr.mean(axis=1)
```
Kết quả:

```python
array([ 1.022 ,  0.1875, -0.502 , -0.0881,  0.3611])
```
Hoặc:

```python
arr.sum(axis=0)
```
Kết quả:

```python
array([ 3.1693, -2.6345,  2.2381,  1.1486])
```
Ở đây, arr.mean(1) có nghĩa là “tính trung bình theo chiều dọc” trong khi arr.sum(0) có nghĩa là “tính tổng theo chiều ngang.”

Các phương pháp khác như cumsum và cumprod không giảm thiểu, mà thay vào đó tạo ra một mảng chứa các kết quả trung gian:

```python
arr = np.array([0, 1, 2, 3, 4, 5, 6, 7])
arr.cumsum()
```
Kết quả:

```python
array([ 0, 1, 3, 6, 10, 15, 21, 28])
```
Trong các mảng đa chiều, các hàm dồn tích như cumsum trả về một mảng cùng kích thước, nhưng với các tích lũy được tính dọc theo trục chỉ định theo mỗi lát dimension thấp hơn:

```python
arr = np.array([[0, 1, 2], [3, 4, 5], [6, 7, 8]])
arr.cumsum(axis=0)
```
Kết quả:

```python
array([[ 0,  1,  2],
       [ 3,  5,  7],
       [ 9, 12, 15]])
```
Hoặc:

```python
arr.cumprod(axis=1)
```
Kết quả:

```python
array([[  0,   0,   0],
       [  3,  12,  60],
       [  6,  42, 336]])
```
### Bảng 4-5. Các phương pháp thống kê mảng cơ bản


| Phương pháp     | Mô tả                                                                 |
|-----------------|-----------------------------------------------------------------------|
| `sum`           | Tổng của tất cả các phần tử trong mảng hoặc theo một trục; mảng có độ dài bằng không có tổng bằng 0 |
| `mean`          | Trung bình số học; mảng có độ dài bằng không có trung bình NaN      |
| `std`, `var`    | Độ lệch chuẩn và phương sai, tương ứng, với điều chỉnh độ tự do tùy chọn (mặc định mẫu chia n) |
| `min`, `max`    | Giá trị nhỏ nhất và lớn nhất                                         |
| `argmin`, `argmax` | Chỉ số của các phần tử nhỏ nhất và lớn nhất, tương ứng             |
| `cumsum`        | Tổng tích lũy của các phần tử bắt đầu từ 0                           |
| `cumprod`       | Tích tích lũy của các phần tử bắt đầu từ 1                           |
      
### Phương pháp cho Mảng Boolean

Các giá trị boolean được chuyển đổi thành 1 (Đúng) và 0 (Sai) trong các phương pháp trước đó. Do đó, hàm tổng thường được sử dụng như một phương tiện để đếm các giá trị Đúng trong mảng boolean:

```python
arr = np.random.randn(100)
(arr > 0).sum()  # Số lượng giá trị dương
```
Kết quả: 
```python
42
```
Có hai phương pháp bổ sung, any và all, đặc biệt hữu ích cho các mảng boolean.

any kiểm tra xem một hoặc nhiều giá trị trong mảng có phải là Đúng hay không, trong khi all kiểm tra xem mọi giá trị có phải là Đúng hay không:

```python
bools = np.array([False, False, True, False])
bools.any()
```
Kết quả: 
```python
True
```
```python
bools.all()
```
Kết quả:

```python
 False
```
Các phương pháp này cũng hoạt động với các mảng không phải boolean, trong đó các phần tử khác không bằng 0 được đánh giá là Đúng.

### Sắp xếp

Giống như kiểu danh sách tích hợp trong Python, mảng NumPy có thể được sắp xếp tại chỗ bằng phương thức `sort`:

```python
arr = np.random.randn(6)
arr
```
Kết quả: 
```python
array([ 0.6095, -0.4938, 1.24 , -0.1357, 1.43 , -0.8469])
```
```python
arr.sort()
arr
```
Kết quả:
```python
 array([-0.8469, -0.4938, -0.1357, 0.6095, 1.24 , 1.43 ])
```

Có thể sắp xếp từng phần một chiều của các giá trị trong một mảng đa chiều tại chỗ theo một trục bằng cách truyền số trục vào sort:

```python
arr = np.random.randn(5, 3)
arr
```
Kết quả:

```css
array([[ 0.6033, 1.2636, -0.2555],  
       [-0.4457, 0.4684, -0.9616],  
       [-1.8245, 0.6254, 1.0229],  
       [ 1.1074, 0.0909, -0.3501],  
       [ 0.218 , -0.8948, -1.7415]])

```
```python
arr.sort(1)
arr
```
Kết quả:

```css
array([[-0.2555, 0.6033, 1.2636],  
       [-0.9616, -0.4457, 0.4684],  
       [-1.8245, 0.6254, 1.0229],  
       [-0.3501, 0.0909, 1.1074],  
       [-1.7415, -0.8948, 0.218 ]])
```
Phương thức cấp đầu np.sort trả về một bản sao đã sắp xếp của một mảng thay vì sửa đổi mảng tại chỗ. Một cách nhanh chóng để tính toán các phân vị của một mảng là sắp xếp nó và chọn giá trị ở một hạng mục cụ thể:

```python
large_arr = np.random.randn(1000)
large_arr.sort()
large_arr[int(0.05 * len(large_arr))]  # phân vị 5%
```
Kết quả:
```    
 -1.5311513550102103
```

Để biết thêm chi tiết về cách sử dụng các phương thức sắp xếp của NumPy, cùng với các kỹ thuật nâng cao hơn như sắp xếp gián tiếp, hãy xem Phụ lục A. Một số loại thao tác dữ liệu khác liên quan đến việc sắp xếp (ví dụ, sắp xếp một bảng dữ liệu theo một hoặc nhiều cột) cũng có thể được tìm thấy trong pandas.

### Logic Tập Hợp Đặc Biệt và Khác

NumPy có một số thao tác tập hợp cơ bản cho các ndarray một chiều. Một trong những thao tác thường được sử dụng là `np.unique`, trả về các giá trị duy nhất đã được sắp xếp trong một mảng:

```python
names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])
np.unique(names)
```
Kết quả:

```pyython
array(['Bob', 'Joe', 'Will'], dtype='<U4')
```
```python
ints = np.array([3, 3, 3, 2, 2, 1, 1, 4, 4])
np.unique(ints)
```
Kết quả:

```scss
array([1, 2, 3, 4])
```
So sánh với lựa chọn thuần túy bằng Python:

```python
sorted(set(names))
```
Kết quả:

```css
['Bob', 'Joe', 'Will']
```
Một hàm khác, np.in1d, kiểm tra thành viên của các giá trị trong một mảng trong mảng khác, trả về một mảng boolean:

```python
values = np.array([6, 0, 0, 3, 2, 5, 6])
np.in1d(values, [2, 3, 6])
```
Kết quả:

```python
array([ True, False, False, True, True, False, True], dtype=bool)
```
### Bảng 4-6. Các thao tác tập hợp mảng

| Phương pháp          | Mô tả                                                                 |
|----------------------|-----------------------------------------------------------------------|
| `unique(x)`          | Tính toán các phần tử duy nhất, đã được sắp xếp trong `x`           |
| `intersect1d(x, y)`  | Tính toán các phần tử chung đã được sắp xếp trong `x` và `y`       |
| `union1d(x, y)`      | Tính toán tổng hợp đã được sắp xếp của các phần tử                   |
| `in1d(x, y)`         | Tính toán một mảng boolean chỉ ra liệu mỗi phần tử của `x` có nằm trong `y` hay không |
| `setdiff1d(x, y)`    | Hiệu tập, các phần tử trong `x` mà không nằm trong `y`              |
| `setxor1d(x, y)`     | Sự khác biệt đối xứng của tập; các phần tử có trong một trong hai mảng, nhưng không phải cả hai |





