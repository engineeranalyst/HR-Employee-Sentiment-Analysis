# HR: A Case Study on Sentiment Analysis

![HR Analytics Dashboard (My Version)](https://github.com/user-attachments/assets/c5c71c60-e6f7-4b59-b1a5-c80114cd4624)

## 📋 Executive Summary
I performed a comprehensive sentiment analysis of a dataset containing HR data.

Fot this project, I transformed a raw, fragmented HR survey dataset into a high-performance, interactive **Diagnostic Dashboard**. 

The goal was to move beyond simple "Ad Hoc" data requests and provide HR leadership with a persistent "Tactical HUD" to monitor organizational health.

By engineering a custom 5-star sentiment scale and utilizing advanced M-code for job-level classification, this dashboard identifies specific "Conflict Zones" (low-performing departments or questions) and allows for precision decision-making. The final result is a corporate-grade reporting tool that balances descriptive statistics (What is happening?) with diagnostic insights (Why is it happening?).

## 🛠️ Data Cleaning & Engineering Strategy
Before the data could be "deployed" to the dashboard, it underwent a rigorous cleaning process to ensure maximum signal and minimum noise.

* **Source Migration:** Transitioned the data source from a multi-layered Excel Workbook to a flat CSV file for faster performance and better scalability.
* **Synthetic Data Generation:** To create a more robust 5-star distribution, the original **Response** column was updated using the `RANDBETWEEN` function. This provided a realistic bell curve for sentiment analysis. Since this project is using sample data and is meant for learning purposes, this methodology is justified.
* **Column Pruning:** Deleted the redundant **Response Text** column to streamline the model and reduce "Chart Junk."
* **Sentiment Categorization:** Used a **Conditional Column** in Power Query to translate raw scores into actionable tactical categories:
    * **Negative:** Scores 1–2 (Requires immediate reinforcement)
    * **Neutral:** Score 3 (The stalemate/middle ground)
    * **Positive:** Scores 4–5 (High-morale zones)

## 💻 Power Query & M-Code Deep Dive
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

## 📈 Data Modeling & DAX Measures
I developed a library of custom DAX measures to serve as the dashboard's "Vital Signs."

* **Total Responses:** Uses the `COUNTROWS` function to calculate the headcount.
    ```
    Total Responses = COUNTROWS('HR Survey Reponses')
    ```
    
* **Average Rating:** Uses the `AVERAGE` function to calculate the average survey response score.
    ```
    Average Rating = AVERAGE('HR Survey Reponses'[Response])
    ```
* **Completion Percentage:** A sophisticated measure using `VAR` and `DIVIDE` to calculate the ratio of "Complete" vs. "Incomplete" statuses. This prevents "Division by Zero" errors and ensures report stability.
   ```
   Completion Percentage = 
   VAR CompletedSurveys = CALCULATE(
                              [Total Responses],
                              'HR Survey Reponses'[Status]="Complete")
   RETURN DIVIDE(
              CompletedSurveys,
              [Total Responses],
              0)
   ```
## 🎨 Report Design & UI/UX
The dashboard was designed with a "KPI-First" hierarchy, inspired by professional grid systems and clean aesthetics.

* **Top-Level KPIs:** Placed critical numbers (Total Responses, Average Rating, and Completion Percentage) at the very top for immediate "Battlefield Recon."
* **Departmental Sentiment:** Used a **Stacked Bar Chart** to show the 3-way split of Positive/Neutral/Negative across departments.
* **Question-Level Intelligence:** A specialized bar chart to pinpoint exactly which survey questions are driving low morale.
* **Slicer Sidebar:** All filters were moved to the left side to follow natural eye-movement patterns, ensuring the user can quickly toggle between different job levels and departments.

## 📊 Key Insights
The analysis reveals clear patterns in how organizational culture is perceived across different functional areas:

* **Sentiment Polarization:** While the overall organizational average is strong, sentiment is highly polarized. A significant portion of the workforce falls into the "Neutral" (3) bucket, indicating a "wait-and-see" approach to engagement.
* **The "Question Gap":** Analysis of the "Rating by Question" chart identified that questions related to specific operational expectations consistently yield lower scores compared to questions regarding team collaboration and support.
* **Departmental Variance:** Sentiment is not uniform. Certain departments report lower-than-average sentiment scores, suggesting localized management challenges or resource constraints compared to the higher-performing divisions.
* **High Completion Integrity:** With a **99% completion rate**, the survey data provides a highly reliable and representative baseline for organizational strategy, minimizing the risk of "selection bias" in our findings.

## 💡 Recommendations
Based on the diagnostic patterns identified, I recommend the following tactical interventions to improve employee sentiment:

1. **Targeted Management Coaching:** Since sentiment variance is linked to specific departments, I recommend deploying specialized engagement workshops for managers in lower-performing areas. Focus these sessions on aligning team objectives with individual contributions.
2. **"Quick Wins" for Low-Performing Questions:** Address the specific questions identified in the "Rating by Question" chart. By focusing on areas where scores are consistently low, the organization can target "lowest-hanging fruit" issues to achieve an immediate lift in sentiment scores.
3. **Convert the "Neutrals":** With a large block of employees in the "Neutral" (Score 3) category, the organization has a significant opportunity. I recommend an internal communication campaign focused on the "Employee Value Proposition" (EVP) to help convert these neutral observers into active, positive contributors.
4. **Iterative Feedback Loops:** Utilize this dashboard as a persistent "Tactical HUD." By refreshing this data regularly, HR leadership can monitor the *velocity* of sentiment change following these interventions, effectively moving from "Reactive" to "Proactive" personnel management.
