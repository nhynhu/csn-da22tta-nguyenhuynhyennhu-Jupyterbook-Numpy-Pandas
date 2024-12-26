# Định Dạng Dữ Liệu Nhị Phân

Một trong những cách dễ dàng nhất để lưu trữ dữ liệu (còn được gọi là tuần tự hóa) hiệu quả trong định dạng nhị phân là sử dụng tuần tự hóa pickle tích hợp của Python. Các đối tượng pandas đều có phương thức `to_pickle` để ghi dữ liệu ra đĩa dưới định dạng pickle:

```python
In [87]: frame = pd.read_csv('examples/ex1.csv')
In [88]: frame
Out[88]:
   a  b   c   d message
0  1  2   3   4   hello
1  5  6   7   8   world
2  9 10  11  12     foo
In [89]: frame.to_pickle('examples/frame_pickle')
```
Bạn có thể đọc bất kỳ đối tượng "pickled" nào được lưu trữ trong một tệp bằng cách sử dụng pickle tích hợp trực tiếp, hoặc thậm chí tiện lợi hơn bằng cách sử dụng pandas.read_pickle:

```python
In [90]: pd.read_pickle('examples/frame_pickle')
Out[90]:
   a  b   c   d message
0  1  2   3   4   hello
1  5  6   7   8   world
2  9 10  11  12     foo
```
pickle chỉ được khuyến nghị sử dụng như một định dạng lưu trữ ngắn hạn. Vấn đề là khó có thể đảm bảo rằng định dạng này sẽ ổn định theo thời gian; một đối tượng pickled hôm nay có thể không unpickle với phiên bản thư viện sau này. Chúng tôi đã cố gắng duy trì tương thích ngược khi có thể, nhưng vào một thời điểm nào đó trong tương lai có thể cần phải "phá vỡ" định dạng pickle.

pandas có hỗ trợ tích hợp cho hai định dạng dữ liệu nhị phân khác: HDF5 và MessagePack. Tôi sẽ đưa ra một số ví dụ về HDF5 trong phần tiếp theo, nhưng tôi khuyến khích bạn khám phá các định dạng tệp khác để xem chúng nhanh như thế nào và hoạt động tốt như thế nào cho phân tích của bạn. Một số định dạng lưu trữ khác cho dữ liệu pandas hoặc NumPy bao gồm:

bcolz: Một định dạng nhị phân hướng cột có khả năng nén dựa trên thư viện nén Blosc.

Feather: Một định dạng tệp hướng cột ngôn ngữ chéo được tôi thiết kế cùng với cộng đồng lập trình R của Hadley Wickham. Feather sử dụng định dạng bộ nhớ cột Apache Arrow.

## Sử dụng Định Dạng HDF5

HDF5 là một định dạng tệp được đánh giá cao, được thiết kế để lưu trữ lượng lớn dữ liệu mảng khoa học. Đây là một thư viện C có các giao diện sẵn có ở nhiều ngôn ngữ khác, bao gồm Java, Julia, MATLAB và Python. "HDF" trong HDF5 là viết tắt của định dạng dữ liệu phân cấp. Mỗi tệp HDF5 có thể lưu trữ nhiều tập dữ liệu và siêu dữ liệu hỗ trợ. So với các định dạng đơn giản hơn, HDF5 hỗ trợ nén tại chỗ với nhiều chế độ nén khác nhau, cho phép lưu trữ dữ liệu với các mẫu lặp lại một cách hiệu quả hơn. HDF5 có thể là một lựa chọn tốt cho việc làm việc với các tập dữ liệu rất lớn không phù hợp với bộ nhớ, vì bạn có thể đọc và ghi các phần nhỏ của các mảng lớn hơn một cách hiệu quả.

Mặc dù có thể truy cập trực tiếp các tệp HDF5 bằng cách sử dụng thư viện PyTables hoặc h5py, pandas cung cấp một giao diện cấp cao giúp đơn giản hóa việc lưu trữ các đối tượng Series và DataFrame. Lớp HDFStore hoạt động như một dict và xử lý các chi tiết cấp thấp:

```python
In [92]: frame = pd.DataFrame({'a': np.random.randn(100)})
In [93]: store = pd.HDFStore('mydata.h5')
In [94]: store['obj1'] = frame
In [95]: store['obj1_col'] = frame['a']
In [96]: store
Out[96]:
<class 'pandas.io.pytables.HDFStore'>
File path: mydata.h5
/obj1            frame        (shape->[100,1])
/obj1_col        series       (shape->[100])
```
Các đối tượng chứa trong tệp HDF5 sau đó có thể được truy xuất bằng cùng một API giống như dict:

```python
In [97]: store['obj1']
Out[97]:
            a
0   -0.204708
1    0.478943
2   -0.519439
3   -0.555730
4    1.965781
..        ...
95   0.795253
96   0.118110
97  -0.748532
98   0.584970
99   0.152677
[100 rows x 1 columns]
```
HDFStore hỗ trợ hai lược đồ lưu trữ, 'fixed' và 'table'. 'table' thường chậm hơn, nhưng nó hỗ trợ các thao tác truy vấn sử dụng cú pháp đặc biệt:

```python
In [98]: store.put('obj2', frame, format='table')
In [99]: store.select('obj2', where=['index >= 10 and index <= 15'])
Out[99]:
            a
10   1.007189
11  -1.296221
12   0.274992
13   0.228913
14   1.352917
15   0.886429
```
Hàm put là một phiên bản tường minh của phương thức store['obj2'] = frame, nhưng cho phép đặt các tùy chọn khác như định dạng lưu trữ.

Hàm pandas.read_hdf cung cấp cho bạn một lối tắt đến các công cụ này:

```python
In [101]: frame.to_hdf('mydata.h5', 'obj3', format='table')
In [102]: pd.read_hdf('mydata.h5', 'obj3', where=['index < 5'])
Out[102]:
            a
0   -0.204708
1    0.478943
2   -0.519439
3   -0.555730
4    1.965781
```