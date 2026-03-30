# Sales Dashboard — Power BI PBIR Report

A multi-page financial sales dashboard built in **Power BI PBIR format** (open folder-based report structure), using the Microsoft Financial Sample dataset. The report covers revenue, profit, cost, and year-over-year performance across products, segments, and countries.

---

## Project Structure

```
PBIR/
├── Sales Report.pbip          # Power BI project entry point — open this in Power BI Desktop
├── Sales Report.pbix          # Legacy PBIX backup
├── Sales Report.Report/       # Report definition (pages, visuals, layout)
│   └── definition/
│       └── pages/             # One folder per report page
│           ├── pages.json     # Page order & active page
│           ├── <page-id>/
│           │   ├── page.json  # Canvas size, background, display options
│           │   └── visuals/
│           │       └── <visual-id>/
│           │           └── visual.json  # Visual type, field bindings, formatting
└── Sales Report.SemanticModel/  # Data model definition (TMDL format)
    └── definition/
        ├── model.tmdl         # Model-level settings
        ├── database.tmdl      # Database metadata
        ├── relationships.tmdl # Table relationships
        └── tables/
            └── financials.tmdl  # Table schema, columns & DAX measures
```

---

## Report Pages

| Tab | Canvas | Description |
|-----|--------|-------------|
| **Sales Dashboard** | 1280×720 | V1 — KPI cards, line chart, bar chart, column chart, donut chart |
| **Sales Dashboard V2** | 1280×720 | CFO-ready redesign with navy header, year slicer, improved layout |
| **Sales Dashboard V3** | 1280×720 | Full-featured: 7 KPI cards, trend line, donut, matrix, bar chart |
| **Overview** | 1366×768 | Pixel-accurate replica of reference design with small-multiples bar chart |

---

## Data Source

**Microsoft Financial Sample** — built-in Excel workbook shipped with Power BI Desktop.

Path (default): `C:\Program Files\WindowsApps\Microsoft.MicrosoftPowerBIDesktop_*\bin\SampleData\Financial Sample.xlsx`

Table: `financials`

| Column | Type | Description |
|--------|------|-------------|
| Segment | Text | Customer segment (Small Business, Enterprise, etc.) |
| Country | Text | 5 countries (Canada, France, Germany, Mexico, USA) |
| Product | Text | 6 products (Paseo, VTT, Velo, Amarilla, Montana, Carretera) |
| Discount Band | Text | Discount tier |
| Units Sold | Number | Units sold per row |
| Manufacturing Price | Integer | Cost to manufacture per unit |
| Sale Price | Integer | Selling price per unit |
| Gross Sales | Number | Revenue before discounts |
| Discounts | Number | Discount value |
| Sales | Number | Net sales (Gross Sales − Discounts) |
| COGS | Number | Cost of goods sold |
| Profit | Number | Sales − COGS |
| Date | Date | Transaction date |
| Month Number | Integer | 1–12 |
| Month Name | Text | Jan–Dec |
| Year | Integer | Fiscal year |

---

## DAX Measures

All measures are defined in `Sales Report.SemanticModel/definition/tables/financials.tmdl` under the `KPIs` display folder.

| Measure | Formula | Format |
|---------|---------|--------|
| `Profit Margin %` | `DIVIDE(SUM([Profit]), SUM([Sales]), 0)` | 0.00% |
| `Total Units` | `SUM([Units Sold])` | #,0 |
| `Total COGS` | `SUM([COGS])` | $#,0 |
| `Total Discounts` | `SUM([Discounts])` | $#,0 |
| `Sales PY` | `CALCULATE(SUM([Sales]), PREVIOUSYEAR([Date]))` | $#,0 |
| `Sales YoY` | `SUM([Sales]) - [Sales PY]` | $#,0 |
| `Sales YoY %` | `DIVIDE([Sales YoY], [Sales PY], 0)` | 0.00% |
| `Units PY` | `CALCULATE(SUM([Units Sold]), PREVIOUSYEAR([Date]))` | #,0 |
| `Units YoY` | `SUM([Units Sold]) - [Units PY]` | #,0 |
| `Units YoY %` | `DIVIDE([Units YoY], [Units PY], 0)` | 0.00% |

---

## How to Open

1. Open **Power BI Desktop** (version 2.152 or later recommended)
2. Go to **File → Open report → Browse**
3. Navigate to this folder and open **`Sales Report.pbip`**
4. Power BI will load the semantic model and all report pages automatically

> **Note:** The data source points to the sample Excel file bundled with Power BI Desktop. If your install path differs, go to **Transform Data → Data source settings** and update the path.

---

## PBIR Format Notes

PBIR (Power BI Project — Report) is Power BI's open, Git-friendly folder format. Key points:

- Every visual is a standalone `visual.json` file — easy to diff and review in pull requests
- The semantic model uses **TMDL** (Tabular Model Definition Language) — plain-text, human-readable
- Report and model are fully separated, making independent version control practical
- Schema versions used: visual container `2.7.0`, page `2.1.0`, pages metadata `1.0.0`

---

## Requirements

- Power BI Desktop **February 2024** or later (PBIR format support)
- Windows 10/11
- Microsoft Financial Sample Excel file (bundled with Power BI Desktop)

---

## License

This project uses Microsoft's publicly distributed Financial Sample dataset for educational and demonstration purposes.
