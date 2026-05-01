**Company: QuickBite India**

QuickBite India is a fictional online food delivery platform operating across 10 major Indian cities — Mumbai, Delhi, Bengaluru, Hyderabad, Chennai, Kolkata, Pune, Ahmedabad, Jaipur, and Lucknow.

Customers use the QuickBite mobile app or website to:

- Browse restaurants and menus  
- Place orders (single or multiple items)  
- Pay using UPI, credit card, debit card, cash on delivery, or wallet  
- Track deliveries handled by registered delivery partners  
- Rate restaurants and delivery partners after an order  

The business earns revenue through:

- A commission (typically 15–25%) on every order placed through the platform
- Delivery fees charged to customers
- Promotional offer subsidies shared between the platform and restaurants

**Your Role**

You are a Data Engineering intern at QuickBite India. The operations team has been storing all transactional data in a MySQL OLTP (Online Transaction Processing) database. Your job is to:

1. Explore and understand the OLTP database  
2. Clean the messy, real-world-style data  
3. Extract, Transform, and Load (ETL) the cleaned data into an OLAP (Online Analytical Processing) data warehouse (galaxy schema in MySQL)  
4. Compute KPIs and generate business insights  
5. Present findings to a mock business stakeholder  

Pre-requistes :

| Task | What to Do                                                                                                     |
| ---- | -------------------------------------------------------------------------------------------------------------- |
| 0.1  | Install MySQL 8.0, Python 3.10+, MySQL Workbench or DBeaver                                                    |
| 0.2  | Install Python packages: pip install mysql-connector-python pandas sqlalchemy pymysql matplotlib seaborn faker |
| 0.3  | Connect to the pre-loaded quickbite_oltp database using MySQL Workbench                                        |
| 0.4  | Run SHOW TABLES; and DESCRIBE customers; to confirm access                                                     |
| 0.5  | Run SELECT COUNT(*) FROM orders; — verify you see ~700,000 rows                                                |

List of steps :


