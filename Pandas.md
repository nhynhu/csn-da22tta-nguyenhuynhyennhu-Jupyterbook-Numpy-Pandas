# Giới Thiệu Về pandas

**pandas** là một công cụ quan trọng trong việc làm sạch và phân tích dữ liệu, được sử dụng rộng rãi trong Python. Thư viện này cung cấp các cấu trúc dữ liệu và công cụ xử lý dữ liệu giúp việc phân tích trở nên nhanh chóng và hiệu quả. **pandas** thường được kết hợp với các công cụ tính toán số học như **NumPy** và **SciPy**, các thư viện phân tích như **statsmodels** và **scikit-learn**, cũng như các thư viện trực quan hóa dữ liệu như **matplotlib**. 

Một trong những điểm đặc biệt của **pandas** là kế thừa nhiều phương pháp lập trình từ **NumPy**, đặc biệt là việc xử lý dữ liệu theo kiểu mảng và hạn chế việc sử dụng vòng lặp. Tuy nhiên, sự khác biệt lớn nhất là **pandas** được thiết kế để làm việc với dữ liệu dạng bảng (tabular) hoặc dữ liệu không đồng nhất, trong khi **NumPy** chủ yếu làm việc với dữ liệu mảng số học đồng nhất.

Kể từ khi trở thành một dự án mã nguồn mở vào năm 2010, **pandas** đã phát triển mạnh mẽ và được sử dụng rộng rãi trong các ứng dụng thực tế. Cộng đồng phát triển của **pandas** hiện đã có hơn 800 người đóng góp, giúp thư viện này ngày càng hoàn thiện để giải quyết các bài toán dữ liệu phức tạp.

Trong sách này, quy ước nhập khẩu **pandas** sẽ được sử dụng như sau:
```python
import pandas as pd
```
Điều này có nghĩa là khi thấy pd. trong mã nguồn, đó là tham chiếu đến thư viện pandas. Các lớp Series và DataFrame cũng có thể được nhập khẩu vào không gian tên cục bộ vì chúng thường xuyên được sử dụng trong các bài toán xử lý dữ liệu:

```python
from pandas import Series, DataFrame
```