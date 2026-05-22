-- ==========================================
-- PHASE 2: DATABASE INITIALIZATION SCRIPT
-- SYSTEM: MYSQL / AZURE SQL
-- ==========================================

-- 1. Create Dimension Table for Business Units
CREATE TABLE Dim_Business_Units (
    unit_id INT NOT NULL,
    business_unit_name VARCHAR(100) NOT NULL,
    department_head VARCHAR(100),
    office_location VARCHAR(100),
    PRIMARY KEY (unit_id)
);

-- 2. Create Dimension Table for Calendar/Time Context
CREATE TABLE Dim_Calendar (
    date_id INT NOT NULL,
    full_date DATE NOT NULL,
    fiscal_year INT NOT NULL,
    quarter VARCHAR(2) NOT NULL,
    month_name VARCHAR(15) NOT NULL,
    PRIMARY KEY (date_id)
);

-- 3. Create Dimension Table for Expense Classifications
CREATE TABLE Dim_Expense_Categories (
    category_id INT NOT NULL,
    vendor_name VARCHAR(100) NOT NULL,
    expense_type VARCHAR(100) NOT NULL,
    risk_classification VARCHAR(50),
    PRIMARY KEY (category_id)
);

-- 4. Create Central Fact Table with Schema Relations
CREATE TABLE Fact_Operational_Transactions (
    transaction_id VARCHAR(50) NOT NULL,
    unit_id INT NOT NULL,
    date_id INT NOT NULL,
    category_id INT NOT NULL,
    actual_cost DECIMAL(15, 2) NOT NULL,
    budgeted_cost DECIMAL(15, 2) NOT NULL,
    variance_status VARCHAR(50) DEFAULT 'NORMAL',
    PRIMARY KEY (transaction_id),
    
    -- Defining relationships linking back to Dimension Tables
    FOREIGN KEY (unit_id) REFERENCES Dim_Business_Units(unit_id),
    FOREIGN KEY (date_id) REFERENCES Dim_Calendar(date_id),
    FOREIGN KEY (category_id) REFERENCES Dim_Expense_Categories(category_id)
);
 