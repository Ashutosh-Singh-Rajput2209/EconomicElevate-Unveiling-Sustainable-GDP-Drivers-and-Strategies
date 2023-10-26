# World Economic Indicator Data Analysis Project

## Purpose of the Project

The project aims to determine the factors that countries should focus on to sustain GDP/Capita. Commissioned by the Organisation for Economic Co-operation and Development, the project focuses on creating a sustainable growth strategy for countries.

## Dataset Description

The dataset encompasses various countries' and regions' GDPs, populations, and other pertinent factors that potentially impact GDP/Capita. The dataset is structured across multiple sheets, each focusing on different indicators related to economic, energy, human resources, tourism, and business aspects.

### Data Dictionary

#### GDP Sheet

- **Region**: Geographic region
- **Country/Region**: Specific country or region
- **Year**: Year of measurement
- **GDP**: Gross domestic product (in USD)
- **Health Exp % GDP**: Health expenditure as a percentage of GDP
- **Health Exp/Capita**: Health expenditure per capita (in USD)
- **Lending Interest**: Lending interest rate

#### ENERGY Sheet

- **Energy Usage**: Energy usage in KWh
- **CO2 Emissions**: Carbon dioxide emissions in CO2e

#### HUMAN RESOURCES Sheet

- **Birth Rate**: Rate of new births
- **Infant Mortality Rate**: Percentage of infants that die before the age of 5 years
- **Life Expectancy Female**: Average life expectancy of females
- **Life Expectancy Male**: Average life expectancy of males
- **Population 0-14**: Percentage of the total population aged 0-14
- **Population 15-64**: Percentage of the total population aged 15-64
- **Population 65+**: Percentage of the total population above 65 years of age
- **Population Total**: Total population count
- **Population Urban**: Percentage of the total population living in urban areas

#### TOURISM Sheet

- **Tourism Inbound**: Income in USD from international tourism within the country
- **Tourism Outbound**: Amount spent by citizens on tourism while visiting other countries

#### BUSINESS Sheet

- **Business Tax Rate**: Tax rate on businesses
- **Days to Start Business**: Average time taken to start a business
- **Ease of Business**: Ease of business index (1 = easy)
- **Hours to do Tax**: Time to prepare and pay taxes in hours per year
- **Internet Usage**: Percentage of the population using the internet
- **Mobile Phone Usage**: Percentage of the population using mobile phones

## Steps to Follow

### 1. Formatting & Data Processing

### 2. Data Analysis

### 3. Storytelling

### Step 1: Creating Unique Identifiers

To initiate the analysis, I faced the challenge of lacking a common unique key among the sheets of the dataset. To address this, I devised a method to generate unique identifiers for each row. By leveraging the three common columns available in all sheets, namely **Region**, **Country/Region**, and **Year**, I merged these columns into a singular key denoted as **"RegionCountry-RegionYear"**.

To implement this, I utilized the Google Sheets formula `=CONCATENATE(B2, C2, D2)`, where columns B, C, and D contain the respective values for **Region**, **Country/Region**, and **Year**. This process ensured the creation of distinct and consistent identifiers across all sheets, facilitating seamless data integration and analysis.

### Handling Missing Values

When confronted with missing values in the critical **GDP** column (E), I adopted a cautious approach due to the nature of GDP data. To maintain data integrity, I employed a strategy that involved imputing missing values with the corresponding region's previous year's data.

Additionally, I undertook the following steps in the **GDP** sheet of the workbook **"Processed Solution_Ashutosh Ranjan"**:

1. Moved the columns **"Region"**, **"Country/Region"**, and **"Year"** to the left, followed by the creation of a new **"search_key"** column (A) using the formula `=CONCATENATE(B2, C2, D2)`.

2. Calculated the percentage of missing values in the columns **"GDP"** (E), **"Health Exp % GDP"** (F), **"Health Exp/Capita"** (G), and **"Lending Interest"** (H), which were found to be 7.32%, 10.99%, 10.99%, and 30.14%, respectively.

3. Employed a filling formula to handle missing values, with the condition to use the previous year's data if available. The formula used was `=IF(E2="",IF(D2<>"2000",IF(E1<>"",E1,E2),E2),E2)`.

Following this process, the **GDP** sheet was effectively processed, ensuring data consistency and integrity.

### Handling Missing Values in **ENERGY** Sheet

For the **Energy Usage** and **CO2 Emissions** columns, I utilized the formula `=IF(A2="",AVERAGEIF($D$2:$D$2692,$D2,A$2:A$2692),A2)` and `=IF(B2="",AVERAGEIF($D$2:$D$2692,$D2,B$2:B$2692),B2)` respectively, where **A** represents the column **Energy Usage**, **B** represents **CO2 Emissions**, and **D** represents **Country/Region**.

These formulas were designed to fill in missing values. They calculate the average value for the respective country/region and fill in the missing cell with this average value. The approach accounted for the lack of a definitive trend over the years, as indicated by the graphs in the **Pivot Table 3** sheet.

The percentages of missing values before and after imputation were as follows:

- **Energy Usage**: 33.67% before and 19.32% after imputation.
- **CO2 Emissions**: 21.03% before and 5.80% after imputation.

Despite a 19% rate of missing values in **Energy Usage** post-imputation, the decision was made to retain the column, considering the exclusion of rows with missing values during bivariate analysis using `=CORREL()`.

Here is a more concise version for handling missing values in the **HUMAN RESOURCES**, **TOURISM**, and **BUSINESS** sheets:

### Handling Missing Values in HUMAN RESOURCES

In the **HUMAN RESOURCES** sheet, I used the formula `=IF(A2="",IF($L2<>"2000",IF(A1<>"",A1,A2),IF(A3<>"",A3,A2)),A2)` for columns from **N to V** to address missing values. Similar imputation methods were applied in the **TOURISM** and **BUSINESS** sheets, with a slight modification for the **BUSINESS** sheet by using **2012** instead of **2000** in the formula. The percentages of missing values before and after imputation were:

- **HUMAN RESOURCES**:

  - **Before Imputation**: Birth Rate (4.42%), Infant Mortality Rate (9.18%), Life Expectancy Female (5.05%), Life Expectancy Male (5.05%), Population 0-14 (8.18%), Population 15-64 (8.18%), Population 65+ (8.18%), Population Total (0%), Population Urban (0.97%).
  - **After Imputation**: Birth Rate (3.86%), Infant Mortality Rate (9.18%), Life Expectancy Female (4.72%), Life Expectancy Male (4.72%), Population 0-14 (8.14%), Population 15-64 (8.14%), Population 65+ (8.14%), Population Total (0%), Population Urban (0.97%).

- **BUSINESS**:
  - **Before Imputation**: Business Tax Rate (47.12%), Days to Start Business (36.16%), Ease of Business (93.13%), Hours to do Tax (47.38%), Internet Usage (6.43%), Mobile Phone Usage (5.95%).
  - **After Imputation**: Business Tax Rate (0%), Days to Start Business (0%), Ease of Business (0%), Hours to do Tax (0%), Internet Usage (5.50%), Mobile Phone Usage (5.28%).

Considering the results, the first four columns should've been dropped(But I kept it).

In the workbook **Final Analysis_Ashutosh Ranjan**, the **Processed_Dataset** sheet was created by combining the data using the `VLOOKUP` function.
`=ARRAYFORMULA(VLOOKUP(A2,IMPORTRANGE("https://docs.google.com/spreadsheets/d/1A_P1bfBREmA7JOWyKv-RbXbZLXN80F8mJRT1h9WAfwM/edit#gid=1931367683","ENERGY!$F$1:$H$2692"),{2,3},0))`
This sheet, along with the **DICTIONARY** sheet, was utilized for univariate and bivariate analysis.

### UNIVARIATE & BIVARIATE ANALYSIS

In the **Final Analysis_Ashutosh Ranjan** workbook's **Processed_Dataset** sheet, I added new columns:

- **EnergyUsage/Capita**
- **CO2Emission/Capita**
- **TourismInbound/Population**
- **TourismOutbound/Population**

For the **missing_EnergyUsage/Capita** and **missing_CO2Emission/Capita** columns, I imputed missing values using the formula `=IF(Z2="",AVERAGEIF($AD2:$AD2692,$AD2,Z$2:Z$2692),Z2)`. These columns were highlighted in yellow for subsequent univariate and bivariate analysis.

Upon the deletion of the #N/A values, the percentages of missing values were as follows:

- **Energy Usage**: 36.68%
- **CO2 Emissions**: 25.98%
- **EnergyUsage/Capita**: 36.68%
- **CO2Emission/Capita**: 25.98%
- **Population**: 0

In the bivariate analysis, I calculated correlation coefficients using the `=CORREL()` function in the "Bivariate01" to "Bivariate05" sheets, enabling the visualization of data trends through scatter plots. Additionally, I conducted comprehensive univariate analyses, obtaining valuable insights for the final conclusion presented in the "Sheet 18."

Insights gained from the analyses indicated significant correlation percentages with GDP/Capita, highlighting crucial factors for sustainable economic growth:

- **Positive Impact**:

  - Health Exp/Capita: 87.68%
  - Internet Usage: 72.90%
  - Life Expectancy Male: 55.36%
  - Population 65+: 53.68%
  - Life Expectancy Female: 50.69%
  - Population 15-64: 48.74%
  - Mobile Phone Usage: 46.13%
  - Population Urban: 45.09%
  - Tourism Outbound: 42.74%
  - Tourism Inbound: 38.81%
  - missing_CO2Emission/Capita: 34.49%
  - missing_EnergyUsage/Capita: 33.20%
  - Missing_Health Exp % GDP: 20.64%

- **Negative Impact**:
  - Population Total: -4.66%
  - Missing_Lending Interest: -9.27%
  - Business Tax Rate: -16.57%
  - Days to Start Business: -17.62%
  - Hours to do Tax: -26.16%
  - Infant Mortality Rate: -46.18%
  - Birth Rate: -47.38%
  - Population 0-14: -56.82%
  - Ease of Business: -57.21%

To foster improved GDP/Capita, enhancing factors such as Health Expenditure per Capita, Internet Usage, Ease of Business, Life Expectancy Male and Female, Population Over 65, Population 15-64, Mobile Phone Usage, Urban Population, Outbound and Inbound Tourism is recommended. Simultaneously, reducing factors including Infant Mortality Rate, Birth Rate, and Population below 14 is vital for sustained economic growth.

![image](https://github.com/Ashutosh-Singh-Rajput2209/EconomicElevate-Unveiling-Sustainable-GDP-Drivers-and-Strategies/assets/113265828/98ba9d1c-8a2f-41dd-b15f-1705c1ba09fd)

