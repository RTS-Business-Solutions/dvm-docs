---
layout: default
title: "Setup Guide"
---

<nav style="margin-bottom:1.5rem">
  <a href=".">Home</a> &nbsp;|&nbsp;
  <a href="guide">Setup Guide</a> &nbsp;|&nbsp;
  <a href="product-info">Product Information</a> &nbsp;|&nbsp;
  <a href="eula">EULA</a> &nbsp;|&nbsp;
  <a href="terms">Terms &amp; Conditions</a> &nbsp;|&nbsp;
  <a href="privacy-policy">Privacy Policy</a>
</nav>
# Dynamic VAT Management

## Table of Contents

- [General](#general)
	- [Timetable](#timetable)
	- [Manual VAT Change](#manual_vat_change)
	- [Integrations](#integrations)
- [General Setup](#generel_setup)
- [Vessels](#vessels)
	- [Vessel Fields](#vessel_fields)
- [Routes](#routes)
	- [Route Fields](#route_fields)
- [Legs](#legs)
	- [Leg Fields](#leg_fields)
- [VAT Store Groups](#vat_store_groups)
	- [Store VAT Fields](#store_vat_fields)
- [VAT Timetable](#vat_timetable)
- [Manual Timetable Import](#manual_timetable_import)
- [Automatic Timetable Import](#automatic_timetable_import)
- [No Timetable](#no_timetable)
- [POS Command](#pos_command)
	- [Register DVM POS Commands](#register_dvm_pos_commands)
	- [Using DVM_CHNGLEG](#using_dvm_chngleg)
- [Blueflow](#blueflow)

<a id="general"></a>
<a id="vessel_setup"></a>
## General

Dynamic VAT Management is tailored to adjust VAT for international transactions based on the destination.

This module modifies VAT by altering the VAT Business Posting Group in POS transactions.

With the possibility to define several Ports, Routes and Legs, Dynamic VAT Management is able to determine the correct VAT.

This can be managed by either:

- Timetable
- Manual Leg Change

<a id="timetable"></a>
### Timetable

To efficiently manage VAT changes, Dynamic VAT Management provides the capability to upload timetables that specify the necessary data and actions required for VAT adjustments.

In the absence of a timetable, the system defaults to the VAT Business Posting Group defined on the Store Card.

The module does not permit immediate changes to dimensions or other parameters not governed by the VAT posting group.

The module is currently available in English only.

<a id="manual_vat_change"></a>
### Manual VAT Change

Disable **Timetable in Use** to manage VAT changes manually.

To change VAT manually, create a POS button using the **DVMCHGLEG** POS command.

> This functionality is not yet implemented.

<a id="integrations"></a>
### Integrations

Dynamic VAT Management provides API integration capabilities with or without timetable usage.

Integration with Blueflow is supported.

> Please submit a support request for technical integration details.

---

<a id="generel_setup"></a>
<a id="general_setup"></a>
## General Setup

Before Dynamic VAT Management can be used, the following master data must be created:

- Vessels
- Routes
- Legs

All routes referenced in timetable files must exist in the Route table before importing.

Otherwise, timetable imports will fail.

> **Note:** Dynamic VAT logic is only active when **DVM Enable** is enabled.

![VesselSetup_DVMEnable.png](images/VesselSetup_DVMEnable.png)

---

<a id="vessels"></a>
## Vessels

All vessels where DVM should control VAT must be created.

The Vessel List is accessible from the Vessel Setup page.

![DVMSetup_Vessel.png](images/DVMSetup_Vessel.png)

Select **Vessels** to open the vessel list.

![Vessels_list.png](images/Vessels_list.png)

<a id="vessel_fields"></a>
### Vessel Fields

| Field | Description |
| --------- | ------------- |
| Code | LS Central internal vessel code |
| External Code | Code used by external systems |
| Active | Indicates that DVM is enabled for the vessel |
| Description | Vessel name |

---

<a id="routes"></a>
## Routes

To support VAT management through timetables, routes must be defined before importing schedules.

![DVMSetup_Route-Setup-1.png](images/DVMSetup_Route-Setup-1.png)

<a id="route_fields"></a>
### Route Fields

| Field | Description |
| --------- | ------------- |
| Code | LS Central internal route code |
| External Code | Code used by external systems |
| Active | Indicates active DVM route |
| Description | Route description |

---

<a id="legs"></a>
## Legs

Legs define the VAT segments used during travel.

Every route contains one or more legs.
<a id="route_legs"></a>
![Route-Leg.png](images/Route-Leg.png)

![Leg.png](images/Leg.png)

<a id="leg_fields"></a>
### Leg Fields

| Field | Description |
| --------- | ------------- |
| Code | LS Central internal leg code |
| Description | Descriptive text |
| External Code | External leg identifier |
| Harbor | Inbound or outbound harbor |
| External Ref. | External integration reference |

---

<a id="vat_store_groups"></a>
## VAT Store Groups

For every leg, one or more VAT Business Posting Groups must be defined.

If Store Code is blank, the VAT setup is considered the default configuration.

![DVMLeg_StoreVATGroups.png](images/DVMLeg_StoreVATGroups.png)

> **Attention:** Best practice is always to create a default VAT setup for each leg.

![StoreVATGroups.png](images/StoreVATGroups.png)

<a id="store_vat_fields"></a>
### Store VAT Fields

| Field | Description |
| --------- | ------------- |
| Store No. | Store identifier |
| Store Description | Store description |
| VAT Bus. Posting Grp. Code | VAT posting group used by the store |
| VAT Bus. Posting Grp. Description | VAT posting group description |

---

<a id="vat_timetable"></a>
## VAT Timetable

RTS VAT Management supports three methods for VAT changes:

1. Timetable
2. Manual input (POS Command)
3. External integrations (e.g. Blueflow)

![VesselSetup_TimetableEnable.png](images/VesselSetup_TimetableEnable.png)

A timetable contains:

- Route
- Leg
- Departure
- Arrival
- Shift Date Time

Based on the Shift Date Time, departure timing is calculated.

![DVM-image006.png](images/DVM-image006.png)

After setup, the **Current Vessel Code** must be selected on Vessel Setup.

The POS uses the current vessel and current leg to determine the correct VAT Business Posting Group.

---

<a id="manual_timetable_import"></a>
## Manual Timetable Import

Manual timetable imports are performed from Vessel Setup.

![VesselSetup_Import_Timetable.png](images/VesselSetup_Import_Timetable.png)

Use **Import Timetable** to load timetable data manually.

> Submit a support request for timetable file format specifications.

---

<a id="automatic_timetable_import"></a>
## Automatic Timetable Import

Timetables can also be imported using the DVM API.

> Submit a support request for API documentation.

---

<a id="no_timetable"></a>
## No Timetable

DVM can operate without timetable usage.

External applications such as Blueflow can control route and leg changes using the API.

![VesselSetup_ExternalMgtEnable.png](images/VesselSetup_ExternalMgtEnable.png)

To use DVM without timetables:

- Configure Current Route
- Configure Current Leg

The POS will determine VAT from the Current Leg.

> **Attention:** Default VAT setup must exist for every leg.

---

<a id="pos_command"></a>
## POS Command

<a id="register_dvm_pos_commands"></a>
### Register DVM POS Commands

Open:

#### POS External Commands → Register

Register:

#### RTS.DVM External POS Commands

After registration, **DVM_CHNGLEG** can be assigned to POS buttons.

![DVMPOSCommandSetup.png](images/DVMPOSCommandSetup.png)

---

<a id="using_dvm_chngleg"></a>
### Using DVM_CHNGLEG

When activated, a confirmation dialog is displayed.

![DVMPOSCommandDialog.png](images/DVMPOSCommandDialog.png)

If the user selects **Yes**, the available legs are shown.

![DVMPOSCommandSelect.png](images/DVMPOSCommandSelect.png)

The selected leg is then activated.

The change follows the same process as an API-triggered leg change (e.g. Blueflow).

> Leg changes are not immediate. A Job Queue entry is created and processed almost instantly.

If the leg change does not occur:

- Check Job Queue Entries
- Check Job Queue Errors
- Verify route and leg configuration

---

<a id="blueflow"></a>
## Blueflow

Integration with Blueflow and other compatible external systems is supported.

Route and leg updates are received through integrations and reflected in Vessel Setup.

For technical integration details, submit a support request.
