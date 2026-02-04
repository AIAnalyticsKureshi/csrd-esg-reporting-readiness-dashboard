# CSRD ESG Reporting Readiness – Power BI Analytics Project

**Project Type:** Business Intelligence | ESG Compliance Analytics | Data Quality Management  
**Tools Used:** Power BI Desktop, Excel, DAX, Power Query, Data Modeling  
**Developer:** Mohammad Musabhai Kureshi  
**Project Status:** Complete ✅

---

## 📋 Executive Summary

This project delivers an end-to-end **ESG (Environmental, Social, Governance) reporting readiness dashboard** designed to monitor compliance with the **Corporate Sustainability Reporting Directive (CSRD)**. The solution provides real-time insights into data completeness, control validation, issue tracking, and audit readiness for sustainability reporting.

### Key Business Value
- **66.7% data completeness** tracked across 12 KPIs spanning E1-E5, S1, and G1 topics
- **100% evidence and ownership coverage** ensuring audit trail transparency
- **60% control pass rate** with automated validation checks
- **10 active data issues** monitored with severity-based prioritization
- Audit-ready reporting pack supporting annual CSRD compliance

---

## 🎯 Project Objectives

1. **Monitor ESG Data Readiness:** Track completeness, quality, and availability of sustainability KPIs
2. **Ensure Audit Compliance:** Maintain evidence linkage and ownership assignment for all metrics
3. **Automate Data Quality Controls:** Implement validation rules to flag missing, duplicate, or invalid data
4. **Enable Issue Management:** Track and prioritize data gaps with severity-based workflow
5. **Support Stakeholder Reporting:** Deliver executive dashboards for ESG leadership and auditors

---

## 📊 Dashboard Features

### 1. **Main KPI Overview Dashboard**
- **High-level Metrics:**
  - Data Completeness: 66.7%
  - Evidence Coverage: 100%
  - Ownership Coverage: 100%
  - Control Pass Rate: 60%
  - Open Data Issues: 10 (6 high-severity)
  
- **KPI Detail Table:**
  - 12 ESG KPIs across 6 ESRS topics (E1, E3, E5, S1, G1)
  - Per-KPI tracking of open issues, control pass rates, evidence, and ownership
  - Visual indicators for data quality status

### 2. **Issue Tracking Dashboard**
- **Issue Log Table:**
  - 10 tracked issues with severity classification (High, Medium, Low)
  - Issue type categorization (missing KPI values, dataset completeness gaps)
  - Owner assignment for resolution accountability
  - Temporal tracking (Year, Quarter, Month, Day)

- **Filters:**
  - Severity level (High/Medium/Low)
  - Topic Name (Climate Change, Water, Own Workforce, etc.)
  - KPI Name

### 3. **Data Quality Controls Dashboard**
- **Control Summary:**
  - Total Controls: 5
  - Controls Passed: 3
  - Control Pass Rate: 60%
  
- **Control Detail Table:**
  - **Completeness Check:** Validates required KPI coverage (24 records checked, 10 failures)
  - **Duplicate Key Check:** Ensures unique Period-Entity-KPI combinations (14 records, 0 failures) ✅
  - **Negative Value Check:** Flags invalid negative values (14 records, 0 failures) ✅
  - **Numeric Type Check:** Validates data type integrity (14 records, 0 failures) ✅

- **Filters:**
  - Fiscal Year
  - Control Type (null/coverage, duplicate, range, type)

---

## 🗂️ Data Architecture

### Data Model Overview
The project implements a **star schema** with the following structure:

#### **Dimension Tables**
1. **Dim_KPI** (12 KPIs)
   - KPI_ID, KPI_Name, ESRS_Topic, Description, Unit, Frequency, Required_Flag, Materiality_Flag
   - Tracks scope 1/2/3 emissions, energy, water, waste, workforce metrics, and governance KPIs

2. **Dim_ESRS_Topic** (6 Topics)
   - ESRS_Topic, Topic_Name, Pillar
   - Categories: E1 (Climate), E2 (Pollution), E3 (Water), E5 (Circular Economy), S1 (Workforce), G1 (Governance)

3. **Dim_Entity_Site**
   - Entity_ID, Entity_Name, Country, Site_ID, Site_Name, Reporting_Boundary
   - Organizational hierarchy for consolidated reporting

4. **Dim_Period** (2 Fiscal Years)
   - Period_ID, Period_End_Date, Fiscal_Year, Period_Type
   - Temporal dimension for annual reporting cycles

5. **Dim_SourceDocument** (5 Evidence Sources)
   - Source_ID, Publisher, Publication_Year, Source_Title, Evidence_Type, Evidence_Location
   - Links to external audit evidence (PDF reports)

6. **Dim_Owner** (6 Owners)
   - Owner_ID, Function, Role, Owner_Type, Notes
   - Data ownership assignment (Finance, Env Health Safety, Supply Chain, HR, Legal)

#### **Fact Tables**
1. **Fact_KPI_Annual** (14 Records)
   - Period_ID, Entity_ID, KPI_ID, Value, Unit, Source_ID, Notes, Source_Page
   - Annual KPI values with source traceability

2. **Fact_KPI_Assignment** (12 Assignments)
   - KPI_ID, Owner_ID, In_Scope_Flag, Evidence_Required_Flag, Expected_Frequency, Data_Source_Method
   - Ownership and collection methodology mapping

3. **Fact_ControlResults** (5 Controls)
   - Control_ID, Control_Name, Control_Type, Period_ID, Status, Fail_Count, Records_Checked
   - Automated data quality validation results

4. **Fact_IssueLog** (10 Issues)
   - Issue_ID, Created_Date, Severity, Issue_Type, Period_ID, KPI_ID, Owner_ID, Status, Description
   - Active issue tracking with resolution workflow

---

## 🔧 Technical Implementation

### Power BI Components

#### **Data Source Connection**
- **Input:** Excel workbook with 10 normalized tables
- **Data Refresh:** Manual (scheduled refresh ready for deployment)
- **Relationships:** Star schema with 1:many cardinality

#### **DAX Measures** (Implemented)
```dax
Data Completeness % = 
DIVIDE(
    COUNTROWS(FILTER(Fact_KPI_Annual, NOT(ISBLANK(Fact_KPI_Annual[Value])))),
    COUNTROWS(Dim_KPI) * DISTINCTCOUNT(Dim_Period[Fiscal_Year])
)

Evidence Coverage % = 
DIVIDE(
    COUNTROWS(FILTER(Fact_KPI_Assignment, Fact_KPI_Assignment[Evidence_Required_Flag] = "Y")),
    COUNTROWS(Fact_KPI_Assignment)
)

Control Pass Rate % = 
DIVIDE(
    CALCULATE(COUNTROWS(Fact_ControlResults), Fact_ControlResults[Status] = "PASS"),
    COUNTROWS(Fact_ControlResults)
)

Open High-Severity Issues = 
CALCULATE(
    COUNTROWS(Fact_IssueLog),
    Fact_IssueLog[Severity] = "High",
    Fact_IssueLog[Status] = "Open"
)
```

#### **Power Query Transformations**
- Column renaming and standardization
- Data type conversion (dates, numbers, text)
- Null value handling
- Table unpivoting for KPI metrics
- Reference data validation

#### **Visualizations Used**
- **Card Visuals:** KPI summary metrics (4 cards)
- **Table Visuals:** Detailed KPI tracking, issue log, control results
- **Filters/Slicers:** Fiscal_Year, Topic_Name, KPI_Name, Severity, Control_Type
- **Conditional Formatting:** Status indicators (PASS/FAIL coloring)

---

## 📈 Business Intelligence Insights

### Key Findings

1. **Data Gaps Identified:**
   - **E1 (Climate):** Total electricity consumption missing (high severity)
   - **E3 (Water):** Water withdrawal data incomplete (medium severity)
   - **G1 (Business Conduct):** Data owner and evidence assignments incomplete (2 high-severity issues)
   - **S1 (Own Workforce):** Training hours and workplace injuries partially missing

2. **Control Validation Success:**
   - ✅ **Duplicate prevention:** No duplicate KPI entries detected
   - ✅ **Data type integrity:** All numeric fields validated
   - ✅ **Range validation:** No negative values in non-applicable KPIs
   - ❌ **Coverage gaps:** 10 missing KPI values across FY2023-2024

3. **Ownership Accountability:**
   - 100% of KPIs assigned to responsible owners (Finance, EHS, Supply Chain, HR, Legal, Compliance)
   - All evidence requirements documented with source document linkage

---

## 🛠️ Skills Demonstrated

### Technical Skills
- **Power BI Development:** Dashboard design, DAX calculations, data modeling
- **Data Modeling:** Star schema design, relationship management, cardinality optimization
- **ETL/Power Query:** Data transformation, cleaning, normalization
- **Excel Integration:** Multi-sheet workbook import, data validation
- **SQL Concepts:** Table relationships, fact/dimension design, aggregation logic

### Business Analysis Skills
- **ESG Domain Knowledge:** Understanding CSRD/ESRS reporting requirements
- **Data Quality Management:** Control design, validation rules, issue tracking
- **Stakeholder Communication:** Executive-level dashboard design
- **Audit Readiness:** Evidence traceability, documentation standards
- **Issue Prioritization:** Severity-based workflow management

### Analytical Skills
- **Gap Analysis:** Identifying missing data across 12 KPIs
- **Performance Metrics:** Completeness, coverage, and control pass rate tracking
- **Root Cause Analysis:** Mapping issues to data sources and owners
- **Trend Monitoring:** Year-over-year comparison setup (FY2023 vs FY2024)

---

## 📁 Project Deliverables

### Files Included
1. **CSRD_ESG_Reporting_Readiness_Dashboard_PowerBI.pbix** – Interactive Power BI dashboard (3 pages)
2. **CSRD_ESG_Reporting_Readiness_Dataset_v2.xlsx** – Source data with 10 tables
3. **CSRD_ESG_AuditReady_ReportingPack_v1.pdf** – Static PDF report snapshots (3 pages)
4. **CSRD_ESG_Reporting_Readiness_Project_Overview.docx** – Project documentation (optional)

### Dashboard Pages
- **Page 1:** KPI Overview & Status Summary
- **Page 2:** Issue Log & Severity Tracking
- **Page 3:** Data Quality Controls & Validation Results

---

## 🎓 Business Context

### What is CSRD?
The **Corporate Sustainability Reporting Directive (CSRD)** is an EU regulation requiring companies to report on sustainability impacts across environmental, social, and governance (ESG) dimensions. This project addresses the **audit readiness challenge** by:
- Tracking 12 mandatory KPIs across ESRS E1, E3, E5, S1, G1 topics
- Ensuring evidence availability for external auditors
- Monitoring data quality through automated controls
- Providing transparency into gaps and ownership

### Use Cases
- **ESG Managers:** Monitor reporting readiness and identify data gaps
- **Data Owners:** Track assigned KPIs and outstanding issues
- **Internal Audit:** Validate control effectiveness and evidence completeness
- **Executive Leadership:** Review compliance status and approve reporting pack
- **External Auditors:** Access audit-ready report pack with full traceability

---

## 🚀 Future Enhancements

### Potential Improvements
1. **Automated Data Refresh:** Schedule daily/weekly Power BI Service refresh from SQL Server or SharePoint
2. **Drill-Through Pages:** Add detail pages for individual KPIs with time-series analysis
3. **Alert System:** Email notifications for new high-severity issues
4. **Mobile Layout:** Optimize dashboard for mobile viewing (Power BI app)
5. **Benchmark Comparison:** Add industry average KPIs for context
6. **Predictive Analytics:** Forecast data completeness trends using Power BI AI visuals
7. **Integration with Source Systems:** Direct API connections to ERP, HRIS, energy management platforms

---

## 📞 Contact Information

**Mohammad Musabhai Kureshi**  
📧 Email: [mohd.kureshi04@gmail.com]  
💼 LinkedIn: [https://www.linkedin.com/in/mohammad-kureshi/]  

---

## 📜 License & Usage

This project is part of a professional portfolio demonstrating Business Intelligence and ESG analytics capabilities. The data used is anonymized and representative of real-world CSRD reporting scenarios.

---

## 🏆 Project Highlights for Recruiters

### Why This Project Stands Out
✅ **Real-World Business Problem:** Addresses EU regulatory compliance (CSRD), a $500B+ market  
✅ **End-to-End BI Solution:** From data modeling to executive dashboards  
✅ **Data Quality Focus:** Implements automated controls and validation rules  
✅ **Audit-Ready Standards:** 100% evidence coverage and ownership traceability  
✅ **Scalable Design:** Star schema supports expansion to 100+ KPIs and multi-entity reporting  

### Relevance to Junior BI Analyst Roles
- **Dashboard Development:** Created 3-page interactive Power BI report
- **DAX Proficiency:** Implemented 10+ measures for KPIs, coverage %, and issue counts
- **Data Modeling:** Designed star schema with 6 dimensions and 4 fact tables
- **Issue Tracking:** Built severity-based workflow management system
- **Stakeholder Focus:** Delivered audit-ready reporting pack for compliance teams

---

**Last Updated:** February 2026  
**Project Version:** 1.0  
**Documentation:** Complete  
**Demo Availability:** Available upon request
