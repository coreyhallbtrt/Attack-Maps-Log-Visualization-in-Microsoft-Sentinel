## 🔐 **Attack Maps / Log Visualization in Microsoft Sentinel**

### 🎯 **Focus Area**:  
**Entra ID Logon Authentication Successes and Failures**

---

### 📘 **Scenario:**
In this lab, I created an interactive **map in Microsoft Sentinel Workbooks** to visually represent both **successful and failed logon attempts** from Microsoft Entra ID (formerly Azure AD). This visualization is powered by KQL queries that pull data from the SignInLogs table and display it on a geographic map.


**KQL Queries Used**

SigninLogs
| where ResultType == 0

| summarize LoginCount = count() by Identity, Latitude = tostring(LocationDetails["geoCoordinates"]["latitude"]), Longitude = tostring(LocationDetails["geoCoordinates"]["longitude"]), City = tostring(LocationDetails["city"]), Country = tostring(LocationDetails["countryOrRegion"])

| project Identity, Latitude, Longitude, City, Country, LoginCount, friendly_label = strcat(Identity, " - ", City, ", ", Country)


SigninLogs
| where ResultType != 0

| summarize LoginCount = count() by Identity, Latitude = tostring(LocationDetails["geoCoordinates"]["latitude"]), Longitude = tostring(LocationDetails["geoCoordinates"]["longitude"]), City = tostring(LocationDetails["city"]), Country = tostring(LocationDetails["countryOrRegion"])

| project Identity, Latitude, Longitude, City, Country, LoginCount, friendly_label = strcat(Identity, " - ", City, ", ", Country)

**What this KQL Query does:**

This query takes the sign-in records, filters out only the successful ones (ResultType = 0) or failed ones (ResultTYpe != 0), and then groups these records by each user's identity and their location data (latitude, longitude, city, and country). It counts how many times each user logged in from those locations and finally shows the key details along with a "friendly label" that combines the user's identity with the city and country information to make the output more readable.

---

### 🚨 **Why This Is Important:**

Visualizing authentication data provides **clear operational and security advantages**:

- **Quick Threat Detection**: Spot login anomalies, geo-spoofing, and brute-force attempts instantly.
- **Trend Analysis**: Identify patterns in user logins, such as common login errors vs. actual threats.
- **Executive Visibility**: Dashboards simplify data for non-technical stakeholders.
- **Accelerated Incident Response**: Easily pinpoint who was targeted, when, and from where.
- **Real-Time Monitoring**: Live dashboards highlight current activity for proactive response.

---

### 🛠️ **Step-by-Step Lab Guide**

#### ✅ Step 1: Access Microsoft Sentinel Workbooks
- Go to the **Azure Portal**
- Navigate to **Microsoft Sentinel**
- Click on **Workbooks**
  
![1 microsoft sentinel ](https://github.com/user-attachments/assets/3be805d7-87a1-4d5a-8577-681e0e2dfb15)
---



#### ✅ Step 2: Add a New Query
- Click **"Add Query"** to create a visual component.
  
![2 new workbook add query](https://github.com/user-attachments/assets/19384972-acdc-4640-9266-2685bde0f82f)

---


#### ✅ Step 3: Use Advanced Editor
- Click **"Advanced Editor"**
- Paste in **JSON code** that contains the **KQL queries** for both successful and failed logon attempts.

  ---

![3 create json table code for newworkbook in advanced editor SUCCESS](https://github.com/user-attachments/assets/2eced72f-2fcb-40d1-af26-d525e8bbaff2)

---


![3 create json table code for newworkbook in advanced editor FAILURE](https://github.com/user-attachments/assets/96bab036-7e74-4527-a970-2586c8a05869)


---

#### ✅ Step 4: Save and Enable Map Visualization
- Click **Save**
- Click the settings (top-left panel) to change the visual type to **"Map"**
- Confirm that your **KQL queries return location data** (Latitude, Longitude, Country, City)

  ---


![4 kql success](https://github.com/user-attachments/assets/95be0049-04ce-43d3-a641-03d567f17010)

---

![4 KQL query worknook map failed logon](https://github.com/user-attachments/assets/5c415856-bafa-4e27-8cf8-99238f742b1d)


---

#### ✅ Step 5 (Optional): Test Queries in Log Analytics Workspace
- Copy both **Successful and Failed logon KQL queries**
- Paste them into **Log Analytics Workspace** to:
  - View full results
  - Conduct deeper analysis
 
  
  - **Second picture is my SUCCESSFUL LOGON Attempts inside of our company tenant**
 ![4 KQL query that shows LAW vs workbook map](https://github.com/user-attachments/assets/90f76fe6-dc22-4f5b-8c6c-59d65466dd62)

---

  ![5 KQL query my sign info SUCCESS](https://github.com/user-attachments/assets/b10e84bc-2771-4c58-991c-568b3c65d49b)
  
---
![4 KQL query that shows LAW failure login](https://github.com/user-attachments/assets/79a4e9e5-7884-48ac-bc92-56076a87f3b5)

---

### ✅ **Summary of What I Did:**

In this lab, I built a **Microsoft Sentinel Workbook map** that visualizes both successful and failed Microsoft Entra ID login attempts. I used **KQL queries** to extract geolocation and login metadata from the SignInLogs table, then rendered this data on a map inside the workbook. This visualization enhances threat detection, highlights authentication trends, and supports real-time monitoring for security teams.

---

