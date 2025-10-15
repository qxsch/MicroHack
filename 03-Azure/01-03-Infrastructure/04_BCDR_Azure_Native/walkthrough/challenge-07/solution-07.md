# Walkthrough Extra Challenge 7 - Protect to Azure with Azure Backup & Restore

[Previous Challenge Solution](../challenge-06/solution-06.md) - **[Home](../../Readme.md)**

## Implement Failover Groups for Azure SQL Database

## Task 1: Create a Failover Group between two Azure SQL databases located in different Azure regions (Germany West central and Sweden Central)

Navigate to the **SQL Server** in the Germany West Central Region. Open the tab **Failover groups**:
![image](./img/03.png)

### Create a Failover Group and select your **SQL Server** in the Sweden Central Region
![image](./img/04.png)

## Task 2: Configure automatic failover policies and test the failover mechanism to ensure seamless transition in case of a disaster.
Open the created **Failover group**
![image](./img/05.png)

### Test failover
![image](./img/06.png)

### When the failover is complete, you should see **SQL Server** in Sweden Central as the Primary server.
![image](./img/07.png)

### Open the Data Science Virtual Machine, and test the connection to the Server using the new **fail over group listener endpoint**:
![image](./img/08.png)

### Connection established! 
![image](./img/09.png)

---

## Connect to the Virtual Machine in order to test connection to SQL Database and storage container in the upcoming tasks.

Reader can refer to the [Previous Challenge Solution](../challenge-04/solution-04.md) to remember how to failover.

## Disaster Recovery for Azure SQL Database

## Task 2: Failback to Germany West Central Region

### Navigate to the **SQL Server**. Open the created **Failover group**:
![image](./img/20.png)

### Failback to Germany West Central Region
![image](./img/21.png)

### **SQL Server** in Germany West Central is now the Primary server.
![image](./img/22.png)

### Open the Data Science Virtual Machine, and test the connection to the Server using the **fail over group listener endpoint**:
![image](./img/08.png)

### Connection secured! 
![image](./img/09.png)

---

## Check connection and restore your sample file.

### From the Data Science Virtual Machine, use **Microsoft Azure Storage Explorer** to restore your file:
![image](./img/16.png)

---

## Check connection and restore your sample file.

### From the Data Science Virtual Machine, use **Microsoft Azure Storage Explorer** to restore your file:
![image](./img/16.png)

---