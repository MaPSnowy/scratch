# Project Title : ProMo_2024
Creating a SQLite Database
This SQLite database (ProMo1database.db) contains two primary tables:

    Shops: Stores information related to each shop, such as shop number, address, status, and partner details.
    Sales: Stores sales transactions related to each shop, including details like sales date, reference number, quantity, and value paid.

The database was designed with various business requirements in mind, including the ability to handle updates to shops and transactions. Later, business change requests led to modifications in the original tables, reflecting additions such as open and closed dates for shops and a value paid column for sales transactions.


Installation Guide for SQLite and DB Browser for SQLite
Step 1: Install SQLite

SQLite is a lightweight, serverless SQL database engine. Follow the steps below to install SQLite.

    Download SQLite:
        Visit the SQLite download page.
        Under the "Precompiled Binaries for Windows" section, download the sqlite-tools-win32-x86-3460000.zip (for Windows) or the appropriate version for User's operating system.

    Extract the Files:
        Extract the downloaded ZIP file to a directory on User's machine, such as C:\sqlite\ on Windows.

    Add SQLite to System Path (Optional, but recommended):
        On Windows:
            Go to Control Panel → System and Security → System → Advanced System Settings.
            Under the Advanced tab, click on Environment Variables.
            In the System Variables section, find Path and click Edit.
            Add a new entry with the path where SQLite was extracted (e.g., C:\sqlite\).
        This allows User to run SQLite from any command prompt without specifying its full path.

    Verify Installation:
        Open a command prompt (Windows) or terminal (Mac/Linux) and type: sqlite3

        User should see the SQLite prompt sqlite>, indicating successful installation.

Step 2: Install DB Browser for SQLite

DB Browser for SQLite is a GUI tool that simplifies working with SQLite databases.

    Download DB Browser:
        Visit the DB Browser for SQLite download page.
        Download the appropriate version for User's operating system (Windows, macOS, or Linux).

    Install the Tool:
        Follow the on-screen instructions to install DB Browser.

    Opening the Database:
        After installation, open DB Browser for SQLite.
        Go to File → Open Database and select the ProMo1database.db file to view and edit the database.

Original Table Descriptions (Before Business Change Request)

Shops Table (Before Business Change Request)

    Shop_Number: TEXT, NOT NULL, Unique identifier for each shop.
    Shop_Address: TEXT, NOT NULL, Address of the shop.
    Country_Name: TEXT, NOT NULL, Country where the shop is located.
    Shop_Name: TEXT, NOT NULL, Name of the shop.
    Status: TEXT, NOT NULL, Status of the shop (e.g., Open, Closed).
    Partner_Number: TEXT, NOT NULL, Partner number associated with the shop.
    Partner_Name: TEXT, NOT NULL, Partner name associated with the shop.

Sales Table (Before Business Change Request)

    Sales_Date: TEXT, NOT NULL, Date of the sales transaction.
    Shop_Number: TEXT, NOT NULL, Shop where the transaction occurred.
    Quantity: INTEGER, NOT NULL, Number of items sold.
    RefCol_Number: TEXT, NOT NULL, Reference number for the transaction.
    Transaction_ID: INTEGER, NOT NULL, Unique transaction identifier.

Business Change Request

    Addition of Open and Closed Date Columns:
        The Shops table was modified to include an Open_Date column (with a default value of 1970-01-01) to track when shops opened and a Closed_Date column (nullable) to track when shops closed.

    Enforcing Quantity of 1 in the Sales Table:
        Initially, the Quantity column allowed values greater than 1, but it was updated to enforce a default value of 1 for all sales. Each transaction now represents the sale of a single item. If multiple items are sold, they should be recorded as multiple rows.

    Adding Value_Paid Column to Sales Table:
        A new Value_Paid column was added to track the amount paid for each sale. The column does not allow a value of 0, but negative values are allowed to represent returns.

Final Table Descriptions (After Business Change Request)

Shops Table (After Business Change Request)

    Shop_Number: TEXT, NOT NULL, UNIQUE, Unique identifier for each shop.
    Shop_Address: TEXT, NOT NULL, Address of the shop.
    Country_Name: TEXT, NOT NULL, COLLATE NOCASE, Country where the shop is located.
    Shop_Name: TEXT, NOT NULL, UNIQUE, COLLATE NOCASE, Name of the shop.
    Status: TEXT, NOT NULL, DEFAULT 'Open', CHECK (Status IN ('Open', 'Closed', 'Going to Open')), Status of the shop.
    Partner_Number: TEXT, NOT NULL, Partner number associated with the shop.
    Partner_Name: TEXT, NOT NULL, Partner name associated with the shop.
    Open_Date: TEXT, NOT NULL, DEFAULT '1970-01-01', Date when the shop opened.
    Closed_Date: TEXT, NULL, Date when the shop closed (nullable).

Sales Table (After Business Change Request)

    Sales_Date: TEXT, NOT NULL, Date of the sales transaction.
    Shop_Number: TEXT, NOT NULL, Shop where the transaction occurred.
    Quantity: INTEGER, NOT NULL, DEFAULT 1, CHECK (Quantity = 1), Number of items sold (always 1).
    RefCol_Number: TEXT, NOT NULL, COLLATE BINARY, Reference number for the transaction.
    Transaction_ID: INTEGER, NOT NULL, UNIQUE, Unique transaction identifier.
    Value_Paid: INTEGER, NOT NULL, CHECK (Value_Paid != 0), Amount paid for the transaction (can be negative for returns).

Final SQL Queries
Table Creation Queries (Before Business Change Request)

CREATE TABLE Shops (
    Shop_Number TEXT NOT NULL UNIQUE,
    Shop_Address TEXT NOT NULL,
    Country_Name TEXT NOT NULL COLLATE NOCASE,
    Shop_Name TEXT NOT NULL UNIQUE COLLATE NOCASE,
    Status TEXT NOT NULL DEFAULT 'Open',
    Partner_Number TEXT NOT NULL,
    Partner_Name TEXT NOT NULL
);

CREATE TABLE Sales (
    Sales_Date TEXT NOT NULL,
    Shop_Number TEXT NOT NULL,
    Quantity INTEGER NOT NULL DEFAULT 1,
    RefCol_Number TEXT NOT NULL COLLATE BINARY,
    Transaction_ID INTEGER NOT NULL UNIQUE
);


Alteration Queries (Business Change Request)

-- Add Open_Date column with a default value for backward compatibility
ALTER TABLE Shops ADD COLUMN Open_Date TEXT NOT NULL DEFAULT '1970-01-01';

-- Add Closed_Date column (nullable)
ALTER TABLE Shops ADD COLUMN Closed_Date TEXT;

-- Add Value_Paid column with a check that prevents it from being 0
ALTER TABLE Sales ADD COLUMN Value_Paid INTEGER NOT NULL DEFAULT 1 CHECK (Value_Paid != 0);


Insert Queries for Shops and Sales (Final Working Queries)
-- Insert new shops
INSERT INTO Shops (Shop_Number, Shop_Address, Country_Name, Shop_Name, Status, Partner_Number, Partner_Name, Open_Date)
VALUES ('235', 'New Address St', 'Germany', 'Berlin Center Mall', 'Open', '235', 'Fashion Partners', '2023-01-01');

INSERT INTO Shops (Shop_Number, Shop_Address, Country_Name, Shop_Name, Status, Partner_Number, Partner_Name, Open_Date, Closed_Date)
VALUES ('236', 'Old Address St', 'Germany', 'Old Town Mall', 'Closed', '236', 'Style Cloths', '2022-05-01', '2024-10-01');

-- Insert new sales
INSERT INTO Sales (Sales_Date, Shop_Number, Quantity, RefCol_Number, Value_Paid)
VALUES ('2024-10-13', 'S001', 1, 'REF123456', 89.99);

INSERT INTO Sales (Sales_Date, Shop_Number, Quantity, RefCol_Number, Value_Paid)
VALUES ('2024-10-13', 'S001', 1, 'REF123456', 32.99);

INSERT INTO Sales (Sales_Date, Shop_Number, Quantity, RefCol_Number, Value_Paid)
VALUES ('2024-10-13', 'S001', 1, 'REF123456', 18.95);


    
