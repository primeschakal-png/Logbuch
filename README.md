# Wartungsprotokoll · Logbuch & Tätigkeitsnachweis

Eine einzelne HTML-Datei, die drei Dinge in einem Werkzeug erledigt: Wartungen dokumentieren und planen, die eigene tägliche Tätigkeit nachweisen und betriebliche Vorkommnisse festhalten. Kein Server, keine Cloud, keine Installation. Die Datei wird im Browser geöffnet, die Daten liegen in einer JSON-Datei auf dem eigenen Rechner.

**Aktueller Stand:** v16 (`schemaVersion: 16`)

---

## Inhalt

1. [Wozu das Ganze](#wozu-das-ganze)
2. [Schnellstart](#schnellstart)
3. [Dateien im Betrieb](#dateien-im-betrieb)
4. [Die vier Datenarten](#die-vier-datenarten)
5. [Bedienung](#bedienung)
6. [Speichern, Backup und Crash-Schutz](#speichern-backup-und-crash-schutz)
7. [Handy und Rechner zusammenführen](#handy-und-rechner-zusammenführen)
8. [Kalender und ICS-Export](#kalender-und-ics-export)
9. [Datenformat](#datenformat)
10. [Migration alter Dateien](#migration-alter-dateien)
11. [Browser-Unterstützung](#browser-unterstützung)
12. [Sicherheit und Datenschutz](#sicherheit-und-datenschutz)
13. [Grenzen und Ideen](#grenzen-und-ideen)
14. [Lizenz](#lizenz)

---

## Wozu das Ganze

Das Werkzeug ist aus der Praxis in der Elektronikfertigung entstanden und deckt drei Fragen ab, die sonst auf Zetteln und in Erinnerungen verstreut sind:

* **Was steht an?** Jede Anlage hat ein Wartungsintervall. Die App rechnet aus dem Datum der letzten Wartung die nächste Fälligkeit aus und zeigt überfällig, heute fällig und planmäßig farbig an.
* **Was wurde gemacht?** Jede durchgeführte Wartung wird mit Fehlerbild, Maßnahme, Ersatzteilen und Facharbeiter erfasst. Dazu kommt die Tätigkeitserfassung: Wer neben der Facharbeit produktiv eingeplant wird, hat am Jahresende einen belastbaren Nachweis darüber, welche Art von Tätigkeit tatsächlich wie oft angefallen ist.
* **Was ist sonst passiert?** Vorkommnisse sind Dinge im Betrieb, die weder Wartung noch eigene Tätigkeit sind: Ausfälle, Fremdfirmeneinsätze, Auffälligkeiten. Sie können einen Messwert mit Einheit und Grenzwert tragen, damit sich ein Verlauf später auswerten lässt und nicht als Freitext verloren geht.

Bewusst nicht enthalten ist eine Soll-Ist-Auswertung. Die App sammelt und zeigt an, bewertet aber nicht.

---

## Schnellstart

1. `Logbuch.html` herunterladen und in einen eigenen Ordner legen.
2. Datei per Doppelklick im Browser öffnen. Empfohlen wird Chrome oder Edge, siehe [Browser-Unterstützung](#browser-unterstützung).
3. Beim ersten Start erscheint der Ladebildschirm. Es gibt noch keine Daten, deshalb den **Notfallstart** benutzen: sieben Mal auf das Schraubenschlüssel-Symbol 🔧 klicken. Die App startet mit leerem Datensatz.
4. Unter **Maschinen verwalten** die eigenen Anlagen anlegen. Das Feld ist mit einem Passwort geschützt, Standard ist `admin123`, siehe [Sicherheit](#sicherheit-und-datenschutz).
5. Auf **💾 Speichern** klicken und die JSON-Datei ablegen, zum Beispiel als `data.json` im selben Ordner.
6. Ab jetzt beim Start **📁 JSON laden** wählen. Der Browser bietet die zuletzt benutzte Datei direkt zum Öffnen an.

Alternativ lässt sich statt des Notfallstarts die mitgelieferte `data.example.json` laden, um die Struktur an einem Beispiel zu sehen.

---

## Dateien im Betrieb

Ein sinnvoller Arbeitsordner sieht so aus:

```
Wartungsprotokoll/
├── Logbuch.html          Die App
├── manual.html           Bedienungsanleitung, wird vom Button "📖 Anleitung" geöffnet
├── data.json             Die eigenen Daten (nicht ins Repository!)
└── Backups/              Zielordner für die Tagesbackups
```

`manual.html` ist nicht Teil dieses Repositories, weil sie den persönlichen Arbeitsstand beschreibt. Fehlt sie, öffnet der Anleitung-Button ins Leere, die App funktioniert davon unabhängig.

---

## Die vier Datenarten

### Maschinen

Der Stamm, auf den sich alles andere bezieht. Eine Maschine hat einen Namen, ein Wartungsintervall in Tagen, das Datum der letzten Wartung, eine Maßnahme oder ToDo-Beschreibung und optional eine Liste von Stationen.

Der Unterschied ist wichtig: Eine Anlage **mit** Stationen ist eine Linie, etwa VAG 2 mit Nutzentrenner, MPS, Verguss und EOL. Eine Anlage **ohne** Stationen ist ein Einzelplatz. Aus dieser Liste speisen sich alle Auswahlfelder. Anlagen werden ausschließlich hier angelegt, in den Erfassungsformularen kann nur ausgewählt und nichts frei eingetippt werden. Das hält die Schreibweisen sauber.

### Wartungen (`entries`)

Eine durchgeführte Arbeit an einer Anlage. Pflichtfelder sind Datum mit Uhrzeit, Maschine und Fehlerbild. Maßnahme und Ersatzteile sind Mehrfachfelder, es lassen sich also mehrere Einträge pro Datensatz setzen. Das Modul spielt hier keine Rolle, denn bei einer Wartung zählt die Anlage.

Achtung: Ein neuer Eintrag verschiebt **nicht** automatisch die Fälligkeit. Das Datum der letzten Wartung wird nur über den Button in der Fälligkeitskarte oder von Hand im Maschinen-Editor gesetzt. So lassen sich Störungsbeseitigungen dokumentieren, ohne dass der Wartungszyklus davon durcheinandergerät.

### Tätigkeiten (`activities`)

Der Tätigkeitsnachweis. Ein Datum, eine oder mehrere Tätigkeiten und optional eine Dauer. Entscheidend ist der Schalter **Produktive Schicht**:

* **Aus** heißt Facharbeitertätigkeit. Bezugspunkt ist die Anlage, gegebenenfalls mit Station. Eine Dauer in Stunden kann angegeben werden, etwa `3,5`.
* **An** heißt produktive Schicht über den ganzen Tag. Bezugspunkt ist das Modul oder die IPN, nicht die Anlage. Solche Einträge liefern bewusst **keine** Stunden und werden nicht aufsummiert, sonst stünde neben einer geplanten Schicht noch die Einzelzeit desselben Tages.

Wer stattdessen direkt `Schicht`, `Schichtplanung`, `ganzer Tag` oder eine ähnliche Variante ins Dauerfeld tippt, erhält dasselbe Verhalten.

### Vorkommnisse (`incidents`)

Datum, Anlage und Beschreibung sind Pflicht. Dazu kommen optional ein Messwert mit Einheit und Grenzwert sowie ein Status aus `offen`, `gemeldet` und `erledigt` mit dem Vermerk, an wen gemeldet wurde. Messwerte gehören ins Zahlenfeld und nicht in den Freitext, nur dann lässt sich später ein Verlauf wie 61 / 71 / 62 Prozent nachvollziehen.

---

## Bedienung

### Kopfzeile

| Button | Funktion |
| --- | --- |
| 📁 JSON laden | Datendatei öffnen. Mit File System Access API wird die Datei später direkt überschrieben. |
| 💾 Speichern | Schreibt in dieselbe Datei zurück. Ohne Handle erscheint "Speichern unter" oder ein Download. |
| 🗄️ Backup | Legt ein Tagesbackup im gewählten Backup-Ordner ab. Rechtsklick setzt den Ordner zurück. |
| 🔀 Zusammenführen | Führt eine zweite JSON-Datei mit dem aktuellen Stand zusammen. |
| 📆 ICS Export | Exportiert geplante Wartungen als Kalenderdatei. |
| 📖 Anleitung | Öffnet `manual.html` im neuen Tab. |
| 🌙 / ☀️ | Schaltet zwischen hellem und dunklem Layout um, die Wahl bleibt gespeichert. |

Der Speichern-Button färbt sich, sobald ungespeicherte Änderungen vorliegen. Der Dateiname daneben zeigt, welche Datei gerade offen ist.

### Statusleiste

Vier Zähler auf einen Blick: überfällig, heute fällig, planmäßig und die Gesamtzahl der Einträge.

### Fällige Wartungen

Karten je Anlage mit Restlaufzeit und Ampelfarbe. Über die Karte lässt sich die letzte Wartung auf heute setzen oder auf ein anderes Datum korrigieren. Der Abschnitt lässt sich ein- und ausklappen.

### Kalender

Monatsansicht auf Basis von FullCalendar mit vier Ebenen, die einzeln ein- und ausgeblendet werden können: geplante Wartungen, erledigte Wartungen, Tätigkeiten und Vorkommnisse. Ein Klick auf einen Termin öffnet die Details. Ein geplanter Termin lässt sich von dort direkt als erledigt abschließen.

### Bearbeiten statt neu anlegen

Jeder Datensatz in den drei Tabellen hat einen Bearbeiten-Button. Der Eintrag wird ins Formular zurückgeladen, die ID bleibt erhalten und der Datensatz wird an Ort und Stelle ersetzt. Abbrechen verwirft die Bearbeitung. Das ist wichtig für die Zusammenführung, weil eine erhaltene ID einen geänderten Datensatz als denselben erkennbar macht.

### Autovervollständigung

Fehlerbilder, Maßnahmen, Ersatzteile, Facharbeiter, Module und Tätigkeiten werden über Tom Select angeboten und aus den bereits erfassten Daten gespeist. Neue Werte lassen sich frei eintippen und stehen ab dann zur Auswahl. Ausgenommen sind die Anlagenfelder, die nur auswählen lassen.

---

## Speichern, Backup und Crash-Schutz

Die App kennt drei Wege, Daten festzuhalten:

**Direktes Überschreiben.** In Chromium-Browsern merkt sich die App den Dateizugriff über die File System Access API. Speichern schreibt dann ohne Dialog in dieselbe Datei zurück. Der Zugriff wird in IndexedDB gemerkt, sodass die zuletzt benutzte Datei beim nächsten Start per Klick wieder geöffnet werden kann. Der Klick ist nötig, weil der Browser die Berechtigung nur aus einer Nutzergeste heraus erneuert.

**Download-Fallback.** Firefox und Safari kennen die API nicht. Dort erzeugt Speichern eine Datei im Download-Ordner mit dem Namensmuster `wartungsdaten_JJJJ-MM-TT.json`, die anschließend selbst an den richtigen Platz gelegt werden muss.

**Stiller Crash-Schutz.** Jede Änderung wird zeitversetzt zusätzlich in IndexedDB abgelegt. Stürzt der Browser ab oder wird das Tab versehentlich geschlossen, bietet der Ladebildschirm beim nächsten Start an, diesen Stand wiederherzustellen. Nach erfolgreichem Speichern wird die Zwischenablage gelöscht.

Davon getrennt liegt das **Tagesbackup**. Beim ersten Klick auf Backup wird ein Zielordner ausgewählt und gemerkt. Jeder weitere Klick legt dort `wartungsdaten_backup_JJJJ-MM-TT.json` ab, ohne den Hauptdateizugriff anzutasten. Ein Rechtsklick auf den Backup-Button setzt den gemerkten Ordner zurück.

---

## Handy und Rechner zusammenführen

Der übliche Ablauf: unterwegs am Telefon kleine Tätigkeiten erfassen, am Rechner alles in die Hauptdatei übernehmen, ohne den dortigen Stand zu verlieren.

1. Am Rechner die Hauptdatei laden.
2. **🔀 Zusammenführen** klicken und die vom Telefon mitgebrachte JSON-Datei wählen.
3. Die App vergleicht beide Stände und zeigt einen Plan an: was neu hinzukommt, was identisch ist und wo derselbe Datensatz auf beiden Seiten unterschiedlich aussieht.
4. Bei Konflikten wird je Datensatz entschieden, welche Fassung gewinnt.
5. Übernehmen und speichern.

Grundlage ist die stabile ID je Datensatz. Datensätze ohne ID, etwa aus sehr alten Dateien, werden über ihren Inhalt verglichen.

---

## Kalender und ICS-Export

Der Export erzeugt `wartungszyklen.ics` und lässt sich in Outlook, Google Kalender oder jede andere Kalender-App importieren. Damit tauchen die Fälligkeiten dort auf, wo ohnehin täglich hingeschaut wird.

Pro Anlage mit gesetztem Intervall entsteht ein Serientermin als ganztägiges Ereignis. Er beginnt am nächsten fälligen Datum und wiederholt sich im Rhythmus des Wartungsintervalls. Die Beschreibung enthält Intervall, Maßnahme und das Datum der letzten Wartung. Tage, an denen bereits eine Wartung dieser Anlage erfasst ist, werden als Ausnahme aus der Serie herausgenommen.

Anlagen ohne Intervall oder ohne berechenbaren nächsten Termin bleiben außen vor. Gibt es davon keine einzige, meldet die App das und exportiert nichts.

---

## Datenformat

Gespeichert wird eine einzelne JSON-Datei. Die Reihenfolge der Schlüssel entspricht der Ausgabe der App:

```json
{
  "schemaVersion": 16,
  "exportedAt": "2026-09-03T06:12:44.000Z",
  "entries": [],
  "machines": [],
  "activities": [],
  "incidents": []
}
```

### `machines`

| Feld | Typ | Bedeutung |
| --- | --- | --- |
| `id` | String | Stabile ID, per `crypto.randomUUID()` erzeugt |
| `name` | String | Anlagenname, eindeutig |
| `cycleDays` | Zahl | Wartungsintervall in Tagen, 0 heißt kein Zyklus |
| `lastService` | String \| null | Datum der letzten Wartung als `JJJJ-MM-TT` |
| `task` | String | Maßnahme oder ToDo zur Wartung |
| `stations` | String[] | Stationen der Linie, leer heißt Einzelplatz |

### `entries`

| Feld | Typ | Bedeutung |
| --- | --- | --- |
| `id` | String | Stabile ID |
| `datetime` | String | Zeitpunkt als `JJJJ-MM-TTThh:mm` |
| `machine` | String | Anlagenname aus `machines` |
| `station` | String \| null | Station innerhalb der Anlage |
| `issue` | String | Fehlerbild, Pflichtfeld |
| `action` | String[] | Durchgeführte Maßnahmen |
| `parts` | String[] | Verbaute Ersatzteile |
| `technician` | String | Ausführender Facharbeiter |

### `activities`

| Feld | Typ | Bedeutung |
| --- | --- | --- |
| `id` | String | Stabile ID |
| `date` | String | Datum als `JJJJ-MM-TT` |
| `module` | String | Modul oder IPN, nur bei `shift: true` gefüllt |
| `machine` | String \| null | Anlage, nur bei `shift: false` gefüllt |
| `station` | String \| null | Station, nur bei `shift: false` gefüllt |
| `task` | String[] | Eine oder mehrere Tätigkeiten |
| `duration` | Zahl \| null | Dauer in Stunden, bei einer Schicht immer `null` |
| `shift` | Boolean | Produktive Schicht über den ganzen Tag |

### `incidents`

| Feld | Typ | Bedeutung |
| --- | --- | --- |
| `id` | String | Stabile ID |
| `date` | String | Datum als `JJJJ-MM-TT` |
| `machine` | String | Anlage, Pflichtfeld |
| `station` | String \| null | Station innerhalb der Anlage |
| `text` | String | Beschreibung des Vorkommnisses |
| `value` | Zahl \| null | Messwert |
| `unit` | String \| null | Einheit, etwa `%`, `°C`, `bar` |
| `limit` | Zahl \| null | Grenzwert für den Messwert |
| `status` | String | `offen`, `gemeldet` oder `erledigt` |
| `reportedTo` | String \| null | An wen gemeldet wurde |

Ein vollständiges Beispiel mit befüllten Datensätzen liegt als [`data.example.json`](data.example.json) bei.

---

## Migration alter Dateien

Beim Laden wird jede Datei normalisiert, bevor sie in die App geht. Das betrifft vor allem den Sprung von v15 auf v16:

* Anlage und Station standen früher gemischt in einem Feld, mal als `"Verguss VAG 2"`, mal als `"Verguss,VAG 2"`, mal als Liste. Beides wird jetzt getrennt, wobei Anlagennamen Vorrang vor Stationsnamen haben.
* `action` war ein einzelner Text und ist jetzt eine Liste.
* Messwerte, die früher im Freitext eines Vorkommnisses standen, werden nach Möglichkeit in Wert, Einheit und Grenzwert überführt.
* Fehlende Abschnitte wie `activities` aus v10 oder `incidents` aus v12 werden als leere Listen ergänzt.
* Datensätze ohne ID bekommen eine.

Die Migration verändert die Datei auf der Platte nicht. Sie greift beim Laden und wird erst mit dem nächsten Speichern festgeschrieben. Wer auf Nummer sicher gehen will, legt vorher eine Kopie an.

---

## Browser-Unterstützung

| Browser | Laden und Speichern | Direktes Überschreiben | Tagesbackup in Ordner |
| --- | --- | --- | --- |
| Chrome, Edge (Desktop) | ja | ja | ja |
| Firefox | ja | nein, Download | nein, Download |
| Safari | ja | nein, Download | nein, Download |
| Mobile Browser | ja | eingeschränkt | nein |

Der Aufruf über `file://` funktioniert. Die File System Access API ist dort je nach Browser und Einstellung eingeschränkt, dann greift automatisch der Download-Fallback. Die Schriften werden von Google Fonts geladen, ohne Internetverbindung nimmt der Browser eine Systemschrift. Alle übrigen Bibliotheken, also Tom Select und FullCalendar, sind fest in die Datei eingebettet.

---

## Sicherheit und Datenschutz

**Das Passwort ist keine Sicherheitsfunktion.** `admin123` steht im Klartext im Quelltext und schützt lediglich davor, den Maschinenstamm versehentlich zu verändern. Wer es ändern will, sucht im Quelltext nach `CORRECT_PASSWORD`. Wer echten Schutz braucht, ist mit einer verschlüsselten Festplatte besser bedient.

**Keine echten Daten ins Repository.** Die `.gitignore` schließt `data.json` und alle `wartungsdaten*.json` bereits aus. Diese Dateien enthalten Anlagennamen, Modulbezeichnungen, Kundenbezüge und Namen von Kollegen. Sie gehören in kein öffentliches Repository. Bei einem versehentlichen Commit reicht es nicht, die Datei im nächsten Commit zu löschen, dann muss die Historie umgeschrieben werden.

**Die Daten bleiben lokal.** Es gibt keinen Server, keine Telemetrie und keinen Cloud-Sync. Der einzige ausgehende Zugriff ist der Schriftabruf bei Google Fonts.

---

## Grenzen und Ideen

Bekannte Grenzen des aktuellen Standes:

* Keine Auswertung und keine Druckansicht in der App selbst.
* Kein automatischer Abgleich zwischen Geräten, das Zusammenführen ist ein bewusster manueller Schritt.
* Der Kalender zeigt Monatsansicht, keine Wochen- oder Listenansicht.
* Das Löschen einer Maschine löst keine Prüfung aus, ob noch Datensätze darauf verweisen.

Geplant beziehungsweise angedacht:

* Ein getrenntes Auswertungswerkzeug, das die JSON einliest und Ausdrucke erzeugt, wahlweise mit oder ohne Anmerkungen und Tätigkeiten.
* Filterbare Jahresübersicht der Tätigkeiten nach Art und Häufigkeit.

---

## Lizenz

© 2024–2026 Torsten Schuchardt. Alle Rechte vorbehalten. Details in [LICENSE](LICENSE).

Der Quelltext ist einsehbar, aber nicht zur Weiterverwendung freigegeben. Wer das Werkzeug einsetzen oder anpassen möchte, fragt vorher an.

Verwendete Fremdbibliotheken, jeweils unter eigener Lizenz und in die HTML-Datei eingebettet:

* [Tom Select](https://tom-select.js.org/) (Apache-2.0)
* [FullCalendar](https://fullcalendar.io/) Core und DayGrid (MIT)
