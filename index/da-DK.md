---
layout: default
title: "RTS Dynamic VAT Management"
---

<nav style="margin-bottom:1.5rem">
    <a href="../index/da-DK">Home</a> &nbsp;|&nbsp;
    <a href="../guide/da-DK">Setup Guide</a> &nbsp;|&nbsp;
    <a href="../product-info/da-DK">Product Information</a> &nbsp;|&nbsp;
    <a href="../eula/da-DK">EULA</a> &nbsp;|&nbsp;
    <a href="../terms/da-DK">Terms &amp; Conditions</a> &nbsp;|&nbsp;
    <a href="../privacy-policy/da-DK">Privacy Policy</a>
</nav>
# Dynamic VAT Management (DVM)

Dynamic VAT Management (DVM) muliggør automatiseret VAT-håndtering for salg udført på vessels, routes og legs, der krydser flere VAT-jurisdiktioner. Appen justerer VAT på POS-transaktioner baseret på det aktuelt aktive leg, så salg bogføres med de korrekte VAT posting groups i Business Central og LS Central.

DVM kan ændre legs baseret på Tidsplan, manuelt på POS af brugeren, der udfører kommandoen, eller ved at modtage positionsinformation fra et External system.

Dette dokument beskriver opsætning, daglig brug og integrationsmuligheder for DVM-appen.

## Funktionsoversigt

| Feature | Description |
|----------|-------------|
| Forudsætninger, installation og opsætning | Beskriver hvordan DVM extension installeres og den indledende konfiguration udføres. |
| Definition af routes og legs | Beskriver hvordan vessels, routes og legs defineres som grundlag for VAT-beregning. |
| Import og håndtering af timetable | Beskriver hvordan timetables importeres manuelt eller via API, og hvordan de påvirker VAT-ændringer. |
| Manuel leg-ændring på POS | Beskriver hvordan aktivt leg kan skiftes direkte fra POS via en POS command/button. |
| External integration / API | Beskriver hvordan DVM kan modtage leg- og timetable-information fra eksterne systemer. |
| Overvågning og fejlfinding | Beskriver hvordan du verificerer, at VAT-ændringer behandles korrekt, og hvordan fejl håndteres. |

## 1. Installation og grundopsætning

### 1.1 Installation

I Business Central åbner du *Extension Management*.

- Sørg for, at DVM-appen (Dynamic VAT Management) er tilgængelig.
- Installer extension i målmiljøet (TEST/PROD).
- Efter installation bliver DVM setup-sider og lister tilgængelige i søgning.

### 1.2 DVM Setup

Åbn siden DVM Setup.

Udfyld følgende felter:

- **Enable DVM** - sæt til Yes for at aktivere logikken.
- **Default Skib** - default vessel sættes specifikt for og i hver vessel-database separat.
- **Use Tidsplan** - marker dette felt, hvis timetable-baserede DVM-ændringer skal bruges.

### 1.3 Definition af Skib, Rute og Strækning

Før VAT kan beregnes, skal hierarkiet som DVM bruger defineres:

- **Skibe** - fysiske vessels eller enheder, hvor POS-enheder kører.
- **Ruter** - forbindelser mellem ports/locations, som vessel sejler på.
- **Strækninger** - individuelle segmenter af en route, hver med egen VAT-konfiguration.

Trin:

1. Åbn listen **Skibe** og opret en post for hvert vessel.
2. Åbn **Ruter** og opret en route for hver linje, som vessel sejler på.
3. Åbn **Strækninger** og opret legs for hver route.
4. For hvert leg tildeler du route og sætter sequence/order, der afspejler den faktiske sejladsrækkefølge.

### 1.4 Butiksmomsgrupper pr. Strækning

For hvert leg skal du definere, hvilken VAT Business Posting Group der skal bruges, når salg udføres på det leg.

For hvert leg kan du:

- Definere en default VAT Business Posting Group (uden Store No.).
- Valgfrit definere specifikke VAT Business Posting Groups pr. store for at overstyre default.

### 1.5 POS / Store Setup

DVM bruger store (LS Central store/location) og aktivt leg til at beregne VAT.

Sørg for at:

- Stores er knyttet til korrekte Business Central locations.
- POS terminals er knyttet til korrekt store og vessel.
- DVM POS commands er tilgængelige på POS.

## 2. Import og håndtering af Tidsplan

Denne mulighed udfyldes kun, hvis DVM skal bruge timetable til leg-ændringer.

På siden DVM Setup skal **Tidsplan in Use** være aktiveret.

### 2.1 Tidsplan-struktur

En timetable definerer, hvornår et vessel forventes at være på en specifik route og et specifikt leg.

En timetable-linje indeholder typisk:

- Skib
- Rute
- Strækning
- Departure date and time
- Arrival date and time
- Optional shift date and time
- VAT Business Posting Group

### 2.2 Manuel timetable-import

For at importere timetables manuelt:

1. Åbn siden **DVM Tidsplan Import**.
2. Vælg **Import**.
3. Vælg filen.
4. Bekræft importen.

Alle refererede vessels, routes og legs skal allerede eksistere. Linjer, der refererer til ikke-eksisterende koder, afvises under import.

### 2.3 Automatisk timetable-import (API / Job Queue)

I miljøer hvor timetable opdateres hyppigt, anbefales automatisk import via API.

- Et web service- eller API-endpoint modtager timetable-data fra det eksterne planlægningssystem.
- Data skrives i DVM timetable-tabeller.
- En Job Queue Entry kører efter plan og behandler importerede timetables samt opdaterer aktive legs.

Kontakt din partner eller systemintegrator for tekniske detaljer om API-integrationen.

## 3. Håndtering af Rute og Strækning under drift

### 3.1 Fastlæggelse af aktivt Strækning

Det aktive leg er det leg, som DVM bruger til at beregne VAT på et givent tidspunkt.

Aktivt leg kan bestemmes på en af følgende måder:

- Fra timetable.
- Fra et external system.
- Manuelt fra POS.

### 3.2 Automatisk VAT-skift fra timetable

Hvis timetables bruges, kontrollerer DVM periodisk timetable for hvert vessel og aktiverer det leg, der matcher aktuel dato og tid.

Når aktivt leg ændres, gør DVM følgende:

- Opdaterer aktivt leg for vessel.
- Opretter en Job Queue Entry, som sikrer, at alle POS terminals modtager den nye VAT-indstilling.
- Bruger VAT Store Group per Strækning setup til at bestemme korrekt VAT Business Posting Group.

### 3.3 Eksempel

Eksempel:

**Skib FERRY01** sejler fra Port A til Port B med følgende legs:

- Strækning 10 - Port A -> International waters (domestic VAT).
- Strækning 20 - International waters -> Port B (foreign VAT).

Når timetable angiver, at vessel er gået ind i Strækning 20, ændrer DVM automatisk aktivt leg til 20, og alle efterfølgende POS-salg bogføres med VAT Business Posting Group konfigureret for Strækning 20.

## 4. External Integration (API)

### 4.1 Oversigt

DVM kan integreres med eksterne systemer (fx trafik- eller planlægningssystemer), som styrer aktuel route og leg.

Det eksterne system kan sende leg-ændringer til DVM i stedet for udelukkende at basere sig på timetables.

### 4.2 Typisk integrationsflow

1. External system fastslår, at et vessel har skiftet leg.
2. Det kalder en DVM API/web service med vessel, route og leg.
3. DVM validerer data og skriver en leg-change request.
4. Job Queue behandler request og opdaterer aktivt leg.
5. POS terminals modtager den nye VAT-opsætning.

### 4.3 Fejlhåndtering

Hvis external system sender ukendt vessel, route eller leg, afvises request og logges.

Integrationsovervågning bør kontrollere fejlede leg-change requests og rette masterdata eller payload.

## 5. POS-kommandoer

### 5.1 Manuel leg-ændring (DVM_CHNGLEG)

Brugere kan ændre aktivt leg manuelt direkte fra POS.

Opsætning i LS Central POS:

1. Åbn siden **POS Functions / Commands**.
2. Opret eller find kommandoen **DVM_CHNGLEG**.
3. Tilføj en POS-knap og tildel kommandoen.
4. Udrul layout til relevante POS terminals.

Brug:

- Kassemedarbejderen trykker på knappen DVM_CHNGLEG.
- En dialog åbner med tilgængelige legs for vessel.
- Kassemedarbejderen vælger ønsket leg.
- DVM opretter et leg-change job.
- Job Queue behandler request.
- Aktiv VAT-opsætning opdateres.

### 5.2 Andre commands (valgfrit)

Afhængigt af konfigurationen kan yderligere commands gøres tilgængelige:

- Vis aktuelt aktivt leg på POS.
- Opdater VAT-konfiguration fra back end.

Disse commands er valgfri og kan variere pr. implementering.

## 6. Overvågning og fejlfinding

### 6.1 Job Queue-overvågning

DVM er afhængig af Business Central Job Queue til behandling af timetable-importer, leg-ændringer og VAT-opdateringer.

Hvis VAT ikke ændres som forventet:

- Åbn **Job Queue Entries** og verificer, at DVM jobs er aktiverede og kører.
- Kontroller **Job Queue Log Entries** for fejl.
- Ret eventuelle datafejl og genstart jobbet.

### 6.2 Typiske problemer

#### VAT ændrede sig ikke, da vessel gik ind i et nyt leg

- Kontroller at timetable indeholder en linje for vessel, route og tidspunkt.
- Verificer at leg har en VAT Store Group defineret.
- Bekræft at Job Queue Entry blev udført korrekt.

#### POS viser forkert VAT-sats

- Kontroller hvilket leg der aktuelt er aktivt.
- Verificer store-mapping i VAT Store Group per Strækning.
- Opdater POS eller genstart sessionen.

#### Manuel leg-ændring fra POS har ingen effekt

- Bekræft at DVM_CHNGLEG command er konfigureret korrekt.
- Kontroller Job Queue for fejlede leg-change jobs.
- Sikr at valgt leg eksisterer og er aktivt.

### 6.3 Supportoplysninger

Hvis problemet ikke kan løses, så vedlæg følgende i en supportsag:

- Miljø (TEST/PROD) og Business Central / LS Central version.
- DVM-version.
- Involveret vessel, route og leg.
- POS-dokumentnummer og timestamp.
- Skærmbilleder af Job Queue Entries og fejlmeddelelser.

---

# Billedliste

Ingen billeder blev fundet i den leverede HTML.

Tilføj billedreferencer her, når de er tilgængelige:

| Placeholder | File Name | Insert Position |
|------------|-----------|-----------------|
| IMG-01 | | |
| IMG-02 | | |
| IMG-03 | | |
| IMG-04 | | |
```
