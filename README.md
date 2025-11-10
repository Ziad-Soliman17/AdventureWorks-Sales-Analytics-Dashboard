# AdventureWorks Sales Analytics Dashboard

A comprehensive Power BI analytics solution for sales performance tracking, product analysis, and customer segmentation using the AdventureWorks data warehouse.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![SQL Server](https://img.shields.io/badge/SQL%20Server-CC2927?style=for-the-badge&logo=microsoft-sql-server&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-blue?style=for-the-badge)

## 📊 Project Overview

This project delivers an end-to-end business intelligence solution that transforms raw AdventureWorks data into actionable insights through interactive dashboards. The solution covers three key business areas:

- **Sales Performance** - Revenue tracking, profit analysis, and channel performance
- **Product Analytics** - Product profitability segmentation and inventory insights
- **Customer Intelligence** - RFM segmentation and behavioral analysis

## 🎯 Key Features

### Sales Overview Dashboard

- **KPI Cards**: Net Sales ($48.4M), Net Profit ($26.6M), Profit Margin (55%)
- **YoY Analysis**: Prior year comparisons with variance percentage
- **Trend Analysis**: Sales over time with Month-over-Month % change
- **Channel Performance**: Sales distribution across Reseller vs Customer channels
- **Geographic Filtering**: Country-level analysis capabilities

### Product Performance Dashboard

- **Product Metrics**: Total products sold (183), Total quantity (166K)
- **Profitability Matrix**: Four-quadrant analysis (Star, Volume Driver, High Potential, Underperformer)
- **Category Analysis**: Performance breakdown by subcategory
- **Detailed Product Table**: Drill-down capabilities with profit margins

### Customer Segmentation Dashboard

- **Customer Metrics**: Total customers (18K), Total orders (21K), AOV ($706.6)
- **RFM Segmentation**: Advanced customer classification (Champions, Loyal, At Risk, Lost, etc.)
- **Demographic Analysis**: Age groups and gender distribution
- **Customer Details**: Individual-level recency, frequency, and monetary value

## 🏗️ Architecture

### Technology Stack

- **Database**: SQL Server (AdventureWorksDW2019)
- **ETL**: T-SQL for data extraction and preparation
- **Visualization**: Power BI Desktop
- **Analytics**: DAX for business logic and calculations

### Data Model

```
Star Schema Design:
- Fact Table: Sales_fact (Internet + Reseller sales combined)
- Dimensions: Date, Product, Customer, Reseller, Channel, Sales Territory
```

## 📁 Project Structure

```
AdventureWorks-Sales-Analytics/
│
├── SQL/
│   ├── 01_FactSales_Extract.sql          # Sales fact table extraction
│   ├── 02_DimDate_Extract.sql            # Date dimension
│   ├── 03_DimChannel_Extract.sql         # Channel dimension
│   ├── 04_DimCustomer_Extract.sql        # Customer dimension
│   ├── 05_DimReseller_Extract.sql        # Reseller dimension
│   ├── 06_DimProduct_Extract.sql         # Product dimension
│   └── 07_DimSalesTerritory_Extract.sql  # Sales territory dimension
│
├── PowerBI/
│   ├── AdventureWorks_Sales_Analytics.pbix
│   └── Report_Documentation.pdf
│
├── Documentation/
│   ├── Data_Model_Diagram.png
│   ├── DAX_Measures.md
│   └── Business_Requirements.md
│
└── README.md
```

## 🚀 Getting Started

### Prerequisites

- SQL Server 2019 or later
- AdventureWorksDW2019 database
- Power BI Desktop (latest version)
- Basic knowledge of SQL and DAX

### Installation Steps

1. **Clone the repository**
   
   ```bash
   git clone https://github.com/yourusername/adventureworks-sales-analytics.git
   cd adventureworks-sales-analytics
   ```
1. **Restore AdventureWorks Database**
- Download [AdventureWorksDW2019.bak](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)
- Restore to your SQL Server instance
1. **Execute SQL Scripts**
- Open SQL Server Management Studio
- Connect to your database
- Execute scripts in the `SQL/` folder in order (01-07)
- Verify data extraction results
1. **Open Power BI Report**
- Open `PowerBI/AdventureWorks_Sales_Analytics.pbix`
- Update data source connections to point to your SQL Server
- Refresh data model
1. **Explore the Dashboard**
- Navigate through the three report pages
- Apply filters and interact with visualizations
- Customize as needed for your use case

## 📊 Key DAX Measures

### Sales Metrics

```dax
Net Sales = 
    SUMX(
        Sales_fact,
        Sales_fact[SalesAmount] - Sales_fact[TaxAmt]
    )

Net Profit = 
    SUMX(
        Sales_fact,
        Sales_fact[SalesAmount] - Sales_fact[TaxAmt] - Sales_fact[TotalProductCost]
    )

Profit Margin = 
    DIVIDE([Net Profit], [Net Sales], 0)
```

### Time Intelligence

```dax
PY Sales = 
    CALCULATE(
        [Net Sales],
        DATEADD(Date_dim[FullDateAlternateKey], -1, YEAR)
    )

YoY % = 
    DIVIDE([Net Sales] - [PY Sales], [PY Sales], 0)

MoM % = 
    VAR CurrentSales = [Net Sales]
    VAR PreviousMonthSales = 
        CALCULATE(
            [Net Sales],
            DATEADD(Date_dim[FullDateAlternateKey], -1, MONTH)
        )
    RETURN DIVIDE(CurrentSales - PreviousMonthSales, PreviousMonthSales, 0)
```

### RFM Segmentation

```dax
RFM Segment = 
    VAR R_Score = [Recency Score]
    VAR F_Score = [Frequency Score]
    VAR M_Score = [Monetary Score]
    RETURN
        SWITCH(
            TRUE(),
            R_Score >= 4 && F_Score >= 4, "Champions",
            R_Score >= 3 && F_Score >= 3, "Loyal Customers",
            R_Score >= 4 && F_Score <= 2, "Potential Loyalist",
            R_Score <= 2 && F_Score >= 3, "At Risk",
            R_Score <= 2 && F_Score <= 2, "Lost",
            "Need Attention"
        )
```

## 📈 Business Insights

### Sales Performance

- **Revenue Growth**: 56% YoY increase with strong seasonal patterns
- **Channel Mix**: Reseller channel drives 68% of volume
- **Profitability**: Healthy 55% profit margin across all channels

### Product Analysis

- **Top Categories**: Touring bikes and mountain bikes lead in volume
- **High Margin Products**: Women’s Mountain Shorts show 85%+ margins
- **Portfolio Mix**: Balanced distribution across Star and Volume Driver segments

### Customer Behavior

- **Segmentation**: 18K customers across 6 RFM segments
- **Order Patterns**: Average order value of $706.6
- **Demographics**: Balanced gender split with concentration in 40-59 age range

## 🔧 Customization Guide

### Adding New Measures

1. Open Power BI file
1. Navigate to “Modeling” tab
1. Click “New Measure”
1. Write DAX formula
1. Format and test

### Modifying Visuals

1. Select visual on report canvas
1. Use “Visualizations” pane to change type
1. Drag fields from “Fields” pane
1. Apply conditional formatting as needed

### Updating Data Source

1. Home → Transform Data → Data Source Settings
1. Update server and database name
1. Test connection
1. Refresh queries

## 🐛 Known Issues & Limitations

- Date range limited to 2011-2014 (AdventureWorks sample data constraint)
- RFM segmentation requires manual refresh for new customer data
- Some products may show “Others” in category due to incomplete hierarchies
- Performance may degrade with very large date ranges

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
1. Create a feature branch (`git checkout -b feature/AmazingFeature`)
1. Commit changes (`git commit -m 'Add AmazingFeature'`)
1. Push to branch (`git push origin feature/AmazingFeature`)
1. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the <LICENSE> file for details.

## 👥 Authors

- **Your Name** - *Initial work* - [YourGitHub](https://github.com/yourusername)

## 🙏 Acknowledgments

- Microsoft for the AdventureWorks sample database
- Power BI community for DAX patterns and best practices
- SQL Server documentation team

## 📞 Contact

- **Email**: your.email@example.com
- **LinkedIn**: [Your LinkedIn](https://linkedin.com/in/yourprofile)
- **Portfolio**: [yourwebsite.com](https://yourwebsite.com)

## 🗺️ Roadmap

- [ ] Add predictive analytics for sales forecasting
- [ ] Implement real-time data refresh
- [ ] Create mobile-optimized report layout
- [ ] Add Python integration for advanced ML models
- [ ] Deploy to Power BI Service with RLS
- [ ] Create automated email subscriptions

-----

⭐ **Star this repository if you found it helpful!**

📫 **Questions?** Open an issue or reach out via email.