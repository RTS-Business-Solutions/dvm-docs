---
layout: default
title: "RTS Dynamic VAT Management"
---

<nav style="margin-bottom:1.5rem">
    <a href="../../index/da-DK">Home</a> &nbsp;|&nbsp;
    <a href="../../guide/da-DK">Setup Guide</a> &nbsp;|&nbsp;
    <a href="../../product-info/da-DK">Product Information</a> &nbsp;|&nbsp;
    <a href="../../eula/da-DK">EULA</a> &nbsp;|&nbsp;
    <a href="../../terms/da-DK">Terms &amp; Conditions</a> &nbsp;|&nbsp;
    <a href="../../privacy-policy/da-DK">Privacy Policy</a>
</nav>
# Dynamic VAT Management (DVM)

Dynamic VAT Management (DVM) muliggÃ¸r automatiseret VAT-hÃ¥ndtering for salg udfÃ¸rt pÃ¥ vessels, routes og legs, der krydser flere VAT-jurisdiktioner. Appen justerer VAT pÃ¥ POS-transaktioner baseret pÃ¥ det aktuelt aktive leg, sÃ¥ salg bogfÃ¸res med de korrekte VAT posting groups i Business Central og LS Central.

DVM kan Ã¦ndre legs baseret pÃ¥ Tidsplan, manuelt pÃ¥ POS af brugeren, der udfÃ¸rer kommandoen, eller ved at modtage positionsinformation fra et External system.

Dette dokument beskriver opsÃ¦tning, daglig brug og integrationsmuligheder for DVM-appen.

## Funktionsoversigt

| Feature | Description |
|----------|-------------|
| ForudsÃ¦tninger, installation og opsÃ¦tning | Beskriver hvordan DVM extension installeres og den indledende konfiguration udfÃ¸res. |
| Definition af routes og legs | Beskriver hvordan vessels, routes og legs defineres som grundlag for VAT-beregning. |
| Import og hÃ¥ndtering af timetable | Beskriver hvordan timetables importeres manuelt eller via API, og hvordan de pÃ¥virker VAT-Ã¦ndringer. |
| Manuel leg-Ã¦ndring pÃ¥ POS | Beskriver hvordan aktivt leg kan skiftes direkte fra POS via en POS command/button. |
| External integration / API | Beskriver hvordan DVM kan modtage leg- og timetable-information fra eksterne systemer. |
| OvervÃ¥gning og fejlfinding | Beskriver hvordan du verificerer, at VAT-Ã¦ndringer behandles korrekt, og hvordan fejl hÃ¥ndteres. |

## 1. Installation og grundopsÃ¦tning

### 1.1 Installation

I Business Central Ã¥bner du *Extension Management*.

- SÃ¸rg for, at DVM-appen (Dynamic VAT Management) er tilgÃ¦ngelig.
- Installer extension i mÃ¥lmiljÃ¸et (TEST/PROD).
- Efter installation bliver DVM setup-sider og lister tilgÃ¦ngelige i sÃ¸gning.

### 1.2 DVM Setup

Ã…bn siden DVM Setup.

Udfyld fÃ¸lgende felter:

- **Enable DVM** - sÃ¦t til Yes for at aktivere logikken.
- **Default Skib** - default vessel sÃ¦ttes specifikt for og i hver vessel-database separat.
- **Use Tidsplan** - marker dette felt, hvis timetable-baserede DVM-Ã¦ndringer skal bruges.

### 1.3 Definition af Skib, Rute og StrÃ¦kning

FÃ¸r VAT kan beregnes, skal hierarkiet som DVM bruger defineres:

- **Skibe** - fysiske vessels eller enheder, hvor POS-enheder kÃ¸rer.
- **Ruter** - forbindelser mellem ports/locations, som vessel sejler pÃ¥.
- **StrÃ¦kninger** - individuelle segmenter af en route, hver med egen VAT-konfiguration.

Trin:

1. Ã…bn listen **Skibe** og opret en post for hvert vessel.
2. Ã…bn **Ruter** og opret en route for hver linje, som vessel sejler pÃ¥.
3. Ã…bn **StrÃ¦kninger** og opret legs for hver route.
4. For hvert leg tildeler du route og sÃ¦tter sequence/order, der afspejler den faktiske sejladsrÃ¦kkefÃ¸lge.

### 1.4 Butiksmomsgrupper pr. StrÃ¦kning

For hvert leg skal du definere, hvilken VAT Business Posting Group der skal bruges, nÃ¥r salg udfÃ¸res pÃ¥ det leg.

For hvert leg kan du:

- Definere en default VAT Business Posting Group (uden Store No.).
- Valgfrit definere specifikke VAT Business Posting Groups pr. store for at overstyre default.

### 1.5 POS / Store Setup

DVM bruger store (LS Central store/location) og aktivt leg til at beregne VAT.

SÃ¸rg for at:

- Stores er knyttet til korrekte Business Central locations.
- POS terminals er knyttet til korrekt store og vessel.
- DVM POS commands er tilgÃ¦ngelige pÃ¥ POS.

## 2. Import og hÃ¥ndtering af Tidsplan

Denne mulighed udfyldes kun, hvis DVM skal bruge timetable til leg-Ã¦ndringer.

PÃ¥ siden DVM Setup skal **Tidsplan in Use** vÃ¦re aktiveret.

### 2.1 Tidsplan-struktur

En timetable definerer, hvornÃ¥r et vessel forventes at vÃ¦re pÃ¥ en specifik route og et specifikt leg.

En timetable-linje indeholder typisk:

- Skib
- Rute
- StrÃ¦kning
- Departure date and time
- Arrival date and time
- Optional shift date and time
- VAT Business Posting Group

### 2.2 Manuel timetable-import

For at importere timetables manuelt:

1. Ã…bn siden **DVM Tidsplan Import**.
2. VÃ¦lg **Import**.
3. VÃ¦lg filen.
4. BekrÃ¦ft importen.

Alle refererede vessels, routes og legs skal allerede eksistere. Linjer, der refererer til ikke-eksisterende koder, afvises under import.

### 2.3 Automatisk timetable-import (API / Job Queue)

I miljÃ¸er hvor timetable opdateres hyppigt, anbefales automatisk import via API.

- Et web service- eller API-endpoint modtager timetable-data fra det eksterne planlÃ¦gningssystem.
- Data skrives i DVM timetable-tabeller.
- En Job Queue Entry kÃ¸rer efter plan og behandler importerede timetables samt opdaterer aktive legs.

Kontakt din partner eller systemintegrator for tekniske detaljer om API-integrationen.

## 3. HÃ¥ndtering af Rute og StrÃ¦kning under drift

### 3.1 FastlÃ¦ggelse af aktivt StrÃ¦kning

Det aktive leg er det leg, som DVM bruger til at beregne VAT pÃ¥ et givent tidspunkt.

Aktivt leg kan bestemmes pÃ¥ en af fÃ¸lgende mÃ¥der:

- Fra timetable.
- Fra et external system.
- Manuelt fra POS.

### 3.2 Automatisk VAT-skift fra timetable

Hvis timetables bruges, kontrollerer DVM periodisk timetable for hvert vessel og aktiverer det leg, der matcher aktuel dato og tid.

NÃ¥r aktivt leg Ã¦ndres, gÃ¸r DVM fÃ¸lgende:

- Opdaterer aktivt leg for vessel.
- Opretter en Job Queue Entry, som sikrer, at alle POS terminals modtager den nye VAT-indstilling.
- Bruger VAT Store Group per StrÃ¦kning setup til at bestemme korrekt VAT Business Posting Group.

### 3.3 Eksempel

Eksempel:

**Skib FERRY01** sejler fra Port A til Port B med fÃ¸lgende legs:

- StrÃ¦kning 10 - Port A -> International waters (domestic VAT).
- StrÃ¦kning 20 - International waters -> Port B (foreign VAT).

NÃ¥r timetable angiver, at vessel er gÃ¥et ind i StrÃ¦kning 20, Ã¦ndrer DVM automatisk aktivt leg til 20, og alle efterfÃ¸lgende POS-salg bogfÃ¸res med VAT Business Posting Group konfigureret for StrÃ¦kning 20.

## 4. External Integration (API)

### 4.1 Oversigt

DVM kan integreres med eksterne systemer (fx trafik- eller planlÃ¦gningssystemer), som styrer aktuel route og leg.

Det eksterne system kan sende leg-Ã¦ndringer til DVM i stedet for udelukkende at basere sig pÃ¥ timetables.

### 4.2 Typisk integrationsflow

1. External system fastslÃ¥r, at et vessel har skiftet leg.
2. Det kalder en DVM API/web service med vessel, route og leg.
3. DVM validerer data og skriver en leg-change request.
4. Job Queue behandler request og opdaterer aktivt leg.
5. POS terminals modtager den nye VAT-opsÃ¦tning.

### 4.3 FejlhÃ¥ndtering

Hvis external system sender ukendt vessel, route eller leg, afvises request og logges.

IntegrationsovervÃ¥gning bÃ¸r kontrollere fejlede leg-change requests og rette masterdata eller payload.

## 5. POS-kommandoer

### 5.1 Manuel leg-Ã¦ndring (DVM_CHNGLEG)

Brugere kan Ã¦ndre aktivt leg manuelt direkte fra POS.

OpsÃ¦tning i LS Central POS:

1. Ã…bn siden **POS Functions / Commands**.
2. Opret eller find kommandoen **DVM_CHNGLEG**.
3. TilfÃ¸j en POS-knap og tildel kommandoen.
4. Udrul layout til relevante POS terminals.

Brug:

- Kassemedarbejderen trykker pÃ¥ knappen DVM_CHNGLEG.
- En dialog Ã¥bner med tilgÃ¦ngelige legs for vessel.
- Kassemedarbejderen vÃ¦lger Ã¸nsket leg.
- DVM opretter et leg-change job.
- Job Queue behandler request.
- Aktiv VAT-opsÃ¦tning opdateres.

### 5.2 Andre commands (valgfrit)

AfhÃ¦ngigt af konfigurationen kan yderligere commands gÃ¸res tilgÃ¦ngelige:

- Vis aktuelt aktivt leg pÃ¥ POS.
- Opdater VAT-konfiguration fra back end.

Disse commands er valgfri og kan variere pr. implementering.

## 6. OvervÃ¥gning og fejlfinding

### 6.1 Job Queue-overvÃ¥gning

DVM er afhÃ¦ngig af Business Central Job Queue til behandling af timetable-importer, leg-Ã¦ndringer og VAT-opdateringer.

Hvis VAT ikke Ã¦ndres som forventet:

- Ã…bn **Job Queue Entries** og verificer, at DVM jobs er aktiverede og kÃ¸rer.
- Kontroller **Job Queue Log Entries** for fejl.
- Ret eventuelle datafejl og genstart jobbet.

### 6.2 Typiske problemer

#### VAT Ã¦ndrede sig ikke, da vessel gik ind i et nyt leg

- Kontroller at timetable indeholder en linje for vessel, route og tidspunkt.
- Verificer at leg har en VAT Store Group defineret.
- BekrÃ¦ft at Job Queue Entry blev udfÃ¸rt korrekt.

#### POS viser forkert VAT-sats

- Kontroller hvilket leg der aktuelt er aktivt.
- Verificer store-mapping i VAT Store Group per StrÃ¦kning.
- Opdater POS eller genstart sessionen.

#### Manuel leg-Ã¦ndring fra POS har ingen effekt

- BekrÃ¦ft at DVM_CHNGLEG command er konfigureret korrekt.
- Kontroller Job Queue for fejlede leg-change jobs.
- Sikr at valgt leg eksisterer og er aktivt.

### 6.3 Supportoplysninger

Hvis problemet ikke kan lÃ¸ses, sÃ¥ vedlÃ¦g fÃ¸lgende i en supportsag:

- MiljÃ¸ (TEST/PROD) og Business Central / LS Central version.
- DVM-version.
- Involveret vessel, route og leg.
- POS-dokumentnummer og timestamp.
- SkÃ¦rmbilleder af Job Queue Entries og fejlmeddelelser.

---

# Billedliste

Ingen billeder blev fundet i den leverede HTML.

TilfÃ¸j billedreferencer her, nÃ¥r de er tilgÃ¦ngelige:

| Placeholder | File Name | Insert Position |
|------------|-----------|-----------------|
| IMG-01 | | |
| IMG-02 | | |
| IMG-03 | | |
| IMG-04 | | |
```
