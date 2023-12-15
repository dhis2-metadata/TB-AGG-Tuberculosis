# TB Notifications and Outcomes System Design Document

Version 2.0.0

## Introduction

This document describes the system design for **TB programmes to integrate routine aggregate reporting into the HMIS**. Depending on the local context, this data may be populated by paper-based reporting or by aggregating data from electronic individual-level systems such as DHIS2 Tracker or other tools.
In particular, the TB aggregate version 2.0.0 aims at outlining the updates made in definitions and data entry forms as a result of the publishing of the Consolidated guidance on tuberculosis data generation and use (the link will be provided upon publication on the WHO website).

## Overview

|              Dataset name             | Periodicity |               Purpose         |
|:-------------------------------------:|:-----------:|:---------------------:|
| TB - Case notification     | Quarterly   | Reporting on new, recurrent and re-registrations  |
| TB - Case notification (Additional)   | Yearly      | Reporting on rifampicin, Isoniazid, Fluoroquinolones, Bedaquiline, and linezolid susceptibility testing and case reporting        |
| TB - Treatment outcomes     | Quarterly   | Reporting on the outcomes of DS TB cases based on the regimen type     |
| TB - Treatment outcomes (second line) | Yearly   | Reporting on the outcomes of DR TB cases based on long and short regimen types     |
| TB - Legacy metadata*   | Quarterly   | Grouping the legacy metadata to maintain continuity and preserve historical data captured under previous guidelines or frameworks |

## Datasets Structure and Design

### TB - Legacy Metadata

Legacy indicators (labeled as “**LEGACY_TB**”) were employed primarily to maintain continuity and preserve historical data captured under previous guidelines or frameworks. These indicators represent information that was valid and relevant in the past but may not align perfectly with the updated or current standards. They serve as a bridge between historical data and the evolving framework, allowing for comparison, analysis, and ensuring a smooth transition between old and new systems. Using legacy indicators enables implementations to track trends over time, understand historical context, and make informed decisions while transitioning to newer methodologies or systems.

Upon the use of legacy indicators and the updated framework of metadata present in version 2.0.0 implementers should be paying particular attention to **data and content consistency**, as legacy indicators might not align perfectly with the current standards or definitions; and **interpretation challenges**, as guidelines and methodologies evolve and legacy indicators might not accurately reflect the current context or priorities and create a ground for potential misinterpretation of data and long-term limitation of content relevance.

More information is provided also in the “Dashboards and Analytics” section of this document.

### TB - Case notification

> **NOTE**
>
> Although this dataset is designed by default as a quarterly report, the same template could be also used for an annual report depending on the needs and the resources of the implementation.

The TB aggregate version 2.0.0 metadata mirrors some key changes in terminology and definitions outlined in the new guidance. The naming convention was applied to new, older, and legacy metadata.

- **The old “Relapses” are now called “Recurrent“ cases** - This change ensures alignment with the case definition for people with TB commonly used in TB clinical trials.
- **The old “Retreatments” are now called “Re-registered for treatment“** - this is a simplified and more accurate description for people who start a new TB treatment regimen (following either treatment failure or LTFU), or for whom the outcome of a previous treatment is undocumented/unknown. 
- this is a simplified description of people newly diagnosed with TB.
- **The old “New or relapse cases” are now called “New episode of TB"**

#### Cases by previous treatment history

![Cases by previous tx history](resources/images/TB_NOTIF_Q_001.png)

The case registration table has been set up as six individual data elements. This table could have been set up as two data elements with “Previous anti-TB treatment status” as a data element category. 
There are a few reasons why a “flat” structure with individual data elements was chosen:

- It has been important to have a structure for the TB configuration toolkit that allows comparisons with the previous reporting framework. Using a flat structure allows certain fields (data elements) in this section to be reused in the previous version of the case registration form.
- Analysis of this data is often done on specific combinations of these rows and columns, which have been defined as indicators. If necessary, this could be re-created using data element group sets.
- A “previous anti-TB treatment status” category would only apply to two data elements. While including a similar concept/classification of previous treatment, both the data set for DS and DR TB and the previous reporting framework are structured differently. As a result, the category could not be used. 
This point is particularly valid for this version 2.0.0 of the tool, where the disaggregation was modified to remove the cases previously disaggregated by “relapse” (now recurrent) and “Previously treated (excluding relapses)” (now re-registered for treatment) into three categories: **New** (never treated for TB or or has not received anti-TB treatment for longer than 30 days, excluding TB preventive treatment), **Recurrent**, and **Previous treatment history unknown**.

The extrapulmonary cases are no longer reported in disaggregated format by treatment history in this version. In order to reduce on the reporting burden, especially for contexts without digital case/based systems, the new guidance has attempted to limit multiple disaggregation of TB cases *e.g. By multiple anatomical sites and previous history) to pulmonary TB cases.
The indicator “TB - Extrapulmonary cases (CC or BC)” (UID: DLLSBjuacCn)  was set up to provide a continuity between the old DEs of extrapulmonary cases by treatment history, and the new single data element currently present in the section.

![ETB cases now without disaggregations](resources/images/TB_NOTIF_Q_008.png)

#### New and relapse cases by age group and sex

![Cases by age and sex](resources/images/TB_NOTIF_Q_002.png)

This section/table has been configured as a single data element (“New episode of TB by age and sex”), with an “age and sex” category combination in order to allow the maximum flexibility in the analysis tools. In version 2.0.0, child, adolescent, and young adult age groups are further disaggregated into the **5-9, 10-14, 15-19, 20-24 age groups** in line with the general surveillance guidance, and recognizing the differences in risk factors among the adolescent and young adult age groups.

A Validation Rule (**“TB - New episodes TB vs age/sex”**, UID: sms4F5mp9T1) was set up to ensure that the number of cases reported in the first section (Cases by previous treatment history) is the same as the one reported in this table. The rule does not prevent the user from saving or entering numbers that are different from the corresponding reported cases by treatment history, though any discrepancy should be explained and justified.

#### HIV testing and ARV coverage among all people with new episodes of TB

![HIV testing among new episodes](resources/images/TB_NOTIF_Q_003.png)

The table was configured with individual data elements. The new version no longer reports the DE "HIV-positive TB patients on cotrimoxazole preventive therapy CPT" as the DE “HIV-positive TB patients on antiretroviral therapy” fully captures the desired end point.

#### RIF Susceptibility testing among PBC TB cases

> **NOTE**
>
> Should this dataset be reported with an annual periodicity, this table could be added instead to the supplemental annual dataset described in the next chapter.

 ![RIF testing](resources/images/TB_NOTIF_Q_007.png)

The table was configured with individual data elements. This section is the only one about test confirmation. Version 1.5.0 presented a “Laboratory Diagnostic Activities” section that is no longer present. 

![Old laboratory section](resources/images/TB_NOTIF_Q_006.png)

The DEs and corresponding indicators are currently present in the “TB - Laboratory” dataset, but that could be added to this dataset if the laboratory dataset is not implemented.

| Metadata                                                                      | Type      | UID         |
|-------------------------------------------------------------------------------|-----------|------------|
| TB-LAB - Presumptive cases tested by any WRD                                  | DE        | N7LIAia2AWf |
| TB-LAB - Presumptive cases tested positive by any WRD                         | DE        | TiwOMtQvbLY |
| TB-LAB - Presumptive TB cases tested using a WHO-recommended rapid diagnostic | Indicator | As2eOxgQM9H |

These are recommended for reporting and use in countries with case-based digital surveillance systems as well as additional indicators that are options to be considered in countries with case-based digital surveillance systems as listed in Table 4.7 and Table 4.9 of Chapter 4 of the guidelines. These data items, including also the DEs present in the “Bedaquiline and linezolid susceptibility testing among people with PBC RR-TB” section of this dataset, could also be collected through periodic surveys based on a random sample of patient records, instead of routine continuous surveillance.

#### Enrolments on treatment among all registered people diagnosed with TB

![Treatment status among all the registered people](resources/images/TB_NOTIF_Q_004.png)

This new section in the quarterly notification reporting was designed to report the status of the treatment for all the RIF-susceptible TB cases registered in the system and enrolled for treatment. The table should be used to report people who died or who were lost to follow up before starting treatment if the exact regimen had yet to be determined, and should include people with other resistance patterns (e.g. isoniazid-resistant TB) with no documented resistance to rifampicin.
The table above reports all the options that could be possibly reported, though just “Registered”, “Died before treatment start”, “LTFU before treatment start”, and “Started on treatment in facility” are the compulsory ones. The options “No treatment - other reason”, “Transferred OUT - start in another facility”, and “Transferred IN - start after registration in another facility” are not compulsory, but could be used to facilitate the calculation of the active cohort size (the number of cases started on treatment in this facility). 

![Optional data points](resources/images/TB_NOTIF_Q_005.png)

> **NOTE**
>
> If these three options are not used, it should be noted then that “Started on treatment in this facility” should also include patients transferred in to start treatment and excludes those who died, were lost to follow-up or transferred out before the start of treatment.

![Manual vs calculated values](resources/images/TB_NOTIF_Q_010.png)

The added value of the three additional DEs becomes evident in the visual representations within the quarterly case notification dashboard. On the left graph, the final column illustrates the count of cases initiating treatment at the facility via manual counts. In contrast, the singular numeric visualization on the right showcases the calculated cases initiating treatment, computed by directly manipulating the pertinent DEs within the treatment enrollment cascade. 

### TB - Case Notification (Additional)

The 2.0.0 version of the Case notification (**previously named “RR/MDR case detection and treatment”**) dataset include extensive changes on the data collected and reported based on the new guidelines, newer treatment classes, and resistance testing that are currently being deployed in the field (e.g. Bedaquiline testing).

> **NOTE**
>
> The number of RR-TB cases can be considered from the annual aggregation of the quarterly notification dataset - “Rifampicin susceptibility testing among people with pulmonary bacteriologically confirmed TB” section. As the quarterly dataset can also be considered for annual reporting, it is up to the implementers to consider whether the RIF resistance information should fall under one dataset or the other.

#### Isoniazid susceptibility among PBC TB with RIF testing

![RIF susceptibility by previous history and Isoniazid susceptibility](resources/images/TB_RR_Y_002.png)

The table is composed of single data elements to combine the susceptibility to INH and RIF together with the previous history. 

#### Fluoroquinolones susceptibility among PBC RR-TB

![RIF resistant cases tested and resistant to FQ](resources/images/TB_RR_Y_003.png)

#### BDQ and LZD susceptibility testing among people with PBC RR-TB

![RIF resistant cases tested for Bedaquiline and linezolid susceptibility](resources/images/TB_RR_Y_004.png)

#### BDQ and/or LZD susceptibility testing among PBC RR-TB and FQ-resistant TB (pre-XDR-TB)

![RIF resistant cases tested for Bedaquiline and linezolid susceptibility](resources/images/TB_RR_Y_005.png)

The table is designed taking in consideration that, as of today, BDQ and LZD testing are not as widespread as other susceptibility tests. As the BDQ and LZD resistance monitoring becomes more available, the number of “No documented tests” should decrease, while the tested values of susceptibility and resistance for both drugs should increase. 

#### Enrolment on tx among all registered people diagnosed with TB

**This table replaces the older “Treatment” section** (with DEs “TB - Patients (laboratory-confirmed RR-TB or MDR-TB) who started treatment for MDR-TB” and “TB - Patients (non laboratory-confirmed RR-TB or MDR-TB) who started treatment for MDR-TB”) as the focus shifts from case confirmation to the regimen and the status of the treatment at the time of reporting. 

![RIF resistant cases by regimen length and treatment enrolment](resources/images/TB_RR_Y_006.png)

The table collects the data for the following RIF and FQ susceptibility combinations:
- Regimen designed to treat rifampicin-susceptible TB (people living with HIV) - this includes patients transferred in to start treatment and excludes those who died, were lost to follow-up or transferred out before the start of treatment. This can also be used to report people who died or who were lost to follow up before starting treatment if the exact regimen had yet to be determined.
- Short (≤12 months) regimen designed to treat rifampicin-resistant but not fluoroquinolone-resistant TB
- Short (≤12 months) regimen designed to treat rifampicin-resistant and fluoroquinolone-resistant TB (pre-XDR-TB/XDR-TB)
- Long (>12 months and ≤24 months) regimen designed to treat rifampicin-resistant but not fluoroquinolone-resistant TB
- Long (>12 months and ≤24 months) regimen designed to treat rifampicin-resistant and fluoroquinolone-resistant TB (pre-XDR-TB/XDR-TB)

For all the short and long term regimens, the data points include people with no drug susceptibility results who are considered eligible for drug-resistant TB regimens, such as people who are household contacts of people with drug-resistant TB. If a person died or was lost to follow up before starting treatment and the exact regimen had yet to be determined choose as default a duration of <12 months.

The table could also include as first data point “Regimen designed to treat rifampicin-susceptible TB (irrespective of HIV status)”, though it was not added in this table as it is a repetition of the variable present in the quarterly notification dataset. If the implementation requires instead a yearly report of the variable, it is suggested to remove it from the quarterly dataset and to add it to this table instead. 

**The table only includes the core statuses of treatment enrollment** contrary to what it was shown in the quarterly notification dataset, Enrolments on treatment among all registered people diagnosed with TB section. This was applied purposefully to show the two types of tables that could be obtained depending on whether the optional treatment statuses are not included (“Did not start treatment for other reasons”, “Transferred out to start treatment in another facility”, and “Transferred in to start treatment after registration in another facility”). As aforementioned, these three points are optional, but could be of support for a correct calculation of the active cohort.

#### Laboratory Information

The following DEs and indicator are currently present in the “TB - Laboratory” dataset, but that could be added to this dataset if the laboratory dataset is not implemented.

| Metadata                                                                      | Type      | UID         |
|-------------------------------------------------------------------------------|-----------|------------|
| TB-LAB - Presumptive cases tested by any WRD                                  | DE        | N7LIAia2AWf |
| TB-LAB - Presumptive cases tested positive by any WRD                         | DE        | TiwOMtQvbLY |
| TB-LAB - Presumptive TB cases tested using a WHO-recommended rapid diagnostic | Indicator | As2eOxgQM9H |

These are recommended for reporting and use in countries with case-based digital surveillance systems as well as additional indicators that are options to be considered in countries with case-based digital surveillance systems as listed in Table 4.7 and Table 4.9 of Chapter 4 of the guidelines. These data items, including also the DEs present in the “Bedaquiline and linezolid susceptibility testing among people with PBC RR-TB” section of this dataset, could also be collected through periodic surveys based on a random sample of patient records, instead of routine continuous surveillance.

### TB - Treatment outcomes 

#### Treatment outcomes for TB cases (excluding cases moved to second-line treatment)

The treatment outcomes table, specifically applicable to first-line treatments, is organized based on various types of TB cases (bacteriologically confirmed, clinical, re/treatments, HIV positive patients). 

> **NOTE**
>
> Note that the BC and CD classification of reported TB cases in this table is designed primarily for implementations using entirely digitalized individual data collection and analysis. For countries employing a hybrid approach (combining paper and digital methods or utilizing both tracker and aggregate data), merging these two data types into a single DE may be considered.

Within each patient category, the table details the number of registered cases (the treatment cohort) along with their respective treatment outcomes. In DHIS2, each patient category is represented as one data element for cases registered/cohort and one for treatment outcomes. The treatment outcome data elements are categorized under 'TB treatment outcome' with six distinct outcomes.

The rationale for selecting these categories encompasses several factors:
- **Meaningful Aggregation**: Categories are structured so that their collective sum remains meaningful, offering a default total in reporting tools. Though the overall total might not directly serve certain analyses, it represents a significant evaluated outcome—a crucial figure for reference. Conversely, categories like 'cases and deaths' often lack meaningful aggregation.
- **Reduction in Data Elements**: By organizing categories for current and past forms, first and second-line treatments, the treatment outcome category streamlines the dataset from 78 to 13 data elements. This reduction simplifies data management and analysis.
- **Enhanced Analytical Flexibility**: Categorization significantly enhances the flexibility in analyzing treatment outcome data. For instance, filtering two category options—'cured' and 'treatment completed' — allows for a straightforward display of the total number of 'treatment success' outcomes.

The use of categories not only ensures a comprehensible aggregation of outcomes but also streamlines data management and enables nuanced analyses for a more informed understanding of treatment effectiveness.

![Treatment outcomes - 1st line](resources/images/TB_OUTCOME_Q_001.png)

The new table for the treatment outcomes focuses on the outcomes by type of confirmation - bacteriological or clinical. Previously the “re-treatments (excluding recurrent)” cases were a separate DE - they have now been moved to the LEGACY DE group and should be kept in mind whenever calculating the total numbers of outcomes for continuity purposes. All the DEs in the table above should include new episodes and re-treatments.
Just as the older framework, **the HIV positive TB cases reported are not mutually exclusive with the BC abd CD cases reported in the same table**.

> **NOTE**
>
> Please note that the treatment outcomes section was designed including only the core variables. The new WHO guidelines set as optional three extra categories that can support the correct calculation of the cohort size, leading to a table that could potentially look like this:

![Treatment outcomes - 1st line with optional categories](resources/images/TB_OUTCOME_Q_002.png)

#### TB/HIV activities

![HIV-TB activities](resources/images/TB_OUTCOME_Q_003.png)

The TB/HIV section/table within the treatment outcomes dataset closely mirrors the corresponding section in the case registration form. Its inclusion is crucial due to the frequent lack of HIV status information for a significant number of cases at the time of compiling the quarterly notification report. The TB/HIV section in the treatment outcomes report facilitates the comprehensive collection of HIV status information. It features similar variables pertaining to HIV status, employing distinct data elements tagged with a “(by time of outcome)” postfix for differentiation. 

### TB - Treatment outcomes - second line

The new version of the outcome reporting form for second line patients is now split in two tables based on the length of the regimens. The 2nd line outcomes are no longer counted by confirmed MDR and XDR cases, rather by the type of regimen they are following - a **short regimen (≤ 12 months), or a long regimen (>12 months and ≤24 months) for RR cases**. This is to ensure consistency with adaptation of treatment regimens that will emerge in future for drug resistant TB, and for maintaining simplicity of reporting.

When working with the new metadata for ongoing analysis, implementers should remember how the older framework was used locally. They need to think about the best ways to analyze past data, considering total numbers, previous reporting methods, and the differences between short and long regimens against an older framework that did not report the same type of information.

Just as outlined in the quarterly outcome dataset, this dataset **also only includes the compulsory data points**. Implementers and TB programmes can evaluate whether it is necessary to add to the table the points to report the cases “Registered in this facility” (which is supposed match the numbers reported in “Enrolment on treatment among all registered people diagnosed with TB disease “ section from one or two calendar years ago depending on the regimen); cases “Transferred in to continue treatment after starting treatment at another facility”; and cases “Transferred out to continue treatment in another facility”.

This represents a significant departure from past protocols—now, **the responsibility for reporting treatment outcomes lies with the facility where an individual completes their treatment, not the facility of their initial registration**. Another notable shift is in the patient categories, encompassing **all new TB episodes and individuals re-registered for TB**, irrespective of their known treatment history. Unlike the 2013 WHO guidance, this inclusive approach poses a challenge for direct comparisons of success rates. 
**The adoption of the new definitions is anticipated to yield lower success rates, introducing a potential disparity when gauged against outcomes reported under the previous framework.**

#### Treatment outcomes for people started on short (≤ 12 months) regimens designed to treat rifampicin-resistant TB, one year before the current reporting year

![Outcome for TB patients on a short second line treatment](resources/images/TB_OUTCOME_Y_001.png)

Depending on the implementation of previous guidelines by countries—where MDR and XDR cases were counted—decisions regarding data analysis against the new reporting framework must be made by implementers and TB programs. Were all previously reported cases already under a shorter regimen? If not, might it be worthwhile to contemplate the creation of an indicator that combines old metadata with both short and long regimens for general analytical purposes? These considerations play a pivotal role in achieving harmonization across reporting frameworks.

#### Treatment outcomes for people started on long (> 12 months and ≤ 24 months) regimens designed to treat rifampicin-resistant TB, two years before the current reporting year

![Outcome for TB patients on a long second line treatment](resources/images/TB_OUTCOME_Y_002.png)

There is no longer need to disaggregate outcomes further by both drug resistance and HIV status, as this adds greater reporting burden to countries using paper based data capture systems. Countries that adopt digital case based surveillance systems are however encouraged to explore further meaningful disaggregations as guided by the updated WHO surveillance guidance (Chapter 4).

## Dashboards and Analytics

**Version 2.0.0 encompasses four dashboards meticulously designed to amalgamate totals, rates, and percentages, streamlining the analysis process - “TB - Notifications (Quarterly)”, “TB - Notifications (Annual)”, “TB - Outcomes (Quarterly)”, and “TB - Outcomes (Annual)”**.

Due to considerable shifts in notification reporting and outcomes, several previous DEs indicators are now categorized within designated “Legacy” indicator and data element groups. In the dashboard visualizations, when feasible, analytics will combine legacy metadata with new metadata, as seen in the “Notification rates”. Alternatively, in cases like “Successful outcomes”, priority will be given to the new metadata, while the legacy metadata will necessitate a thorough review by implementers. The latter approach was devised to prevent the blending of potentially incongruous entities within continuity indicators (e.g., outcomes by TB type and case vs outcomes by treatment regimen). This prudent observation becomes especially crucial considering that implementations frequently involve modifying the original design to align more closely with the specific requirements of local execution.

![This mapping](resources/dashboards_1.5.0_vs_2.0.0.xlsx) serves as a guide for implementers, aiding in the transition between older visualizations and their updated counterparts, contingent upon the local mapping conducted during the implementation of the previous DHIS2 TB aggregate version.

### TB Notifications - Quarterly and Annual

The TB notifications metadata is distributed across two datasets: **'TB - Notifications (Quarterly)'**  (whose visualizations are named starting with **TB_CNQ** (Case Notification Quarterly)) and **'TB - Notifications (Annual).'** (whose visualizations are named starting with **TB_CNA** (Case Notification Annual)). The structure of the dashboards follows the same of their respective datasets’ sections. 
As highlighted in the note within the quarterly reporting form for notifications, local implementations may utilize the quarterly dataset to report the same metadata annually, alongside other notifications data found in 'TB - Case notification (Additional).' Several primary visualizations featured in the quarterly dashboard are also included in the annual dashboard. However, implementers will need to tailor the dashboards and visualizations according to the specifics of the local implementation.
Whenever feasible,the dashboards aim to repurpose existing visualizations by updating their naming conventions. 

![Sex ratio with new naming convention, old “TBm_3.3_Ratio of male:female in new and relapse TB cases (annual)”](resources/images/TB_DASHBOARD_002.png)

Nonetheless, several prior visualizations, which are often based on legacy indicators, will necessitate thorough analysis to map and interpret the evolving data trends.
For instance, taking the number of notified cases, where new data elements (DEs) are combined with existing ones to ensure a seamless analytical continuity. While the indicator itself wasn't initially classified as 'legacy,' certain foundational components rely on an older framework. Therefore, those implementing it should verify if the indicator aligns effectively with their specific local implementation, as it may necessitate cross-referencing with the older framework.

![Notified TB cases indicators](resources/images/TB_DASHBOARD_001.png)

### TB Outcomes - Quarterly and Annual

The outcomes data can be analyzed in two indicators: **“TB3 - Outcomes (Quarterly)”** (whose visualizations are named starting with **TB_OUTQ** (Outcomes Quarterly)), and **“TB4 - Outcomes (Annual)”** (whose visualizations are named starting with **TB_OUTA** (Outcomes Annual)).

Version 2.0.0 introduces significant changes in outcome counting and reporting methodologies for both first and second lines of treatments, distinct from the Notifications datasets and dashboards. Consequently, these dashboards do not incorporate legacy indicators or reused visualizations, nor provide a thorough mapping of the older visualizations. 

![Outcomes for cases enrolled in shorter treatment regimenfor RR cases - continuation of the old  “TBc_5.5_Treatment outcomes for DR-TB (%)”?](resources/images/TB_DASHBOARD_003.png)

It is imperative for implementers and NTB Programs to collectively comprehend the previously utilized reporting metadata and adapt to the new reporting framework, structured around regimens and drug susceptibility. A key example to highlight the importance of this mapping is the potential to analyze outcomes of XDR cases (old framework) by incorporating the outcomes of RIF and FQ resistant cases (new framework) into the same indicators- local implementations will dictate the feasibility of this adjustment and whether this new indicator for continuity would analyze all cases or specific regimens.
This adaptation is crucial for establishing connections and sustaining analytical insights. It's important to note that achieving complete alignment between the old and new frameworks might pose challenges in certain contexts, influenced by the implementation choices made during the initial rollout of the older TB aggregate DHIS2 toolkit.

## References

Consolidated guidance on tuberculosis data generation and use. Module 1. Tuberculosis surveillance. Geneva: World Health Organization; 2023. Licence: CC BY-NC-SA 3.0 IGO (in press)
