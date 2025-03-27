# Cloud-Based Data Analysis Platform (DAP) on AWS  
**Vancouver School Distribution Analysis**  
*Individual Project by Chinmay Papnai (2305599)*  

---

## 🌟 Overview  
This project builds a **cloud-based data analysis platform** using Amazon Web Services (AWS) to answer a critical question:  
**"Which areas in Vancouver have the fewest public schools?"**  

The solution uses AWS tools like S3, EC2, Glue, and Athena to store, clean, analyze, and visualize school data. It also calculates monthly operational costs and ensures data security.  

**Why This Matters**:  
- Helps the Vancouver School Board identify underserved neighborhoods.  
- Demonstrates a scalable, low-cost cloud analytics pipeline.  

---

## 📂 Project Structure  
1. **Data Ingestion**: Collect raw school data into the cloud.  
2. **Data Cleaning**: Fix errors and prepare data for analysis.  
3. **Data Analysis**: Use SQL queries to find school distribution.  
4. **Cost Calculation**: Estimate monthly AWS expenses.  
5. **Security**: Protect data with encryption and backups.  
![Data Uploaded to S3](https://github.com/chinmaypapnai/AWS_UCW/blob/main/assets/Chinmay-DAP.drawio.png)
---

## 🛠️ Step-by-Step Implementation  

### 1. **Data Ingestion (Getting Data into AWS)**  
- **Tools Used**: Amazon EC2 (a virtual server) + Amazon S3 (cloud storage).  
- **Process**:  
  - A CSV file (`school-list.csv`) is uploaded from a Vancouver School Board server to an EC2 instance.  
  - The EC2 instance copies the file to an S3 bucket named `vsb-raw-chi` for storage.  
  - Data is organized in folders by year and quarter (e.g., `year=2025/quarter=01/`).  

```bash
# Example command to upload data to S3 from EC2
aws s3 cp school-list.csv s3://vsb-raw-chi/year=2025/quarter=01/
```

![Data Uploaded to S3](https://github.com/chinmaypapnai/AWS_UCW/blob/main/assets/ingestioncomplete.png)  

---

### 2. **Data Cleaning (Fixing Errors)**  
- **Tools Used**: AWS Glue Databrew (no-code data cleaning).  
- **Steps**:  
  - **Profiling**: Check data for mistakes (e.g., wrong data types, missing values).  
  - **Fix the "geom" Column**: This column had messy geographic coordinates. We cleaned it for future mapping.  
  - **Save Clean Data**: Stored cleaned data in two formats:  
    - **Parquet** (for fast processing).  
    - **CSV** (for easy human reading).  
  - Cleaned data goes to a new S3 bucket: `vsb-trf-chi`.  

![Data Cleaning in Glue Databrew](https://github.com/chinmaypapnai/AWS_UCW/blob/main/assets/projectdatabrew.png)  

---

### 3. **Data Cataloging (Organizing Data)**  
- **Tools Used**: AWS Glue Crawler (automatically scans data).  
- **Process**:  
  - A "crawler" named `vsb-crw-chi` scans the cleaned data in `vsb-trf-chi`.  
  - It creates a **data catalog** (like a table of contents) to track:  
    - Column names and types (e.g., `geo_local_area` = text, `school_count` = number).  
    - Data location in S3.  
  - This makes it easy to query data later with SQL.  

![Glue Crawler Results](https://github.com/chinmaypapnai/AWS_UCW/blob/main/assets/crw-run.png)  

---

### 4. **Data Analysis (Finding the Answer)**  
- **Tools Used**: AWS Athena (serverless SQL queries).  
- **SQL Query**:  
```sql
SELECT geo_local_area, COUNT(school_name) AS school_count 
FROM curated_data 
WHERE school_category = 'Public'  -- Focus only on public schools
GROUP BY geo_local_area 
ORDER BY school_count ASC;        -- Sort from lowest to highest
```

- **Result**:  
  - **Top 3 Areas with Fewest Public Schools**:  
    1. Downtown  
    2. Strathcona  
    3. Arbutus Ridge  

![Analysis Results](https://github.com/chinmaypapnai/AWS_UCW/blob/main/assets/athena.png)  

---

### 5. **Cost Calculation (Monthly AWS Expenses)**  
| Service           | What’s Used?                          | Monthly Cost |  
|-------------------|---------------------------------------|--------------|  
| **S3 Storage**    | Storing 150 GB of school data         | $2.50        |  
| **AWS Glue**      | Running data crawlers and ETL jobs    | $0.66        |  
| **Glue Databrew** | Cleaning data with 10 jobs            | $51.00       |  
| **EC2**           | Virtual server running 24/7          | $30.00       |  
| **Athena**        | SQL queries analyzing 1 TB of data    | $5.00        |  
| **Data Transfer** | Moving data between AWS services      | $1.00        |  
| **Total**         |                                       | **$89.16**   |  

---

## 🔒 Security & Data Protection  
- **Encryption**: All S3 buckets use AWS KMS keys to encrypt data.  
- **Backups**: Automatic backups with versioning (if data is deleted, we can restore it).  
- **Quality Checks**:  
  - Data is split into "passed" (good) and "failed" (needs review) folders.  
  - Example checks: No missing school names, valid geographic areas.  

![Data Quality Pineline](https://github.com/chinmaypapnai/AWS_UCW/blob/main/assets/QC%20pipeline.png)  

---

## 📂 Repository Structure  
```
├── assets/                 # Screenshots and diagrams  
└── README.md  
```

---

## 📝 Conclusion  
This project shows how cloud tools like AWS can turn raw data into actionable insights **without expensive hardware**. By following these steps, organizations can identify gaps in public services (like schools) and optimize budgets.  

**Student ID**: 2305599  
**License**: MIT (Free to use and modify)  

--- 

🔗 **Connect with Me**: [LinkedIn](https://www.linkedin.com/in/chinmaypapnai) | [GitHub](https://github.com/chinmaypapnai)  
*Chinmay Papnai* 🏙️
