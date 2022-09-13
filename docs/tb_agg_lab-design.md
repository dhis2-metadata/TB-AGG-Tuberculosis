# TB Laboratory - System Design Guide { #tb-agg-lab-design }

## Introduction

The **TB Laboratory (TB-LAB) aggregate digital data package** was developed in response to an expressed need from countries to rapidly adapt a solution for managing the data originating from the planned/undertaken TB programs activities. The TB Lab aggregate metadata package has therefore been designed as an installable solution for countries to update their DHIS2-based HMIS according to the updated version of the [“**WHO consolidated guidelines on tuberculosis: module 1: prevention: tuberculosis preventive treatment**”](https://www.who.int/publications/i/item/9789240001503) and on the [**WHO recording and reporting framework**](https://apps.who.int/iris/handle/10665/79199) from 2013..

This design document provides an overview of the design principles and global technical guidance used to develop a WHO standard metadata package for monitoring the preventive activities for household contacts of suspected and/or confirmed TB cases. This document is intended for use by DHIS2 implementers at country and local level to be able to support implementation and localisation of the package. The TB Lab metadata package can be adapted to local needs and national guidelines.

## Use Case

The TB aggregate package compiles the key information on TB cases from the point of notification to final case outcome, inclusive of laboratory results. The package captures a minimum set of data points required for epidemiological analysis. These risk factors, laboratory results for diagnosis and routine check-ups, drug resistance type classification, treatment regimens provided, and case outcome.

The aggregate TB Lab dataset has been designed to **fully match** the revised [TB Case Surveillance](#tb-cs-design), which has been conceptualized to reflect a **more generic workflow** able to integrate data entry both from the health facility side (e.g. clinicians and nurses), and the laboratory side - for more information on the structure and its rationale for the TB CS tracker program, please refer to the “System design overview” chapter of the tracker’s design guide. Workflows in countries may vary and the package should be adapted as needed to local context.

[Data flow within the tracker stages](resources/image/tblab_agg_001.png)

The design of the tracker, as shown in the flowchart above, has been improved to expand the information that could potentially be collected from the laboratory side. The expansion touched of course the metadata, which is now more comprehensive and allows to discern between diagnostic and monitoring tests, but it also created a more generic baseline upon which implementers and countries could build their own workflow. Similarly, the TB Lab aggregate dataset within the TB aggregate package has been created to expand the laboratory-related data that can be collected and analyzed **for diagnostic purposes among suspected cases, or for monitoring purposes among confirmed cases** (orange squares in the flowchart above).
The tracker’s key program indicators and indicators for laboratory data have been fully mapped for their aggregation. As a result, fully case-based implementations, fully aggregate implementations, or hybrid implementations of aggregate and case-based data, can benefit from the monitoring of the same key information for the monitoring of the TB programs.

## Intended Users

- **System admins/HMIS focal points**: those responsible for installing metadata packages, designing and maintaining HMIS and/or TB data systems
- **TB program focal points** responsible for overseeing data collection, management, analysis and reporting functions of the national TB programme
- **Laboratory technicians** responsible for reporting laboratory data and **Laboratory supervisors** responsible for monitoring the load and quality of the work carried out.
- **Implementation support specialists**, e.g. those providing technical assistance to the TB programme or the core HMIS unit to support & maintain DHIS2 as a national health information system and/or TB data system

## Dataset Configuration

The TB Lab dataset has a monthly periodicity. Countries can adopt it as it is or they can align it to their reporting requirements and change the periodicity of the dataset accordingly.
**The paramount detail to observe in this dataset is that, in order to match the tracker design, some of the data are disaggregated by suspected and confirmed cases. Should the implementation only collect the data of confirmed cases, the category combinations should be adjusted correspondingly.**

## Important definitions

Throughout the dataset implementers and user should not mix and devalue the meaning behind **CASES (patients)**, **SAMPLES (the matter gathered from the body to aid in the process of a medical diagnosis)**, and **TESTS (medical procedure performed to detect, diagnose, or monitor diseases, disease processes, susceptibility, or to determine a course of treatment)**.
While for the tracker there is a specific assumption behind the reported number of tests per case’s sample (_see the “Diagnostic Laboratory Results” chapter of the tracker’s design guide for more information_), **the aggregate data could potentially report multiple samples per case, multiple tests per sample, and multiple tests per sample by type depending on the reporting of the laboratory facility**. This is important to keep in mind as there is a potential risk of misunderstanding and fake errors when cases are calculated - if there are several tests registered per patient, the results will rightfully differ from one another. 

In order to avoid misunderstandings, here below there are the assumptions behind the key information that can be extrapolated from the dataset:

**Cases tested with microscopy**

- Possible options: **Positive/Negative**
	- Positive == If at least one sample is tested positive (even if negative results are available). _Diagnostics only - the monitoring of already confirmed cases will be looking to get negative results overtime, so this rule does not apply)._
	- Negative == If ALL samples are tested negative.
- For **positivity grade**: +++, ++, +, scanty
	- "+++" if at least one result has +++
	- "++" if at least one result is ++ and no +++
	- "+" if at least one result is + and no ++ or +++
	- "scanty" if at least one result is scanty and no +, ++ or +++

**Cases tested with culture (solid or liquid medium)**
- Possible options: **MTB, NTM**<br> _NB. Cases with contaminated culture or no growth are not counted._
	- MTB if at least one test is MTB
	- NTM if at least one test is NTM and no MTB <br>

**Cases tested with Genexpert or GeneXpert Ultra**
- Possible options: **MTB positive, No MTB detected**<br> _NB. We do not count cases with errors, no result or invalid results._
	- MTB if at least one test is MTB
	- MTB not detected if not detected and no MTB-positive tests.
- **For MTB-detected cases** - Options: Resistant, Indeterminate, susceptible:
	- Resistant: If at least one result shows resistance.
	- Susceptible: If at least one result shows susceptibility and no resistance.
	- Indeterminate: If at least one result is indeterminate and no susceptibility or resistance is detected.

## Data Elements

All the data elements belonging to the Tb Lab dataset are grouped in the **“TB - Laboratory” data element group**.
The full list of data elements is available in the [**reference file**](resources/TB_LAB_reference).

# Dataset Details

## Suspected Cases

[Suspected cases](resources/image/tblab_agg_002.png)

The section collectd the total number and the positive tests carried out for the purpose of **diagnosing presumptive cases**. Should the implementation collect only data for confirmed cases, the section and the DEs should be removed.

## TB Samples

[Samples](resources/image/tblab_agg_003.png)

The section collects the basic information of the samples received at the lab. Each DE is a **subgroup of the previous**: out of the collected samples, the lab should report how many were physically received, and finally, among those samples, how many were accepted for testing. The data is **disaggregated by the type of patient** from which the samples were taken: suspected cases, or confirmed cases, for whom the purpose is case monitoring rather than diagnosis. Should the implementation collect only data for confirmed cases, the category combination should be removed.

## Sputum Smear Microscopy 

The section collects the information related to smear microscopy tests. It collects the number of cases and samples tested by microscopy(by case type), and how many tests have been run **disaggregated by test result** (scanty, +, ++, +++, negative).

[Samples, cases and microscopy test volumes](resources/image/tblab_agg_063.png)

The table below allows to report the amount of tests **disaggregated by the turnaround days** (from 0 days - samples processed the same day as reception- up to 6+ days).

[Samples, cases and microscopy test volumes](resources/image/tblab_agg_064.png)

The total TAT days days should be **manually calculated on the side** based on the information collected somewhere else (be it on paper, excel, or on the locally existing laboratory management system) and entered in DHIS2 as an already calculated total and average.

This table could also be used to report the data coming from the tracker if a hybrid implementation is in place (tracker and aggregate implemented in parallel in different places). The Tracker's PIs can be mapped to report in the agrgegate system and automatically populate these DEs in the sites where individual data are collected.
>NOTE:
The reason why the average TAT has not been set as an automatic calculation from the table above is because the average will only represent the Average of the AVERAGE days overtime/across facilities and NOT the Average Turnaround time for all the samples in question. As it will work for indicators **within 1 facility and 1 month**, as a result it would give an aggregation over time or across orgUnits that will report inaccurate results. **The average should therefore be used to cross-chek the total number of tests and the total number of TAT days only at facility level and only with a monthly periodicity**.

[Manual data entry of already calculated total and average TAT](resources/image/tblab_agg_075.png)

## Culture media - Liquid and solid

The solid and liquid culture sections follow the same structure. This document will provide only the screenshots of the liquid medium culture as an illustrative example.
Similarly to the section on smear microscopy, the culture sections first collect the relevant information on the volume of cases, tests, and samples (by type of case) along with the test results (No growth, NTB, MTB, or negative).

[Samples, cases and culture test volumes](resources/image/tblab_agg_089.png)

Again, similarly to the description in the microscopy section, users should manually report the total and average TAT for LM and SM cultures. 

The table below allows to report the amount of tests disaggregated by the turnaround days (from 0 days - samples processed the same day as reception- up to 11+ days)

[TAT by days](resources/image/tblab_agg_061.png)

## GeneXpert and GeneXpert Ultra

The sections for the data entry of the info for the test run by **Xpert and Xpert Ultra are identical**. This document will only show the Xpert section as an illustrative example. 

[Samples, cases and Xpert test volumes](resources/image/tblab_agg_098.png)

The case monitoring disaggregation option has been grayed-out as the **Xpert tests are not commonly used among confirmed cases for monitoring purpose**s.

# Validation rules

All the validation rules belonging to the Tb Lab dataset are grouped in the **“TB - Laboratory” group**.
The full list of validation rules is available in the [**reference file**](resources/TB_LAB_reference)

# Analytics

## Indicators

All the indicators belonging to the Tb Lab dataset are grouped in the **“TB - Laboratory” indicators group - one for core and one for optional indicators**.
The full list of indicators is available in the [**reference file**](resources/TB_LAB_reference).

# Dashboards

The TB Lab dataset is downloadable with a predefined dashboard (**TB9. laboratory**) summarizing the key indicators for the monitoring of the laboratory activities (volumes test, cases, or samples, positivity rates, turnaround times, and results). 
The dashboard presents first the overall data for any test type and is then sectioned by test type (smear microscopy, GeneXpert, GeneXpert Ultra, and culture tests - solid and liquid media). The sections are labeled with a text box indicating the test type. Depending on the local context, type of implementation, and test availability, the dashboard can and should be adapted to better mirror the implementation’s needs.

>NOTE:
The average and total number of TAT for microscopy and culture tests can be added to the dashboards, though, as aforementioned, users should note that it should be visualized **AT FACILITY LEVEL** and on a **MONTHLY BASIS**. 

[The Tb lab dashboards and the sections](resources/image/tblab_agg_050.png)

# User groups

The package includes the following user groups:

| **UID**         | **Name**            | **Access rights**    |
|-------------|-----------------|----|
| pyu2ZlNKbzQ | **TB access**       | _Data Elements/Data element groups_	- can view data and metadata<br>_Indicators/Indicator groups_ - can view data and metadata<br>_Data sets_ - can view data and metadata<br>_Dashboards_ - can view data and metadata     |
| Ubzlyfqm1gO | **TB admin**        | _Data Elements/Data element groups_	- can view data, and edit and view metadata<br>_Indicators/Indicator groups_ - can view data, and edit and view metadata<br>_Data sets_ - can view data, and edit and view metadata<br>_Dashboards_ - can view data, and edit and view metadata |
| UKWx4jJcrKt | **TB data capture** | _Data Elements/Data element groups_	- can view data and metadata<br>_Indicators/Indicator groups_ - can view data and metadata<br>_Data sets_ - can capture and view data, and metadata<br>_Dashboards_ - can capture and view data, and metadata    |

# Special considerations

## Tracker to Aggregate Data

As mentioned in the first chapter of this document, **the aggregate laboratory data is fully mapped to the tracker’s indicators to collate the individual data in the aggregate dashboard**. 
More information on the utilization of the aggregate dataset for individual data is available in the [“**Use of Aggregate Data Model with Tracker Deployments**”](https://docs.dhis2.org/en/implement/tracker-implementation/tracker-performance-at-scale.html#use-of-aggregate-data-model-with-tracker-deployments) section of the [**Tracker performance at scale**](https://docs.dhis2.org/en/implement/tracker-implementation/tracker-performance-at-scale.html#tracker-performance-at-scale) documentation. 
Moreover, the [**Integrating tracker and aggregate data**](https://docs.dhis2.org/en/implement/maintenance-and-use/tracker-and-aggregate-data-integration.html#:~:text=Tracker%20data%20can%20be%20aggregated,month%20to%20produce%20monthly%20reports.) document details different approaches to the implementation of combined aggregate and individual data.

## Laboratory Data Mapping Within the TB Aggregate Package

While the TB Lab dataset is an expansion on the amount and quality of data that can be captured and analyzed in the laboratories, the TB aggregate package already previously contained a smaller amount of information about diagnostics. 

This [**mapping**](resources/TB_AGG_Laboratory_mapping.xls) will provide the necessary information to overview the diagnostic-related TB vs the TB-LAB metadata. The “Mapping” tab provides a list of TB DEs and the eventual correspondent or proxy information that can be extrapolated from the lab dataset. The list of DEs can also be triangulated with laboratory data to monitor discrepancies of reporting. 
Depending on the type of implementation, the laboratory data collected in other TB aggregate datasets should either be removed, or should be carefully integrated and mapped to the TBLAB indicators. The latter is particularly important if a TB CS tracker is used in parallel in the same implementation. 

# References

World Health Organization, (2013). Definitions and reporting framework for tuberculosis – 2013 revision (updated December 2014 and January 2020). Geneva. URL: https://apps.who.int/iris/bitstream/handle/10665/79199/9789241505345_eng.pdf?sequence=1&isAllowed=y [accessed 20 July 2022]

World Health Organization, (2020). WHO operational handbook on tuberculosis (Module 1 – Prevention):  Tuberculosis preventive treatment . Geneva, URL: https://apps.who.int/iris/bitstream/handle/10665/331525/9789240002906-eng.pdf [Accessed 22 July 2022]
