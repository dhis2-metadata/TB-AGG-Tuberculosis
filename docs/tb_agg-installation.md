# TB Aggregate Installation Guide { #tb-agg-installation }

This document includes an installation guide for TB HMIS packages.

System default language: English

## Overview

The package includes metadata json files containing the following components:

1. TB HMIS - Complete package
2. TB HMIS - Dashboard package
3. TB Laboratory
4. TB Laboratory (dashboard)
5. TB Household Contacts
6. TB Household Contacts (dashboard)
7. TB Stock
8. TB Stock (dashboard)
9. TB Data Quality (dashboard)

## Installation

Installation of the module consists of several steps:

1. [Preparing the metadata file with DHIS2 metadata](#preparing-the-metadata-file).
2. [Importing the metadata file into DHIS2](#importing-metadata).
3. [Configuring the imported metadata](#configuration).
4. [Adapting the program after import](#adapting-the-program)

It is recommended to first read through each section of the installation guide before starting the installation and configuration process in DHIS2. Identify applicable sections depending on the type of your import:

1. import into a blank DHIS2 instance
2. import into a DHIS2 instance with existing metadata.

The steps outlined in this document should be tested in a test/staging DHIS2 instance and only then applied to a production environment.

## Requirements

In order to install the module, an administrator user account on DHIS2 is required.

Great care should be taken to ensure that the server itself and the DHIS2 application are well secured, access rights to collected data should be defined. Details on securing a DHIS2 system is outside the scope of this document, and we refer to the [DHIS2 documentation](https://docs.dhis2.org/).

## Metadata files

While not always necessary, it can often be advantageous to make certain modifications to the metadata file before importing it into DHIS2.

## Preparing the metadata file

It is required to apply some changes to the metadata file before it can be imported. The scope of work may vary from package to package.

### Default data dimension

In early versions of DHIS2, the UIDs of the default data dimensions were auto-generated. Thus, while all DHIS2 instances have a default category option, data element category, category combination and category option combination, the UIDs of these defaults can be different. Later versions of DHIS2 have hardcoded UIDs for the default dimension, and these UIDs are used in the configuration packages.

To avoid conflicts when importing the metadata, it is advisable to search and replace the entire .json file for all occurrences of these default objects, replacing UIDs of the .json file with the UIDs from the instance in which the file will be imported. Table 1 shows the UIDs which should be replaced, as well as the API endpoints to identify the existing UIDs

|Object                       | UID           | API endpoint                                              |
|-----------------------------|---------------|-----------------------------------------------------------|
| Category                    | `GLevLNI9wkl` | `../api/categories.json?filter=name:eq:default`           |
| Category option             | `xYerKDKCefk` | `../api/categoryOptions.json?filter=name:eq:default`      |
| Category combination        | `bjDvmb4bfuf` | `../api/categoryCombos.json?filter=name:eq:default`       |
| Category option combination | `HllvX50cXC0` | `../api/categoryOptionCombos.json?filter=name:eq:default` |

Identify the UIDs of the default dimesions in your instance using the listed API requests and replace the UIDs in the json file with the UIDs from the instance.

> **NOTE**
>
> Note that this search and replace operation must be done with a plain text editor, not a word processor like Microsoft Word.

### Indicator types

Indicator type is another type of object that can create import conflict because certain names are used in different DHIS2 databases (.e.g "Percentage"). Since Indicator types are defined by their factor (including 1 for "numerator only" indicators), they are unambiguous and can be replaced through a search and replace of the UIDs. This method helps avoid potential import conflicts, and prevents the implementer from creating duplicate indicator types. The table below contains the UIDs which could be replaced, as well as the API endpoints to identify the existing UIDs:

|Object                  | UID           | API endpoint                                                          |
|------------------------|---------------|-----------------------------------------------------------------------|
| Numerator only (number)| `CqNPn5KzksS` | `../api/indicatorTypes.json?filter=number:eq:true&filter=factor:eq:1` |

### Visualizations using Root Organisation Unit UID

Visualizations, report tables, maps, validation rules that reference a specific organisation unit level or organisation unit group, contain placeholders eg. `<OU_ROOT_UID>` or `<OU_LEVEL_FACILITY_UID>`. Use the search function in the .json file editor to identify such placeholders and replace `<OU_ROOT_UID>` with the UID of the level 1 organisation unit in the target instance and `<OU_LEVEL_FACILITY_UID>` with the UID of the facility organisation unit level .

## Importing metadata

Use [Import/Export](#import_export) DHIS2 app to import metadata packages. It is advisable to use the "dry run" feature to identify issues before attempting to do an actual import of the metadata. If "dry run" reports any issues or conflicts, see the [import conflicts](#handling-import-conflicts) section below. If the "dry run"/"validate" import works without error, attempt to import the metadata. If the import succeeds without any errors, you can proceed to [configuring](#configuration) the module. In some cases, import conflicts or issues are not shown during the "dry run", but appear when the actual import is attempted. In this case, the import summary will list any errors that need to be resolved.

### Updating TB HMIS to version 2.0.0 from previous versions

An update of the TB HMIS package from previous versions is possible with following considerations:

  1. All existing metadata and data have to be backed up prior to the update.
  2. Existing TB HMIS indicators have to be accessible for reference (in a separate dev instance)
  3. TB HMIS version 2 package uses reuses a number of metadata objects from the previous package version. While UIDs of the objects remain the same, the metadata objects may have changed: data elements have new names, validation rules have been adapted for the new data sets, indicator numerators and denominators are based on the metadata, visualizations use new indicators. When importing new package into an instance with a previous version of the TB HMIS package, the existing metadata objects will be overwritten.
  4. TB HMIS version 2.0.0 package contains additional age and sex disaggregations. The data element `tr0lVojK425` **TB - New episodes of TB by age and sex** has been assigned a new Category Combo that contains these additional category option combinations. The data element was reused from the previous version where it had a name **TB - New and relapse TB cases by age and sex**. After importing the new package into an instance with the previous version of TB HMIS, please check the old data sets and use the category option override function and reassign the legacy category combo to this data element.
  5. When planning to use the legacy data and combine it with the new data to preserve the consistency of notification and outcomes data overtime, it is important to combine legacy data elements and new data elements within the new indicators where possible. Please refer to this [mapping guide](resources/dashboards_1.5.0_vs_2.0.0.xlsx) when aligning old and new data.

### Tracker-to-aggregate data transfer

If you are implementing tracker to aggregate data transfer between TB tracker and TB HMIS packages, please consider that the configuration of such transfer has to be reconfigured based on new guidelines, data element definitions, data element codes, etc.

### Handling import conflicts

> **NOTE**
>
> If you are importing the package into a new DHIS2 instance, you will not experience import conflicts, as there is no metadata in the target database. After import the metadata, proceed to the “[Configuration](#configuration)” section.

There are a number of different conflicts that may occur, though the most common is that there are metadata objects in the configuration package with a name, shortname and/or code that already exist in the target database. There are a couple of alternative solutions to these problems, with different advantages and disadvantages. Which one is more appropriate will depend, for example, on the type of object for which a conflict occurs.

#### Alternative 1

Rename the existing object in your DHIS2 database for which there is a conflict. The advantage of this approach is that there is no need to modify the .json file, as changes are instead done through the user interface of DHIS2. This is likely to be less error prone. It also means that the configuration package is left as is, which can be an advantage for example when updates to the package are released. The original package objects are also often referenced in training materials and documentation.

#### Alternative 2

Rename the object for which there is a conflict in the .json file. The advantage of this approach is that the existing DHIS2 metadata is left as-is. This can be a factor when there is training material or documentation such as SOPs of data dictionaries linked to the object in question, and it does not involve any risk of confusing users by modifying the metadata they are familiar with.

Note that for both alternative 1 and 2, the modification can be as simple as adding a small pre/post-fix to the name, to minimise the risk of confusion.

#### Alternative 3

A third and more complicated approach is to modify the .json file to re-use existing metadata. For example, in cases where an option set already exists for a certain concept (e.g. "sex"), that option set could be removed from the .json file and all references to its UID replaced with the corresponding option set already in the database. The big advantage of this (which is not limited to the cases where there is a direct import conflict) is to avoid creating duplicate metadata in the database. There are some key considerations to make when performing this type of modification:

- it requires expert knowledge of the detailed metadata structure of DHIS2
- the approach does not work for all types of objects. In particular, certain types of objects have dependencies which are complicated to solve in this way, for example related to disaggregations.
- future updates to the configuration package will be complicated.

## Configuration

Once all metadata has been successfully imported, there are a few steps that need to be taken before the module is functional.

### Sharing

First, you will have to use the *Sharing* functionality of DHIS2 to configure which users (user groups) should see the metadata and data associated with the program as well as who can register/enter data into the program. By default, sharing has been configured for the following:

- Dashboards
- Visualizations, maps, report tables
- Data sets
- Category options

Please refer to the [DHIS2 documentation](#sharing) for more information on sharing.

Three core user groups are included in the packages:

- TB access (view metadata/view data)
- TB admin (view and edit metadata/no access to data)
- TB data capture - (view metadata/capture and view data)

For TB Stock package the groups are:

- TB access (view metadata/view data)
- TB admin (view and edit metadata/no access to data)
- TB stock data capture - (view metadata/capture and view data)

The users are assigned to the appropriate user group based on their role within the system. Sharing for other objects in the package may be adjusted depending on the set up. Refer to the [DHIS2 Documentation on sharing](#sharing) for more information.

### User Roles

Users will need user roles in order to engage with the various applications within DHIS2. The following minimum roles are recommended:

1. Tracker data analysis : Can see event analytics and access dashboards, event reports, event visualizer, data visualizer, pivot tables, reports and maps.
2. Tracker data capture : Can add data values, update tracked entities, search tracked entities across org units and access tracker capture

Refer to the [DHIS2 Documentation](https://docs.dhis2.org/) for more information on configuring user roles.

### Organisation unit assignment

The data sets must be assigned to organisation units within existing hierarchy in order to be accessible via capture app.

### Indicator mapping (dashboard package)

When implementing the _dashboard package_ only, the indicator numerators and denominators have to be configured using the metadata objects in the existing instance. Configuration information is available in the documentation and the description of numerators and denominators in the metadata file.

### Duplicated metadata

> **NOTE**
>
> This section only applies if you are importing into a DHIS2 database where other metadata is already present. If you are working in a new DHIS2 instance, please skip this section and go to [Adapting the tracker program](#adapting-the-tracker-program).
> If you are using any third party applications that rely on the current metadata, please take into account that this update could "break" their functionality.

Even when metadata has been successfully imported without any import conflicts, there can be duplicates in the metadata - data elements, tracked entity attributes or option sets that already exist. As was noted in the section above on resolving conflict, an important issue to keep in mind is that decisions on making changes to the metadata in DHIS2 also needs to take into account other documents and resources that are in different ways associated with both the existing metadata, and the metadata that has been imported through the configuration package. Resolving duplicates is thus not only a matter of "cleaning up the database", but also making sure that this is done without, for example, breaking potential integrating with other systems, the possibility to use training material, breaking SOPs etc. This will very much be context-dependent.

One important thing to keep in mind is that DHIS2 has tools that can hide some of the complexities of potential duplications in metadata. For example, where duplicate option sets exist, they can be hidden for groups of users through [sharing](#sharing).

## Adapting the program

Once the program has been imported, you might want to make certain modifications to the program. Examples of local adaptations that *could* be made include:

- Adding additional variables to the form.
- Adapting data element/option names according to national conventions.
- Adding translations to variables and/or the data entry form.
- Modifying indicators based on local case definitions

However, it is strongly recommended to take great caution if you decide to change or remove any of the included form/metadata. There is a danger that modifications could break functionality, for example program rules and program indicators.

## Removing metadata

In order to keep your instance clean and avoid errors, it is recommended that you remove the unnecessary metadata from your instance. Removing unnecessary metadata requires advanced knowledge of DHIS2 and various dependencies.
