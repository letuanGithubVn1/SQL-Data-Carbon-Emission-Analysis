# SQL-Data-Carbon-Emission-Analysis
Dự án phân tích lượng khí thải carbon của các ngành công nghiệp khác nhau. Chúng tôi muốn xác định các lĩnh vực có mức phát thải cao nhất bằng cách phân tích theo quốc gia, năm và khám phá các xu hướng

## Khám phá dữ liệu
Bảng `product_emissions`
```sql
Select * From product_emissions pe limit 3;
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|

## Phân tích dữ liệu
### 1. Những sản phẩm nào đóng góp nhiều nhất vào lượng khí thải carbon?
***Mục tiêu của truy vấn:***
Truy vấn này giúp tìm ra 10 sản phẩm có mức phát thải carbon trung bình cao nhất trong cơ sở dữ liệu.
```sql
Select product_name, ROUND(AVG(carbon_footprint_pcf), 2) as average_pcf
From product_emissions pe 
Group by pe.product_name 
Order by average_pcf DESC
Limit 10;
```
|product_name|average_pcf|
|------------|-----------|
|Wind Turbine G128 5 Megawats|3718044.00|
|Wind Turbine G132 5 Megawats|3276187.00|
|Wind Turbine G114 2 Megawats|1532608.00|
|Wind Turbine G90 2 Megawats|1251625.00|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.00|
|TCDE|99075.00|
|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.00|
|Mercedes-Benz S-Class (S 500)|85000.00|
|Mercedes-Benz SL (SL 350)|72000.00|

***Kết luận***
- Mặc dù tuabin gió được coi là nguồn năng lượng tái tạo thân thiện với môi trường, nhưng quá trình sản xuất và lắp đặt chúng tạo ra lượng khí thải carbon rất lớn. 
- Các mẫu xe cao cấp như Mercedes-Benz GLE và S-Class thải carbon đáng kể, phản ánh tác động môi trường từ quá trình sản xuất, vận hành và tiêu thụ nhiên liệu.
- ngành xây dựng, đặc biệt là các công trình có sử dụng thép và bê tông, đóng góp đáng kể vào lượng phát thải CO2.




























