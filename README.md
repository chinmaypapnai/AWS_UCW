# AWS_UCW
**AWS  Portfolio**
**Jonel-Cloud-Computing-Projects**  
**Project Part 1**  

---

### **Project Description**  
This descriptive analysis project details the design and implementation of a cloud-based Data Analytics Platform (DAP) on AWS to identify Vancouver’s geographic areas with the lowest concentration of public schools. The project focuses on ingesting, profiling, cleaning, and analyzing school distribution data to support evidence-based decision-making for the City of Vancouver.  

---

### **Project Title**  
**City of Vancouver DAP Analysis: Public School Distribution (Project Part 1)**  

---

### **Objectives**  
The primary objectives are:  
1. To develop and deploy an AWS-based analytical platform that identifies Vancouver localities with the fewest public schools using descriptive methods.  
2. To estimate monthly operational costs for maintaining the platform on AWS.  
3. To ensure data security, governance, and scalability while adhering to cost-effectiveness.  

---

### **Dataset**  
**Vancouver School Data**:  
- **Variables**:  
  - `geo_local_area`: Locality of the school.  
  - `school_name`: Unique identifier (primary key).  
  - `school_category`: Classification (Public, Independent, SafeStart BC).  
- **Source**: BC Ministry (updated in 2023, metadata updated in 2025).  
- **Storage**: Raw CSV files stored in S3 bucket `vsb-raw-chi` (Standard-IA storage class).  

---

### **Methodology**  

#### **1. Data Ingestion (AWS EC2, S3)**  
1. **EC2 Instance Setup**:  
   - Launched a t3.medium EC2 instance to emulate data ingestion from the Vancouver School Board (VSB) server.  
   - Transferred `school-list.csv` to the S3 bucket `vsb-raw-chi` under `year=2025/quarter=01/`.  
2. **S3 Bucket Structure**:  
   - **Raw Zone**: `vsb-raw-chi` for unprocessed data.  
   - **Transformed Zone**: `vsb-trf-chi` for cleaned data.  
   - **Curated Zone**: `vsb-cur-chi` for summarized results.  

#### **2. Data Profiling (AWS Glue DataBrew)**  
1. **Profiling Jobs**:  
   - Created DataBrew projects to analyze schema, data types, and inconsistencies (e.g., invalid formatting in the `geom` column).  
   - Generated reports highlighting null values and schema validation results.  
2. **Key Findings**:  
   - `geo_local_area` validated against Vancouver’s official locality list.  
   - `school_category` standardized to "Public," "Independent," or "SafeStart BC."  

#### **3. Data Cleaning (AWS Glue DataBrew)**  
1. **Cleaning Jobs**:  
   - Removed redundant characters from the `geom` column for spatial accuracy.  
   - Deduplicated records using `school_name` as the primary key.  
   - Saved cleaned data to `vsb-trf-chi` in CSV (user-friendly) and Parquet (system-optimized) formats.  

#### **4. Data Cataloging (AWS Glue)**  
1. **Glue Database**: Created `vsb-schools-db` to catalog metadata.  
2. **Crawler Configuration**:  
   - Deployed crawler `vsb-crw-chi` to infer schemas from `vsb-trf-chi` and populate tables in the Glue Data Catalog.  

#### **5. Data Summarization (AWS Glue ETL)**  
1. **ETL Pipeline**:  
   - Grouped data by `geo_local_area` and calculated `school_count` using `COUNT(school_name)`.  
   - Filtered for `school_category = "Public"` to isolate public schools.  
   - Stored summarized results in `vsb-cur-chi` for analysis.  

#### **6. Cost Estimation**  
1. **Monthly Cost Breakdown**:  
   - **S3**: $2.50 (150 GB storage).  
   - **Glue**: $0.66 (crawler + ETL jobs).  
   - **EC2**: $30.00 (t3.medium instance).  
   - **Athena**: $5.00 (1 TB data scanned).  
   - **Total**: **$89.16/month**.  

#### **7. Data Security (AWS KMS, CloudWatch)**  
1. **Encryption**:  
   - Used AWS KMS to create unique keys for each S3 bucket (`vsb-raw-chi`, `vsb-trf-chi`, `vsb-cur-chi`).  
2. **Monitoring**:  
   - Configured CloudWatch alarms for bucket size anomalies and unauthorized access.  

---

### **Descriptive Statistics**  
1. **AWS Athena Queries**:  
   ```sql  
   -- Count public schools per locality  
   SELECT geo_local_area, COUNT(school_name) AS school_count  
   FROM vsb_curated  
   WHERE school_category = 'Public'  
   GROUP BY geo_local_area  
   ORDER BY school_count ASC;  
   ```  
2. **Key Results**:  
   - **Lowest Public School Density**: Downtown, Strathcona, and Arbutus Ridge.  

---

### **Tools and Technologies**  
1. **AWS Services**:  
   - **S3**: Raw/processed data storage.  
   - **EC2**: Data ingestion.  
   - **Glue**: Data profiling, ETL, and cataloging.  
   - **Athena**: SQL-based analysis.  
   - **KMS/CloudWatch**: Security and monitoring.  
2. **Other**:  
   - **PowerShell**: Scripted data transfers.  

---

### **Deliverables**  
1. **Cleaned Datasets**:  
   - Raw data in `vsb-raw-chi`, transformed data in `vsb-trf-chi`, curated results in `vsb-cur-chi`.  
2. **Data Catalog**:  
   - Tables for `vsb_raw`, `vsb_transformed`, and `vsb_curated` in the Glue database.  
3. **Cost Report**:  
   - Detailed breakdown of monthly AWS operational costs.  
4. **Security Framework**:  
   - Encryption keys, replication rules, and monitoring dashboards.  

---

### **Conclusion**  
1. **Analytical Outcome**: Downtown Vancouver has the lowest public school density, highlighting potential needs for educational resource allocation.  
2. **Platform Scalability**: The AWS pipeline ensures repeatability for future updates (quarterly ingestion).  
3. **Cost Efficiency**: Optimized storage and serverless tools (Athena/Glue) minimize expenses.  

---

**Course Completion Screenshot**    

--- 

