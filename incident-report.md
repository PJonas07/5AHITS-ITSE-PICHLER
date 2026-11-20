---
title: "Incident-Report: AdFection – Zero-Click-Spyware über Online-Werbung bei Al Jazeera"
---

# Incident-Report: AdFection – Zero-Click-Spyware über Online-Werbung bei Al Jazeera

| | |
|---|---|
| **Namen** | [Name 1], [Name 2] |
| **Klasse** | [Klasse] |
| **Fach** | ITSE (Theorie) – Incident-Briefing |
| **Datum** | [Datum des Briefings] |
| **Thema der Präsentation** | Zero-Click Spyware |
| **KI-Unterstützung** | Recherche und Textentwurf mit Unterstützung von **Claude Opus 5.5** (Anthropic). Anteil der KI-Unterstützung: [vom Team ergänzen, z. B. „hoch – Entwurf durch KI, Prüfung und Überarbeitung durch das Team“] |

---

## 1. Zusammenfassung

Am 15. Oktober 2026 veröffentlichten das Citizen Lab der Universität Toronto und die Werbe-Sicherheitsfirma Confiant den Bericht „Exposing AdFection“. Er belegt, dass mindestens 45 Geräte des Nachrichtensenders Al Jazeera in den Jahren 2024 und 2025 mit der Spyware des israelischen Herstellers Candiru (heute Integrity Labs) angegriffen wurden – ausgeliefert über gezielt geschaltete Online-Werbung des israelischen Werbetechnik-Unternehmens Discover-Tech. Die Opfer mussten nichts anklicken: Schon das Laden einer Werbeanzeige reichte (Zero-Click). Es ist der erste öffentlich und technisch bestätigte Fall, in dem Söldner-Spyware über das reguläre Werbe-Ökosystem ausgeliefert wurde. Nach der Veröffentlichung sperrten Google, IAB Europe und weitere Werbeplattformen Discover-Tech.

## 2. Zeitablauf

| Zeitpunkt | Ereignis |
|---|---|
| 2014 | Gründung von Candiru in Israel |
| 2020 | Discover-Tech wird in Israel registriert; Aufnahme in die Anbieterlisten von Google und IAB Europe |
| Nov. 2021 | Die USA setzen Candiru auf die Entity List (Handelsbeschränkung) |
| 2024 | Beginn der Angriffe auf Al Jazeera. Das interne Sicherheitsteam von Al Jazeera bemerkt auffälliges Geräte- und Netzwerkverhalten und zieht Citizen Lab hinzu |
| Anfang 2025 | Candiru wird übernommen, die Technologie läuft seither unter dem Namen Integrity Labs |
| ab April 2025 | Discover-Tech-Anzeigen leiten Geräte zu einer Domain, die zusammen mit bekannten Candiru-Servern registriert wurde, später zu einer Amazon-CloudFront-Adresse |
| 2024–2025 | Mindestens 45 Al-Jazeera-Geräte werden angegriffen |
| Jan.–Juni 2026 | Citizen Lab identifiziert rund 3,6 Mio. „geeignete Nutzer“ im Targeting-System |
| März–Juni 2026 | Discover-Tech gibt rund 3,77 Mrd. Gebote in Werbeauktionen ab (analysierter Datensatz) |
| Anfang Okt. 2026 | Google und IAB Europe entfernen Discover-Tech aus ihren Anbieterverzeichnissen (IAB Europe am 8. Oktober) |
| 15. Okt. 2026 | Veröffentlichung der Berichte von Citizen Lab und Confiant; Al Jazeera verurteilt die Angriffe öffentlich |
| danach | Beeswax (Comcast) entfernt Discover-Tech, Liftoff pausiert den Zugang, Eskimi sperrt Konten und Domains |

## 3. Betroffene Organisation und Systeme

- **Organisation:** Al Jazeera Media Network, ein internationaler Nachrichtensender mit Sitz in Katar. Al Jazeera war schon 2020 Ziel von Spyware: Damals wurden 36 Handys von Mitarbeitern mit Pegasus (NSO Group) gehackt.
- **Systeme:** Mindestens 45 Geräte von Mitarbeitern. Das sichergestellte Implantat stammt von einem **Windows-Computer**; laut Tech Times handelte es sich bei den betroffenen Al-Jazeera-Geräten um Windows-Rechner. Andere Gerätetypen sind nicht belegt.
- **Weitere Ziele:** Anzeigen von Discover-Tech liefen auch in Netzen großer Firmen und Organisationen (u. a. NATO, Microsoft, Intel, Nvidia). Laut Bericht waren auch Mitarbeiter des saudischen Außenministeriums unter den Zielen. Das zeigt Reichweite, beweist aber keine weiteren Infektionen.
- **Wichtige Einschränkung:** Die Zahl 45 zählt **angegriffene** Geräte. Wie viele davon erfolgreich infiziert wurden, ist öffentlich nicht vollständig geklärt.

## 4. Angriffsvektor und ausgenutzte Schwachstelle

**Angriffsvektor: Programmatische Werbung (Real-Time-Bidding, RTB).** Bei jedem Aufruf einer Website oder App mit Werbung wird in Millisekunden eine Auktion durchgeführt. Dabei gehen Daten wie Werbe-ID des Geräts, IP-Adresse und Standort an viele Bieter. Genau diese Daten wurden zur Zielauswahl missbraucht.

**Ablauf des Angriffs:**

1. **Werbeauktion:** Discover-Tech bietet gezielt auf die Geräte der Zielpersonen und gewinnt die Auktion.
2. **Anzeige lädt:** Der Browser lädt die Anzeige und meldet sich beim „NOAS“-Server (getAd-Server) von Discover-Tech.
3. **Zielprüfung:** Der Server vergleicht Werbe-ID, Cookie-Hash und IP-Adresse mit einer Zielliste, die in der Discover-Tech-Plattform **„Controlium“** verwaltet wird.
4. **Tarnung:** Nicht-Zielen liefert der Server **0 Byte** zurück. Deshalb sehen Sicherheitsscanner der Werbenetzwerke nur harmlose Anzeigen.
5. **Validierung:** Ein Candiru-Validierungsserver prüft das Gerät (ca. 1–2 Sekunden nach der Anzeige).
6. **Exploit:** Für bestätigte Ziele wird ein JavaScript-Exploit ausgeliefert (Kontakt zum Exploit-Server ca. 5–10 Sekunden nach der Anzeige).
7. **Implantat:** Installiert wird die Candiru-Spyware als Windows-DLL; nach ca. 1–3 Minuten besteht die Verbindung zum Command-and-Control-Server.

**Ausgenutzte Schwachstelle:** **Keine CVE-Nummer bekannt.** Citizen Lab konnte den eigentlichen Browser-Exploit nicht sicherstellen. Fest steht nur, dass ein JavaScript-Exploit im Browser ausgeführt wurde.

**Sichergestelltes Implantat (IOC):**

- Windows-DLL, SHA256 `255869de85e2a171993fc5eb8a556d873a1b8966e040f6f55926f2fa2d595cc8`
- nicht in öffentlichen Malware-Datenbanken gefunden, also vermutlich eigens für diese Kampagne gebaut
- fest einprogrammierte C2-Adressen, Persistenz über **COM-Hijacking**, Verschleierung von Zeichenketten mit 48-Byte-XOR
- Diese Merkmale verbindet Citizen Lab mit Candirus Spyware **„DevilsTongue“**, die 2021 dokumentiert wurde.

## 5. Täter und Motiv

| Rolle | Akteur |
|---|---|
| Spyware-Hersteller | **Candiru / Integrity Labs** (Israel), seit 2021 auf der US-Entity-List |
| Auslieferung über Werbung | **Discover-Tech** (Israel), Werbeplattform „Controlium“ |
| Weiterer Nutzer der Plattform | **Bindecy**, laut Citizen Lab ebenfalls ein Anbieter von Hacking-Werkzeugen |
| Auftraggeber | **Unbekannt.** Citizen Lab kann die Angriffe keinem bestimmten Regierungskunden zuordnen. |

**Motiv:** Candiru verkauft Spyware ausschließlich an Regierungen. Das wahrscheinliche Motiv ist daher **staatliche Überwachung und Spionage** gegen Journalisten, etwa um Quellen und Recherchen auszuspähen. Ein finanzielles Motiv (Erpressung, Datenverkauf) ist nicht erkennbar. Discover-Tech und Candiru haben auf Anfragen nicht reagiert.

## 6. Auswirkungen

- **Daten:** Spyware dieser Art kann Dateien, Kommunikation und Zugangsdaten eines Geräts abgreifen. Welche Daten konkret abgeflossen sind, ist öffentlich nicht bekannt.
- **Journalismus:** Gefahr für Quellenschutz und Pressefreiheit. Das Committee to Protect Journalists (CPJ) sieht dadurch die Überwachungsrisiken für Journalisten noch deutlicher bestätigt.
- **Werbe-Ökosystem:** Discover-Tech war sechs Jahre lang bei Google und IAB Europe gelistet, ohne dass der Missbrauch auffiel. Die Anzeigen erreichten 120.768 Websites und 75.373 Apps.
- **Datenleck bei Discover-Tech:** Die Plattform Controlium hatte einen ungeschützten CSV-Download mit 120 Login-Datensätzen, der 128 Tage lang offen war.
- **Finanziell:** Keine Schadenssumme veröffentlicht.
- **Betrieb:** Keine bekannten Ausfälle des Sendebetriebs. Spyware ist auf Unauffälligkeit ausgelegt, nicht auf Störung.

## 7. Reaktion und Behebung

- **Al Jazeera:** Das eigene Sicherheitsteam erkannte die Anomalien in den Netzwerk-Logs, startete eine interne Untersuchung und übergab die Logs an Citizen Lab. Öffentlich verurteilte Al Jazeera die Angriffe und kündigte an, Mitarbeiter und Systeme weiter zu schützen.
- **Citizen Lab und Confiant:** Forensische Analyse, Zuordnung zu Candiru mit hoher Sicherheit, Veröffentlichung am 15. Oktober 2026; Aufruf an Behörden, zu ermitteln.
- **Google:** Bestätigte die Beteiligung von Discover-Tech an der Spyware-Auslieferung und sperrte die Firma wegen systematischer Richtlinienverstöße von seinen Werbeplattformen.
- **IAB Europe:** Suspendierte Discover-Tech aus seiner Liste zugelassener Anbieter (TCF Global Vendor List).
- **Weitere Werbeplattformen:** Beeswax (Comcast) entfernte den Zugang, Liftoff pausierte ihn, Eskimi sperrte Konten und Domains. Yandex reagierte nicht auf Medienanfragen.
- **Behörden:** Bis zur Veröffentlichung keine bestätigten Ermittlungen.
- **Offen:** Es gibt keinen Patch, der „AdFection“ allgemein schließt. Die Sperren richten sich gegen einzelne Firmen; ähnliche Angriffe über andere Werbefirmen sind weiterhin denkbar.

## 8. Lessons Learned und Gegenmaßnahmen

**Lessons Learned**

1. **Werbung ist Angriffsfläche.** Jede Website und jede App mit Werbung kann ein Einfallstor sein, auch ohne Klick.
2. **Werbedaten ermöglichen gezielte Angriffe.** Werbe-ID, IP und Standort reichen, um einzelne Personen in Echtzeit auszuwählen.
3. **Scanner sehen nichts, wenn nur Ziele den Schadcode bekommen.** Die 0-Byte-Antwort an alle anderen umging die Kontrollen der Werbenetzwerke.
4. **Zertifizierungen schützen nicht.** Discover-Tech war jahrelang offiziell gelistet.
5. **Netzwerk-Monitoring wirkt.** Entdeckt wurde der Angriff, weil Al Jazeera auffällige Verbindungen in den eigenen Logs bemerkte.

**Gegenmaßnahmen**

| Ebene | Maßnahme |
|---|---|
| Unternehmen | Werbung und Tracking am **Netzwerk-Gateway blockieren** (DNS-Filter), besonders für gefährdete Mitarbeiter |
| Unternehmen | Netzwerk-Logs zentral sammeln und auf Verbindungen zu verdächtiger Infrastruktur überwachen (SIEM, Threat Intelligence, IOCs aus dem Bericht einspielen) |
| Unternehmen | EDR auf Endgeräten; auf Persistenzmechanismen wie COM-Hijacking achten |
| Endgerät | Werbeblocker im Browser, **Werbe-ID begrenzen oder zurücksetzen**, Browser und Betriebssystem sofort aktualisieren |
| Gefährdete Personen | Spezialisierte Hilfe holen (z. B. Citizen Lab, Access Now Helpline) |
| Werbebranche | Strengere Prüfung von Anbietern, Monitoring von auffälligem Zielgruppen-Targeting |
| Politik | Regulierung der Datenweitergabe beim Real-Time-Bidding, Ermittlungen gegen beteiligte Firmen |

## 9. Testfragen mit Antworten

**Frage 1:** Was ist „AdFection“ und warum gilt es als Zero-Click-Angriff?
**Antwort:** AdFection ist die Auslieferung von Spyware über gezielt geschaltete Online-Werbung. Es ist Zero-Click, weil schon das Laden der Anzeige im Browser oder in einer App reicht; das Opfer muss nichts anklicken oder herunterladen.

**Frage 2:** Warum konnten die Sicherheitsprüfungen der Werbenetzwerke den Angriff nicht erkennen?
**Antwort:** Der Server von Discover-Tech lieferte den Exploit nur an Geräte auf der Zielliste aus. Alle anderen, auch Sicherheitsscanner, bekamen eine leere Antwort (0 Byte) und sahen daher nur eine harmlose Anzeige.

**Frage 3:** Nenne zwei Gegenmaßnahmen gegen AdFection.
**Antwort:** Zum Beispiel: Werbung und Tracking am Netzwerk-Gateway blockieren; Werbe-ID des Geräts begrenzen oder zurücksetzen; Netzwerk-Logs überwachen; Browser und System aktuell halten.

---

## Quellen

- Citizen Lab: *How Discover-Tech turned targeted ads into a pipeline for spyware* („Exposing AdFection“), 15.10.2026 – https://citizenlab.ca/research/how-discover-tech-turned-targeted-ads-into-a-pipeline-for-spyware/
- Al Jazeera: *Israeli ad-tech firm helped place spyware on Al Jazeera devices*, 15.10.2026 – https://www.aljazeera.com/news/2026/10/15/israeli-ad-tech-firm-helped-spy-on-al-jazeera-devices
- Al Jazeera English (Video): https://www.youtube.com/watch?v=oTNqhH4KzxY
- Committee to Protect Journalists: *Report: Israeli ad-tech firm enables spyware targeting of at least 45 Al Jazeera devices*, Okt. 2026 – https://cpj.org/2026/10/report-israeli-ad-tech-firm-enables-spyware-targeting-of-at-least-45-al-jazeera-devices/
- Tech Times: *Candiru Spyware Hacked Al Jazeera Through Banner Ads With No User Click*, 16.10.2026 – https://www.techtimes.com/articles/329124/20261016/candiru-spyware-hacked-al-jazeera-through-banner-ads-no-user-click.htm
- Gridinsoft: *AdFection: Targeted Ads Delivered Candiru Spyware Without a Click* – https://blog.gridinsoft.com/adfection-targeted-ads-candiru-spyware/
- The Star / Reuters: *Israeli firms put spyware at Al Jazeera via ads, report says*, 16.10.2026 – https://www.thestar.com.my/tech/tech-news/2026/10/16/israeli-firms-put-spyware-at-al-jazeera-via-ads-report-says
- The Online Citizen: *Citizen Lab: Al Jazeera devices hit with Candiru spyware via targeted online ads*, 17.10.2026 – https://www.theonlinecitizen.com/2026/10/17/citizen-lab-al-jazeera-devices-hit-with-candiru-spyware-via-targeted-online-ads
- Cybernews: *Al Jazeera journalists targeted by spyware through online ads* – https://cybernews.com/news/al-jazeera-spyware/
- Cybermerge: *Citizen Lab: ads bought by Discover-Tech delivered zero-click Candiru spyware to Al Jazeera*, 16.10.2026 – https://cybermerge.com/stories/citizen-lab-adfection-candiru-zero-click-ads-al-jazeera
- The Hacker News: *iPhones of 36 Journalists Hacked Using Zero-Click iMessage Exploit* (Al Jazeera 2020) – https://thehackernews.com/2020/12/iphones-of-36-journalists-hacked-using.html
