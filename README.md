# End-to-End E-Commerce Customer 360 View on Microsoft Fabric

## 📌 Project Overview & Business Impact
In modern e-commerce, customer data is traditionally siloed across isolated applications (web sessions, billing systems, support lines, order forms). This makes calculating comprehensive lifetime metrics and analyzing retention behavior impossible.

This project designs and implements an automated data pipeline using **Microsoft Fabric** to ingest, process, clean, and model multi-source e-commerce datasets into a single, unified **Gold Customer 360 table**. This empowers businesses to track cross-channel health, optimize user retention, and maximize customer lifetime value (LTV).

---

## 🛠 Tech Stack
* **Orchestration & Ingestion:** Microsoft Fabric Data Factory (Pipelines & Get Metadata)
* **Storage Layer:** Azure Data Lake Storage (ADLS Gen2) & Fabric Lakehouse (Delta Lake / Parquet)
* **Processing Engine:** Apache Spark (PySpark Core DataFrames API)
## 📊 Business Intelligence & Power BI Layer) The final Gold flat view table (`gold_customer_360`) leverages Fabric's cutting-edge **Direct Lake mode**. Unlike traditional Import mode (which incurs massive compute/refresh overhead) or DirectQuery (which suffers from sluggish SQL processing latencies), Direct Lake mode allows Power BI to run DAX expressions straight over the underlying Delta Parquet storage files.

---

## 📐 Data Architecture (Medallion Approach)
The data flows sequentially across three isolated environments to maintain compliance, tracking, and structural integrity:

1. **Bronze Layer (Raw Landing):** Automated pipelines fetch unstructured/messy `.csv` files natively from ADLS Gen2, converting them to optimized `.parquet` tables with no schema enforcement.
2. **Silver Layer (Enriched & Cleaned):** PySpark processing addresses real-world anomalies:
   * Standardizes text features (e.g., handling variable casings like `M` vs `male`, `F` vs `female`, and using `initcap()` on strings).
   * Rectifies broken formats (`RegEx` data conversions for varying date formats).
   * Trims trailing spaces, evaluates negative dollar signs, and drops missing indexing constraints.
3. **Gold Layer (Business Aggregations):** Employs robust **Sequential Left Joins** driven by a master customer list to assemble a flat data table without losing transactional footprints (e.g., tracking a customer with an active web session/support ticket who has not yet placed an order).

---

## 📊 Key Insights & Dashboards
The Power BI semantic layer uses Fabric's **Direct Lake mode** for real-time streaming without query processing overhead. Key visuals engineered:
* **Distinct Customer and Order Metrics:** Tracking baseline volume metrics.
* **Customer Ticket Distribution:** Pie/Donut breakdowns showing pending vs resolved user complaints to pinpoint operations choke points.
* **Geographic Spend Mapping:** Pinpointing high-value regional user cohorts.
* **Device Trapping Metrics:** Tracks channel patterns (Mobile vs Web) which flagged critical data-capture issues to product managers due to null device identifiers.

---

