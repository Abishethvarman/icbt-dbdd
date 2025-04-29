# TechVille Centralized Database Management System

## Task 1: Explanation of Data Models

A **data model** is a conceptual representation of data structures and the relationships between them. It describes how data is stored, organized, and manipulated within a database. Data models are crucial in ensuring that data is accurately represented and that the system can be easily queried and maintained.

### Types of Data Models:
1. **Hierarchical Model**: Data is organized in a tree-like structure where each record has a single parent and potentially multiple children. This model is rigid, and data retrieval can be difficult when relationships are complex.
   - **Example**: IBM's Information Management System (IMS).
   - **Limitations**: It lacks flexibility in representing many-to-many relationships and is difficult to scale.

2. **Network Model**: Similar to the hierarchical model, but with more flexibility, allowing records to have multiple parent and child records. It is represented as a graph, allowing many-to-many relationships.
   - **Example**: Integrated Data Store (IDS).
   - **Limitations**: More flexible than the hierarchical model but still complex and difficult to work with due to the interconnectivity of the data.

3. **Relational Model**: Organizes data into tables (or relations) that can be related to each other using keys. It uses a set of mathematical concepts, including set theory, which allows for more efficient and flexible querying.
   - **Example**: SQL databases (MySQL, PostgreSQL, Oracle).
   - **Advantages**: Easier to query with SQL, highly flexible, and supports data integrity through constraints.
   - **Limitations**: Performance can be a concern for very large datasets or highly complex queries.

4. **Object-Oriented Model**: Data is represented as objects, similar to objects in object-oriented programming languages. This model is well-suited for applications that require complex data representations, such as multimedia or geographical information systems.
   - **Example**: ObjectDB.
   - **Advantages**: Supports complex data types, integrates well with object-oriented programming languages.
   - **Limitations**: More complex and less standardized than the relational model.

5. **Document Model**: Data is stored as documents, typically in formats like JSON or XML. This model is popular in NoSQL databases and is useful for unstructured or semi-structured data.
   - **Example**: MongoDB.
   - **Advantages**: Flexible, easy to scale horizontally, and can handle unstructured data.
   - **Limitations**: Less efficient for complex queries or data requiring strict relationships.

### Why Older Data Models Are Being Replaced:
- **Flexibility**: Relational models and newer models (like document-based) provide greater flexibility compared to hierarchical and network models.
- **Complex Queries**: The relational model allows for the use of SQL, making it easier to perform complex queries.
- **Scalability**: Modern databases like NoSQL allow for horizontal scaling, which is vital in handling large amounts of data.
- **Standardization**: Relational databases are widely standardized, making them easier to adopt and use across various industries.

## Task 2: Entity-Relationship Diagram (ERD)

### Entities:
- **Citizen** (CitizenID, Name, Address, ContactDetails)
- **UtilityAccount** (AccountID, AccountType, AccountStatus, BillAmount, DueDate)
- **Complaint** (ComplaintID, CitizenID, ServiceID, Description, DateFiled, ResolutionStatus)
- **EmergencyIncident** (IncidentID, CitizenID, EmergencyType, Location, ResponseStatus)
- **MunicipalAgency** (AgencyID, AgencyName, ServiceType)
- **Service** (ServiceID, ServiceName, AgencyID)

### Relationships:
- **Citizen** to **UtilityAccount**: One-to-many (A citizen can have multiple utility accounts).
- **Citizen** to **Complaint**: One-to-many (A citizen can file multiple complaints).
- **Citizen** to **EmergencyIncident**: One-to-many (A citizen can report multiple emergencies).
- **Service** to **MunicipalAgency**: Many-to-one (A service is managed by one municipal agency).
- **Complaint** to **Service**: Many-to-one (Each complaint is related to a specific service).

### ERD Representation:
+----------------+      +-----------------+        +-----------------+
|     Citizen    |<---->| UtilityAccount  |        | EmergencyIncident|
+----------------+      +-----------------+        +-----------------+
| CitizenID (PK) |      | AccountID (PK)  |        | IncidentID (PK)  |
| Name           |      | AccountType     |        | CitizenID (FK)   |
| Address        |      | BillAmount      |        | EmergencyType    |
| ContactDetails |      | DueDate         |        | Location         |
+----------------+      +-----------------+        | ResponseStatus   |
                      +-----------------+        +-----------------+
                                                  
                      +-----------------+         +-----------------+
                      |     Service     |<--------| MunicipalAgency |
                      +-----------------+         +-----------------+
                      | ServiceID (PK)  |         | AgencyID (PK)   |
                      | ServiceName     |         | AgencyName      |
                      | AgencyID (FK)   |         | ServiceType     |
                      +-----------------+         +-----------------+
                      |                 |
                      +-----------------+

## Task 3: Relational Schema

### Step 1: Convert ERD to Relational Schema

The entities from the ERD will map to the following relational schemas:

- **Citizen (CitizenID PK, Name, Address, ContactDetails)**
- **UtilityAccount (AccountID PK, AccountType, BillAmount, DueDate, CitizenID FK)**
- **Complaint (ComplaintID PK, CitizenID FK, ServiceID FK, Description, DateFiled, ResolutionStatus)**
- **EmergencyIncident (IncidentID PK, CitizenID FK, EmergencyType, Location, ResponseStatus)**
- **MunicipalAgency (AgencyID PK, AgencyName, ServiceType)**
- **Service (ServiceID PK, ServiceName, AgencyID FK)**

### Step 2: Normalization to 3NF
- **1NF**: All tables are in 1NF as there are no repeating groups, and each field contains only atomic values.
- **2NF**: The tables are in 2NF because all non-key attributes are fully dependent on the primary key.
- **3NF**: The tables are in 3NF because there are no transitive dependencies (i.e., non-key attributes depending on other non-key attributes).

## Task 4: SQL Server Database Creation

```sql
CREATE DATABASE TechVilleDB;
GO

USE TechVilleDB;

CREATE TABLE Citizen (
    CitizenID INT PRIMARY KEY,
    Name VARCHAR(100),
    Address VARCHAR(255),
    ContactDetails VARCHAR(50)
);

CREATE TABLE UtilityAccount (
    AccountID INT PRIMARY KEY,
    AccountType VARCHAR(50),
    BillAmount DECIMAL(10, 2),
    DueDate DATE,
    CitizenID INT,
    FOREIGN KEY (CitizenID) REFERENCES Citizen(CitizenID)
);

CREATE TABLE Complaint (
    ComplaintID INT PRIMARY KEY,
    CitizenID INT,
    ServiceID INT,
    Description TEXT,
    DateFiled DATE,
    ResolutionStatus VARCHAR(50),
    FOREIGN KEY (CitizenID) REFERENCES Citizen(CitizenID),
    FOREIGN KEY (ServiceID) REFERENCES Service(ServiceID)
);

CREATE TABLE EmergencyIncident (
    IncidentID INT PRIMARY KEY,
    CitizenID INT,
    EmergencyType VARCHAR(50),
    Location VARCHAR(255),
    ResponseStatus VARCHAR(50),
    FOREIGN KEY (CitizenID) REFERENCES Citizen(CitizenID)
);

CREATE TABLE MunicipalAgency (
    AgencyID INT PRIMARY KEY,
    AgencyName VARCHAR(100),
    ServiceType VARCHAR(50)
);

CREATE TABLE Service (
    ServiceID INT PRIMARY KEY,
    ServiceName VARCHAR(100),
    AgencyID INT,
    FOREIGN KEY (AgencyID) REFERENCES MunicipalAgency(AgencyID)
);


