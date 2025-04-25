# Carbon_Emission_Analysis

## 1. Introduction
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

![image](https://github.com/user-attachments/assets/8c53ff49-dd3e-4afd-b681-394fc6801007)

Photo by Chris LeBoutillier (unsplash.com)

### 1.1 Data source
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

### 1.2 Data model
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
![image](https://github.com/user-attachments/assets/2123bee2-71fb-4119-a15d-8527f353ea2e)

### 1.3 Data structure
**Table 'product_emissions'**
```sql
SELECT * FROM product_emissions LIMIT 10
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|
|10261-3-2017|14|16|25|2017|Multifunction Printers|110.0|2274|20.05|3.61|76.34|
|10324-1-2016|15|16|19|2016|KURALON  fiber|1500.0|10000|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10418-1-2013|84|9|19|2013|Portland Cement|1000.0|1102|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10661-10-2014|85|28|11|2014|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10661-10-2015|85|28|6|2015|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|

**Table 'industry_groups'**
```sql
SELECT * FROM industry_groups LIMIT 10
```
| id | industry_group                                                         | 
| -: | ---------------------------------------------------------------------: | 
| 1  | "Consumer Durables, Household and Personal Products"                   | 
| 2  | "Food, Beverage & Tobacco"                                             | 
| 3  | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4  | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5  | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 
| 6  | "Textiles, Apparel, Footwear and Luxury Goods"                         | 
| 7  | Automobiles & Components                                               | 
| 8  | Capital Goods                                                          | 
| 9  | Chemicals                                                              | 
| 10 | Commercial & Professional Services                                     | 

**Table 'companies'**
```sql
SELECT * FROM companies LIMIT 10
```
| id | company_name                                   | 
| -: | ---------------------------------------------: | 
| 1  | "Autodesk, Inc."                               | 
| 2  | "Casio Computer Co., Ltd."                     | 
| 3  | "Cisco Systems, Inc."                          | 
| 4  | "CNX Coal Resources, LP"                       | 
| 5  | "Coca-Cola Enterprises, Inc."                  | 
| 6  | "Compañía Española de Petróleos, S.A.U. CEPSA" | 
| 7  | "Daikin Industries, Ltd."                      | 
| 8  | "Elitegroup computer systems co., Ltd."        | 
| 9  | "Fuji Xerox Co., Ltd."                         | 
| 10 | "Gamesa Corporación Tecnológica, S.A."         | 

**Table 'countries'**
```sql
SELECT * FROM countries LIMIT 10
```
| id | country_name | 
| -: | -----------: | 
| 1  | Australia    | 
| 2  | Belgium      | 
| 3  | Brazil       | 
| 4  | Canada       | 
| 5  | Chile        | 
| 6  | China        | 
| 7  | Colombia     | 
| 8  | Finland      | 
| 9  | France       | 
| 10 | Germany      | 

## 2. Data Explore
**Data duplicate**
```sql
SELECT*, 
	COUNT(*) AS duplicate_count
FROM product_emissions pe 
GROUP BY 
	id,
	company_id ,
	country_id ,
	industry_group_id ,
	year,
	product_name ,
	weight_kg ,
	carbon_footprint_pcf ,
	upstream_percent_total_pcf ,
	operations_percent_total_pcf ,
	downstream_percent_total_pcf 
HAVING COUNT(*)>1
LIMIT 10
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|duplicate_count|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|---------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|2|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|2|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|2|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|2|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|2|
|10261-3-2017|14|16|25|2017|Multifunction Printers|110.0|2274|20.05|3.61|76.34|2|
|10324-1-2016|15|16|19|2016|KURALON  fiber|1500.0|10000|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10418-1-2013|84|9|19|2013|Portland Cement|1000.0|1102|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-1-2014|85|28|11|2014|501® Original Jeans – Dark Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-1-2015|85|28|6|2015|501® Original Jeans – Dark Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|

**Duplicate result**
```sql
SELECT COUNT(product_name) AS 'Total number of products',
       COUNT(DISTINCT product_name) as 'Number of unique products'
FROM product_emissions pe;
```
|Total number of products|Number of unique products|
|------------------------|-------------------------|
|1037|661|

## 3. Data Analysis
### 3.1 Which products contribute the most to carbon emissions?
```sql
SELECT product_name,
       ROUND(AVG(carbon_footprint_pcf),2) AS 'Average PCF'
FROM product_emissions pe 
GROUP BY pe.product_name
ORDER BY carbon_footprint_pcf DESC 
LIMIT 10
```
|product_name|Average PCF|
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

**Answer:**
+ The top 4 products with the highest average PCF are wind turbines due to manufacturing, transportation, and installation but wind energy is considered clean.
=> High PCF doesn't mean poor environmental performance if the product offsets emissions over time.
+ Automobiles: Vehicles like the Land Cruiser Prado and Mercedes-Benz models show PCFs between 70,000–190,000.
=> More luxury or heavy models have higher emissions due to materials and production complexity.
+ Industrial Infrastructure: The retaining wall structure with steel sheet piles also ranks high, energy-intensive to produce.
=> Heavy construction materials are major emission contributors.
  
### 3.2 What are the industry groups of these products?
```sql
SELECT ig.industry_group, 
       pe. product_name,
       ROUND(AVG(pe.carbon_footprint_pcf),2) AS 'Average PCF'
FROM product_emissions pe
JOIN industry_groups ig ON ig.id = pe.industry_group_id
GROUP BY product_name
ORDER BY carbon_footprint_pcf DESC 
LIMIT 10
```
|industry_group|product_name|Average PCF|
|--------------|------------|-----------|
|Electrical Equipment and Machinery|Wind Turbine G128 5 Megawats|3718044.00|
|Electrical Equipment and Machinery|Wind Turbine G132 5 Megawats|3276187.00|
|Electrical Equipment and Machinery|Wind Turbine G114 2 Megawats|1532608.00|
|Electrical Equipment and Machinery|Wind Turbine G90 2 Megawats|1251625.00|
|Automobiles & Components|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|
|Materials|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.00|
|Materials|TCDE|99075.00|
|Automobiles & Components|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.00|
|Automobiles & Components|Mercedes-Benz S-Class (S 500)|85000.00|
|Automobiles & Components|Mercedes-Benz SL (SL 350)|72000.00|

**Answer:**
+ Electrical Equipment and Machinery
+ Automobiles & Components
+ Materials

### 3.3 What are the industries with the highest contribution to carbon emissions?
```sql
SELECT ig.industry_group, 
       ROUND(SUM(pe.carbon_footprint_pcf),2) AS 'Total PCF'
FROM product_emissions pe
JOIN industry_groups ig ON ig.id = pe.industry_group_id
GROUP BY ig.industry_group
ORDER BY ROUND(SUM(pe.carbon_footprint_pcf),2) DESC 
LIMIT 10
```
|industry_group|Total PCF|
|--------------|---------|
|Electrical Equipment and Machinery|9801558.00|
|Automobiles & Components|2582264.00|
|Materials|577595.00|
|Technology Hardware & Equipment|363776.00|
|Capital Goods|258712.00|
|"Food, Beverage & Tobacco"|111131.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|72486.00|
|Chemicals|62369.00|
|Software & Services|46544.00|
|Media|23017.00|

![image](https://github.com/user-attachments/assets/03fa22e6-81d6-40fe-886e-013a4cdf0c91)

**Answer:**
+ Top 1: Electrical Equipment and Machinery
=> This industry group accounts for a massive share of total emissions — nearly 10 million PCF — due to large, energy-intensive products like wind turbines. Even though these are part of renewable energy infrastructure, their production still has a high environmental cost.
+ Top 2: Automobiles & Components, represents 4 out of the top 10 emission-heavy products.
=> This industry is a major carbon contributor.
+ Top 3: Materials
=> Reflects the carbon intensity of raw material extraction and processing, which is often upstream in value chains.
* General Trend: Heavy industrial products > Vehicles > Light infrastructure or components in terms of PCF.

### 3.4 What are the companies with the highest contribution to carbon emissions?
```sql
SELECT c.company_name,  
       ROUND(SUM(pe.carbon_footprint_pcf),2) AS 'Total PCF'
FROM product_emissions pe
JOIN companies c ON c.id = pe.company_id
GROUP BY c.company_name
ORDER BY ROUND(SUM(pe.carbon_footprint_pcf),2) DESC 
LIMIT 10
```
|company_name|Total PCF|
|------------|---------|
|"Gamesa Corporación Tecnológica, S.A."|9778464.00|
|Daimler AG|1594300.00|
|Volkswagen AG|655960.00|
|"Mitsubishi Gas Chemical Company, Inc."|212016.00|
|"Hino Motors, Ltd."|191687.00|
|Arcelor Mittal|167007.00|
|Weg S/A|160655.00|
|General Motors Company|137007.00|
|"Lexmark International, Inc."|132012.00|
|"Daikin Industries, Ltd."|105600.00|

![image](https://github.com/user-attachments/assets/17788108-b55d-4edd-9794-a0f66619863b)

**Answer:**
+ Gamesa is the largest contributor, Gamesa alone accounts for ~86% of the total emissions in this top 10 list,  likely due to wind turbine manufacturing
+ Daimler, Volkswagen, Hino, and General Motors, all automotive companies dominate the rest of the list
+ Arcelor Mittal (steel), Mitsubishi Gas Chemical, and Daikin (HVAC systems) are examples of heavy industry or manufacturing sectors with high emissions per product. Lexmark (printing) also appears, showing how even office equipment can accumulate notable emissions.
  
### 3.5 What are the countries with the highest contribution to carbon emissions?
```sql
SELECT co.country_name, 
       ROUND(SUM(pe.carbon_footprint_pcf),2) AS 'Total PCF'
FROM product_emissions pe
JOIN countries co ON co.id = pe.country_id
GROUP BY co.country_name
ORDER BY ROUND(SUM(pe.carbon_footprint_pcf),2) DESC 
LIMIT 10
```
|country_name|Total PCF|
|------------|---------|
|Spain|9786130.00|
|Germany|2251225.00|
|Japan|653237.00|
|USA|518381.00|
|South Korea|186965.00|
|Brazil|169337.00|
|Luxembourg|167007.00|
|Netherlands|70417.00|
|Taiwan|62875.00|
|India|24574.00|

![image](https://github.com/user-attachments/assets/1c4d2769-13a8-486e-a7fc-88b0f0febc73)

**Answer:**
+ Spain dominates the list, more than 4x higher than Germany’s, the second-highest, because Gamesa’s manufacturing is based in Spain, and their wind turbines account for most of the emissions here.
+ Germany, Japan, and the USA form a strong middle tier, likely associated with emissions from automotive, electronics, and chemical manufacturing.
+ South Korea, Brazil, and Luxembourg have smaller but significant PCFs, these might reflect operations from Daikin, Weg S/A, or Arcelor Mittal.

### 3.6 What is the trend of carbon footprints (PCFs) over the years?
```sql
SELECT 
    year,
    ROUND(AVG(carbon_footprint_pcf), 2) AS 'Average PCF'
FROM product_emissions
GROUP BY year
```
| year | Average PCF | 
| ---: | ----------: | 
| 2013 | 2399.32     | 
| 2014 | 2457.58     | 
| 2015 | 43188.90    | 
| 2016 | 6891.52     | 
| 2017 | 4050.85     | 

![image](https://github.com/user-attachments/assets/789ba4fc-d71b-4247-b81a-069306900edb)

**Answer:**
+ 2013 - 2014: Stable emissions, suggesting similar product and manufacturing types.
+ 2015 at peak: Greatly increase, possibly due to inclusion of high-impact industrial products (e.g., wind turbines, construction structures).
+ 2016 – 2017 decline: maybe because fewer high-emission products were logged or companies improved efficiency and emission-reducing strategies.
+ The trend shows progress toward lower emissions per product on average after 2015.

### 3.7 Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
```sql
-- Emissions per year
SELECT ig.industry_group AS 'Industry Group',
	ROUND(SUM(CASE WHEN pe.year = 2013 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2013 Emission',
	ROUND(SUM(CASE WHEN pe.year = 2014 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2014 Emission',
	ROUND(SUM(CASE WHEN pe.year = 2015 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2015 Emission',
	ROUND(SUM(CASE WHEN pe.year = 2016 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2016 Emission',
	ROUND(SUM(CASE WHEN pe.year = 2017 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2017 Emission'
FROM product_emissions AS pe
LEFT JOIN industry_groups AS ig ON ig.id = pe.industry_group_id 
GROUP BY ig.industry_group 
ORDER BY
	`2017 Emission`,
	`2016 Emission`,
	`2015 Emission`,
	`2014 Emission`,
	`2013 Emission`;
```
|Industry Group|2013 Emission|2014 Emission|2015 Emission|2016 Emission|2017 Emission|
|--------------|-------------|-------------|-------------|-------------|-------------|
|Household & Personal Products|0.00|0.00|0.00|0.00|0.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|32271.00|40215.00|0.00|0.00|0.00|
|Tobacco|0.00|0.00|1.00|0.00|0.00|
|Semiconductors & Semiconductors Equipment|0.00|0.00|3.00|0.00|0.00|
|Retailing|0.00|19.00|11.00|0.00|0.00|
|Gas Utilities|0.00|0.00|122.00|0.00|0.00|
|Food & Beverage Processing|0.00|0.00|141.00|0.00|0.00|
|Telecommunication Services|52.00|183.00|183.00|0.00|0.00|
|Trading Companies & Distributors and Commercial Services & Supplies|0.00|0.00|239.00|0.00|0.00|
|"Textiles, Apparel, Footwear and Luxury Goods"|0.00|0.00|387.00|0.00|0.00|
|"Consumer Durables, Household and Personal Products"|0.00|0.00|931.00|0.00|0.00|
|Tires|0.00|0.00|2022.00|0.00|0.00|
|Containers & Packaging|0.00|0.00|2988.00|0.00|0.00|
|"Mining - Iron, Aluminum, Other Metals"|0.00|0.00|8181.00|0.00|0.00|
|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|0.00|0.00|8909.00|0.00|0.00|
|Chemicals|0.00|0.00|62369.00|0.00|0.00|
|Electrical Equipment and Machinery|0.00|0.00|9801558.00|0.00|0.00|
|Food & Staples Retailing|0.00|773.00|706.00|2.00|0.00|
|Semiconductors & Semiconductor Equipment|0.00|50.00|0.00|4.00|0.00|
|Utilities|122.00|0.00|0.00|122.00|0.00|
|Consumer Durables & Apparel|2867.00|3280.00|0.00|1162.00|0.00|
|Media|9645.00|9645.00|1919.00|1808.00|0.00|
|Energy|750.00|0.00|0.00|10024.00|0.00|
|Automobiles & Components|130189.00|230015.00|817227.00|1404833.00|0.00|
|Software & Services|6.00|146.00|22856.00|22846.00|690.00|
|Commercial & Professional Services|1157.00|477.00|0.00|2890.00|741.00|
|"Food, Beverage & Tobacco"|4995.00|2685.00|0.00|100289.00|3162.00|
|Technology Hardware & Equipment|61100.00|167361.00|106157.00|1566.00|27592.00|
|Capital Goods|60190.00|93699.00|3505.00|6369.00|94949.00|
|Materials|200513.00|75678.00|0.00|88267.00|213137.00|

![image](https://github.com/user-attachments/assets/aebc0f85-d5f5-4a2a-8cd9-7974fe8e714e)

**Answer:**
+ Industries show spikes in emissions for only one year: Electrical Equipment and Machinery had an enormous spike in 2015 (~9.8 million), Chemicals also peaked in 2015 (~62k) and no data in other years  likely influenced by specific events or products - they're likely influenced by specific events or outlier product.
+ Industries show a notable decrease over time
  * Software & Services and Commercial & Professional Services: Mild downward trend over time, still active but showing progress.
  * Media: Steady decline from ~9.6k in 2013 to 1808 in 2016.
=>This could indicate successful emission reduction through innovation or less product data available in later years.

