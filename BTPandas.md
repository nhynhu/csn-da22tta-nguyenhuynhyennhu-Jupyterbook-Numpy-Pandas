# Cài đặt và tải dữ liệu
```python 
import math
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import warnings
warnings.filterwarnings("ignore")

pd.options.display.max_rows = 100
pd.options.display.max_columns = 50

dtype = {
    'customer_id': str,
    'gender_cd': str,
    'postal_cd': str,
    'application_store_cd': str,
    'status_cd': str,
    'category_major_cd': str,
    'category_medium_cd': str,
    'category_small_cd': str,
    'product_cd': str,
    'store_cd': str,
    'prefecture_cd': str,
    'tel_no': str,
    'postal_cd': str,
    'street': str
}

df_customer = pd.read_csv("../../data/preprocessed/customer_eng.csv", dtype=dtype)
df_category = pd.read_csv("../../data/preprocessed/category_eng.csv", dtype=dtype)
df_product = pd.read_csv("../../data/preprocessed/product.csv", dtype=dtype)
df_receipt = pd.read_csv("../../data/preprocessed/receipt.csv", dtype=dtype)
df_store = pd.read_csv("../../data/preprocessed/store_eng.csv", dtype=dtype)
df_geocode = pd.read_csv("../../data/preprocessed/geocode_eng.csv", dtype=dtype)
```
# Bài Tập
P-001: Từ dữ liệu biên nhận (df_receipt), hiển thị 10 bản ghi đầu tiên và kiểm tra xem loại dữ liệu nào được lưu trữ trong các cột.
```pythonpython
df_receipt.head(10)
```
```
sales_ymd	sales_epoch	store_cd	receipt_no	receipt_sub_no	customer_id	product_cd	quantity	amount
0	20181103	1541203200	S14006	112	1	CS006214000001	P070305012	1	158
1	20181118	1542499200	S13008	1132	2	CS008415000097	P070701017	1	81
2	20170712	1499817600	S14028	1102	1	CS028414000014	P060101005	1	170
3	20190205	1549324800	S14042	1132	1	ZZ000000000000	P050301001	1	25
4	20180821	1534809600	S14025	1102	2	CS025415000050	P060102007	1	90
5	20190605	1559692800	S13003	1112	1	CS003515000195	P050102002	1	138
6	20181205	1543968000	S14024	1102	2	CS024514000042	P080101005	1	30
7	20190922	1569110400	S14040	1102	1	CS040415000178	P070501004	1	128
8	20170504	1493856000	S13020	1112	2	ZZ000000000000	P071302010	1	770
9	20191010	1570665600	S14027	1102	1	CS027514000015	P071101003	1	680
```
```python 
df_receipt.info()
```
```python 
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 104681 entries, 0 to 104680
Data columns (total 9 columns):
 #   Column          Non-Null Count   Dtype 
---  ------          --------------   ----- 
 0   sales_ymd       104681 non-null  int64 
 1   sales_epoch     104681 non-null  int64 
 2   store_cd        104681 non-null  object
 3   receipt_no      104681 non-null  int64 
 4   receipt_sub_no  104681 non-null  int64 
 5   customer_id     104681 non-null  object
 6   product_cd      104681 non-null  object
 7   quantity        104681 non-null  int64 
 8   amount          104681 non-null  int64 
dtypes: int64(6), object(3)
memory usage: 7.2+ MB
```
P-002: Từ dữ liệu biên nhận (df_receipt), chỉ định các cột theo thứ tự sau và hiển thị 10 bản ghi: ngày bán hàng (sales_ymd), ID khách hàng (customer_id), mã sản phẩm (product_cd), số tiền bán hàng (amount).
```python 
df_receipt[["sales_ymd", "customer_id", "product_cd"]].head(10)
```
```python 
sales_ymd	customer_id	product_cd
0	20181103	CS006214000001	P070305012
1	20181118	CS008415000097	P070701017
2	20170712	CS028414000014	P060101005
3	20190205	ZZ000000000000	P050301001
4	20180821	CS025415000050	P060102007
5	20190605	CS003515000195	P050102002
6	20181205	CS024514000042	P080101005
7	20190922	CS040415000178	P070501004
8	20170504	ZZ000000000000	P071302010
9	20191010	CS027514000015	P071101003
```
P-003: Từ dữ liệu biên nhận (df_receipt), chỉ định các cột theo thứ tự sau và hiển thị 10 bản ghi: ngày bán hàng (sales_ymd), ID khách hàng (customer_id), mã sản phẩm (product_cd), số tiền bán hàng (amount). Tuy nhiên, đổi tên cột sales_ymd thành sales_date trước khi hiển thị.
```python 
(
    df_receipt[["sales_ymd", "customer_id", "product_cd", "amount"]]
    .rename(columns={"sales_ymd": "sales_date"})
).head()
```
```python 
sales_date	customer_id	product_cd	amount
0	20181103	CS006214000001	P070305012	158
1	20181118	CS008415000097	P070701017	81
2	20170712	CS028414000014	P060101005	170
3	20190205	ZZ000000000000	P050301001	25
4	20180821	CS025415000050	P060102007	90
```
P-004: Từ dữ liệu biên nhận (df_receipt), chỉ định các cột theo thứ tự sau và trích xuất dữ liệu đáp ứng các điều kiện dưới đây: ngày bán hàng (sales_ymd), ID khách hàng (customer_id), mã sản phẩm (product_cd), số tiền bán hàng (amount).

- ID khách hàng (customer_id) là "CS018205000001"
```python 
(
    df_receipt[["sales_ymd", "customer_id", "product_cd", "amount"]]
    .query("customer_id == 'CS018205000001'")
)
```
```python 
sales_ymd	customer_id	product_cd	amount
36	20180911	CS018205000001	P071401012	2200
9843	20180414	CS018205000001	P060104007	600
21110	20170614	CS018205000001	P050206001	990
27673	20170614	CS018205000001	P060702015	108
27840	20190216	CS018205000001	P071005024	102
28757	20180414	CS018205000001	P071101002	278
39256	20190226	CS018205000001	P070902035	168
58121	20190924	CS018205000001	P060805001	495
68117	20190226	CS018205000001	P071401020	2200
72254	20180911	CS018205000001	P071401005	1100
88508	20190216	CS018205000001	P040101002	218
91525	20190924	CS018205000001	P091503001	280
```
P-005: Từ dữ liệu biên nhận (df_receipt), chỉ định các cột theo thứ tự sau và trích xuất dữ liệu đáp ứng tất cả các điều kiện dưới đây: ngày bán hàng (sales_ymd), ID khách hàng (customer_id), mã sản phẩm (product_cd), số tiền bán hàng (amount).

- ID khách hàng (customer_id) là "CS018205000001"
- Số tiền bán hàng (amount) lớn hơn hoặc bằng 1000
```python 
(
    df_receipt[["sales_ymd", "customer_id", "product_cd", "amount"]]
    .query("(customer_id == 'CS018205000001') & (amount >= 1000)")
)
```
```python 
sales_ymd	customer_id	product_cd	amount
36	20180911	CS018205000001	P071401012	2200
68117	20190226	CS018205000001	P071401020	2200
72254	20180911	CS018205000001	P071401005	1100
```
P-006: Từ dữ liệu biên nhận (df_receipt), chỉ định các cột theo thứ tự sau và trích xuất dữ liệu đáp ứng tất cả các điều kiện dưới đây: ngày bán hàng (sales_ymd), ID khách hàng (customer_id), mã sản phẩm (product_cd), số lượng bán hàng (quantity), số tiền bán hàng (amount).
- ID khách hàng (customer_id) là "CS018205000001"
- Số tiền bán hàng (amount) lớn hơn hoặc bằng 1000 hoặc số lượng bán hàng (quantity) lớn hơn hoặc bằng 5
```python 
(
    df_receipt[["sales_ymd", "customer_id", "product_cd", "quantity", "amount"]]
    .query("(customer_id == 'CS018205000001') & (amount >= 1000 | quantity >=5)")
)
```
```python 
sales_ymd	customer_id	product_cd	quantity	amount
36	20180911	CS018205000001	P071401012	1	2200
9843	20180414	CS018205000001	P060104007	6	600
21110	20170614	CS018205000001	P050206001	5	990
68117	20190226	CS018205000001	P071401020	1	2200
72254	20180911	CS018205000001	P071401005	1	1100
```
P-007: Từ dữ liệu biên nhận (df_receipt), chỉ định các cột theo thứ tự sau và trích xuất dữ liệu đáp ứng tất cả các điều kiện dưới đây: ngày bán hàng (sales_ymd), ID khách hàng (customer_id), mã sản phẩm (product_cd), số tiền bán hàng (amount).

- ID khách hàng (customer_id) là "CS018205000001"
- Số tiền bán hàng (amount) lớn hơn hoặc bằng 1000 và nhỏ hơn hoặc bằng 2000
```python 
# (
#     df_receipt[["sales_ymd", "customer_id", "product_cd", "amount"]]
#     .query("(customer_id == 'CS018205000001') & (amount >= 1000 & amount <= 2000)")
# )

(
    df_receipt[["sales_ymd", "customer_id", "product_cd", "amount"]]
    .query("(customer_id == 'CS018205000001') & (1000 <= amount <= 2000)")
)
```
```python 
sales_ymd	customer_id	product_cd	amount
72254	20180911	CS018205000001	P071401005	1100
```
P-008: Từ dữ liệu biên nhận (df_receipt), chỉ định các cột theo thứ tự sau và trích xuất dữ liệu đáp ứng tất cả các điều kiện dưới đây: ngày bán hàng (sales_ymd), ID khách hàng (customer_id), mã sản phẩm (product_cd), số tiền bán hàng (amount).

- ID khách hàng (customer_id) là "CS018205000001"
- Mã sản phẩm (product_cd) không phải là "P071401019"
```python 
(
    df_receipt[["sales_ymd", "customer_id", "product_cd", "amount"]]
    .query("(customer_id == 'CS018205000001') & (product_cd != 'P071401019')")
)
```
```python 
sales_ymd	customer_id	product_cd	amount
36	20180911	CS018205000001	P071401012	2200
9843	20180414	CS018205000001	P060104007	600
21110	20170614	CS018205000001	P050206001	990
27673	20170614	CS018205000001	P060702015	108
27840	20190216	CS018205000001	P071005024	102
28757	20180414	CS018205000001	P071101002	278
39256	20190226	CS018205000001	P070902035	168
58121	20190924	CS018205000001	P060805001	495
68117	20190226	CS018205000001	P071401020	2200
72254	20180911	CS018205000001	P071401005	1100
88508	20190216	CS018205000001	P040101002	218
91525	20190924	CS018205000001	P091503001	280
```
