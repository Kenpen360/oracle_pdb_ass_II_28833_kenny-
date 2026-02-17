# Oracle Pluggable Database (PDB) Management Assignment

**Course:** INSY 8311 – Database Development with PL/SQL  
**Instructor:** Eric Maniraguha  

---

## Student Information

| Field | Value |
|------|------|
| **Full Name** | Kenny |
| **Student ID** | 28833 |
| **PDB Name** | `ke_pdb_28833` |
| **Username Created** | `kenny_plsqlauca_28833` |
| **GitHub Repository** | `oracle_pdb_ass_II_28833_kenny` |
| **Submission Date** | February 16, 2026 |

---

## Assignment Overview

This assignment is a practical implementation of Oracle Multitenant Architecture. It focuses on creating, managing, deleting, and monitoring Pluggable Databases (PDBs), as well as configuring Oracle Enterprise Manager (OEM). All steps are documented and supported with screenshots.

### Tasks Performed
- Creation of a permanent PDB with an administrative user
- Creation and complete deletion of a temporary PDB
- Configuration and access of Oracle Enterprise Manager (OEM)
- Documentation of errors encountered and their resolutions

---

## Environment Used

| Component | Details |
|---------|--------|
| **Database** | Oracle Database 21c Express Edition (21.3.0.0.0) |
| **Vendor** | Oracle Corporation |
| **Operating System** | Microsoft Windows 64-bit |
| **Container Database (CDB)** | `XE` |
| **OEM HTTPS Port** | `5501` |
| **OEM URL** | https://localhost:5501/em |

---

## Task 1: Creating the Main Pluggable Database

### Configuration Details

- **PDB Name:** `ke_pdb_28833`
- **Admin User:** `kenny_plsqlauca_28833`
- **Password:** `12345`
- **Datafile Location:**  
  `C:\APP\ORADATA\XE\KE_PDB_28833\`

### SQL Commands Executed

```sql
CREATE PLUGGABLE DATABASE ke_pdb_28833
  ADMIN USER kenny_plsqlauca_28833 IDENTIFIED BY 12345
  FILE_NAME_CONVERT = (
    'C:\APP\ORADATA\XE\PDBSEED',
    'C:\APP\ORADATA\XE\KE_PDB_28833'
  );

ALTER PLUGGABLE DATABASE ke_pdb_28833 OPEN;

ALTER SESSION SET CONTAINER = ke_pdb_28833;
GRANT DBA TO kenny_plsqlauca_28833;
```

### Verification
PDB status shows READ WRITE

User kenny_plsqlauca_28833 successfully created

User granted DBA privileges

PDB visible when running SHOW PDBS

### Screenshots
screenshots/task1_pdb_creation.jpg

screenshots/task1_user_grant.jpg

## Task 2: Temporary PDB Creation and Deletion
### Temporary PDB Details

**PDB Name:** `ke_to_delete_pdb_28833`

**Admin User:** `temp_admin`

**Password:** `temp123`

**Datafile Location:**
`C:\APP\ORADATA\XE\KE_TO_DELETE_PDB_28833\`

### SQL Commands Executed

```sql
ALTER SESSION SET CONTAINER = CDB$ROOT;

CREATE PLUGGABLE DATABASE ke_to_delete_pdb_28833
  ADMIN USER temp_admin IDENTIFIED BY temp123
  FILE_NAME_CONVERT = (
    'C:\APP\ORADATA\XE\PDBSEED\',
    'C:\APP\ORADATA\XE\KE_TO_DELETE_PDB_28833\'
  );

SHOW PDB;

ALTER PLUGGABLE DATABASE ke_to_delete_pdb_28833 CLOSE;

DROP PLUGGABLE DATABASE ke_to_delete_pdb_28833 INCLUDING DATAFILES;

SHOW PDB;
```

### Verification
+ Temporary PDB appeared in the PDB list

+ PDB was successfully dropped

* Datafiles were completely removed

* PDB no longer appears when running SHOW PDBS

### Error Encountered
ORA-65040: Attempted to create a PDB while inside another PDB

### Resolution
ALTER SESSION SET CONTAINER = CDB$ROOT;

### Screenshots
screenshots/task2_temp_pdb_create.jpg

screenshots/task2_temp_pdb_delete.jpg

## Task 3: Oracle Enterprise Manager (OEM) Configuration
SQL Commands Executed
ALTER SESSION SET CONTAINER = ke_pdb_28833;

SELECT dbms_xdb_config.gethttpsport() FROM dual;

EXEC DBMS_XDB_CONFIG.SETHTTPSPORT(5501);

SELECT dbms_xdb_config.gethttpsport() FROM dual;

### Verification
HTTPS port changed from 0 to 5501

OEM accessible via browser

Database and PDB visible on the OEM dashboard

Access Details
URL: https://localhost:5501/em

Database: XE / KE_PDB_28833

Version: 21.3.0.0.0 Express Edition

### Screenshots
screenshots/task3_oem_config.jpg

screenshots/task3_oem_dashboard1.jpg

screenshots/task3_oem_dashboard2.jpg

### Issues Encountered and Solutions
Issue	Solution
ORA-65040	Switched to CDB$ROOT
ORA-65020	Verified PDB state before dropping
OEM HTTPS port returned 0	Enabled port using DBMS_XDB_CONFIG.SETHTTPSPORT(5501)
File path errors	Corrected directory formatting

### Repository Structure
oracle_pdb_ass_II_28833_kenny/
│
├── README.md
│
└── screenshots/
    ├── task1_pdb_creation.jpg
    ├── task1_user_grant.jpg
    ├── task2_temp_pdb_create.jpg
    ├── task2_temp_pdb_delete.jpg
    ├── task3_oem_config.jpg
    ├── task3_oem_dashboard1.jpg
    └── task3_oem_dashboard2.jpg
