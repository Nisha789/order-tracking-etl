# Order Tracking Incremental Load Project

This project implements an **order-tracking ETL process** using **Databricks** and **GCP (Google Cloud Platform) buckets** to handle incremental loads efficiently. The entire process is automated using **Databricks Workflows**, triggered by the arrival of new files in the staging bucket. 


## Features
- **Event-Driven Automation**: Workflows are triggered automatically when a new file arrives in the staging bucket.
- **Data Ingestion**: Reads data files from a GCP staging bucket.
- **Incremental Load**: Performs an upsert operation (merge) for efficient data synchronization.
- **Archival Process**: Moves processed files to an archive bucket to maintain storage hygiene.
- **Delta Lake Integration**: Leverages Delta Lake for ACID-compliant data handling and schema enforcement.


## Project Workflow

1. **File Arrival**  
   New CSV files are uploaded to the GCP bucket path `tracking_orders/staging_zn`.  
   - File arrival triggers the Databricks Workflow.

2. **Stage Data Processing**  
   The `stage-load-data.ipynb` notebook:  
   - Reads CSV files from the staging bucket.
   - Writes the data to the **stage table** (`stage_zn_new`) in Delta Lake.
   - Moves the processed files to the **archive folder** in the GCP bucket.

3. **Target Data Load**  
   The `target_data_load.ipynb` notebook:  
   - Reads data from the stage table.
   - Performs an upsert operation on the **target table** (`target_zn_new`) based on the unique `tracking_num` column.  
   - Ensures that the target table reflects the latest state of the data.

4. **Automation**  
   The Databricks Workflow is configured to:  
   - Monitor the staging bucket for file arrival.  
   - Trigger the `stage-load-data.ipynb` and `target_data_load.ipynb` notebooks sequentially.


**Technologies Used**
- **Databricks:** For notebook development, workflow automation, and event-based triggering.
- **GCP Buckets:** For staging and archiving files.
- **Delta Lake:** For reliable, ACID-compliant data storage.


**How to Run**
1. **Upload Data:** Place the CSV files in the GCP bucket directory tracking_orders/staging_zn.
2. **Workflow Automation:**
    - The Databricks Workflow automatically triggers the notebooks based on file arrival in the staging bucket.
    - The notebooks are executed in the following order:
        stage-load-data.ipynb
        target_data_load.ipynb
3. **Check Results:**
    - Verify that the processed data is available in the Delta tables (stage_zn_new, target_zn_new).
    - Confirm that the source files are moved to the archive directory.


**Future Enhancements**
    - Add validation and monitoring for data quality.
    - Introduce schema evolution handling.
    - Implement notification systems for process status updates.


**Contributions**
Contributions are welcome! If you'd like to contribute, please fork the repository and submit a pull request with your enhancements.


**License**
This project is licensed under the MIT License - see the LICENSE file for details.


**Acknowledgments**
Special thanks to:
- **Databricks** for providing a robust platform for automation and data processing.
- **Google Cloud Platform (GCP)** for reliable storage and seamless integration.
- **Delta Lake** for enabling efficient and reliable data handling.
Grateful for all the resources and support that made this project possible.
