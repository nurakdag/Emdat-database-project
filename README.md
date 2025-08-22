# 🌍 EM-DAT Public Disaster Database Project  

## 📖 Project Overview  

The **EM-DAT Public Disaster Database Project** is built on the EM-DAT disaster dataset.  
This project aims to transform raw disaster data into a **relational database** for structured analysis and decision support.  

Primary goal is to provide **researchers, policymakers, and organizations** with a well-structured schema that allows them to query disaster information efficiently and gain insights into disaster impacts, financials, and responses.  



## 📂 Dataset Information  

- **Source:** EM-DAT Disaster Dataset (Kaggle)  
- **Files:** 20 CSV files  
- **Rows:** 15,784  
- **Columns:** 45  
- **Non-numeric columns:** 21  

---

## 🏗 Database Design  

### 🔹 ER Diagram  

🔹 Main Tables

Disaster_classification → Groups, subgroups, types, and subtypes.

Disaster_date → Start/end year, month, day, and duration.

Disaster_financials → Aid contribution, damages (total & insured).

Impact → Deaths, injured, homeless, total affected.

Country / Region / Location → Geographical information.

d_admin_unit, admin1, admin2 → Administrative units.

Disaster_Response → OFDA responses, appeals, declarations.

🔹 Relationship Tables

Disaster_classification_has_Country

Disaster_classification_has_Impact

Disaster_date_has_Impact

Disaster_financials_has_Disaster_date

Disaster_classification_has_d_river_basin

<img width="1225" height="837" alt="image" src="https://github.com/user-attachments/assets/c7dd4875-8043-47e1-86de-a9e9ed2781cd" />

```markdown
## 🛠 Stored Procedure  

```sql
DELIMITER //
CREATE PROCEDURE GetDisastersByYear(IN year_in INT)
BEGIN
  SELECT d.Disaster_type, dd.Start_Year, i.Total_deaths
  FROM disaster_classification d
  JOIN disaster_classification_has_impact di 
    ON d.classification_key = di.Disaster_classification_classification_key
  JOIN impact i 
    ON di.Impact_impact_id = i.impact_id
  JOIN disaster_date dd 
    ON dd.DisNo = i.impact_id
  WHERE dd.Start_Year = year_in;
END //
DELIMITER ;
```

---

## 👀 Example View  

```sql
CREATE VIEW NaturalDisasters AS
SELECT d.Disaster_type, i.Total_deaths, i.Total_affected
FROM disaster_classification d
JOIN disaster_classification_has_impact di 
  ON d.classification_key = di.Disaster_classification_classification_key
JOIN impact i 
  ON di.Impact_impact_id = i.impact_id
WHERE d.disaster_group = 'Natural';




## 📌 References

Kaggle Dataset

EM-DAT Official Website






