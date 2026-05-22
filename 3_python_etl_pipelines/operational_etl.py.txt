import pandas as pd
import numpy as np
import sqlalchemy

# ==========================================
# PHASE 3: AUTOMATED ENTERPRISE ETL SCRIPT
# PURPOSE: AUTOMATE CLEANING & INTEGRATE BR001
# ==========================================

def run_operational_etl():
    # ----------------------------------------------------
    # STEP 1: EXTRACT (Pulling the raw, messy business data)
    # ----------------------------------------------------
    print("Initializing Extraction Phase...")
    # In a real environment, this reads a raw CSV dropped by external systems
    raw_data_path = "raw_operational_data.csv"
    
    try:
        # Reading the data into a Pandas DataFrame (a digital table)
        df = pd.read_csv(raw_data_path)
        print(f"Extraction Successful. Loaded {len(df)} records.")
    except FileNotFoundError:
        print("Raw file not found. Generating mock enterprise data for pipeline execution...")
        # Simulating our 100k+ enterprise scenario with a mock sample
        mock_data = {
            'transaction_id': ['TXN001', 'TXN002', 'TXN003', 'TXN004'],
            'unit_id': [101, 102, 103, 101],
            'date_id': [20260520, 20260521, 20260521, 20260522],
            'category_id': [501, 502, 503, 501],
            'actual_cost': [12000.00, 4500.00, 9500.00, 15000.00],
            'budgeted_cost': [10000.00, 5000.00, 7000.00, 15000.00]
        }
        df = pd.DataFrame(mock_data)

    # ----------------------------------------------------
    # STEP 2: TRANSFORM (Data cleaning & Business Rules)
    # ----------------------------------------------------
    print("Initializing Transformation & Validation Phase...")
    
    # Rule A: Remove any completely identical rows to protect data integrity
    df = df.drop_duplicates()
    
    # Rule B: Clean missing values (Fill blanks if any exist)
    df['actual_cost'] = df['actual_cost'].fillna(0.00)
    df['budgeted_cost'] = df['budgeted_cost'].fillna(0.00)

    # Rule C: Apply Business Rule 001 (BR001 - Check for Spikes > 15%)
    # Mathematically: ((Actual - Budget) / Budget) * 100
    variance_percentage = ((df['actual_cost'] - df['budgeted_cost']) / df['budgeted_cost']) * 100
    
    # Using NumPy to instantly flag rows that breach our 15% law
    df['variance_status'] = np.where(
        variance_percentage > 15.0, 
        'CRITICAL_VARIANCE', 
        'NORMAL'
    )
    
    print("Transformation Completed. Business Rule 001 (BR001) successfully applied.")

    # ----------------------------------------------------
    # STEP 3: LOAD (Pushing clean data into the Database)
    # ----------------------------------------------------
    print("Initializing Database Loading Phase...")
    
    # Setting up the connection parameters to our MySQL/Azure warehouse
    # (Format: mysql+pymysql://username:password@hostname:port/database_name)
    database_connection_string = "mysql+pymysql://root:password@localhost:3306/nova_tech_analytics"
    
    try:
        engine = sqlalchemy.create_engine(database_connection_string)
        
        # Pushing the clean table directly into our Fact table built in Phase 2
        # 'if_exists=append' ensures we add new rows without deleting old data
        df.to_sql('Fact_Operational_Transactions', con=engine, if_exists='append', index=False)
        print("Load Phase Successful! Data warehouse tables successfully updated.")
        
    except Exception as e:
        print(f"Database Load paused. System running in safe local portfolio mode.")
        print("Cleaned Data Preview with BR001 Flags:")
        print(df.to_string())

# Execute the pipeline
if __name__ == "__main__":
    run_operational_etl()