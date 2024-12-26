# Bài tập lập trình Python - Numpy
Hướng dẫn:
Cố gắng không sử dụng các vòng lặp (for, while).
Không chỉnh sửa các tên hàm.
Sau khi bạn viết Code của mình xong, hãy chạy dòng Code đó để xem kết quả bên dưới.


## Import thư viện numpy
```pythonpython
import numpy as np
```
Câu 1: Thay thế tất cả các phần tử trong một mảng có giá trị lớn hơn 30 thành 30 và dưới 10 thành 10.

Gợi ý: sử dụng np.where
```python 
# input: [28. 15. 22. 42.  1.  7. 34. 41.  8. 29.]
# output: [28. 15. 22. 30. 10. 10. 30. 30. 10. 29.]
```
```python 
ex1 = np.array([28, 15, 22, 42,  1,  7, 34, 41,  8, 29], dtype=np.float64)
```
```python 
# Trả về những phần tử < 30; những phần tử > 30 thay bằng 30

### START CODE HERE ### (≈ 1 line of code)
ex1 = None
### END CODE HERE ###


# Trả về những phần tử > 10; những phần tử < 10 thay bằng 10

### START CODE HERE ### (≈ 1 line of code)
ex1 = None
### END CODE HERE ###

print(ex1)
```
Đầu ra kỳ vọng:
```python 
[28. 15. 22. 30. 10. 10. 30. 30. 10. 29.]
```
Câu 2: Lấy vị trí của 3 giá trị lớn nhất trong một mảng numpy

Gợi ý: sử dụng np.argpartition 
```python 
# input: [28. 15. 22. 42.  1.  7. 34. 41.  8. 29.]
# output: [6 7 3]
```
```python 
ex2 = np.array([28, 15, 22, 42,  1,  7, 34, 41,  8, 29], dtype=np.float64)
```
```python 
# Sử dụng `np.argpartition` 
### START CODE HERE ### (≈ 1 line of code)
output = None
### END CODE HERE ###

print(output)
```
Đầu ra kỳ vọng:
```python 
[6 3 7]
```
Câu 3: Chuyển một mảng nhiều chiều thành một mảng 1 chiều
```python 
# input: [array([0, 1, 2]) array([3, 4, 5, 6]) array([7, 8, 9])]
# output: [0 1 2 3 4 5 6 7 8 9]
```
```python 
ex3 = np.array([
    [0, 1, 2],
    [3, 4, 5],
    [7, 8, 9]
])
```
```python 
### START CODE HERE ### (≈ 1 line of code)
output = None
### END CODE HERE ###

print(output)
```
Đầu ra kỳ vọng:
```python 
[0 1 2 3 4 5 7 8 9]
```
Câu 4: Tạo one-hot encoding cho một mảng numpy. One-hot encoding nhằm chuyển đổi mỗi giá trị (số nguyên) n thành một vector v mà vị trí thứ n trong vector v mang giá trị 1 và tất cả vị trí khác đều mang giá trị 0.
```python 
# input: [2 1 3 3 1 2]
# output: array([[0., 1., 0.],
#               [1., 0., 0.],
#               [0., 0., 1.],
#               [0., 0., 1.],
#               [1., 0., 0.],
#               [0., 1., 0.]])
```
```
ex4 = np.array([2, 1, 3, 3, 1, 2])
### START CODE HERE ### 

None # Các bạn hãy viết thuật toán của mình vào đây !

### END CODE HERE ###
```
Câu 5: Sắp xếp các phần tử trong một mảng 1 chiều
```python
# input: [3 6 4 8]
# output: [3 4 6 8]
```
```python
ex5 = np.array([3, 6, 4, 8])
### START CODE HERE ### (≈ 1 line of code)
output = None
### END CODE HERE ###

print(output)
```
Đầu ra kỳ vọng:
```python 
[3 4 6 8]
```
Câu 6: Tìm giá trị lớn nhất trong mỗi hàng và mỗi cột của một mảng 2d
```python 
# input
# [[5 1 7]
#  [3 5 2]
#  [6 4 5]]
 
# output
# Max by column
# [6 5 7]
# Max by row
# [7 5 6]
```
```python 
ex6 = np.array([
    [5, 1, 7],
    [3, 5, 2],
    [6, 4, 5]
])
```
```python 
# Max by column
### START CODE HERE ### (≈ 1 line of code)
output = None
### END CODE HERE ###

print(output)
```
Đầu ra kỳ vọng:
```python 
[6, 5, 7]
```
```python 
# Max by row
### START CODE HERE ### (≈ 1 line of code)
output = None
### END CODE HERE ###

print(output)
```
Đầu ra kỳ vọng:
```python 
[7, 5, 6]
```
Câu 7: Tìm các phần tử trùng lặp (lần xuất hiện thứ 2 trở đi) trong mảng đã cho và đánh dấu chúng là True. Lần đầu tiên xuất hiện là False.
```python 
# [0 3 3 1 2 0 2 0]
# [False False  True False False  True  True  True]
```
```python 
ex7 = np.array([0, 3, 3, 1, 2, 0, 2, 0])
### START CODE HERE ### 

None # Các bạn hãy viết thuật toán của mình vào đây !

### END CODE HERE ###
```
Đầu ra kỳ vọng:
```python 
[False False True False False True True True]
```
Câu 8: Trừ theo dòng mảng 2 chiều arr2d bằng mảng 1 chiều arr1d
```python 
# arr2d
# [[3 3 3]
#  [4 4 4]
#  [5 5 5]]
# arr1d
# [1 1 1]
# output
# [[2 2 2]
#  [3 3 3]
#  [4 4 4]]
```
```python 
arr2d = np.array([
    [3, 3, 3],
    [4, 4, 4],
    [5, 5, 5]
])

arr1d = np.array([1, 1, 1])
```
```python 
### START CODE HERE ### (≈ 1 line of code)
output = None
### END CODE HERE ###

print(output)
```
Đầu ra kỳ vọng:
```python 
[[2 2 2]
[3 3 3]
[4 4 4]]
```
Câu 9: Bỏ tất cả các giá trị nan từ một mảng numpy
```python 
#  input: [1.  2.  3. nan  5.  6.  7. nan]
# output: [1., 2., 3., 5., 6., 7.]
```
```python 
from numpy import nan
ex9 = np.array([1, 2, 3, nan, 5, 6, 7, nan], dtype=np.float64)
print(ex9)
```
```python 
### START CODE HERE ### (≈ 1 line of code)
output = None
### END CODE HERE ###

print(output)
```
Đầu ra kỳ vọng:
```python 
[1. 2. 3. 5. 6. 7.]
```
Câu 10: Lấy tất cả vị trí nơi các phần tử có giá trị khác nhau
```python 
# input
# arr1: [3 4 5 6 7 8]
# arr2: [3 3 6 6 7 7]
# output
# [1, 2, 5]
```
```python 
arr1 = np.array([3, 4, 5, 6, 7, 8])
arr2 = np.array([3, 3, 6, 6, 7, 7])
```
```python 
### START CODE HERE ### (≈ 1 line of code)
output = None
### END CODE HERE ###

print(output)
```
Đầu ra kỳ vọng:
```python 
[1 2 5]
```