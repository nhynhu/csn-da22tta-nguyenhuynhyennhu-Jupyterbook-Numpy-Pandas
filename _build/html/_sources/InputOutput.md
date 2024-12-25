# Nhập và Xuất Tệp với Mảng NumPy

NumPy cung cấp các phương thức để lưu trữ và tải dữ liệu từ đĩa dưới định dạng văn bản hoặc nhị phân. Trong phần này, chỉ tập trung vào định dạng nhị phân của NumPy, vì hầu hết người dùng sẽ sử dụng các công cụ như pandas để tải dữ liệu văn bản hoặc dữ liệu dạng bảng (xem Chương 6 để biết thêm chi tiết).

## Lưu và Tải Mảng với NumPy

- **Lưu mảng**: Dùng `np.save` để lưu mảng vào tệp với định dạng nhị phân `.npy`. Nếu tên tệp không có đuôi `.npy`, NumPy sẽ tự động thêm vào.

- **Tải mảng**: Dùng `np.load` để tải mảng đã lưu từ tệp.

Ví dụ:

```python
import numpy as np

# Tạo một mảng
arr = np.arange(10)

# Lưu mảng vào tệp .npy
np.save('some_array', arr)

# Tải mảng từ tệp .npy
loaded_arr = np.load('some_array.npy')
print(loaded_arr)  # Output: [0 1 2 3 4 5 6 7 8 9]
```
Lưu nhiều mảng: Dùng np.savez để lưu nhiều mảng vào một tệp nén .npz. Các mảng được truyền dưới dạng các đối số từ khóa.

```python
# Lưu nhiều mảng vào một tệp .npz
np.savez('array_archive.npz', a=arr, b=arr)

# Tải tệp .npz và truy xuất các mảng
arch = np.load('array_archive.npz')
print(arch['b'])  # Output: [0 1 2 3 4 5 6 7 8 9]
```
Lưu mảng nén: Nếu dữ liệu có thể nén tốt, có thể sử dụng np.savez_compressed để lưu mảng trong định dạng nén.
```python
# Lưu mảng trong tệp nén .npz
np.savez_compressed('arrays_compressed.npz', a=arr, b=arr)
```