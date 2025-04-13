# Phân tích Dữ liệu Kinh Doanh - Sales Analysis

## Mục lục

- [Giới thiệu](#giới-thiệu)
- [Cài đặt](#cài-đặt)
- [Sử dụng](#sử-dụng)
- [Cấu trúc thư mục](#cấu-trúc-thư-mục)
- [Phân tích dữ liệu](#phân-tích-dữ-liệu)
  - [Question 1: Tháng nào kinh doanh tốt nhất? Doanh thu tháng đó là bao nhiêu?](#question-1-tháng-nào-kinh-doanh-tốt-nhất-doanh-thu-tháng-đó-là-bao-nhiêu)
  - [Question 2: Thành phố nào bán được nhiều sản phẩm (product) nhất?](#question-2-thành-phố-nào-bán-được-nhiều-sản-phẩm-product-nhất)
  - [Question 3: Khoảng thời gian nào ta nên hiển thị quảng cáo để tăng khả năng mua hàng của khách?](#question-3-khoảng-thời-gian-nào-ta-nên-hiển-thị-quảng-cáo-để-tăng-khả-năng-mua-hàng-của-khách)
  - [Question 4: Sản phẩm được bán nhiều nhất là gì? Tại sao?](#question-4-sản-phẩm-được-bán-nhiều-nhất-là-gì-tại-sao)
- [Tác giả](#tác-giả)

## Giới thiệu

Dự án này thực hiện phân tích dữ liệu kinh doanh từ bộ dữ liệu bán hàng với mục tiêu trả lời các câu hỏi về doanh thu, thành phố bán hàng, thời gian quảng cáo hiệu quả và các sản phẩm bán chạy nhất. Dữ liệu bán hàng được lấy từ nhiều nguồn khác nhau và đã được tổng hợp và làm sạch để chuẩn bị cho việc phân tích.

## Cài đặt

Để chạy dự án này, bạn cần cài đặt các thư viện sau:

- `pandas`: Thư viện xử lý dữ liệu.
- `numpy`: Thư viện tính toán khoa học.
- `matplotlib`: Thư viện vẽ đồ thị.
- `seaborn`: Thư viện vẽ đồ thị nâng cao.
- `google.colab`: Thư viện hỗ trợ Google Colab (nếu bạn sử dụng Colab).

```bash
pip install pandas numpy matplotlib seaborn google.colab
```
## Phân tích dữ liệu
Question 1: Tháng nào kinh doanh tốt nhất? Doanh thu tháng đó là bao nhiêu?
Để trả lời câu hỏi này, ta cần tính tổng doanh thu hàng tháng và xác định tháng có doanh thu cao nhất.
```
all_data['Sales'] = all_data['Quantity Ordered'].astype('int') * all_data['Price Each'].astype('float')

monthly_sales_df = all_data.groupby(['Month']).sum()
monthly_sales_df

import matplotlib.pyplot as plt
import seaborn as sns

months = range(1, 13)
plt.bar(months, monthly_sales_df['Sales']/1000000)
plt.xticks(months)
plt.ylabel('Sales in USD ($ Million)')
plt.xlabel('Month number')
plt.show()
```
Question 2: Thành phố nào bán được nhiều sản phẩm (product) nhất?
Để trả lời câu hỏi này, ta cần nhóm dữ liệu theo thành phố và tính tổng số sản phẩm bán ra.
```
city_sales_df = all_data.groupby(['City']).sum()

import matplotlib.pyplot as plt
keys = city_sales_df.index

plt.bar(keys, city_sales_df['Sales']/1000000)
plt.ylabel('Sales in USD ($Million)')
plt.xlabel('City')
plt.xticks(keys, rotation='vertical', size=8)
plt.show()
```
Question 3: Khoảng thời gian nào ta nên hiển thị quảng cáo để tăng khả năng mua hàng của khách?
Chúng ta cần phân tích theo giờ và tìm ra giờ nào có lượng mua hàng cao nhất.
```
from datetime import datetime

def func(date):
    date = datetime.strptime(date, '%m/%d/%y %H:%M')
    return date.hour, date.minute

hour_handler = np.frompyfunc(func, 1, 2)
all_data['Hour'], all_data['Minute'] = hour_handler(all_data['Order Date'])

hourly_sale_df = all_data.groupby(['Hour']).count()

plt.plot(hourly_sale_df.index, hourly_sale_df['Count'])
plt.xticks(hourly_sale_df.index)
plt.grid()
plt.show()
```
Question 4: Sản phẩm được bán nhiều nhất là gì? Tại sao?
Chúng ta sẽ nhóm theo sản phẩm và tính số lượng bán ra, sau đó trực quan hóa dữ liệu.
```
product_df = all_data.groupby('Product').sum()

keys = product_df.index
plt.bar(keys, product_df['Quantity Ordered'])
plt.xticks(keys, rotation='vertical', size=8)
plt.show()

prices = all_data.groupby('Product').mean()['Price Each']

fig, ax1 = plt.subplots()
ax2 = ax1.twinx()
ax1.bar(keys, product_df['Quantity Ordered'], color='#1f77b4')
ax2.plot(keys, prices, color='#ff7f0e')

ax1.set_xlabel('Product Name')
ax1.set_ylabel('Quantity Ordered', color='#1f77b4', weight='bold')
ax2.set_ylabel('Price ($)', color='#ff7f0e', weight='bold')
ax1.set_xticklabels(keys, rotation='vertical', size=8)

fig.show()
```
Cấu trúc thư mục
```
Sales-Analysis-Project/
│
├── README.md                    # Tệp README mô tả dự án và hướng dẫn sử dụng.
├── main.ipynb                    # Mã nguồn chính sử dụng Jupyter Notebook.
│
├── Sales_Data/                   # Thư mục chứa dữ liệu bán hàng theo các tháng.
│   ├── SA56C9~1.CSV              # Dữ liệu bán hàng tháng từ SA56C9~1.
│   ├── SA5E69~1.CSV              # Dữ liệu bán hàng tháng từ SA5E69~1.
│   ├── SA72F9~1.CSV              # Dữ liệu bán hàng tháng từ SA72F9~1.
│   ├── SAFA26~1.CSV              # Dữ liệu bán hàng tháng từ SAFA26~1.
│   ├── SALES~2.CSV               # Dữ liệu bán hàng tháng từ SALES~2.
│   ├── Sales_April_2019.csv       # Dữ liệu bán hàng tháng 4/2019.
│   ├── Sales_July_2019.csv        # Dữ liệu bán hàng tháng 7/2019.
│   ├── Sales_June_2019.csv        # Dữ liệu bán hàng tháng 6/2019.
│   ├── Sales_March_2019.csv       # Dữ liệu bán hàng tháng 3/2019.
│   ├── Sales_May_2019.csv         # Dữ liệu bán hàng tháng 5/2019.
│
└── all_data.csv                  # Dữ liệu tổng hợp từ các tệp CSV trên.
```
Tác giả
Dự án này được thực hiện bởi CuongDAol.

Email: buipg0801@gmail.com

Cảm ơn bạn đã xem qua dự án này! Nếu bạn có bất kỳ câu hỏi nào hoặc muốn hợp tác, đừng ngần ngại liên hệ với tôi.

### Giải thích các phần trong `README.md`:

1. **Giới thiệu**: Trình bày mục tiêu và nội dung của dự án.
2. **Cài đặt**: Hướng dẫn cài đặt các thư viện cần thiết.
3. **Sử dụng**: Các bước để chạy mã và phân tích dữ liệu.
4. **Phân tích dữ liệu**: Các câu hỏi và cách phân tích dữ liệu để trả lời các câu hỏi kinh doanh.
5. **Cấu trúc thư mục**: Cấu trúc thư mục của dự án.
6. **Tác giả**: Thông tin về tác giả và cách liên hệ.

Đây là hướng dẫn chi tiết và cấu trúc tệp README để bạn dễ dàng hiểu và thực hiện phân tích dữ liệu kinh doanh với Python.

