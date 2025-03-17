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
Truy vấn này giúp tìm ra 10 sản phẩm có mức phát thải carbon trung bình cao nhất và nhóm ngành của các sản phẩm.
```sql
Select pe.product_name, ROUND(AVG(pe.carbon_footprint_pcf), 2) as average_pcf, ig.industry_group
FRom product_emissions pe
Join industry_groups ig ON pe.industry_group_id = ig.id
Group by pe.product_name
Order By  pe.carbon_footprint_pcf DESC
Limit 10;
```
|product_name|average_pcf|industry_group|
|------------|-----------|--------------|
|Wind Turbine G128 5 Megawats|3718044.00|Electrical Equipment and Machinery|
|Wind Turbine G132 5 Megawats|3276187.00|Electrical Equipment and Machinery|
|Wind Turbine G114 2 Megawats|1532608.00|Electrical Equipment and Machinery|
|Wind Turbine G90 2 Megawats|1251625.00|Electrical Equipment and Machinery|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|Automobiles & Components|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.00|Materials|
|TCDE|99075.00|Materials|
|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.00|Automobiles & Components|
|Mercedes-Benz S-Class (S 500)|85000.00|Automobiles & Components|
|Mercedes-Benz SL (SL 350)|72000.00|Automobiles & Components|


![Biểu đồ Top 10 sản phẩm thải carbon nhiều nhất](https://raw.githubusercontent.com/letuanGithubVn1/SQL-Data-Carbon-Emission-Analysis/main/images/Average%20of%20carbon_footprint_pcf.png)

***Kết luận***
- Mặc dù tuabin gió được coi là nguồn năng lượng tái tạo thân thiện với môi trường, nhưng quá trình sản xuất và lắp đặt chúng tạo ra lượng khí thải carbon rất lớn. 
- Các mẫu xe cao cấp như Mercedes-Benz GLE và S-Class thải carbon đáng kể, phản ánh tác động môi trường từ quá trình sản xuất, vận hành và tiêu thụ nhiên liệu.
- ngành xây dựng, đặc biệt là các công trình có sử dụng thép và bê tông, đóng góp đáng kể vào lượng phát thải CO2.


### 2. Những ngành công nghiệp nào có mức đóng góp khí thải carbon cao nhất?
***Mục tiêu của truy vấn:***
Tìm ra 10 nhóm ngành có mức phát thải carbon cao nhất. Xác định tổng lượng khí thải CO₂ của từng ngành để so sánh.
```sql
Select ig.industry_group, ROUND(SUM(pe.carbon_footprint_pcf), 2) as total_pcf
From product_emissions pe
Join industry_groups ig ON pe.industry_group_id = ig.id
Group by ig.industry_group
Order By  total_pcf DESC
LIMIT 10;
```
<table style="width:100%;">
  <tr>
    <td style="width:30%; vertical-align:top;">
      <h3>Top 10 Ngành Có Lượng Carbon Footprint Cao Nhất</h3>
      <table>
        <tr>
          <th>Industry Group</th>
          <th>Total PCF</th>
        </tr>
        <tr>
          <td>Electrical Equipment and Machinery</td>
          <td>9,801,558.00</td>
        </tr>
        <tr>
          <td>Automobiles & Components</td>
          <td>2,582,264.00</td>
        </tr>
        <tr>
          <td>Materials</td>
          <td>577,595.00</td>
        </tr>
        <tr>
          <td>Technology Hardware & Equipment</td>
          <td>363,776.00</td>
        </tr>
        <tr>
          <td>Capital Goods</td>
          <td>258,712.00</td>
        </tr>
        <tr>
          <td>Food, Beverage & Tobacco</td>
          <td>111,131.00</td>
        </tr>
        <tr>
          <td>Pharmaceuticals, Biotechnology & Life Sciences</td>
          <td>72,486.00</td>
        </tr>
        <tr>
          <td>Chemicals</td>
          <td>62,369.00</td>
        </tr>
        <tr>
          <td>Software & Services</td>
          <td>46,544.00</td>
        </tr>
        <tr>
          <td>Media</td>
          <td>23,017.00</td>
        </tr>
      </table>
    </td>
    <td style="width:70%; text-align:center;">
      <h3>Biểu đồ Top 10 sản phẩm thải carbon nhiều nhất</h3>
      <img src="https://raw.githubusercontent.com/letuanGithubVn1/SQL-Data-Carbon-Emission-Analysis/main/images/Sum%20of%20carbon_footprint_pcf.png" alt="Biểu đồ Top 10 sản phẩm thải carbon nhiều nhất" style="max-width:100%; height:auto;">
    </td>
  </tr>
</table>


***Nhận Định***
- Ngành thiết bị điện & máy móc dẫn đầu về phát thải CO₂ với hơn 9.8 triệu tấn CO₂, cho thấy quy mô lớn và mức tiêu thụ năng lượng cao của ngành này.
- Ngành ô tô & linh kiện đứng thứ hai với 2.58 triệu tấn CO₂, do chuỗi cung ứng phức tạp và mức tiêu thụ nguyên liệu cao.
- Các ngành sản xuất vật liệu và phần cứng công nghệ cũng có mức phát thải đáng kể, phản ánh tác động môi trường của sản xuất công nghiệp.
- Ngành dược phẩm và công nghệ phần mềm có mức phát thải thấp hơn, nhưng vẫn có tác động đến môi trường.

***Kết Luận***
- Các ngành công nghiệp có mức phát thải CO₂ cao nhất chủ yếu thuộc lĩnh vực sản xuất và công nghiệp nặng.
- Các ngành dịch vụ như phần mềm và truyền thông có mức phát thải thấp hơn, nhưng vẫn cần xem xét tác động môi trường.
- Kết quả này giúp các doanh nghiệp và chính phủ định hướng giảm thiểu lượng phát thải, áp dụng năng lượng sạch và cải tiến quy trình sản xuất để hướng đến mục tiêu Net Zero.


### 3. Những công ty nào có mức đóng góp khí thải carbon cao nhất?
***Mục tiêu của truy vấn:***
Tìm ra 10 công ty có mức phát thải carbon cao nhất. Xác định tổng lượng khí thải CO₂ của từng công ty để so sánh.
```sql
Select c.company_name, ROUND(SUM(pe.carbon_footprint_pcf), 2) as total_pcf_by_company, ig.industry_group
From product_emissions pe 
Join companies c On pe.company_id = c.id 
Join industry_groups ig On pe.industry_group_id = ig.id
Group by c.company_name
order by total_pcf_by_company DESC
Limit 10;
```
<table style="width:100%;">
  <tr>
    <td style="width:30%; vertical-align:top;">
      <h3>Top 10 công ty Có Lượng Carbon Footprint Cao Nhất</h3>
      <table>
        <tr>
          <th>Comany name</th>
          <th>Total PCF</th>
          <th>Industry Group</th>
        </tr>
        <tr>
          <td>Gamesa Corporación Tecnológica, S.A.</td>
          <td>9,778,464.00</td>
          <td>Electrical Equipment and Machinery</td>
        </tr>
        <tr>
          <td>Daimler AG</td>
          <td>1,594,300.00</td>
          <td>Automobiles & Components</td>
        </tr>
        <tr>
          <td>Volkswagen AG</td>
          <td>655,960.00</td>
          <td>Automobiles & Components</td>
        </tr>
        <tr>
          <td>Mitsubishi Gas Chemical Company, Inc.</td>
          <td>212,016.00</td>
          <td>Materials</td>
        </tr>
        <tr>
          <td>Hino Motors, Ltd.</td>
          <td>191,687.00</td>
          <td>Automobiles & Components</td>
        </tr>
        <tr>
          <td>Arcelor Mittal</td>
          <td>167,007.00</td>
          <td>Materials</td>
        </tr>
        <tr>
          <td>Weg S/A</td>
          <td>160,655.00</td>
          <td>Capital Goods</td>
        </tr>
        <tr>
          <td>General Motors Company</td>
          <td>137,007.00</td>
          <td>Automobiles & Components</td>
        </tr>
        <tr>
          <td>Lexmark International, Inc.</td>
          <td>132,012.00</td>
          <td>Technology Hardware & Equipment</td>
        </tr>
        <tr>
          <td>Daikin Industries, Ltd.</td>
          <td>105,600.00</td>
          <td>Capital Goods</td>
        </tr>
      </table>
    </td>
    <td style="width:70%; text-align:center;">
      <h3>Biểu đồ Top 10 công ty thải carbon nhiều nhất</h3>
      <img src="https://raw.githubusercontent.com/letuanGithubVn1/SQL-Data-Carbon-Emission-Analysis/main/images/Sum%20of%20carbon_footprint_pcf.png" alt="Biểu đồ Top 10 công ty thải carbon nhiều nhất"
      style="max-width:100%; height:auto;">
    </td>
  </tr>
</table>

***Nhận Định:***
- Gamesa Corporación Tecnológica, S.A. đứng đầu danh sách với gần 9.8 triệu tấn CO₂, chủ yếu do sản xuất tua-bin gió, mặc dù là một nguồn năng lượng tái tạo nhưng quá trình sản xuất lại tạo ra lượng phát thải lớn.
- Các hãng ô tô như Daimler AG, Volkswagen AG, General Motors, và Hino Motors có tổng phát thải cao do chuỗi cung ứng sản xuất xe hơi và linh kiện tiêu tốn nhiều nguyên liệu và năng lượng.
- Công ty Mitsubishi Gas Chemical và Arcelor Mittal có lượng phát thải đáng kể, phản ánh mức độ ảnh hưởng của ngành hóa chất và sản xuất thép đến môi trường.
- Lexmark International và Daikin Industries có mức phát thải thấp hơn so với các công ty trong ngành công nghiệp nặng, nhưng vẫn đáng kể trong lĩnh vực công nghệ và thiết bị điện.

### 4. Những quốc gia nào có mức đóng góp khí thải carbon cao nhất?
***Mục tiêu của truy vấn:***
Tìm ra 10 nhóm quốc gia có mức phát thải carbon cao nhất.
```sql
Select c.country_name , ROUND(SUM(pe.carbon_footprint_pcf), 2) as total_pcf_by_company
From product_emissions pe 
Join countries c On pe.country_id = c.id 
Group by c.country_name
order by total_pcf_by_company DESC
Limit 10;
```

<table style="width:100%;">
  <tr>
    <td style="width:30%; vertical-align:top;">
      <h3>Top 10 quốc gia Có Lượng Carbon Footprint Cao Nhất</h3>
      <table>
        <tr>
          <th>Country name</th>
          <th>Total PCF</th>
        </tr>
         <tr>
          <td>Spain</td>
          <td>9,778,464.00</td>
        </tr>
        <tr>
          <td>Germany</td>
          <td>1,594,300.00</td>
        </tr>
        <tr>
          <td>Germany</td>
          <td>655,960.00</td>
        </tr>
        <tr>
          <td>Japan</td>
          <td>212,016.00</td>
        </tr>
        <tr>
          <td>Japan</td>
          <td>191,687.00</td>
        </tr>
        <tr>
          <td>Luxembourg</td>
          <td>167,007.00</td>
        </tr>
        <tr>
          <td>Brazil</td>
          <td>160,655.00</td>
        </tr>
        <tr>
          <td>USA</td>
          <td>137,007.00</td>
        </tr>
        <tr>
          <td>USA</td>
          <td>132,012.00</td>
        </tr>
        <tr>
          <td>Japan</td>
          <td>105,600.00</td>
        </tr>
      </table>
    </td>
    <td style="width:70%; text-align:center;">
      <h3>Biểu đồ Top 10 quốc gia thải carbon nhiều nhất</h3>
      <img src="https://raw.githubusercontent.com/letuanGithubVn1/SQL-Data-Carbon-Emission-Analysis/main/images/Sum%20of%20carbon_footprint_pcf%20by%20country.png" alt="Biểu đồ Top 10 quốc gia thải carbon nhiều nhất"
      style="max-width:100%; height:auto;">
    </td>
  </tr>
</table>

***Nhận định:***
- Tây Ban Nha (Spain) có mức phát thải cao nhất, chủ yếu đến từ Gamesa Corporación Tecnológica, S.A., công ty chuyên sản xuất tua-bin gió. Điều này cho thấy dù là năng lượng tái tạo, quá trình sản xuất vẫn tạo ra lượng khí thải lớn.
- Đức (Germany) đứng thứ hai, chủ yếu do các công ty sản xuất ô tô như Daimler AG và Volkswagen AG. Điều này phản ánh tác động môi trường lớn từ ngành công nghiệp ô tô.
- Nhật Bản và Mỹ có mức phát thải tương đối cao, liên quan đến ngành công nghiệp ô tô (Hino Motors, General Motors) và công nghiệp hóa chất (Mitsubishi Gas Chemical).
- Hàn Quốc, Brazil, Luxembourg và Hà Lan có lượng phát thải ở mức trung bình, cho thấy các nước này có một số ngành công nghiệp lớn nhưng không phải là trung tâm phát thải chính.
- Ấn Độ có mức phát thải thấp nhất, có thể do sự tập trung vào ngành dịch vụ và sản xuất quy mô nhỏ hơn so với các nước phát triển.

### 5. Xu hướng thải CO2 theo thời gian
***Mục tiêu của truy vấn:***
Lấy tổng lượng khí thải theo từng năm.
```sql
Select year, ROUND(SUM(carbon_footprint_pcf), 2) as total_pcf
From product_emissions pe 
GROUP by year
ORDER BY total_pcf 
```
|year|total_pcf|
|----|---------|
|2017|340271.00|
|2013|503857.00|
|2014|624226.00|
|2016|1640182.00|
|2015|10840415.00|

***Kết luận:***
- **Sự gia tăng đột biến vào năm 2015**
Năm 2015 có mức phát thải cao nhất (10,840,415 tấn CO₂), gấp nhiều lần so với các năm còn lại.  
Đây có thể là do sự mở rộng quy mô sản xuất hoặc một số ngành công nghiệp có mức phát thải cao gia tăng đáng kể hoạt động trong năm này.
- **Mức phát thải dao động qua các năm trước đó**
Từ năm 2013 đến 2014, lượng phát thải tăng nhẹ từ **503,857 tấn CO₂** lên **624,226 tấn CO₂**.  
Năm 2016, phát thải tiếp tục tăng lên **1,640,182 tấn CO₂**, cho thấy sự phát triển dần của các hoạt động công nghiệp có lượng khí thải cao.
- **Năm 2017 có lượng phát thải thấp nhất trong dữ liệu**
Chỉ **340,271 tấn CO₂**, thấp hơn hẳn so với các năm trước.  
Điều này có thể là do các biện pháp giảm phát thải được áp dụng, sự thay đổi trong công nghệ sản xuất, hoặc có sự sụt giảm trong hoạt động công nghiệp.















