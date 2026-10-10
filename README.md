<div align="center">

# 🇸🇦 Saudi Arabia Population Dashboard (2010–2022)
### لوحة تحليل التعداد السكاني في المملكة العربية السعودية

An interactive **Power BI** dashboard exploring population estimates across Saudi Arabia's 13 regions by gender and nationality.
<br>
لوحة تفاعلية على **Power BI** تستعرض تقديرات السكان في مناطق المملكة الـ 13 حسب الجنس والجنسية.
</div>

---

## 📸 Preview

### 🗺️ Full Dashboard · نظرة عامة على اللوحة
<img width="2121" height="1191" alt="image" src="https://github.com/user-attachments/assets/2be1d257-1749-4bd8-96e4-65c2ddcad87e" />

### 🏙️ Riyadh in 2019 · منطقة الرياض عام 2019
<img width="2117" height="1191" alt="image" src="https://github.com/user-attachments/assets/84f85de7-c15e-4462-9225-abe179909e59" />

### 🕋 Makkah in 2013 · منطقة مكة المكرمة عام 2013
<img width="2120" height="1191" alt="image" src="https://github.com/user-attachments/assets/b70929b9-4ac0-4a17-aea2-084c4f6eba7f" />

---

## 🎯 Overview | نظرة عامة

This project turns a population dataset (**676 rows · 13 regions · 2010–2022**) into a single-page, fully interactive report. It answers questions such as:

- How has the Kingdom's population grown between 2010 and 2022?
- Which regions are growing fastest?
- How do Saudi and non-Saudi populations compare across regions?
- What is the gender balance, and how has it shifted over time?

يحوّل هذا المشروع بيانات السكان إلى تقرير تفاعلي في صفحة واحدة، يجيب عن أسئلة مثل: كيف تطور عدد السكان؟ أي المناطق الأسرع نمواً؟ وكيف تتوزع الجنسية والجنس بين المناطق؟

---

## ✨ Key Insights | أبرز النتائج

| Metric | Value (All years / latest) |
|---|---|
| 👥 Total population (2022) | **~32M** |
| 👨 Males | **~20M** |
| 👩 Females | **~12M** |
| ⚖️ Sex ratio (males per 100 females) | **157.47** |
| 📈 Compound annual growth rate (CAGR) | **2.48%** |

- Population grew from roughly **24M in 2010 to 32M in 2022**.
- **Riyadh** and **Makkah** are the most populated regions by a wide margin, followed by the Eastern Region.
- **Hail** ranks highest in compound annual growth among regions, while **Makkah** ranks lowest.
- The non-Saudi share of the population stays within roughly **40–50%** across the period, peaking around 2016.

> 📌 Figures are read from the dashboard at the default (all years) selection and will change with the slicer.

---

## 🧩 Dashboard Components | مكونات اللوحة

| Visual | Purpose |
|---|---|
| **Year slicer (Tile)** | Filter the entire report by year |
| **KPI cards** | Total population, males, females, sex ratio, CAGR |
| **Area chart** | Non-Saudi share of the population over time |
| **Azure Map** | Geographic distribution by region, using custom latitude/longitude |
| **Clustered bar chart** | Population by region, split by Saudi / non-Saudi |
| **Area chart** | Total population growth 2010–2022 |
| **Bar chart** | CAGR by region |

All visuals are **cross-filtered**: selecting a region on the map or in any chart updates every KPI and chart on the page.

---

## 🛠️ Technical Details | التفاصيل التقنية

### Tools
- **Power BI Desktop** — data modeling, DAX, report design
- **Azure Maps visual** — region map with dark grayscale style
- **Custom background & card styling** — Saudi-themed dark green design with glow effect

### Data Model
A single table, `PopData`, with the following key fields:

`Year` · `Region` · `Gender` · `Nationality` · `Population`

### Calculated Columns
```DAX
Region AR = SWITCH([Region],
    "Riyadh", "الرياض",
    "Makkah", "مكة المكرمة",
    "Madinah", "المدينة المنورة",
    "Eastern Region", "المنطقة الشرقية",
    "Qaseem", "القصيم",
    "Aseer", "عسير",
    "Tabouk", "تبوك",
    "Hail", "حائل",
    "Northern Region", "الحدود الشمالية",
    "Al-Jouf", "الجوف",
    "Jazan", "جازان",
    "Najran", "نجران",
    "Al-Baha", "الباحة",
    [Region])

Nationality AR = IF([Nationality] = "Saudi", "سعودي", "غير سعودي")
```
Latitude / Longitude columns (`Lat`, `Lon`) are also added per region so the map plots accurate positions instead of relying on text geocoding.

### Selected Measures
```DAX
Male Population =
CALCULATE(
    SUM(PopData[Population]),
    PopData[Gender] = "Male",
    PopData[Year] = MAX(PopData[Year])
)

Female Population =
CALCULATE(
    SUM(PopData[Population]),
    PopData[Gender] = "Female",
    PopData[Year] = MAX(PopData[Year])
)

Sex Ratio = DIVIDE([Male Population], [Female Population]) * 100
```
> Measures use the latest selected year (`MAX(Year)`) so totals stay consistent when no year is selected.

Additional measures include `Total Population`, `CAGR`, `YoY %`, `Non-Saudi %` and `Region Share`.

---

## 💡 What I Learned | ما تعلمته

- Fixing **map geocoding issues** by supplying explicit latitude/longitude instead of text-only locations.
- Writing **time-aware DAX measures** that avoid summing across years.
- Designing a **bilingual, RTL-friendly** report with Arabic titles, labels and legends.
- Using **cross-filtering and slicers** to keep a dense one-page layout readable.

---

## 📁 Repository Structure

```
├── 📄 README.md
├── 📊 Saudi-Population-Dashboard.pbix
├── 📂 data/            # source dataset
└── 📂 images/          # dashboard screenshots
```

---

## 👤 Author | المطوّر

**Dhari Talal Alshammari**
Information Systems graduate, Northern Border University — aspiring Data Analyst.

[![GitHub](https://img.shields.io/badge/GitHub-DhariTL-181717?style=flat&logo=github)](https://github.com/DhariTL)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Dhari%20Alshammari-0A66C2?style=flat&logo=linkedin)](https://linkedin.com/in/dhari-alshammari-4b9797299)

---

<div align="center">

⭐ If you found this project useful, consider giving it a star.

</div>
