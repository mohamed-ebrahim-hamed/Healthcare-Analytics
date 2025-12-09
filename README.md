# Healthcare Analytics Project

## Overview
This is a healthcare analytics project for a medical center. The project collects, cleans, and analyzes data about patients, doctors, appointments, revenues, and expenses. The analyzed data is displayed in an interactive dashboard using Power BI. The project also includes data analysis using Python and SQL.

## Project Structure
```
Healthcare-Analytics/
├── data_raw/                # Raw data files (Excel, CSV)
├── data_processed/          # Processed data files
├── sql_scripts/             # SQL scripts
├── python_scripts/          # Python scripts
├── powerbi_dashboard/       # Power BI files
└── README.md
```

## Main File
The main analysis file is located at:
- `python_scripts/Healthcare_Data_Analysis.ipynb` - This is a Jupyter notebook that contains all the data analysis steps

## Requirements
- Python 3.8 or higher
- Python libraries: pandas, numpy, matplotlib, scikit-learn
- Power BI Desktop
- Git

## Steps to Follow

### Step 1: Clone the Repository
```bash
git clone https://github.com/mohamed-ebrahim-hamed/Healthcare-Analytics.git
cd Healthcare-Analytics
```

### Step 2: Install Dependencies
```bash
pip install pandas numpy matplotlib scikit-learn
```

### Step 3: Run the Analysis
Open the Jupyter notebook and run the cells:
```bash
jupyter notebook python_scripts/Healthcare_Data_Analysis.ipynb
```

### Step 4: View the Dashboard
Open the Power BI file located at `powerbi_dashboard/Medical center.pbix` using Power BI Desktop.

## Project Flow
1. Load the raw data from Excel files
2. Clean and process the data using Python
3. Analyze the data and create visualizations
4. Export processed data for Power BI
5. Build interactive dashboards in Power BI
6. Use SQL queries for database operations

## Dashboards and Reports
The Power BI dashboard includes:
- Total patients, doctors, and appointments
- Attendance rates and no-show rates
- Revenue and expense tracking
- Time-based analysis
- Analysis by medical specialty
- Expense breakdown by category

## License
MIT License - Free to use and modify.
