# carbon-emissions-analysis.md
## 1. Explore data
Table 'product_emissions'
```sql
SELECT * FROM product_emissions
LIMIT 5;
```
| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        |

Table 'industry_groups'
```sql
SELECT * FROM industry_groups
LIMIT 5;
```
| id | industry_group                                                         | 
| -: | ---------------------------------------------------------------------: | 
| 1  | "Consumer Durables, Household and Personal Products"                   | 
| 2  | "Food, Beverage & Tobacco"                                             | 
| 3  | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4  | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5  | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 

Table 'companies'
```sql
SELECT * FROM companies
LIMIT 5;
```
| id | company_name                  | 
| -: | ----------------------------: | 
| 1  | "Autodesk, Inc."              | 
| 2  | "Casio Computer Co., Ltd."    | 
| 3  | "Cisco Systems, Inc."         | 
| 4  | "CNX Coal Resources, LP"      | 
| 5  | "Coca-Cola Enterprises, Inc." | 

Table 'countries'
```sql
SELECT * FROM countries
LIMIT 5;
```
| id | country_name | 
| -: | -----------: | 
| 1  | Australia    | 
| 2  | Belgium      | 
| 3  | Brazil       | 
| 4  | Canada       | 
| 5  | Chile        | 

## 2. Carbon Emissions Analysis
### 1. Which products contribute the most to carbon emissions?
```sql
SELECT product_name, ROUND(AVG(carbon_footprint_pcf),2) AS Avg_pcf
FROM product_emissions
GROUP BY product_name
ORDER BY Avg_pcf DESC
LIMIT 10;
```
| product_name                                                                                                                       | Avg_pcf    | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00 | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00 | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00 | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00  | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00  | 
| TCDE                                                                                                                               | 99075.00   | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00   | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00   | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00   | 

### 2. What are the industry groups of these products?
```sql
SELECT pe.product_name, ROUND(AVG(carbon_footprint_pcf),2) AS Avg_pcf, ig.industry_group
FROM product_emissions pe
JOIN industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY pe.product_name, pe.industry_group_id
ORDER BY Avg_pcf DESC
LIMIT 10;
```
| product_name                                                                                                                       | Avg_pcf    | industry_group                     | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------: | ---------------------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00 | Electrical Equipment and Machinery | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00 | Electrical Equipment and Machinery | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00 | Electrical Equipment and Machinery | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00 | Electrical Equipment and Machinery | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00  | Automobiles & Components           | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00  | Materials                          | 
| TCDE                                                                                                                               | 99075.00   | Materials                          | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00   | Automobiles & Components           | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00   | Automobiles & Components           | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00   | Automobiles & Components           | 

### 3. What are the industries with the highest contribution to carbon emissions?
```sql
SELECT ig.industry_group, ROUND(AVG(carbon_footprint_pcf),2) AS Avg_pcf
FROM product_emissions pe
JOIN industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY ig.industry_group
ORDER BY Avg_pcf DESC
LIMIT 10;
```
| industry_group                                   | Avg_pcf   | 
| -----------------------------------------------: | --------: | 
| Electrical Equipment and Machinery               | 891050.73 | 
| Automobiles & Components                         | 35373.48  | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 24162.00  | 
| Capital Goods                                    | 7391.77   | 
| Materials                                        | 3208.86   | 
| "Mining - Iron, Aluminum, Other Metals"          | 2727.00   | 
| Energy                                           | 2154.80   | 
| Chemicals                                        | 1949.03   | 
| Media                                            | 1534.47   | 
| Software & Services                              | 1368.94   | 

### 4. What are the companies with the highest contribution to carbon emissions?
```sql
SELECT c.company_name, ROUND(AVG(carbon_footprint_pcf),2) AS Avg_pcf
FROM product_emissions pe
JOIN companies c ON pe.company_id = c.id
GROUP BY c.company_name
ORDER BY Avg_pcf DESC
LIMIT 10;
```
| company_name                           | Avg_pcf    | 
| -------------------------------------: | ---------: | 
| "Gamesa Corporación Tecnológica, S.A." | 2444616.00 | 
| "Hino Motors, Ltd."                    | 191687.00  | 
| Arcelor Mittal                         | 83503.50   | 
| Weg S/A                                | 53551.67   | 
| Daimler AG                             | 43089.19   | 
| General Motors Company                 | 34251.75   | 
| Volkswagen AG                          | 26238.40   | 
| Waters Corporation                     | 24162.00   | 
| "Daikin Industries, Ltd."              | 17600.00   | 
| CJ Cheiljedang                         | 15802.83   | 

### 5. What are the countries with the highest contribution to carbon emissions?
```sql
SELECT co.country_name, ROUND(AVG(carbon_footprint_pcf),2) AS Avg_pcf
FROM product_emissions pe
JOIN countries co ON pe.country_id = co.id
GROUP BY co.country_name
ORDER BY Avg_pcf DESC
LIMIT 10;
```
| country_name | Avg_pcf   | 
| -----------: | --------: | 
| Spain        | 699009.29 | 
| Luxembourg   | 83503.50  | 
| Germany      | 33600.37  | 
| Brazil       | 9407.61   | 
| South Korea  | 5665.61   | 
| Japan        | 4600.26   | 
| Netherlands  | 2011.91   | 
| India        | 1535.88   | 
| USA          | 1332.60   | 
| South Africa | 1119.27   | 

### 6. What is the trend of carbon footprints (PCFs) over the years?
```sql
SELECT year, ROUND(AVG(carbon_footprint_pcf),2) AS Avg_pcf
FROM product_emissions
GROUP BY year
ORDER BY year;
```
| year | Avg_pcf  | 
| ---: | -------: | 
| 2013 | 2399.32  | 
| 2014 | 2457.58  | 
| 2015 | 43188.90 | 
| 2016 | 6891.52  | 
| 2017 | 4050.85  | 

### 7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
```sql
SELECT 	ig.industry_group, 
		ROUND(AVG(CASE WHEN pe.year = 2013 THEN carbon_footprint_pcf ELSE 0 END),2)  AS '2013 Avg pcf',
		ROUND(AVG(CASE WHEN pe.year = 2014 THEN carbon_footprint_pcf ELSE 0 END),2)  AS '2014 Avg pcf',
		ROUND(AVG(CASE WHEN pe.year = 2015 THEN carbon_footprint_pcf ELSE 0 END),2)  AS '2015 Avg pcf',
		ROUND(AVG(CASE WHEN pe.year = 2016 THEN carbon_footprint_pcf ELSE 0 END),2)  AS '2016 Avg pcf',
		ROUND(AVG(CASE WHEN pe.year = 2017 THEN carbon_footprint_pcf ELSE 0 END),2)  AS '2017 Avg pcf'
FROM product_emissions pe
LEFT JOIN industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY ig.industry_group
ORDER BY ig.industry_group;
```
| industry_group                                                         | 2013 Avg pcf | 2014 Avg pcf | 2015 Avg pcf | 2016 Avg pcf | 2017 Avg pcf | 
| ---------------------------------------------------------------------: | -----------: | -----------: | -----------: | -----------: | -----------: | 
| "Consumer Durables, Household and Personal Products"                   | 0.00         | 0.00         | 116.38       | 0.00         | 0.00         | 
| "Food, Beverage & Tobacco"                                             | 39.02        | 20.98        | 0.00         | 783.51       | 24.70        | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 0.00         | 0.00         | 685.31       | 0.00         | 0.00         | 
| "Mining - Iron, Aluminum, Other Metals"                                | 0.00         | 0.00         | 2727.00      | 0.00         | 0.00         | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 10757.00     | 13405.00     | 0.00         | 0.00         | 0.00         | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | 0.00         | 0.00         | 14.33        | 0.00         | 0.00         | 
| Automobiles & Components                                               | 1783.41      | 3150.89      | 11194.89     | 19244.29     | 0.00         | 
| Capital Goods                                                          | 1719.71      | 2677.11      | 100.14       | 181.97       | 2712.83      | 
| Chemicals                                                              | 0.00         | 0.00         | 1949.03      | 0.00         | 0.00         | 
| Commercial & Professional Services                                     | 26.30        | 10.84        | 0.00         | 65.68        | 16.84        | 
| Consumer Durables & Apparel                                            | 42.16        | 48.24        | 0.00         | 17.09        | 0.00         | 
| Containers & Packaging                                                 | 0.00         | 0.00         | 373.50       | 0.00         | 0.00         | 
| Electrical Equipment and Machinery                                     | 0.00         | 0.00         | 891050.73    | 0.00         | 0.00         | 
| Energy                                                                 | 150.00       | 0.00         | 0.00         | 2004.80      | 0.00         | 
| Food & Beverage Processing                                             | 0.00         | 0.00         | 7.05         | 0.00         | 0.00         | 
| Food & Staples Retailing                                               | 0.00         | 32.21        | 29.42        | 0.08         | 0.00         | 
| Gas Utilities                                                          | 0.00         | 0.00         | 61.00        | 0.00         | 0.00         | 
| Household & Personal Products                                          | 0.00         | 0.00         | 0.00         | 0.00         | 0.00         | 
| Materials                                                              | 1113.96      | 420.43       | 0.00         | 490.37       | 1184.09      | 
| Media                                                                  | 643.00       | 643.00       | 127.93       | 120.53       | 0.00         | 
| Retailing                                                              | 0.00         | 3.80         | 2.20         | 0.00         | 0.00         | 
| Semiconductors & Semiconductor Equipment                               | 0.00         | 10.00        | 0.00         | 0.80         | 0.00         | 
| Semiconductors & Semiconductors Equipment                              | 0.00         | 0.00         | 1.00         | 0.00         | 0.00         | 
| Software & Services                                                    | 0.18         | 4.29         | 672.24       | 671.94       | 20.29        | 
| Technology Hardware & Equipment                                        | 228.84       | 626.82       | 397.59       | 5.87         | 103.34       | 
| Telecommunication Services                                             | 5.78         | 20.33        | 20.33        | 0.00         | 0.00         | 
| Tires                                                                  | 0.00         | 0.00         | 1011.00      | 0.00         | 0.00         | 
| Tobacco                                                                | 0.00         | 0.00         | 1.00         | 0.00         | 0.00         | 
| Trading Companies & Distributors and Commercial Services & Supplies    | 0.00         | 0.00         | 39.83        | 0.00         | 0.00         | 
| Utilities                                                              | 30.50        | 0.00         | 0.00         | 30.50        | 0.00         | 
