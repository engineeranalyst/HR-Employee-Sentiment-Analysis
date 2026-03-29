## 📋 I. Project Overview
This project transformed a raw, fragmented HR survey dataset into a high-performance, interactive **Diagnostic Dashboard**. The goal was to move beyond simple "Ad Hoc" data requests and provide HR leadership with a persistent "Tactical HUD" to monitor organizational health.

By engineering a custom 5-star sentiment scale and utilizing advanced M-code for job-level classification, this dashboard identifies specific "Conflict Zones" (low-performing departments or questions) and allows for precision decision-making. The final result is a corporate-grade reporting tool that balances descriptive statistics (What is happening?) with diagnostic insights (Why is it happening?).

---

## 🛠️ II. Data Cleaning & Engineering Strategy
Before the data could be "deployed" to the dashboard, it underwent a rigorous cleaning process to ensure maximum signal and minimum noise.

* **Source Migration:** Transitioned the data source from a multi-layered Excel Workbook to a flat CSV file for faster performance and better scalability.
* **Synthetic Data Generation:** To create a more robust 5-star distribution, the original **Response** column was updated using the `RANDBETWEEN(1,5)` function. This provided a realistic bell curve for sentiment analysis. (**Note: this project is using sample data and is meant for learning purposes. So this step is justififed)
* **Column Pruning:** Deleted the redundant **Response Text** column to streamline the model and reduce "Chart Junk."
* **Sentiment Categorization:** Used a **Conditional Column** in Power Query to translate raw scores into actionable tactical categories:
    * **Negative:** Scores 1–2 (Requires immediate reinforcement)
    * **Neutral:** Score 3 (The stalemate/middle ground)
    * **Positive:** Scores 4–5 (High-morale zones)

---

## 💻 III. Power Query & M-Code Deep Dive
The "Engine Room" of this project involved custom M-code to handle complex organizational hierarchies that the standard UI couldn't process alone.

* **Job Level Classification:** I wrote a custom `if...then...else` statement in the Power Query Advanced Editor. This code scanned four separate binary columns (Director, Manager, Supervisor, Staff) to create a single, unified **Job Level** column.
    ```
    Table.AddColumn(#"Added Response Category", "Job Level", each 
       if [Director] = 1 then "Director" 
       else if [Manager] = 1 then "Manager" 
       else if [Supervisor] = 1 then "Supervisor" 
       else if [Staff] = 1 then "Staff" 
       else "Other"
    )
    ```
    *This allowed for a single "Slicer" to filter the entire dashboard by rank, providing streamlined tactical oversight.*

---

## 📈 IV. Data Modeling & DAX Measures
I developed a library of custom DAX measures to serve as the dashboard's "Vital Signs."

* **Total Responses:** Uses the `COUNTROWS` to calculate the headcount.
    ```
    Total Responses = COUNTROWS('HR Survey Reponses')
    ```
    
* **Average Rating:** A clean `AVERAGE` function (optimized after pre-cleaning the 0/null values at the source).
    ```
    Average Rating = AVERAGE('HR Survey Reponses'[Response])
    ```
* **Completion Percentage:** A sophisticated measure using `VAR` and `DIVIDE` to calculate the ratio of "Complete" vs. "Incomplete" statuses. This prevents "Division by Zero" errors and ensures report stability.
   ```
   Completion Percentage = 
   VAR CompletedSurveys = CALCULATE(
                              COUNTROWS('HR Survey Reponses'),
                              'HR Survey Reponses'[Status]="Complete")
   RETURN DIVIDE(
              CompletedSurveys,
              [Total Responses],
              0)
   ```
---

## 🎨 V. Report Design & UI/UX (The Visual HUD)
The dashboard was designed with a "KPI-First" hierarchy, inspired by professional grid systems and clean aesthetics.

* **Top-Level KPIs:** Placed critical numbers (Total Responses, Average Rating) at the very top for immediate "Battlefield Recon."
* **Departmental Sentiment:** Used a **Stacked Bar Chart** to show the 3-way split of Positive/Neutral/Negative across departments.
* **Question-Level Intelligence:** A specialized bar chart to pinpoint exactly which survey questions are driving low morale.
* **Slicer Sidebar:** All filters were moved to the left side to follow natural eye-movement patterns, ensuring the user can quickly toggle between different job levels and departments.

---

## 🏁 VI. Final Analysis & Conclusion
This project demonstrates advanced proficiency in **End-to-End Business Intelligence**. 
By successfully managing the ETL process (Power Query), the Data Model (DAX), and the User Interface (Visuals), I have created a tool that provides "Corporate Level" insights. 
It doesn't just show data; it tells a story of where the organization is winning and where it is "under fire."
