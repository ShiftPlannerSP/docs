# ShiftPlanner – Initiale Produktidee

*Software-Projekt WS 2026/27, Hochschule Flensburg*

## Das Problem

In vielen kleinen Betrieben und Filialen wird die Schichtplanung noch von Hand erledigt – in Excel, auf Papier oder über WhatsApp-Gruppen. Führungskräfte verbringen jeden Monat mehrere Stunden damit, Verfügbarkeiten einzusammeln, Vertragsstunden und gesetzliche Vorgaben zu berücksichtigen und Konflikte aufzulösen. Mitarbeitende empfinden das Ergebnis oft als willkürlich: Scheinbar bekommen immer dieselben Personen die guten Schichten, und niemand kann erklären, warum. Bestehende Tools wie Papershift oder Planday sind für dieses Segment häufig zu komplex oder zu teuer und machen Fairness nur selten transparent.

## Zielgruppe

ShiftPlanner richtet sich an Betriebe mit **5–30 Mitarbeitenden**, die ihre Schichten manuell planen. Dazu gehören unabhängige Kleinbetriebe (Cafés, Bäckereien, kleiner Einzelhandel) ebenso wie Filialen großer Unternehmen, die vor Ort noch von Hand planen. Letzteres hat das Team in Ketten wie Hugo Boss, Deichmann und Netto selbst erlebt. Nutzende sind Mitarbeitende und Filialleitungen. Ob eine Filialleitung in einer Kette das Tool selbst auswählen darf, ist noch offen und wird in Interviews überprüft.

## Wertversprechen

**Faire, nachvollziehbare und automatische Schichtplanung.** ShiftPlanner erstellt den monatlichen Schichtplan automatisch aus den Präferenzen der Mitarbeitenden, dem Personalbedarf des Betriebs und den gesetzlichen Regeln. Dabei gleicht das System Fairness über mehrere Monate hinweg aus und kann jede Zuteilung begründen. Die Arbeit der Führungskraft schrumpft von stundenlangem Puzzeln auf das Prüfen eines Entwurfs und wenige zentrale Entscheidungen.

## Rollen

- **Mitarbeitende:** geben Verfügbarkeiten und Prioritäten ein und sehen den veröffentlichten Plan.
- **Planende (Chef/Filialleitung):** erfassen Bedarf und Betriebsdaten, prüfen und korrigieren den erzeugten Plan.
- **Freigebende:** genehmigen und veröffentlichen den Plan. Je nach Organisation ist das die planende Person selbst oder eine vorgesetzte Stelle.

## Ablauf eines Planungsmonats

Geplant wird im laufenden Monat für den Folgemonat, in drei Stufen:

1. **Bis Ende Woche 1** schließt der Chef seine Eingaben ab: Bedarf, Urlaube und eventuelle Änderungen an Vorlagen.
2. **Bis Ende Woche 2** schließen die Mitarbeitenden ihre Verfügbarkeiten und Prioritäten ab.
3. **Zu Beginn von Woche 3** erzeugt das System den Plan. Der Chef prüft ihn, nimmt bei Bedarf manuelle Korrekturen vor, und der Plan wird freigegeben und veröffentlicht.

So haben Mitarbeitende rund zwei Wochen Vorlauf vor Beginn des neuen Monats.

## Funktionen

**Eingaben des Chefs** – die meisten werden einmalig eingerichtet und nur bei Bedarf angepasst:

- Schichtvorlagen (Tage, Beginn- und Endzeiten)
- Benötigte Anzahl an Personen pro Schicht
- Erforderliche Qualifikationen oder Rollen pro Schicht (z. B. Schlüsselträger, Kasse, geschultes Personal)
- Besetzungsregeln wie Mindestbesetzung und gewünschte Zusammensetzung (z. B. nie zwei Auszubildende allein)
- Arbeitsverträge nach Typ (Vollzeit, Teilzeit, Werkstudent, Minijob) mit jeweils einem Minimal-/Maximalstundenrahmen und individuellen Anpassungen
- Umgang mit Feiertagen je Betrieb (geschlossen oder geöffnet mit Sondervorlage)
- Genehmigte Urlaubstage, monatlich erfasst
- Bedarf auf Basis betrieblicher Statistiken oder der Erfahrung des Chefs – die App selbst erstellt keine Prognosen

**Eingaben der Mitarbeitenden:**

- Verfügbarkeitszeiträume für den Folgemonat
- Eine Priorität für jeden Verfügbarkeitszeitraum: **1** ist am besten geeignet, **2** mittel, **3** noch akzeptabel, aber am wenigsten bevorzugt
- Zeiten ohne eingetragenen Zeitraum gelten als nicht verfügbar

**Automatische Planungskomponente:**

- Sie hält harte Regeln ein, die nie verletzt werden:
  - maximale Arbeitszeit pro Tag und Woche
  - Mindestruhezeit zwischen Schichten
  - vertraglicher Stundenrahmen
  - keine Schichten an Urlaubstagen
  - gesetzliche Feiertage in Schleswig-Holstein
  - Personenzahl und Qualifikationen pro Schicht
- Sie maximiert die Zufriedenheit der Mitarbeitenden, indem erfüllte Prioritäten gewichtet werden (1 > 2 > 3).
- Sie löst Konflikte fair: Wünschen sich mehrere Personen dieselbe Schicht, entscheidet die Fairness-Historie.
- Sie lässt Lücken nie stillschweigend offen. Kann eine Schicht nicht besetzt werden, erzeugt das System trotzdem den bestmöglichen Plan und meldet die Lücke dem Chef zur manuellen Entscheidung.

**Fairness-Historie:**

- Ab dem Tag der Einführung wird jeder veröffentlichte Plan gespeichert.
- Aus dieser Historie berechnet das System pro Person einen Fairness-Wert, z. B. den Anteil erfüllter Prioritäts-1-Wünsche.
- Wer zuletzt benachteiligt wurde, erhält beim nächsten Mal Vorrang, sodass niemand systematisch bevorzugt wird.

**Nachvollziehbarkeit:**

- Mitarbeitende können einsehen, warum sie eine Schicht bekommen haben oder nicht, z. B. „Die Schicht ging an eine Kollegin, deren Erfüllungsquote niedriger war als deine.“
- Die Erklärungen stammen aus den eigenen Daten von ShiftPlanner (Prioritäten, Fairness-Werte, Regeln), nicht aus dem Inneren des Solvers.

**Prüfung, Freigabe und Veröffentlichung:**

- Der Chef kann den erzeugten Plan manuell anpassen.
- Ein optionaler Freigabeschritt ermöglicht die Genehmigung und Veröffentlichung durch eine vorgesetzte Stelle. Der Plan durchläuft die Status Entwurf → in Freigabe → veröffentlicht.
- Der veröffentlichte Plan ist für alle Mitarbeitenden in der App sichtbar.

**Datenschutz (DSGVO):**

- Verfügbarkeiten und Prioritäten sind nur für die jeweilige Person und den Chef sichtbar.
- Kolleginnen und Kollegen sehen ausschließlich den veröffentlichten Plan.

## Technische Ausrichtung

Geplant ist eine Webanwendung in Python mit Schichtenarchitektur. Die Planungslogik liegt in einer eigenen Komponente, bevorzugt auf Basis von **Google OR-Tools (CP-SAT)** – einem Constraint-Solver, bei dem das Team Regeln und Ziele beschreibt und die Bibliothek den besten Plan sucht. Als Rückfallebene dient ein einfacherer Greedy-Algorithmus. Eine separate Erklärungskomponente erzeugt die Fairness-Begründungen. Dieser modulare Aufbau macht die Kernlogik zudem gut testbar.

## Nicht Teil des MVP

Diese Punkte sind bewusst ausgeklammert, werden in Interviews aber trotzdem abgefragt und im Backlog geführt:

- Krankmeldungen, Schichttausch und kurzfristige Lücken nach der Veröffentlichung
- Bedarfsprognosen
- Gesetzliche Regeln über den MVP-Umfang hinaus (z. B. Jugendarbeitsschutz, Tarifverträge)
- Urlaubsanträge durch Mitarbeitende

## Noch offen

Das Produktkonzept ist eine fundierte Ausgangsposition, beruht aber auf Annahmen, die echtes Feedback von Kundinnen, Kunden und Fachleuten überprüfen und verändern wird. Folgende Punkte sind bis P2 zu klären:

- Die genaue Fairness-Kennzahl: Zeitfenster und Startwert für neue Mitarbeitende
- Die Regel für verspätete Eingaben
- MoSCoW-Priorisierung der Funktionen
- Ein kleiner Solver-Prototyp zur Bestätigung der Machbarkeit
