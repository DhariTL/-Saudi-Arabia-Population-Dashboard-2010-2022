<div align="center">

# 🇸🇦 Saudi Arabia Population Dashboard 2010–2022
### لوحة تحليل التعداد السكاني في المملكة العربية السعودية

An interactive **Power BI** dashboard analysing population growth, nationality mix and gender balance across the 13 regions of Saudi Arabia.


</div>

<div dir="rtl">
  
## 🇸🇦 نبذة عن المشروع

لوحة تفاعلية بـ **Power BI** تحلل التعداد السكاني في المملكة العربية السعودية خلال الفترة **2010–2022** حسب المنطقة والجنس والجنسية، وتجيب على الأسئلة التالية:

- ما المناطق الأسرع نمواً؟
- كيف تغيّرت نسبة غير السعوديين مع الوقت؟
- ما التوازن بين الذكور والإناث؟
- أي المناطق تضم النصيب الأكبر من السكان؟

### أهم المقاييس

| المقياس | الوصف |
|---|---|
| **CAGR** | معدل النمو السنوي المركب بين 2010 و2022 |
| **Non-Saudi %** | نسبة غير السعوديين من السكان |
| **Sex Ratio** | عدد الذكور مقابل كل 100 أنثى |
| **Region Share** | حصة المنطقة من إجمالي السكان |
| **YoY %** | التغير السنوي في عدد السكان |

### أبرز النتائج

- نما عدد السكان من حوالي **24 مليون** إلى **32 مليون** بمعدل نمو سنوي مركب **2.48%**.
- بقيت نسبة غير السعوديين بين **41% و47%** تقريباً طوال الفترة.
- سجّلت **حائل** أعلى معدل نمو بين المناطق، و**مكة المكرمة** أقل معدل.

</div>


---

## 📸 Preview

<img width="1954" height="1098" alt="image" src="https://github.com/user-attachments/assets/3b4805bb-4877-47dd-8045-d3499301e231" />

---
<img width="1952" height="1093" alt="image" src="https://github.com/user-attachments/assets/70cc32c3-8479-49e5-a377-2d6ea3230fc4" />

---
<img width="1955" height="1100" alt="image" src="https://github.com/user-attachments/assets/53155a04-4930-4011-8fc3-8d8a70e373c7" />

---

## 🎯 Project Overview

This project turns raw population estimates into a clear, interactive report that answers four questions:

1. **How fast is each region growing?**
2. **How has the Saudi / non-Saudi composition changed over time?**
3. **How balanced is the population by gender?**
4. **Which regions carry the largest share of the population?**

---

## 📂 Dataset

| Item | Details |
|---|---|
| **File** | `Population estimates by gender nationality and region 2010 - 2022_data.csv` |
| **Rows** | 676 |
| **Period** | 2010 – 2022 |
| **Granularity** | Region × Year × Gender × Nationality |
| **Columns** | `Region`, `Year`, `Gender`, `Nationality`, `Population` |
| **Regions** | 13 administrative regions |

---

## 🧹 Data Preparation (Power Query)

- Converted `Year` and `Population` from text to **whole numbers** using *Change Type → Using Locale (English – United States)* to remove thousands separators (e.g. `2,014` → `2014`).
- Renamed the table to `PopData` to keep DAX readable.
- Verified there were no duplicates or missing values (13 regions × 13 years × 2 genders × 2 nationalities = 676 rows).

---

## 📐 DAX Measures

```DAX
Total Population = SUM(PopData[Population])

Pop 2010 = CALCULATE([Total Population], PopData[Year] = 2010)
Pop 2022 = CALCULATE([Total Population], PopData[Year] = 2022)

Pop Latest =
CALCULATE([Total Population], PopData[Year] = MAX(PopData[Year]))

-- Compound Annual Growth Rate (blank-safe)
CAGR =
VAR p10 = [Pop 2010]
VAR p22 = [Pop 2022]
RETURN
IF(
    NOT ISBLANK(p10) && NOT ISBLANK(p22),
    DIVIDE(p22, p10) ^ (1 / 12) - 1
)

Non-Saudi Population =
CALCULATE([Total Population], PopData[Nationality] = "Non-Saudi")

Non-Saudi % = DIVIDE([Non-Saudi Population], [Total Population])

Male Population =
CALCULATE([Total Population], PopData[Gender] = "Male")

Female Population =
CALCULATE([Total Population], PopData[Gender] = "Female")

Sex Ratio = DIVIDE([Male Population], [Female Population]) * 100

Region Share =
DIVIDE(
    [Total Population],
    CALCULATE([Total Population], ALL(PopData[Region]))
)

Pop Prev Year =
CALCULATE([Total Population], PopData[Year] = MAX(PopData[Year]) - 1)

YoY % =
DIVIDE([Total Population] - [Pop Prev Year], [Pop Prev Year])
```

| Measure | Meaning |
|---|---|
| **CAGR** | Average yearly growth between 2010 and 2022 |
| **Non-Saudi %** | Share of non-Saudi residents in the population |
| **Sex Ratio** | Males per 100 females (100 = balanced) |
| **Region Share** | A region's share of the national total |
| **YoY %** | Year-over-year population change |

---

## 📊 Dashboard Components

| Visual | Purpose |
|---|---|
| **KPI cards** (CAGR, Sex Ratio) | Headline indicators at a glance |
| **Year slicer** | Filter the report to any year |
| **Area chart** – Non-Saudi share | Composition trend over time |
| **Donut chart** – Population by gender | Latest-year gender split |
| **Clustered bar** – Population by region & nationality | Regional size comparison |
| **Area chart** – Population trend | Total growth 2010–2022 |
| **Bar chart** – CAGR by region | Fastest- vs slowest-growing regions |

---

## 🔍 Key Insights

- 📈 Total population grew from roughly **24M (2010)** to **32M (2022)**, a CAGR of about **2.48%**.
- 🌍 The non-Saudi share stayed within a narrow band of roughly **41%–47%** across the period.
- 🏆 **Hail** shows the highest regional growth rate (~2.9% per year), while **Makkah** shows the lowest (~2%).
- 🏙️ **Riyadh**, **Makkah** and the **Eastern Region** hold the largest populations.
- ⚖️ The male-to-female ratio is well above 100, driven mainly by the non-Saudi population.

---

## 🛠️ Challenges & Solutions

| Challenge | Solution |
|---|---|
| Numbers stored as text with thousands separators | Converted types with an English (US) locale in Power Query |
| `Pop Prev Year` returned blanks when 2010 was selected | Used `Pop Latest` for visuals showing the selected year; kept `Pop Prev Year` only for `YoY %` |
| CAGR showed **-100%** when a region was cross-filtered | Made the measure blank-safe with `ISBLANK` checks |
| Cross-filtering distorted the CAGR chart | Disabled the interaction via *Edit interactions → None* |
| Ambiguous region names plotted on the wrong map | Added a `Map Location` column that includes the country name |

---

## 🚀 How to Use

1. Clone or download this repository.
2. Open `Saudi_Population_Dashboard.pbix` in **Power BI Desktop**.
3. Use the **Year** slicer and click any region to explore the data.

```
📁 Saudi-Population-Dashboard
 ├── 📄 Saudi_Population_Dashboard.pbix
 ├── 📄 Population estimates by gender nationality and region 2010 - 2022_data.csv
 ├── 📁 images
 │    └── dashboard.png
 └── 📄 README.md
```

---

## 🧰 Tools

- **Power BI Desktop**: report design and interactivity
- **Power Query**: data cleaning and type conversion
- **DAX**: calculated measures

---

---

## 👤 Author

**Dhari Talal Alshammari**
Information Systems Graduate | Aspiring Data Analyst

[![GitHub](https://img.shields.io/badge/GitHub-DhariTL-181717?logo=github)](https://github.com/DhariTL)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Dhari%20Alshammari-0A66C2?logo=linkedin)](https://linkedin.com/in/dhari-alshammari-4b9797299)
