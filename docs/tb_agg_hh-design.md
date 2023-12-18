# TB Household Contacts - System Design Document { #tb-agg-hh-design }

## Background and Purpose

The TB Household Contacts (TB HH) digital data package was developed in response to an expressed need from countries to rapidly adapt a solution for managing the data originating from the planned/undertaken TB programs activities.The TB HH aggregate metadata package has therefore been designed as an installable solution for countries to update their DHIS2-based HMIS according to the updated version of the Consolidated guidance on tuberculosis data generation and use (the link will be provided upon publication on the WHO website).

This TB HH Aggregate System Design document provides an overview of the design principles and global technical guidance used to develop a WHO standard metadata package for monitoring the preventive activities for household contacts of TB index cases. This document is intended for use by DHIS2 implementers at country and local level to be able to support implementation and localisation of the package. The TB HH metadata package can be adapted to local needs and national guidelines.

This document is intended for use by DHIS2 implementers at country and regional level to be able to support implementation and localization of the configuration package while adhering to the design principles set forth.

## Intended Users

The intended users of this package are:

Clinical and program management users at local level for data entry and monitoring of the identification and adherence activities.

Program managers at district and national levels for the monitoring and performance evaluation of the program.

## System Design Overview

### Use Case

The TB HH module follows the logic behind TB preventive activities among household contacts:

![Cascade for the HH contacts of identified positive TB cases](resources/images/tb_hh_000_en.png)

### Data flow

It is recommended that the TB HH module gets assigned to Organisation Units at the lowest level of the health system feasible for reporting data.

Digital data packages are optimized for Android data collection with the DHIS2 Capture App, free to download on the GooglePlay store.

### Program Structure

The data are collected quarterly to allow for the correct follow-up timeframe of the TPT course.
The structure of the TB HH modules is very simple:

One quarterly aggregated dataset (TB-HH - Household Contacts) for the identification, screening, and TPT eligibility. The metadata captures a minimum number of data points that meet the current WHO reporting requirements.

One quarterly aggregated dataset (TB-HH - TPT outcomes) for the outcomes out of the HH contacts who started the TPT treatment.

One predefined dashboard (TB5. HH Contacts).The analytics and dashboards included in the package are designed to support the routine analysis and use of TPT data at national and sub-national levels.

### Dataset Configuration - Household Contacts

> **NOTE**
>
> ALL the data elements are used to build indicators
> ALL data elements are linked to validation rules
> The zero value has been set as significant

#### TB Index Cases

![Cascade for the HH contacts of identified positive TB cases](resources/images/tb_hh_001_en.png)

The section reports the number of index cases who have been notified in the quarter under analysis.

#### Household Contacts of Index TB Cases

![HH contacts identified and screened](resources/images/tb_hh_010_en.png)

The section reports the ICs’ HH contacts who have been identified and screened. Please note that the updated version only disaggregates the data by age groups.

#### TB Preventive Treatment

![TPT eligibility and initiation](resources/images/tb_hh_011_en.png)

The section provides the necessary information on TT eligibility and initiation.

### Dataset Configuration - TPT Outcomes

In this updated version, outcomes were segregated from screenings and eligibility reporting to accurately capture cohorts of HH contacts initiating TPT treatment within the same analysis time frame. Following WHO guidance, outcomes can be reported by 6 or 12-month periods. The dataset now aligns with reporting outcomes within 12 months after the identification of HH contacts (three quarters earlier).

#### Cohort of household contacts identified and started on TPT

The section reports the cohort of the HH contacts for which the dataset is reporting the outcomes. In this case, users should be reporting the cases who started the TPT treatment out of those identified 3 quarters ago.

![Cohort of HH contacts on TPT](resources/images/tb_hh_012_en.png)

#### TPT outcomes for Household Contacts

The section clearly outlines the criteria for the inclusion in the period of reporting.

- **Identified 3 quarters ago**: Refers to household contacts that were identified or registered in the system three quarters (or nine months) prior to the current time frame or reporting period.
- **Started on TPT**: This subset comprises those identified household contacts who began TPT within the aforementioned three-quarter period.
- **Received an outcome within 12 months after the start of the period when they were identified**: This specifies that among the identified household contacts who initiated TPT, the dataset focuses on those who have had an outcome recorded within 12 months after their initial identification. This outcome could be related to the completion of treatment, or the discontinuation of the treatment for any reason within this specified time frame.

![Outcomes](resources/images/tb_hh_013_en.png)

### Validation Rules

None of the VRs are set to block the data entry. Should any of the numbers not match the logic behind the rules, the description of the broken rule will simply highlight the gap and will nonetheless let the system save the data. The following validation rules have been created in order to ensure the logic and the quality of the collected data:

> **NOTE**
>
> The “TB-HH - Notified ICs vs Identified contacts” rule: In the VERY rare case that it is a low burden rural health facility, should we make an exception if there is 1 index case, with 0 household contacts. In this case the implementer should consider whether the rule would not be logical for the local context.

The package includes the following user groups:

| UID | Name |
|-|-|
| pyu2ZlNKbzQ | TB access |
| Ubzlyfqm1gO | TB admin |
| UKWx4jJcrKt | TB data capture |

## Analytics and Indicators

The indicators are organized in the TB - HH Indicators group. The numerators and denominators are using the abbreviations commonly used in TB programs.

TB Nomenclature¶

|                        | ICs        | Contacts Identified    | Contacts Screened       | Contacts Eligible     | Contacts Started        |
|------------------------|------------|------------------------|-------------------------|-----------------------|-------------------------|
| Total Number           | newinc_con | newinc_con_ident       | newinc_con_screen       | newinc_con_elig       | newinc_con_prevtx       |
| Number of Children <5y |      x     | newinc_con_04_ident    | newinc_con_04_screen    | newinc_con_04_elig    | newinc_con_04_prevtx    |
| Number of PLHIV        |      x     | newinc_con_plhiv_ident | newinc_con_plhiv_screen | newinc_con_plhiv_elig | newinc_con_plhiv_prevtx |

### Data Triangulation

In order to ensure consistency across programs and data, the TB-HH - TB index cases identified indicator can be checked against the TB - Notified TB cases (all cases, all forms) indicator belonging to the TB case notification dataset based on the “Definitions and reporting framework for tuberculosis – 2013 revision” document. The dataset is part of the TB Aggregate System Design.

This of course can only be done if other datasets belonging to the TB general package are downloaded and used in the same instance.

## Android Compatibility

Digital data packages are optimized for Android data collection with the DHIS2 Capture App, free to download on the GooglePlay store. Key considerations in design was to ensure that all forms are configured with sections that structure form layout for better display on the android devices. Also, with the requirement of minimal data disaggregation in this package, all the dataSet layout should provide a concise view for Android display.

## References

World Health Organization (20 March 2020). WHO consolidated guidelines on tuberculosis: module 1: prevention: tuberculosis preventive treatment. URL: <https://www.who.int/publications/i/item/9789240001503>

World Health Organization (January 2020). “Definitions and reporting framework for tuberculosis – 2013 revision (updated December 2014 and January 2020)”. URL: <http://apps.who.int/iris/bitstream/handle/10665/79199/9789241505345_eng.pdf;jsessionid=0911B3678B5EF8EBD24C834EA882F199?sequence=1>

World Health Organization (2023). “Consolidated guidance on tuberculosis data generation and use. Module 1. Tuberculosis Surveillance”. Geneva: World Health Organization; 2023. Licence: CC BY-NC-SA 3.0 IGO (in press)
