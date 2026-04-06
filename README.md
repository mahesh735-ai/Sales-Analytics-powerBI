# Sales-Analytics-powerBI
📊 Built a Sales Analytics Dashboard in Power BI from scratch! Raw CSV → Power Query → DAX → Interactive Dashboard. KPIs, 6 visuals, Bookmarks &amp; custom Figma UI. Insights on sales, customer retention &amp; monthly trends. 🚀 #PowerBI #DataAnalytics
# 📊 Sales Analytics Dashboard — Power BI End-to-End Project

> A professional Sales Analytics Dashboard built in Power BI using retail transaction data. This project covers the full data pipeline: data import → cleaning → DAX measures → visualization → UI/UX design with bookmarks and filter pane.

---

## 🖼️ Dashboard Preview

![Sales Analytics Dashboard](./1775380901482_image.png)

---

## 📁 Project Structure

```
Sales-Analytics-PowerBI/
│
├── Sales_Dashboard.pbix        # Main Power BI file
├── retail_sales.csv            # Raw dataset (1126 transactions)
├── assets/
│   ├── BG.png                  # Dashboard background (designed in Figma)
│   ├── Filter_BG.png           # Filter pane background
│   ├── Slicer_Open.png         # Filter open button icon
│   ├── Slicer_Close.png        # Filter close button icon
│   ├── sales.png               # KPI icon - Sales
│   ├── increase.png            # KPI icon - Transactions
│   ├── coupon.png              # KPI icon - Avg Order Value
│   ├── price-tag.png           # KPI icon - Avg Basket Size
│   └── sale-tag.png            # KPI icon - Customers
└── README.md
```

---

## 📂 Dataset Overview

**File:** `retail_sales.csv`  
**Rows:** 1,126 transactions | **Customers:** 269 unique

| Column | Data Type | Description |
|---|---|---|
| Transaction ID | Whole Number | Unique transaction identifier |
| Date | Date | Transaction date |
| Customer ID | Text | Unique customer identifier |
| Gender | Text | Male / Female |
| Age | Whole Number | Customer age |
| Product Category | Text | Beauty, Clothing, Electronics, Groceries, Home Decor |
| Quantity | Whole Number | Items purchased |
| Price per Unit | Whole Number | Unit price |
| Total Amount | Whole Number | Transaction value |

---

## 📐 Data Preparation Steps

### Power Query (ETL)
- Loaded CSV via **Get Data → Text/CSV**
- Verified auto-applied steps: Source → Promoted Headers → Changed Types
- Validated all columns — no nulls or blank values found
- Confirmed correct data types for all numerical and date fields

### Calculated Column (DAX)
```dax
Age Bucket =
SWITCH(
    TRUE(),
    retail_sales[Age] >= 51, ">51",
    retail_sales[Age] >= 41, "41-50",
    retail_sales[Age] >= 31, "31-40",
    retail_sales[Age] >= 21, "21-30",
    retail_sales[Age] <= 20, "<20"
)
```

```dax
Month = FORMAT(retail_sales[Date], "MM")
```

---

## 📊 DAX Measures

All measures are stored in a dedicated **Measure Table** for clean organization.

```dax
Total Sales = SUM(retail_sales[Total Amount])

Total Customers = DISTINCTCOUNT(retail_sales[Customer ID])

Total Transactions = DISTINCTCOUNT(retail_sales[Transaction ID])

Average Order Value = [Total Sales] / [Total Transactions]

Average Basket Size =
VAR TotalQty = SUM(retail_sales[Quantity])
RETURN TotalQty / [Total Transactions]
```

### Customer Type Logic (Summarized Table)
```dax
Customer Type_2 =
SUMMARIZE(
    retail_sales,
    retail_sales[Customer ID],
    "Total Transaction", DISTINCTCOUNT(retail_sales[Transaction ID]),
    "Customer Type",
        IF(DISTINCTCOUNT(retail_sales[Transaction ID]) >= 2, "Repeat", "One Time")
)
```

**Relationship:** `Customer Type_2[Customer ID]` → `retail_sales[Customer ID]`  
**Cardinality:** Many-to-Many | **Cross Filter Direction:** retail_sales → Customer Type_2

---

## 📈 Dashboard KPIs

| KPI | Value |
|---|---|
| 💰 Total Sales | ₹574,000 |
| 👥 Total Customers | 269 |
| 🧾 Total Transactions | 1,126 |
| 📦 Avg Order Value | ₹509.77 |
| 🛒 Avg Basket Size | 2.50 |

---

## 📉 Chart Insights

### 1. Sales & Avg Basket Size by Product Category
| Category | Sales | Avg Basket Size |
|---|---|---|
| Groceries | ₹130,650 | ~2.55 |
| Clothing | ₹114,900 | ~2.48 |
| Home Decor | ₹109,750 | — |
| Beauty | ₹109,750 | — |
| Electronics | ₹108,950 | — |

> **Insight:** Groceries leads in both sales volume and basket size, suggesting customers buy groceries in larger quantities per visit.

### 2. Sales by Gender (Donut Chart)
| Gender | Sales | Share |
|---|---|---|
| Male | ₹296,950 | 51.73% |
| Female | ₹277,050 | 48.27% |

> **Insight:** Sales are nearly balanced across genders, indicating broad demographic appeal with a slight male skew.

### 3. Sales by Age Bucket (Bar Chart)
| Age Bucket | Sales |
|---|---|
| 31-40 | ₹154,850 ⬆️ Highest |
| 41-50 | ₹139,450 |
| 21-30 | ₹129,700 |
| >51 | ₹112,900 |
| <20 | ₹37,100 |

> **Insight:** The 31-40 age group drives the most revenue. The under-20 segment is significantly underperforming — potential growth opportunity.

### 4. Customer Type (Donut Chart)
| Type | Count |
|---|---|
| Repeat Customers | 185 (69%) |
| One-Time Customers | 84 (31%) |

> **Insight:** Strong customer retention at 69%. Focus on converting the 31% one-time customers through loyalty programs or targeted campaigns.

### 5. Sales Matrix — Age Bucket × Product Category
> Cross-analysis reveals which products resonate with which age groups, enabling category-specific marketing strategies.

### 6. Monthly Sales Trend (Line Chart)
| Month | Sales | Notes |
|---|---|---|
| January | ₹51,050 | Strong start |
| May | ₹53,200 | Peak month |
| July | ₹53,150 | Second highest |
| March | ₹41,700 | Lowest month |

> **Insight:** May and July are peak sales months. March shows the lowest sales — a potential opportunity for mid-quarter promotions.

---

## 🎨 UI/UX Features

- **Custom Background:** Designed in Figma with a dark navy glass-morphism theme
- **Conditional Formatting:** Matrix visual uses gradient blue shading — darker = higher sales
- **Interactive Filter Pane:** Bookmarks used to create open/close slicer animation
- **Custom Icons:** Embedded PNG icons on each KPI card for visual appeal
- **Slicers:** Product Category, Age Bucket, Gender, Month, Customer Type

---

## 🧠 What I Learned

- **Power Query** — Importing, transforming, and validating CSV data
- **DAX Measures** — Writing calculated measures and columns (SWITCH, SUMMARIZE, IF)
- **Data Modeling** — Creating table relationships with correct cardinality and filter direction
- **Bookmarks & Selections** — Building an interactive filter pane toggle with bookmarks
- **Conditional Formatting** — Applying gradient background colors to matrix visuals
- **Dashboard Design** — Using Figma-designed backgrounds and custom icons in Power BI
- **Business Thinking** — Translating raw data into actionable customer and sales insights

---

## ⚡ Challenges Faced & Solutions

| Challenge | Solution |
|---|---|
| Filter pane slicer not filtering other visuals | Changed relationship cardinality from Many-to-One to Many-to-Many and set cross-filter direction correctly |
| Month column showing wrong order | Created a numeric `Month` column using `FORMAT(Date, "MM")` for proper sorting |
| Matrix values not visible (text color issue) | Applied conditional formatting specifically to values; manually set text color for clarity |
| KPI cards alignment inconsistency | Set exact Height (90) and Width (195) in DXA properties and used Power BI alignment tools |

---

## 🛠️ Tools & Technologies

- **Power BI Desktop** — Dashboard development
- **Power Query** — Data transformation (ETL)
- **DAX** — Measures, calculated columns, summarized tables
- **Figma** — Custom background and UI design
- **CSV (retail_sales.csv)** — Source dataset

---

## 🚀 How to Use

1. Clone/download this repository
2. Open `Sales_Dashboard.pbix` in **Power BI Desktop**
3. If prompted, update the data source path to point to `retail_sales.csv` in your local folder
4. Explore the dashboard — use the 🔽 filter icon (top right) to open the filter pane

---

## 📌 Project Reference

This is a guided project inspired by the tutorial:  
**[Sales Analytics Dashboard In Power BI End to End Project | Data Minds Academy ](https://www.youtube.com/watch?v=WIOlpCVFQXc)**

---

## 👤 Author

**[Mahesh Thakare]**  
Aspiring Data Analyst | Power BI | Python | SQL  
📧 [maheshthakare225@gmail.com]  
🔗 [LinkedIn Profile](www.linkedin.com/in/mahesh-thakare-75817b2a7)

---

*If you found this project helpful, feel free to ⭐ star the repository!*
