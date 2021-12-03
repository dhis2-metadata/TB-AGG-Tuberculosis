# TB Household Contacts - System Design Document { #tb-hh-contacts }

## Background and Purpose

The **TB Household Contacts (TB HH)**  digital data package was developed in response to an expressed need from countries to rapidly adapt a solution for managing the data originating from the planned/undertaken TB programs activities.The TB HH aggregate metadata package has therefore been designed as an installable solution for countries to update their DHIS2-based HMIS according to the updated version of the [“WHO consolidated guidelines on tuberculosis: module 1: prevention: tuberculosis preventive treatment”](https://www.who.int/publications/i/item/9789240001503).

This TB HH Aggregate System Design document provides an overview of the design principles and global technical guidance used to develop a WHO standard metadata package for monitoring the preventive activities for household contacts of TB index cases. This document is intended for use by DHIS2 implementers at country and local level to be able to support implementation and localisation of the package. The TB HH metadata package can be adapted to local needs and national guidelines.

This document is intended for use by DHIS2 implementers at country and regional level to be able to support implementation and localization of the configuration package while adhering to the design principles set forth.

## System Design Overview

### Use Case

The TB HH module follows the logic behind TB preventive activities among household contacts:

![Data flow](resources/images/tb_hh_000_en.png)

It is recommended that the TB HH module gets assigned to Organisation Units at the **lowest level** of the health system feasible for reporting data.

Digital data packages are optimized for Android data collection with the DHIS2 Capture App, free to download on the [GooglePlay store](https://play.google.com/store/apps/details?id=com.dhis2&hl=en).

### Program Structure

The structure of the TB HH module is very simple:

* **One quarterly aggregated dataset (TB Household Contacts)** for the identification and follow up of HH contacts who are eligible and have started TPT. The package captures a minimum number of data points that meet the current WHO reporting requirements. The data are collected quarterly to allow for the correct follow-up timeframe of the TPT course.

* **One predefined dashboard (TB7. HH Contacts)**.The analytics and dashboards included in the package are designed to support the routine analysis (with particular focus on HIV+ contacts and contacts under the age of 5 years), and use of TPT data at national and sub-national levels.

### Intended Users

The intended users of this package are:

* **Clinical and program management users** at local level for data entry and monitoring of the identification and adherence activities.

* **Program managers** at district and national levels for the monitoring and performance evaluation of the program.

## Dataset Configuration

The table below lists the data elements configured in the TB HH dataset.

> **Note**
>
> **ALL** the data elements are used to build indicators
> **ALL** data elements are linked to validation rules
> The **zero value** has been set as **significant**
> They are organized in the **TB-HH data element group**

| UID           | Name                                | Description                                                      |
|---------------|-------------------------------------|------------------------------------------------------------------|
| `fbAdmzvjLuY` | TB-HH - Notified ICs                | Number of index TB cases registered and notified                 |
| `UIHbSe2XmPa` | TB-HH - Contacts identified         | Number of contacts who have been identified                      |
| `CyOAgl64a0b` | TB-HH - Contacts screened           | Number of contacts who have been screened                        |
| `nyzwxN9M9t7` | TB-HH - Contacts eligible for TPT   | Number of contacts eligible to start TPT in the last year        |
| `vHJg9RWzMyo` | TB-HH - Contacts started on TPT     | Number of contacts who have been started on TPT in the last year |
| `RWNXWTrRGG9` | TB-HH - TPT outcomes - Stopped      | Number of contacts who stopped their TPT                         |
| `i0A7NK3fzXI` | TB-HH - TPT outcomes - Completed    | Number of contacts who completed their TPT                       |

## Datasets Details

### TB Index Cases

![Index cases](resources/images/tb_hh_001_en.png)

The section reports the number of index cases who have been notified in the quarter under analysis.

### Household Contacts of Index TB Cases

![HH contacts](resources/images/tb_hh_002_en.png)

The section reports the ICs’ HH contacts who have been identified and screened.

### TB Preventive Treatment

![TPT eligibility and initiation](resources/images/tb_hh_003_en.png)

The section provides the necessary information on TT eligibility and initiation.

### TB Preventive Treatment Outcomes

![TPT outcomes](resources/images/tb_hh_004_en.png)

The section reports the information on contacts’ adherence to the TPT.

## Validation Rules

**None of the VRs are set to block the data entry**. Should any of the numbers not match the logic behind the rules, the description of the broken rule will simply highlight the gap and will nonetheless let the system save the data.
The following validation rules have been created in order to ensure the logic and the quality of the collected data:

> **Note**
>
> The “TB-HH - Notified ICs vs Identified contacts” rule: In the VERY rare case that it is a low burden rural health facility, should we make an exception if there is 1 index case, with 0 household contacts. In this case the implementer should consider whether the rule would not be logical for the local context.

| Name        | Left side      | Operator      | Right side            |
|-------------|-------------|--------|--------------------|
| TB-HH - Notified ICs vs Identified contacts               | TB-HH - Notified ICs                  | Less than or equal to    | TB-HH - Identified contacts                   |
| TB-HH - Notified ICs vs screened contacts                 | TB-HH - Notified ICs                  | Less than or equal to    | TB-HH - Screened contacts                     |
| TB-HH - Notified ICs vs TPT eligible contacts             | TB-HH - Notified ICs                  | Less than or equal to    | TB-HH - Eligible TPT contacts                 |
| TB-HH - Notified ICs vs TPT started contacts              | TB-HH - Notified ICs                  | Less than or equal to    | TB-HH - TPT started contacts                  |
| TB-HH - Contacts Screened vs Started                      | TB-HH - Contacts screened             | Equal to or greater than | TB-HH - Contacts TPT Started                  |
| TB-HH - Contacts identified vs Screened                   | TB-HH - Contacts identified           | Equal to or greater than | TB-HH - Contacts screened                     |
| TB-HH - Contacts identified vs Eligible                   | TB-HH - Contacts identified           | Equal to or greater than | TB-HH - Contacts eligible                     |
| TB-HH - Contacts identified vs Started TPT                | TB-HH - Contacts identified           | Equal to or greater than | TB-HH - Contacts Started TPT                  |
| TB-HH - Contacts Screened vs Eligible                     | TB-HH - Contacts screened             | Equal to or greater than | TB-HH - Contacts TPT Eligible                 |
| TB-HH - Contacts started on TPT vs Completed              | TB-HH - Contacts started on TPT       | Equal to or greater than | TB-HH - TPT outcomes - Completed              |
| TB-HH - Contacts started on TPT vs Stopped                | TB-HH - Contacts started on TPT       | Equal to or greater than | TB-HH - TPT outcomes - Stopped                |
| TB-HH - Contacts started on TPT vs Stopped+complete       | TB-HH - Contacts started on TPT       | Equal to                 | TB-HH - TPT outcomes - Stopped+complete       |
| TB-HH - Contacts TPT eligible vs started                  | TB-HH - TPT Contacts eligible         | Equal to or greater than | TB-HH - TPT contacts started                  |
| TB-HH - HIV+ Contacts Screened vs Started                 | TB-HH - HIV+ Contacts screened        | Equal to or greater than | TB-HH - HIV+ Contacts Started TPT             |
| TB-HH - HIV+ Contacts identified vs Screened              | TB-HH - HIV+ Contacts identified      | Equal to or greater than | TB-HH - HIV+ Contacts screened                |
| TB-HH - HIV+ Contacts identified vs Eligible              | TB-HH - HIV+ Contacts identified      | Equal to or greater than | TB-HH - HIV+ Contacts eligible                |
| TB-HH - HIV+ Contacts identified vs Started TPT           | TB-HH - HIV+ Contacts identified      | Equal to or greater than | TB-HH - HIV+ Contacts started TPT             |
| TB-HH - HIV+ Contacts Screened vs Eligible                | TB-HH - HIV+ Contacts screened        | Equal to or greater than | TB-HH - HIV+ Contacts TPT Eligible            |
| TB-HH - HIV + Contacts started on TPT vs Completed        | TB-HH - HIV + Contacts started on TPT | Equal to or greater than | TB-HH - HIV+ TPT outcomes - Completed         |
| TB-HH - HIV+ Contacts started on TPT vs Stopped           | TB-HH - HIV+ Contacts started on TPT  | Equal to or greater than | TB-HH - HIV+ TPT outcomes - Stopped           |
| TB-HH - HIV+ Contacts started on TPT vs Stopped+Completed | TB-HH - HIV+ Contacts started on TPT  | Equal to                 | TB-HH - HIV+ TPT outcomes - Stopped+Completed |
| TB-HH - HIV+ Contacts TPT eligible vs started             | TB-HH - HIV+ TPT Contacts eligible    | Equal to or greater than | TB-HH - HIV+ TPT contacts started             |
| TB-HH - U5 Contacts identified vs Screened                | TB-HH - U5 Contacts identified        | Equal to or greater than | TB-HH - U5 Contacts screened                  |
| TB-HH - U5 Contacts identified vs Eligible                | TB-HH - U5 Contacts identified        | Equal to or greater than | TB-HH - U5 Contacts eligible                  |
| TB-HH - U5 Contacts identified vs Started TPT             | TB-HH - U5 Contacts identified        | Equal to or greater than | TB-HH - U5 Contacts TPT start                 |
| TB-HH - U5 Contacts Screened vs Started                   | TB-HH - U5 Contacts screened          | Equal to or greater than | TB-HH - U5 Contacts TPT Started               |
| TB-HH - U5 Contacts Screened vs Eligible                  | TB-HH - U5 Contacts screened          | Equal to or greater than | TB-HH - U5 Contacts TPT Eligible              |
| TB-HH - U5 Contacts started on TPT vs Stopped             | TB-HH - U5 Contacts started on TPT    | Equal to or greater than | TB-HH - U5 TPT outcomes - Stopped             |
| TB-HH - U5 Contacts started on TPT vs Stopped+Completed   | TB-HH - U5 Contacts started on TPT    | Equal to                 | TB-HH - U5 TPT outcomes - Stopped+Completed   |
| TB-HH - U5 Contacts started on TPT vs U5 Completed        | TB-HH - Contacts U5 started on TPT    | Equal to or greater than | TB-HH - U5 TPT outcomes - Completed           |
| TB-HH - U5 Contacts TPT eligible vs started               | TB-HH - U5 TPT Contacts eligible      | Equal to or greater than | TB-HH - U5 TPT contacts started               |

## User Groups

The package includes the following user groups:

| UID | Name |
|-----------|---------------------|
| `pyu2ZlNKbzQ` | TB access       |
| `Ubzlyfqm1gO` | TB admin        |
| `UKWx4jJcrKt` | TB data capture |

Access rights for members within these user groups are described in the table below:

|Object                               | User Group |||
|-------------------------------------|----------------------------------------|-------------------------------------------------|--------------------------------------------------------|
|                                     | _TB access_                            | _TB admin_                                      | _TB data capture_                                      |
| _Data Elements/Data element groups_ | Metadata: can view <br> Data: can view | Metadata: can edit and view <br> Data: can view | Metadata: can view <br> Data: can view                 |
| _Indicators/Indicator groups_       | Metadata: can view <br> Data: can view | Metadata: can edit and view <br> Data: can view | Metadata: can view <br> Data: can view                 |
| _Data sets_                         | Metadata: can view <br> Data: can view | Metadata: can edit and view <br> Data: can view | Metadata: can view <br> Data: can capture and can view |
| _Dashboards_                        | Metadata: can view <br> Data: can view | Metadata: can edit and view <br> Data: can view | Metadata: can view <br> Data: can view                 |

## Analytics and Indicators

The following indicators have been predefined for the monitoring of the activities. The indicators are organized in the TB - HH Indicators group. The numerators and denominators are using the abbreviations commonly used in TB programs. Nonetheless, in [Annex 1](#tb-nomenclature-translation), there is a table with the “translation” of the nomenclature in order to avoid any confusion.

| UID           | Name         | Description     | Numerator     | Denominator          |
|---------------|---------|-----------|-------------|--------------|
| `d4w3j5nmQbb` | TB-HH - Contact screening coverage among PLHIV (%)                         | Proportion of contacts (PLHIV) screened out of those (PLHIV) identified                                                  | newinc_con_plhiv_screen       | newinc_con_plhiv_ident      |
| `NQjV7Lw96D6` | TB-HH - Contact screening coverage among children aged under 5 (%)         | Proportion of contacts (children aged under 5) screened out of those (children aged under 5) identified                  | newinc_con_04_screen          | newinc_con_04_ident         |
| `d4w3j5nmQbb` | TB-HH - Contact screening coverage (%)                                     | Proportion of contacts screened out of those identified                                                                  | newinc_con_screen             | newinc_con_ident            |
| `ETPT25L7LLY` | TB-HH - TB preventive treatment completion among PLHIV (%)                 | Proportion of contacts (PLHIV) who completed TPT out of those (PLHIV) who started TPT                                    | newinc_con_plhiv_prevtx_cmplt | newinc_con_plhiv_prevtx_coh |
| `zDCLWcEok43` | TB-HH - TB preventive treatment completion among children aged under 5 (%) | Proportion of contacts (children under 5) who completed TPT out of those (children under 5) who started TPT              | newinc_con_04_prevtx_cmplt    | newinc_con_04_prevtx_coh    |
| `oemqpmb3dsz` | TB-HH - TB preventive treatment initiation among PLHIV (%)                 | Proportion of contacts (PLHIV) who started TPT out of those (PLHIV)who were eligible TPT                                 | newinc_con_plhiv_prevtx       | newinc_con_plhiv_elig       |
| `f8lltD3tQUu` | TB-HH - TB preventive treatment initiation among children aged under 5 (%) | Proportion of contacts (children aged under 5) who started TPT out of those (children aged under 5)who were eligible TPT | newinc_con_04_prevtx          | newinc_con_04_elig          |
| `G8gM2ieiAUL` | TB-HH - TB preventive treatment initiation (%)                             | Proportion of contacts who started TPT out of those who were eligible TPT                                                | newinc_con_prevtx             | newinc_con_elig             |
| `bRhf49ZNckC` | TB-HH - TB preventive treatment outcomes - completed (%)                   | Proportion of contacts who completed TPT out of those who started TPT                                                    | newinc_con_prevtx_cmplt       | newinc_con_prevtx_coh       |
| `cu3Jmt4ssWN` | TB-HH - TB preventive treatment outcomes - stopped (%)                     | Proportion of contacts who stopped TPT out of those who started TPT                                                      | newinc_con_prevtx_stop        | newinc_con_prevtx_coh       |
| `h8PkcXUYxJV` | TB-HH - TB index cases identified                                          | Number of TB index cases identified                                                                                      | newinc_con                    | 1                           |
| `gFvh80a5vdF` | TB-HH - Contacts identified                                                | Number of contacts identified                                                                                            | newinc_con_ident              | 1                           |
| `hGuPYnbkgi5` | TB-HH - Contacts screened                                                  | Number of contacts screened                                                                                              | newinc_con_screen             | 1                           |
| `NQjV7Lw96D6` | TB-HH - Contacts under the age of 5 screened                               | Number of contacts under the age of 5 screened                                                                           | newinc_con_04_screen          | 1                           |
| `ZckswElv9Yb` | TB-HH - Contacts with known HIV + status screened                          | Number of contacts with a known HIV + status screened                                                                    | newinc_con_plhiv_screen       | 1                           |
| `zckiMYozzGG` | TB-HH - Contacts eligible for TPT                                          | Number of contacts eligible for TPT                                                                                      | newinc_con_elig               | 1                           |
| `gVGb56B4e0o` | TB-HH - Contacts eligible for TPT under the age of 5                       | Number of contacts eligible for TPT under the age of 5                                                                   | newinc_con_04_elig            | 1                           |
| `Zy3hE0pkLSi` | TB-HH - PLHIV Contacts eligible for TPT                                    | Number of HIV+ contacts eligible for TPT                                                                                 | newinc_con_plhiv_elig         | 1                           |
| `FIktfdU11d2` | TB-HH - Contacts started TPT                                               | Number of contacts who started TPT                                                                                       | newinc_con_prevtx             | 1                           |
| `VG2F54Lx35r` | TB-HH - Contacts stopped TPT                                               | Number of contacts who stopped TPT                                                                                       | newinc_con_prevtx_stop        | 1                           |
| `DPgSFO4P0G1` | TB-HH - Contacts completed TPT                                             | Number of contacts who completed TPT                                                                                     | newinc_con_prevtx_cmplt       | 1                           |

## Dashboards

The TB HH module includes a predefined dashboard called **TB7. TB HH Contacts.**

The dashboard is numbered as number 7 to fit in the bigger picture of the aggregate TB activities. Dashboards numbers 1 to 6 are linked to the datasets described in the [TB Aggregate System Design Document](#tb-aggregate-design).

![TB HH dashboard](resources/images/tb_hh_005_en.png)

The predefined dashboard contains the following items:

| Name      | Title        | Indicators Used         | Type               |
|-----------|----------------------------------------------------------------------------|--------|---------|
| TB-HH-001 | TBc_7.1_TPT cascade                                                        | TB-HH - TB index cases identified <br> TB-HH - Contacts identified <br> TB-HH - Contacts screened <br> TB-HH - Contacts eligible for TPT <br> TB-HH - Contacts started TPT                                            | Columns            |
| TB-HH-002 | TBc_7.2_Contact screening coverage                                         | TB-HH - Contact screening coverage (%) <br> TB-HH - Contact screening coverage among PLHIV (%) <br> TB-HH - Contact screening coverage among children under age of 5 (%)                                                  | Lines              |
| TB-HH-003 | TBc_7.3_TPT initiation among eligible contacts                             | TB-HH - TB preventive treatment initiation among PLHIV (%) <br> TB-HH - TB preventive treatment initiation among children under the age of 5 (%) <br> TB-HH - TB preventive treatment initiation (%)                      | Lines              |
| TB-HH-004 | TBc_7.4_TPT completion                                                     | TB-HH - TB preventive treatment outcomes - completed (%) <br> TB-HH - TB preventive treatment completion among children aged under 5 (%) <br> TB-HH - TB preventive treatment completion among PLHIV (%)                  | Lines              |
| TB-HH-005 | TBc_7.1a_TPT overview                                                      | TB-HH - TB index cases identified <br> TB-HH - Contacts identified <br> TB-HH - Contacts screened <br> TB-HH - Contacts eligible for TPT <br> TB-HH - Contacts started TPT <br> TB-HH - Contacts stopped TPT <br> TB-HH - Contact completed TPT | Table              |
| TB-HH-006 | TBc_7.4_TPT stopped                                                        | TB-HH - TB preventive treatment outcomes - stopped (%)                                                                                                                                                                        | Lines              |
| TB-HH-007 | TBc_7.3a_TPT eligibility vs initiation                                     | TB-HH - Contacts eligible for TPT <br> TB-HH - TB preventive treatment initiation (%)                                                                                                                                       | Lines, double axes |
| TB-HH-008 | TBc_7.3b_TPT eligibility vs initiation among children under 5 years of age | TB-HH - Contacts eligible for TPT under the age of 5 <br> TB-HH - TB preventive treatment initiation among children under the age of 5 (%)                                                                                  | Lines, double axes |
| TB-HH-009 | TBc_7.3c_TPT eligibility vs initiation among PLHIV                         | TB-HH - Contacts eligible for TPT among PLHIV <br> TB-HH - TB preventive treatment initiation among PLHIV (%)                                                                                                               | Lines, double axes |

## Data Triangulation

In order to ensure consistency across programs and data, the TB-HH - TB index cases identified indicator can be checked against the TB - Notified TB cases (all cases, all forms) indicator belonging to the TB case notification dataset based on the [“Definitions and reporting framework for tuberculosis – 2013 revision”](http://apps.who.int/iris/bitstream/handle/10665/79199/9789241505345_eng.pdf;jsessionid=0911B3678B5EF8EBD24C834EA882F199?sequence=1) document. The dataset is part of the [TB Aggregate System Design](#tb-aggregate-design).

This of course can only be done if other datasets belonging to the TB general package are downloaded and used in the same instance.

## Android Compatibility

Digital data packages are optimized for Android data collection with the DHIS2 Capture App, free to download on the [GooglePlay store](https://play.google.com/store/apps/details?id=com.dhis2&hl=en).  Key considerations in design was to ensure that all forms are configured with sections that structure form layout for better display on the android devices.  Also, with the requirement of minimal data disaggregation in this package, all the dataSet layout should provide a concise view for Android display.

## References

World Health Organization (20 March 2020). WHO consolidated guidelines on tuberculosis: module 1: prevention: tuberculosis preventive treatment. URL: <https://www.who.int/publications/i/item/9789240001503>

World Health Organization (January 2020). “Definitions and reporting framework for tuberculosis
– 2013 revision (updated December 2014 and January 2020)”. URL: <http://apps.who.int/iris/bitstream/handle/10665/79199/9789241505345_eng.pdf;jsessionid=0911B3678B5EF8EBD24C834EA882F199?sequence=1>

## Annex 1. TB Nomenclature { #tb-nomenclature-translation }

|                        | ICs        | Contacts Identified    | Contacts Screened       | Contacts Eligible     | Contacts Started        |
|------------------------|------------|------------------------|-------------------------|-----------------------|-------------------------|
| Total Number           | newinc_con | newinc_con_ident       | newinc_con_screen       | newinc_con_elig       | newinc_con_prevtx       |
| Number of Children <5y |      x     | newinc_con_04_ident    | newinc_con_04_screen    | newinc_con_04_elig    | newinc_con_04_prevtx    |
| Number of PLHIV        |      x     | newinc_con_plhiv_ident | newinc_con_plhiv_screen | newinc_con_plhiv_elig | newinc_con_plhiv_prevtx |
