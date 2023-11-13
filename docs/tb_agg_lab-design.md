# TB Laboratory - System Design Guide { #tb-agg-lab-design }

## Introduction

The **TB Laboratory (TB-Lab) design** in DHIS2 was developed in response to an expressed need from countries to rapidly adapt a solution for managing lab sample collection, testing and lab result data for suspected and presumptive TB cases. The datasets allow for reporting from health facilities where lab samples are collected and may also be processed, as well as from laboratories to generate key programme performance indicators and improved visibility on TB diagnoses. This section documents the design to support a core set of indicators and dashboard analyses through the aggregate data model for countries to incorporate lab-based indicators into the HMIS.  updated version of the [“**WHO consolidated guidelines on tuberculosis: module 1: prevention: tuberculosis preventive treatment**”](https://www.who.int/publications/i/item/9789240001503) and on the [**WHO recording and reporting framework**](https://apps.who.int/iris/handle/10665/79199) from 2013. 

## Use Case

This component captures a minimum set of data points required for epidemiological analysis based on laboratory doagnoses among suspected TB cases. These risk factors, laboratory results for diagnosis and routine check-ups, drug resistance type classification, treatment regimens provided, and case outcome. Source data may come from electronic case surveillance tools such as the DHIS2 TB Case Surveillance tracker or may be aggregated and reported into DHIS2 from paper or other sources, depending on country context. Throughout the document, we provide basic guidance on how the aggregate data elements for core indicators can be populated using source data from the DHIS2 TB case surveillance tracker.  

The aggregate TB Lab dataset has been designed to **fully align** with the [TB Case Surveillance](#tb-cs-design) Tracker program, which has been conceptualized to reflect a **generic workflow** for integrating data captuyred in health facilities (e.g. clinicians and nurses), as well asfrom laboratories. For more information on the structure and its rationale for the TB CS tracker program, please refer to the “System design overview” chapter of the [tracker’s design guide](https://docs.dhis2.org/en/topics/metadata/tuberculosis/tb-case-surveillance-tracker/version-200/design.html). Workflows in countries may vary and the DHIS2 design should be adapted as needed to local context.

![Data flow within the tracker stages](resources/images/tb_agg_lab_001.png)

The design of the tracker, as shown in the flowchart above, has been improved to expand the information that could potentially be collected in the laboratory. The expansion touched of course the metadata, which is now more comprehensive and allows to discern between the tests carried out to diagnose suspected cases and those to monitor already confirmed cases. In addition, it also created a more generic baseline upon which implementers and countries could build their own workflow. Similarly, the TB Lab aggregate dataset within the TB aggregate package has been created to expand the laboratory-related data that can be collected and analyzed **for diagnostic purposes among suspected cases, or for monitoring purposes among confirmed cases** (orange squares in the flowchart above).

Aggregation from the TB Case Surveillance tracker is achieved through *program indicators*, which are mapped to the corresponding aggregate data elements described in this design document and included in the metadata reference file. This design allows for fully-scaled electronic case-based implementations (least common), paper-based implementations (common), or hybrid electronic/paper-based implementations (most common) to generate the same set of core monitoring indicators at national scale and benefit from the analysis recommendations included in the dashboards. For more information on the aggregation of individual TB data, please refer to the ["Reporting case-based data into aggregate data TB reports"](https://docs.dhis2.org/en/topics/metadata/tuberculosis/tb-case-surveillance-tracker/version-200/design.html#reporting-case-based-data-into-aggregate-tb-reports) section in the TB case surveillance design guide. For a more generic guide on how to integrate tracker and aggregate data, please refer to the page linked [here](https://docs.dhis2.org/en/implement/maintenance-and-use/tracker-and-aggregate-data-integration.html).

## Intended Users

- **System admins & core HIS teams**: those responsible for designing and maintaining the national HMIS and/or TB data systems
- **TB program focal points** responsible for overseeing data collection, management, analysis and reporting functions of the national TB programme
- **Laboratory technicians** responsible for reporting laboratory data and **Laboratory supervisors** responsible for monitoring the load and quality of the work carried out.
- **Implementation support specialists**, e.g. those providing technical assistance to the TB programme or the core HMIS unit to support & maintain DHIS2 as a national health information system and/or TB data system

## Dataset Configuration

The TB Lab dataset has a monthly periodicity. Countries can adopt it as it is or they can align it to their reporting requirements and change the periodicity of the dataset accordingly.
**Note that in order to remain flexible for variations in country adoption of electronic case-based surveillance, some data elements are disaggregated by suspected and confirmed cases.** If a country implementation only collects the data of confirmed cases, the category combinations should be adjusted correspondingly to simplify the dataset. 

### Important Definitions

Throughout the dataset, implementers and user should not mix and devalue the meaning behind **Cases (patients)**, **Samples (the matter gathered from the body to aid in the process of a medical diagnosis)**, and **Tests (medical procedure performed to detect, diagnose, or monitor diseases, disease processes, susceptibility, or to determine a course of treatment)**.
The corresponding TB case surveillance tracker suggests a set workflow for reporting on a number of tests per sample (_see “Diagnostic Laboratory Results” chapter of the tracker design guide for more information_). In the aggregate data entry form, multiple samples per case, multiple tests per sample, and multiple tests per sample by type could be reported. This is important to keep in mind as there is a potential risk of number mismatches in calculations - if there are several tests registered per patient, the results will rightfully differ from one another.

The following program logic for counting cases is applicable for the TB diagnostics stage only. It will help users avoid calculation errors in scenarios where several samples are collected per case or certain tests are repeated:

- Sputum Smear Microscopy
  - **Presumptive TB cases with valid microscopy result** during TB diagnostics stage
    - "+++"  - if at least one test result is "+++"
    - "++" - if at least one test result is "++" and no "+++" results are recorded  
    - "+" - if at least one test result is "+" and no "++" or "+++" results are recorded
    - "Scanty" - if at least one test result is scanty and no "+", "++" or "+++" results are recorded
    - "Negative" - if all test results are negative and no positive test results are recorded
- Culture (solid or liquid medium)
  - **Presumptive TB cases with valid culture result**
    - "MTB" - if at least one test result is MTB
    - "NTM" - if at least one test result is NTM and no MTB results are recorded.
    - Cases with contaminated culture or no growth are not counted.
- Xpert MTB/RIF or Xpert MTB/RIF Ultra
  - **Presumptive TB cases with valid GeneXpert result**
    - "MTB detected" - if at least one test result is "MTB detected"
    - "MTB not detected" - if at least one test result is "MTB not detected" and no "MTB detected" results are recorded
  - MTB-positive cases with Rifampicin result by Xpert MTB/RIF
    - "Resistant" - if at least one result shows resistance.
    - "Susceptible" - if at least one result shows susceptibility and no resistance.
    - "Indeterminate" - if at least one result is indeterminate and no susceptibility or resistance is detected.

### Data Elements

All the data elements belonging to the Tb Lab dataset are grouped in the **“TB - Laboratory” data element group**.
The full list of data elements is available in the [reference file](resources/tb_agg_lab_reference.xlsx)

## Dataset Details

### Presumptive Cases

![Presumptive cases](resources/images/tb_agg_lab_002.png)

Data entered in this section is needed to calculate the overall positivity rate during TB diagnostics. Should the implementation collect only data for confirmed cases, this section and the data elements should be removed.

### TB Samples

![TB Samples](resources/images/tb_agg_lab_003.png)

In this section, basic sample information is recorded on samples collected, tested, received or lost. Each data element is a **subgroup of the previous**: out of the collected samples, the lab should report how many were physically received, and finally, among those samples, how many were accepted for testing. The data is **disaggregated by the type of patient** from which the samples were taken: presumptive cases (TB diagnostics) and confirmed cases (Case monitoring)Should the implementation collect only data related to confirmed cases, the category combination should be removed.

### Sputum Smear Microscopy

![Samples, cases and microscopy test volumes](resources/images/tb_agg_lab_004.png)

The section contains data related to smear microscopy tests:

- Samples tested by microscopy (TB diagnostics and Case monitoring)
- Cases tested by microscopy (TB diagnostics and Case monitoring)
- Presumptive TB cases with valid microscopy result (scanty, +, ++, +++, negative)
- Diagnostic microscopy tests by result (scanty, +, ++, +++, negative)

### Culture - Liquid and Solid Media

![Samples, cases and culture test volumes](resources/images/tb_agg_lab_005.png)

The solid and liquid culture media sections follow the same structure. Screenshots of the liquid culture medium section are included in this document for illustrative purposes.

- Samples tested by culture (TB diagnostics and Case monitoring)
- Cases tested by culture (TB diagnostics and Case monitoring)
- Presumptive TB cases with valid culture result (MTB, NTM)
- Diagnostic culture tests by result (MTB, NTM, no growth, contamination)

### GeneXpert - Xpert MTB/RIF and Xpert MTB/RIF Ultra

[Samples, cases and GeneXpert test volumes](resources/images/tb_agg_lab_006.png)

Xpert MTB/RIF and Xpert MTB/RIF Ultra sections follow the same structure. Screenshots of the Xpert MTB/RIF section are included in this document for illustrative purposes.

- Samples tested by GeneXpert (TB diagnostics)
- Cases tested by GeneXpert (TB diagnostics)
- Presumptive TB cases with valid GeneXpert result (MTB detected, MTB not detected)
- MTB-positive cases with Rifampicin result by GeneXpert (resistant, susceptible, indeterminate)
- GeneXpert tests by Rifampicin result (resistant, susceptible, indeterminate)
- Diagnostic Xpert MTB/RIF tests by result (MTB detected, MTB not detected, error, invalid, no result)

The case monitoring disaggregation option has been grayed-out as the **GeneXpert tests are not commonly used for case monitoring purposes**.

### Turnaround time for sample processing

![Turnaround time for sample processing](resources/images/tb_agg_lab_007.png)

The section contains data related to turnaround times for microscopy and culture samples. Microscopy samples are disaggregated by turnaround days between sample collection to test result (from 0 to 6+ days). Culture samples are disaggregated by turnaround days between sample collection to sample inoculation (from 0 to 11+ days).

- Microscopy turnaround time (total days)
- Solid culture turnaround time for sample inoculation (total days)
- Liquid culture turnaround time for sample inoculation (total days)
- Microscopy tests by turnaround time (0 to 6+)
- Solid culture tests by turnaround time (0 to 11+)
- Liquid culture tests by turnaround time (0 to 11+)

> **NOTE**
>
>The total turnaround time should be **manually calculated** based on the information from paper registers, excel documents or other electronic systems and entered in DHIS2.

## Validation rules

The package includes a set of validation rules to increase the quality of reported data. These rules check for consistency of data related to total numbers and disaggregations for cases, tests and samples.
All the validation rules belonging to the Tb-Lab dataset are grouped in the **“TB - Laboratory” validation rule group**. The full list of validation rules is available in the metadata reference file.

## Dashboards

![The Tb lab dashboards and the sections](resources/images/tb_agg_lab_008.png)

The TB-Lab dataset includes a predefined dashboard (**TB9. laboratory**) summarizing the key indicators for the monitoring of the laboratory activities (tests, cases, samples, positivity rates, turnaround times, and test results).

The dashboard contains a section including overall test data as well as separate sections for smear microscopy, Xpert MTB/RIF, Xpert MTB/RIF Ultra, solid and liquid cultures. Depending on the local context, type of implementation and test availability, the dashboard can and should be adapted to meet the implementation needs.

## User Groups

The module includes the same standard user groups as the TB HMIS package.

| Name | UID | Access rights |
|-----|------|---------------|
| TB access | `pyu2ZlNKbzQ` | View metadata, view data |
| TB admin  | `Ubzlyfqm1gO` | Edit and view metadata, view data |
| TB data capture | `UKWx4jJcrKt` | View metadata, capture and view data |

## Special considerations

### Tracker-to-aggregate Data Transfer

All data elements in the TB-Lab aggregate packageare mapped to Program Indicators in TB Case Surveillance tracker. This allows countries to run implementations where tracker and aggregate data collection and reporting have to be implemented in parallel. More information on the utilization of the aggregate dataset for individual data is available in the [“**Use of Aggregate Data Model with Tracker Deployments**”](https://docs.dhis2.org/en/implement/tracker-implementation/tracker-performance-at-scale.html#use-of-aggregate-data-model-with-tracker-deployments) section of the [**Tracker performance at scale**](https://docs.dhis2.org/en/implement/tracker-implementation/tracker-performance-at-scale.html#tracker-performance-at-scale) document.
In addition, [**Integrating tracker and aggregate data**](https://docs.dhis2.org/en/implement/maintenance-and-use/tracker-and-aggregate-data-integration.html#:~:text=Tracker%20data%20can%20be%20aggregated,month%20to%20produce%20monthly%20reports) document provides more details on different approaches to the implementation of combined aggregate and individual data.

## References

World Health Organization, (2013). Definitions and reporting framework for tuberculosis – 2013 revision (updated December 2014 and January 2020). Geneva. URL: <https://apps.who.int/iris/bitstream/handle/10665/79199/9789241505345_eng.pdf?sequence=1&isAllowed=y> [accessed 20 July 2022]

World Health Organization, (2020). WHO operational handbook on tuberculosis (Module 1 – Prevention):  Tuberculosis preventive treatment . Geneva, URL: <https://apps.who.int/iris/bitstream/handle/10665/331525/9789240002906-eng.pdf> [Accessed 22 July 2022]
