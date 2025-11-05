
# Azure SQL Database Tutorial

*Getting started with Azure SQL Database in the cloud.*

---

## 📌 Overview

Azure SQL Database is Microsoft’s fully managed, cloud-based relational database service built on SQL Server.  

**Key benefits:**

- Fully managed (patching, backups, high availability handled by Azure)
- Scalable performance (DTU or vCore models)
- Built-in security (encryption, firewall rules, threat detection)
- Integration with Azure services (Power BI, Azure Functions, Logic Apps)

This tutorial will guide you through:

1. Creating an Azure SQL Database.
2. Configuring firewall and authentication.
3. Connecting with Azure Data Studio / SQL Server Management Studio (SSMS).
4. Running SQL queries.
5. Backing up and scaling.
6. Best practices.

---

## 1️⃣ Create an Azure SQL Database

### Steps (Portal)

1. Sign in to [Azure Portal](https://portal.azure.com/).
2. Click **Create a resource** → **Databases** → **SQL Database**.
3. Fill in:
   - Subscription & Resource Group
   - Database Name: `myazuresqldb`
   - Server: **Create new** or select an existing one  
     - Server name (unique in Azure)
     - Admin login & password
     - Location
   - Compute + Storage: Choose **DTU** or **vCore** model.
4. Review and **Create**.

### CLI Example

```bash
# Variables
RESOURCE_GROUP="my-rg"
LOCATION="eastus"
SERVER_NAME="my-sql-server-1234"
DB_NAME="myazuresqldb"
ADMIN_USER="sqladmin"
ADMIN_PASS="P@ssw0rd123!"

# Create resource group
az group create --name $RESOURCE_GROUP --location $LOCATION

# Create SQL Server
az sql server create \
  --name $SERVER_NAME \
  --resource-group $RESOURCE_GROUP \
  --location $LOCATION \
  --admin-user $ADMIN_USER \
  --admin-password $ADMIN_PASS

# Create SQL Database
az sql db create \
  --resource-group $RESOURCE_GROUP \
  --server $SERVER_NAME \
  --name $DB_NAME \
  --service-objective S0
```

---

## 2️⃣ Configure Firewall Rules

By default, Azure SQL blocks all external connections.

**Portal:**

1. Navigate to your SQL Server in the portal.
2. Click **Firewalls and virtual networks**.
3. Add your client IP and save.

**CLI:**

```bash
az sql server firewall-rule create \
  --resource-group $RESOURCE_GROUP \
  --server $SERVER_NAME \
  --name AllowClientIP \
  --start-ip-address <YOUR_IP> \
  --end-ip-address <YOUR_IP>
```

---

## 3️⃣ Connect to Azure SQL

You can connect using:

- **Azure Data Studio** (cross-platform)
- **SQL Server Management Studio** (Windows)
- **Command-line tools** (`sqlcmd`)

**Example (sqlcmd):**

```bash
sqlcmd -S $SERVER_NAME.database.windows.net \
  -U $ADMIN_USER \
  -P $ADMIN_PASS \
  -d $DB_NAME
```

---

## 4️⃣ Run SQL Queries

### Create a Table

```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY IDENTITY(1,1),
    FirstName NVARCHAR(50),
    LastName NVARCHAR(50),
    HireDate DATE
);
```

### Insert Data

```sql
INSERT INTO Employees (FirstName, LastName, HireDate)
VALUES ('John', 'Doe', '2023-01-15');
```

### Query Data

```sql
SELECT * FROM Employees;
```

---

## 5️⃣ Backup and Restore

Azure SQL automatically:

- Backs up databases (7–35 days retention based on service tier)
- Enables **Point-in-Time Restore**

**Portal Restore:**

1. Go to the database → **Restore**.
2. Choose a restore point.
3. Provide a new database name.

**CLI Restore:**

```bash
az sql db restore \
  --dest-name myazuresqldb-restore \
  --name $DB_NAME \
  --server $SERVER_NAME \
  --resource-group $RESOURCE_GROUP \
  --time "2023-08-15T12:00:00Z"
```

---

## 6️⃣ Scale Performance

**Portal:**

- Go to the database → **Compute + storage** → Change DTU/vCore.

**CLI:**

```bash
az sql db update \
  --name $DB_NAME \
  --resource-group $RESOURCE_GROUP \
  --server $SERVER_NAME \
  --service-objective S1
```

---

## 🔒 Security Best Practices

- Use **Azure Active Directory authentication** for central identity management.
- Enable **Transparent Data Encryption (TDE)** (on by default).
- Turn on **Advanced Threat Protection**.
- Use **Private Endpoints** to avoid public exposure.

---

## 📚 Useful Links

- [Azure SQL Documentation](https://learn.microsoft.com/azure/azure-sql/)
- [Azure CLI SQL Commands](https://learn.microsoft.com/cli/azure/sql)
- [Security Best Practices](https://learn.microsoft.com/azure/azure-sql/database/security-overview)

---

✅ You now have a working Azure SQL database in the cloud with firewall rules, connections, queries, and scaling in place.
