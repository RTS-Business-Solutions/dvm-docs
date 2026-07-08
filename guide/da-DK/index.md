---
layout: default
title: "Setup Guide"
---

<nav style="margin-bottom:1.5rem">
    <a href="../../index/da-DK">Home</a> &nbsp;|&nbsp;
    <a href="../../guide/da-DK">Setup Guide</a> &nbsp;|&nbsp;
    <a href="../../product-info/da-DK">Product Information</a> &nbsp;|&nbsp;
    <a href="../../eula/da-DK">EULA</a> &nbsp;|&nbsp;
    <a href="../../terms/da-DK">Terms &amp; Conditions</a> &nbsp;|&nbsp;
    <a href="../../privacy-policy/da-DK">Privacy Policy</a>
</nav>
# Dynamic VAT Management

## Indholdsfortegnelse

- [Generelt](#general)
- [Tidsplan](#timetable)
- [Manuel VAT-Ã¦ndring](#manual_vat_change)
- [Integrationer](#integrations)
- [Generel opsÃ¦tning](#generel_setup)
- [Skibe](#vessels)
- [Skibsfelter](#vessel_fields)
- [Ruter](#routes)
- [Rutefelter](#route_fields)
- [StrÃ¦kninger](#legs)
- [StrÃ¦kningsfelter](#leg_fields)
- [Butiksmomsgrupper](#vat_store_groups)
- [Butiksmomsfelter](#store_vat_fields)
- [VAT-tidsplan](#vat_timetable)
- [Manuel tidsplan-import](#manual_timetable_import)
- [Automatisk tidsplan-import](#automatic_timetable_import)
- [Uden tidsplan](#no_timetable)
- [POS-kommando](#pos_command)
- [Registrer DVM POS-kommandoer](#register_dvm_pos_commands)
- [Brug af DVM_CHNGLEG](#using_dvm_chngleg)
- [Blueflow](#blueflow)

<a id="general"></a>
<a id="vessel_setup"></a>
## Generelt

Dynamic VAT Management er designet til at justere VAT for internationale transaktioner baseret pÃ¥ destination.

Modulet Ã¦ndrer VAT ved at opdatere VAT Business Posting Group i POS-transaktioner.

Med mulighed for at definere flere Ports, Ruter og StrÃ¦kninger kan Dynamic VAT Management bestemme korrekt VAT.

Dette kan styres via:

- Tidsplan
- Manuel leg-Ã¦ndring

<a id="timetable"></a>
### Tidsplan

For effektiv hÃ¥ndtering af VAT-Ã¦ndringer tilbyder Dynamic VAT Management mulighed for at uploade tidsplaner med de nÃ¸dvendige data og handlinger til VAT-justering.

Hvis en tidsplan ikke findes, anvender systemet VAT Business Posting Group fra Store Card.

Modulet tillader ikke Ã¸jeblikkelige Ã¦ndringer af dimensioner eller andre parametre, der ikke styres af VAT posting group.

Modulet er i Ã¸jeblikket kun tilgÃ¦ngeligt pÃ¥ engelsk.

<a id="manual_vat_change"></a>
### Manuel VAT-Ã¦ndring

Deaktiver **Tidsplan in Use** for at hÃ¥ndtere VAT-Ã¦ndringer manuelt.

For at Ã¦ndre VAT manuelt oprettes en POS-knap med **DVMCHGLEG** POS command.

> Denne funktionalitet er endnu ikke implementeret.

<a id="integrations"></a>
### Integrationer

Dynamic VAT Management understÃ¸tter API-integration med eller uden brug af tidsplan.

Integration med Blueflow understÃ¸ttes.

> Opret en supporthenvendelse for tekniske integrationsdetaljer.

---

<a id="generel_setup"></a>
<a id="general_setup"></a>
## Generel opsÃ¦tning

FÃ¸r Dynamic VAT Management kan anvendes, skal fÃ¸lgende masterdata oprettes:

- Skibe
- Ruter
- StrÃ¦kninger

Alle ruter, der refereres i tidsplanfiler, skal eksistere i Rute-tabellen fÃ¸r import.

Ellers vil tidsplan-import fejle.

> **Note:** Dynamic VAT-logik er kun aktiv, nÃ¥r **DVM Enable** er slÃ¥et til.

![VesselSetup_DVMEnable.png](../../images/VesselSetup_DVMEnable.png)

---

<a id="vessels"></a>
## Skibe

Alle skibe, hvor DVM skal styre VAT, skal oprettes.

Skibsliste er tilgÃ¦ngelig fra Skib Setup-siden.

![DVMSetup_Vessel.png](../../images/DVMSetup_Vessel.png)

VÃ¦lg **Skibe** for at Ã¥bne skibslisten.

![Vessels_list.png](../../images/Vessels_list.png)

<a id="vessel_fields"></a>
### Skibsfelter

| Field | Description |
| --------- | ------------- |
| Code | LS Central intern skibskode |
| External Code | Kode brugt af eksterne systemer |
| Active | Angiver at DVM er aktiv for skibet |
| Description | Skib-navn |

---

<a id="routes"></a>
## Ruter

For at understÃ¸tte VAT-hÃ¥ndtering via tidsplaner skal ruter vÃ¦re defineret fÃ¸r import af tidsplaner.

![DVMSetup_Route-Setup-1.png](../../images/DVMSetup_Route-Setup-1.png)

<a id="route_fields"></a>
### Rutefelter

| Field | Description |
| --------- | ------------- |
| Code | LS Central intern rutekode |
| External Code | Kode brugt af eksterne systemer |
| Active | Angiver aktiv DVM-rute |
| Description | Rute-beskrivelse |

---

<a id="legs"></a>
## StrÃ¦kninger

StrÃ¦kninger definerer de VAT-segmenter, der anvendes under rejsen.

Hver rute indeholder Ã©n eller flere strÃ¦kninger.
<a id="route_legs"></a>
![Route-Leg.png](../../images/Route-Leg.png)

![Leg.png](../../images/Leg.png)

<a id="leg_fields"></a>
### StrÃ¦kningsfelter

| Field | Description |
| --------- | ------------- |
| Code | LS Central intern leg-kode |
| Description | Beskrivende tekst |
| External Code | Ekstern leg-identifikator |
| Harbor | IndgÃ¥ende eller udgÃ¥ende havn |
| External Ref. | Ekstern integrationsreference |

---

<a id="vat_store_groups"></a>
## Butiksmomsgrupper

For hver strÃ¦kning skal Ã©n eller flere VAT Business Posting Groups defineres.

Hvis Store Code er tom, betragtes VAT-opsÃ¦tningen som standardkonfiguration.

![DVMLeg_StoreVATGroups.png](../../images/DVMLeg_StoreVATGroups.png)

> **Attention:** Best practice er altid at oprette en standard VAT-opsÃ¦tning for hver strÃ¦kning.

![StoreVATGroups.png](../../images/StoreVATGroups.png)

<a id="store_vat_fields"></a>
### Butiksmomsfelter

| Field | Description |
| --------- | ------------- |
| Store No. | Store-identifikator |
| Store Description | Store-beskrivelse |
| VAT Bus. Posting Grp. Code | VAT posting group anvendt af store |
| VAT Bus. Posting Grp. Description | Beskrivelse af VAT posting group |

---

<a id="vat_timetable"></a>
## VAT-tidsplan

RTS VAT Management understÃ¸tter tre metoder til VAT-Ã¦ndringer:

1. Tidsplan
2. Manuel input (POS-kommando)
3. Eksterne integrationer (fx Blueflow)

![VesselSetup_TimetableEnable.png](../../images/VesselSetup_TimetableEnable.png)

En tidsplan indeholder:

- Rute
- StrÃ¦kning
- Departure
- Arrival
- Shift Date Time

Baseret pÃ¥ Shift Date Time beregnes departure-timing.

![DVM-image006.png](../../images/DVM-image006.png)

Efter opsÃ¦tning skal **Current Vessel Code** vÃ¦lges i Skib Setup.

POS bruger current vessel og current leg til at bestemme korrekt VAT Business Posting Group.

---

<a id="manual_timetable_import"></a>
## Manuel tidsplan-import

Manuelle tidsplan-importer udfÃ¸res fra Skib Setup.

![VesselSetup_Import_Timetable.png](../../images/VesselSetup_Import_Timetable.png)

Brug **Import Tidsplan** til manuel indlÃ¦sning af tidsplandata.

> Opret en supporthenvendelse for specifikation af tidsplanfilformat.

---

<a id="automatic_timetable_import"></a>
## Automatisk tidsplan-import

Tidsplaner kan ogsÃ¥ importeres via DVM API.

> Opret en supporthenvendelse for API-dokumentation.

---

<a id="no_timetable"></a>
## Uden tidsplan

DVM kan kÃ¸re uden brug af tidsplan.

Eksterne applikationer som Blueflow kan styre rute- og strÃ¦kningsÃ¦ndringer via API.

![VesselSetup_ExternalMgtEnable.png](../../images/VesselSetup_ExternalMgtEnable.png)

For at bruge DVM uden tidsplaner:

- Konfigurer Current Rute
- Konfigurer Current StrÃ¦kning

POS bestemmer VAT ud fra Current StrÃ¦kning.

> **Attention:** Standard VAT-opsÃ¦tning skal eksistere for hver strÃ¦kning.

---

<a id="pos_command"></a>
## POS-kommando

<a id="register_dvm_pos_commands"></a>
### Registrer DVM POS-kommandoer

Ã…bn:

#### POS External Commands â†’ Register

Registrer:

#### RTS.DVM External POS Commands

Efter registrering kan **DVM_CHNGLEG** tildeles POS-knapper.

![DVMPOSCommandSetup.png](../../images/DVMPOSCommandSetup.png)

---

<a id="using_dvm_chngleg"></a>
### Brug af DVM_CHNGLEG

NÃ¥r funktionen aktiveres, vises en bekrÃ¦ftelsesdialog.

![DVMPOSCommandDialog.png](../../images/DVMPOSCommandDialog.png)

Hvis brugeren vÃ¦lger **Yes**, vises tilgÃ¦ngelige strÃ¦kninger.

![DVMPOSCommandSelect.png](../../images/DVMPOSCommandSelect.png)

Den valgte strÃ¦kning aktiveres derefter.

Ã†ndringen fÃ¸lger samme proces som et API-udlÃ¸st strÃ¦kningsskift (fx Blueflow).

> StrÃ¦kning-skift sker ikke Ã¸jeblikkeligt. En Job Queue Entry oprettes og behandles nÃ¦sten med det samme.

Hvis strÃ¦kningsskiftet ikke sker:

- Kontroller Job Queue Entries
- Kontroller Job Queue Errors
- Verificer rute- og strÃ¦kningskonfiguration

---

<a id="blueflow"></a>
## Blueflow

Integration med Blueflow og andre kompatible eksterne systemer understÃ¸ttes.

Rute- og strÃ¦kningsopdateringer modtages via integrationer og afspejles i Skib Setup.

For tekniske integrationsdetaljer opret en supporthenvendelse.
