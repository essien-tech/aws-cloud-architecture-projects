# Setup Steps (From Scratch)

## Step 1: Sign in to AWS

- Go to AWS Management Console
- Log in with your credentials

---

## Step 2: Create DynamoDB Table

1. Navigate to DynamoDB
2. Click **Create Table**

### Configuration:

- Table Name: `RoadSafetyIncidents`
- Partition Key: `IncidentID` (String)

Leave other settings as default.

Click **Create Table**

---

## Step 3: Create Global Secondary Index (GSI)

1. Open the table
2. Go to **Indexes**
3. Click **Create Index**

### GSI Configuration:

- Partition Key: `IncidentDate` (String)
- Sort Key: `SeverityLevel` (Number)
- Index Name: `IncidentDate-Severity-index`

Click **Create Index**

---

## Step 4: Insert Sample Data

Go to **Explore Table Items → Create Item**

Example:

```json
{
  "IncidentID": "INC001",
  "IncidentDate": "2026-03-17",
  "SeverityLevel": 5,
  "Location": "Accra - Kumasi Road",
  "VehiclesInvolved": 2,
  "Status": "Pending"
}
```

---

Step 5: Query Using GSI

1. Go to Explore Items

2. Select the GSI

3. Query: `IncidentDate = "2026-03-01"`

You’ll get sorted results based on severity.

---

Step 6: Validate
1. Ensure data is stored

2. Confirm GSI works correctly

3. Test multiple queries
   
---

Step 7: Clean Up

1. Delete table

3. Remove resources to avoid charges
