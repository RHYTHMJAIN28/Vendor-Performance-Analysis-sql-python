### ​‍​‌‍​‍‌​‍​‌‍​‍‌ Vendor Performance Analysis 

### Overview 

The Vendor Performance Analysis project aims to evaluate the efficiency, reliability, and profitability of vendors through a comprehensive data-driven approach.
Organizations often deal with multiple suppliers and face challenges in tracking which vendors deliver the best value in terms of cost, timeliness, and sales impact.
This project leverages data analytics techniques to assess vendor performance across several dimensions including purchase behavior, sales outcomes, and freight costs.

By connecting to a centralized SQLite database (inventory.db), the analysis consolidates data from multiple tables such as purchases, purchase_prices, vendor_invoice, and sales.
It utilizes Python-based data analysis and visualization to uncover hidden patterns, relationships, and performance gaps between vendors and brands.

Through Exploratory Data Analysis (EDA), the project identifies:

Vendors generating the highest sales and profit margins

Cost discrepancies between purchase and sales prices

Freight cost contributions to overall expenses

Brand-level performance linked to specific suppliers

This study not only helps in understanding vendor efficiency but also assists decision-makers in optimizing supply chain operations, negotiating better terms, and prioritizing top-performing vendors for future procurement.
--- 

###  Key Objectives 

- Use of sales, purchase, and freight data to analyze vendor performance. 
- Purchases prices, sales quantities, and vendor invoice details comparison. 
- Identify top brands and vendors contributing to revenue. 
- Analyze vendor cost-effectiveness and delivery efficiency. 
--- 

###  Project Workflow 

1. **Database Connection:** 
Links an SQLite database (`inventory.db`) with have tables such as: 
- `purchases` 
- `purchase_prices` 
- `vendor_invoice` 
- `sales` 

2. **Data Extraction & Cleaning:** 

Data is read through `pandas` and SQL queries where data integrity and consistency are maintained. 

3. **Exploratory Data Analysis (EDA):** 

Descriptive statistics and the visual inspection of the main vendor metrics are carried out. 

4. **Vendor Insights:** 

Shows total purchases, sales revenue, and freight costs by vendor and brand through summarization. 

5. **Visualization:** 

Creates different visuals and comparisons to emphasize top vendors and cost trends. 

--- 

### Tech Stack 
- **Language:** Python 
- **Database:** SQLite 
- **Libraries:** 
- `pandas` - for data manipulation and analysis 
- `sqlite3` - for database connection and queries 
- `matplotlib` - for the visualization of data 
- `seaborn` – Statistical plotting

---

### Insights & Metrics
- **Total Purchase Quantity and Value** per vendor  
- **Sales Performance** by brand  
- **Freight Cost Summaries** for each vendor  
- **Comparison of Purchase vs. Sales Prices**  
- **Top Vendors** based on revenue contribution  

---

###  How to Run the Project
1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/vendor-performance-analysis.git
   cd vendor-performance-analysis

2. Install dependencies:
pip install pandas matplotlib seaborn

3. Run the notebooks in Jupyter:
jupyter notebook "Vendor Performance Analysis.ipynb"

### Future Enhancements

Incorporate machine learning models for vendor rating prediction.

Automate dashboards for vendor KPIs.

Add interactive visualizations using Plotly or Power BI.

###  Author

Rhythm Jain
📧 rhythmjain2021@gmail.com
📍 Data Analytics & Business Insights Enthusiast
🏷️ License
This project is open-source and available under the MIT License
.



--- 

### 📂 Repository ​‍​‌‍​‍‌​‍​‌‍​‍‌Structure 
