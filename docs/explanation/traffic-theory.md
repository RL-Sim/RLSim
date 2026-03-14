# Traffic Rule Theory

Understanding real-world traffic rules and their variations across regions.

## Overview

RLSim implements traffic rules based on real-world regulations.
This guide provides links to official traffic rule documentation
and resources for different countries and regions.

For implementation details, see [Traffic Rule Specifications].

## Right-Hand Traffic (RHT) Countries

### 🇩🇪 Germany

**Official Traffic Rules:**

- [StVO (Straßenverkehrsordnung)] - German Road Traffic Regulations
- [StVZO (Straßenverkehrs-Zulassungs-Ordnung)] - Road Traffic Licensing Regulations

**Road Signs and Markings:**

- [Road signs in Germany] - Wikipedia reference with comprehensive sign documentation
- [German Traffic Signs] - Official ADAC reference guide

**Priority Rules:**

- Rechts vor Links (Right-before-Left) - Priority to vehicles from the right
- Official documentation in StVO Section 8

**Key Resources:**

- [ADAC - German Motoring Association] - Comprehensive traffic rules and regulations
- [Bundesanstalt für Straßenwesen (BASt)] - Federal Highway Research Institute

### 🇲🇩 Moldova

**Official Traffic Rules:**

- [Codul Rutier al Republicii Moldova] - Moldovan Road Code
- [Regulamentul de aplicare a Codului Rutier] - Road Code Implementation Regulations

**Priority Rules:**

- `Prioritate din dreapta` (Priority from the right) - Similar to German RHT rules
- Regulations in Chapter 3 of the Road Code

**Key Resources:**

- [Ministerul Afacerilor Interne - Poliția de Circulație] - Moldovan Traffic Police
- [Agenția Servicii Publice] - Public Services Agency

### 🇷🇴 Romania

**Official Traffic Rules:**

- [Codul Rutier Român] - Romanian Road Code
- [Norme de aplicare a Codului Rutier] - Road Code Implementation Norms

**Priority Rules:**

- Prioritate din dreapta (Priority from the right) - RHT priority rule
- Regulations in Title III of the Road Code

**Key Resources:**

- [Poliția Rutieră Română] - Romanian Traffic Police
- [Ministerul Transporturilor] - Ministry of Transport

### 🇷🇺 Russia

**Official Traffic Rules:**

- [Правила дорожного движения (ПДД)] - Russian Road Traffic Rules
- [ГОСТ Р 52289-2004] - GOST Standard for Road Signs and Markings

**Priority Rules:**

- Помеха справа (Obstacle from the right) - Priority to vehicles from the right
- Regulations in Section 13 of the ПДД

**Key Resources:**

- [ГИБДД (Госавтоинспекция)] - Russian State Traffic Safety Inspectorate
- [ГОСТ Standards] - Russian State Standards for traffic signs

## Left-Hand Traffic (LHT) Countries

### 🇬🇧 United Kingdom

**Official Traffic Rules:**

- [The Highway Code] - Official UK traffic rules and regulations
- [Traffic Regulation Orders] - Local traffic regulations

**Priority Rules:**

- Priority to the left at unregulated intersections
- Detailed in The Highway Code, Rules 170-183

**Key Resources:**

- [Department for Transport] - UK transport authority
- [DVSA (Driver and Vehicle Standards Agency)] - Official testing and standards

### 🇯🇵 Japan

**Official Traffic Rules:**

- [道路交通法 (Road Traffic Act)] - Japanese Road Traffic Law
- [道路標識、区画線及び道路標示に関する命令] - Road Signs and Markings Regulations

**Priority Rules:**

- 右側通行 (Right-side driving) with LHT priority rules
- Regulations in Chapter 2 of the Road Traffic Act

**Key Resources:**

- [National Police Agency] - Japanese traffic authority
- [Japan Automobile Federation (JAF)] - Comprehensive traffic information

### 🇮🇳 India

**Official Traffic Rules:**

- [Motor Vehicles Act, 1988] - Indian traffic law
- [Central Motor Vehicles Rules, 1989] - Implementation rules

**Priority Rules:**

- Left-hand traffic with priority rules
- Regulations in Part VIII of the Motor Vehicles Act

**Key Resources:**

- [Ministry of Road Transport and Highways] - Indian transport authority
- [Indian Road Safety Council] - Traffic safety information

### 🇦🇺 Australia

**Official Traffic Rules:**

- [Australian Road Rules] - National traffic rules
- [Road Safety Act 1986] - Victorian legislation (varies by state)

**Priority Rules:**

- Left-hand traffic with priority rules
- Detailed in the Australian Road Rules, Part 10

**Key Resources:**

- [Austroads] - Australasian road transport authority
- [National Road Safety Strategy] - Australian traffic safety framework

## Traffic Sign Standards

### International Standards

- [Vienna Convention on Road Signs and Signals] - International traffic sign standards
- [ISO 3864] - Graphical symbols and safety signs
- [ISO 39001] - Road traffic safety management systems

### Regional Standards

- [European Standard EN 12899] - Road signs and markings (EU)
- [MUTCD (Manual on Uniform Traffic Control Devices)] - USA standards
- [Australian Standard AS 1742] - Road signs and markings (Australia)

## Priority Rule Variations

### Right-Hand Traffic (RHT) Variations

**Standard RHT (Right-before-Left):**

- 🇩🇪 Germany, 🇲🇩 Moldova, 🇷🇴 Romania, 🇷🇺 Russia
- 🇫🇷 France, 🇪🇸 Spain, 🇮🇹 Italy, 🇵🇱 Poland

**Modified RHT Rules:**

- Some countries have specific exceptions for roundabouts
- Traffic light regulations override priority rules
- Yield signs create explicit priority changes

### Left-Hand Traffic (LHT) Variations

**Standard LHT (Left-before-Left):**

- 🇬🇧 UK, 🇯🇵 Japan, 🇮🇳 India, 🇦🇺 Australia
- 🇮🇩 Indonesia, 🇲🇾 Malaysia, 🇹🇭 Thailand

**Modified LHT Rules:**

- Roundabout priority varies by country
- Traffic light regulations override priority rules
- Yield signs create explicit priority changes

## Deadlock Scenarios

### Four-Way Stop Problem

When all four vehicles arrive simultaneously at an unregulated intersection:

- Each vehicle has someone on their priority side
- No vehicle has clear priority
- Resolution requires:

  - Mutual yielding
  - Eye contact and communication
  - Arbitrary decision-making

**References:**

- [Four-way stop] - Wikipedia article on deadlock scenarios
- Traffic safety manuals in each country address this scenario

### Roundabout Priority

Different countries have different roundabout priority rules:
- **Priority to vehicles in roundabout:** Most European countries
- **Priority to entering vehicles:** Some countries (varies)
- **Traffic lights override:** In regulated roundabouts

## Edge Cases and Special Situations

### Tram and Bus Priority

- Trams have special priority in many European countries
- Buses may have priority lanes
- Emergency vehicles override all priority rules

### Pedestrian and Bicycle Priority

- Pedestrian crossings create explicit priority
- Bicycle lanes have specific rules
- School zones have reduced speed limits

### Weather and Visibility

- Reduced visibility may affect priority rules
- Slippery roads require adjusted driving
- Night driving has specific regulations

## Related Documentation

- [Rendering Engine Choice] - Why SVG over Canvas
- [Internationalization Strategy] - Localization approach
- [Project Structure] - Directory layout and file descriptions
- [Architecture] - System design and data flow

<!-- Reference Links -->

[ADAC - German Motoring Association]: https://www.adac.de/
[Agenția Servicii Publice]: https://asp.gov.md/
[Architecture]: ../reference/architecture.md
[Australian Road Rules]: https://www.legislation.gov.au/
[Australian Standard AS 1742]: https://www.standards.org.au/
[Austroads]: https://www.austroads.com.au/
[Bundesanstalt für Straßenwesen (BASt)]: https://www.bast.de/
[Central Motor Vehicles Rules, 1989]: https://morth.gov.in/
[Codul Rutier al Republicii Moldova]: https://www.mai.gov.md/
[Codul Rutier Român]: https://www.politiarutiera.ro/
[Department for Transport]: https://www.gov.uk/government/organisations/department-for-transport
[DVSA (Driver and Vehicle Standards Agency)]: https://www.gov.uk/government/organisations/driver-and-vehicle-standards-agency
[European Standard EN 12899]: https://www.en-standard.eu/
[Four-way stop]: https://en.wikipedia.org/wiki/All-way_stop
[German Traffic Signs]: https://www.adac.de/verkehr/verkehrsregeln/verkehrszeichen/
[ГИБДД (Госавтоинспекция)]: https://www.gibdd.ru/
[ГОСТ Standards]: https://www.gost.ru/
[ГОСТ Р 52289-2004]: https://www.gost.ru/
[Internationalization Strategy]: ../explanation/internationalization.md
[ISO 3864]: https://www.iso.org/standard/34652.html
[ISO 39001]: https://www.iso.org/standard/44958.html
[Japan Automobile Federation (JAF)]: https://jaf.or.jp/
[Ministry of Road Transport and Highways]: https://morth.gov.in/
[Ministry of Transport]: https://mt.gov.ro/
[Motor Vehicles Act, 1988]: https://morth.gov.in/
[MUTCD (Manual on Uniform Traffic Control Devices)]: https://mutcd.fhwa.dot.gov/
[National Police Agency]: https://www.npa.go.jp/
[National Road Safety Strategy]: https://www.infrastructure.gov.au/
[Norme di applicazione del Codice della Strada]: https://www.poliziadistato.it/
[Poliția Rutieră Română]: https://www.politiarutiera.ro/
[Poliția de Circulație]: https://www.mai.gov.md/
[Правила дорожного движения (ПДД)]: https://www.gibdd.ru/
[Rendering Engine Choice]: ../explanation/rendering-choice.md
[Road Safety Act 1986]: https://www.legislation.vic.gov.au/
[Road signs in Germany]: https://en.wikipedia.org/wiki/Road_signs_in_Germany
[StVO (Straßenverkehrsordnung)]: https://www.gesetze-im-internet.de/stvo/
[StVZO (Straßenverkehrs-Zulassungs-Ordnung)]: https://www.gesetze-im-internet.de/stvzo_2013/
[The Highway Code]: https://www.gov.uk/guidance/the-highway-code
[Traffic Regulation Orders]: https://www.gov.uk/guidance/traffic-regulation-orders-guidance
[Traffic Rule Specifications]: ../reference/rules-logic.md
[Traffic Safety Council]: https://www.irsc.ie/
[Vienna Convention on Road Signs and Signals]: https://www.unece.org/trans/danger/publi/undoc/dotpdf/dotpdf70e.pdf
[道路交通法 (Road Traffic Act)]: https://elaws.e-gov.go.jp/
[道路標識、区画線及び道路標示に関する命令]: https://elaws.e-gov.go.jp/
