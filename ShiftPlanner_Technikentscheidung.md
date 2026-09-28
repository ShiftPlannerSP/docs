# ShiftPlanner – Technikentscheidung

## Kurzentscheidung

Für ShiftPlanner wird **Python mit Django**, **Google OR-Tools** und **PostgreSQL** empfohlen.

```text
Weboberfläche: Django Templates + HTML/CSS + wenig JavaScript
Backend:       Django / Python
Planung:       Python + Google OR-Tools (CP-SAT)
Datenbank:     PostgreSQL, optional über Supabase betrieben
```

Für den MVP sollte die Anwendung als ein zusammenhängendes Django-Projekt gebaut werden. Ein separates Frontend, mehrere Backends oder Microservices sind zunächst nicht nötig.

## Warum Django die beste Wahl ist

Die zentrale Besonderheit von ShiftPlanner ist nicht die reine Weboberfläche, sondern die automatische Schichtplanung. Sie muss Arbeitszeitregeln, Ruhezeiten, Qualifikationen, Verfügbarkeiten, Urlaube, Vertragsstunden und Fairness gleichzeitig berücksichtigen.

Python passt dafür besonders gut, weil Google OR-Tools mit dem CP-SAT-Solver sehr gut unterstützt wird. Die Planungslogik, Fairnessberechnung und Erklärungen können als klar getrennte und gut testbare Python-Module umgesetzt werden.

Django ergänzt das sinnvoll, weil es die Standardaufgaben einer Webanwendung bereits mitbringt:

- Benutzerkonten, Login und Passwortverwaltung
- Rollen und Rechte für Mitarbeitende, Planende und Freigebende
- Datenbankzugriff und Datenmodelle
- Formulare für Verfügbarkeiten, Urlaube und Schichtvorlagen
- Eine Administrationsoberfläche für Stammdaten
- Sicherheitsmechanismen und Datenvalidierung

Dadurch kann das Team sich auf die fachlich wichtige Planungslogik konzentrieren, statt viele Grundfunktionen selbst zu entwickeln.

## Empfohlene Umsetzung für den MVP

Die Oberfläche wird zunächst direkt von Django gerendert. Dafür reichen Django Templates, HTML, CSS und etwas JavaScript, beispielsweise für Kalender und interaktive Eingaben.

Diese Entscheidung hält das Projekt überschaubar:

- nur eine zentrale Anwendung statt Frontend- und Backend-Projekt
- nur eine Hauptsprache im Serverbereich: Python
- einfache lokale Entwicklung und Tests
- klare Trennung innerhalb des Projekts zwischen Oberfläche, Datenmodellen und Solver

React oder Next.js können später ergänzt werden, falls die Oberfläche deutlich komplexer wird. Für den ersten funktionierenden Schichtplan sind sie nicht notwendig.

## Datenbank: PostgreSQL und Supabase

**PostgreSQL** ist die passende Datenbank, weil die Daten stark miteinander verknüpft sind: Betriebe, Mitarbeitende, Verträge, Schichtvorlagen, Verfügbarkeiten, Schichten, Freigaben und Fairness-Historie.

**Supabase** ist eine gute Option, um PostgreSQL verwaltet zu betreiben. Es stellt eine vollständige PostgreSQL-Datenbank bereit und kann zusätzlich Funktionen wie Authentifizierung und Dateispeicher anbieten. Für den MVP sollte jedoch Django die zentrale Stelle für Anmeldung, Rollen und Geschäftsregeln bleiben. So sind Zugriffsrechte und der Freigabeprozess an einer Stelle nachvollziehbar.

Wichtig für den Datenschutz: Verfügbarkeiten und Prioritäten dürfen nur der betreffenden Person und berechtigten Planenden zugänglich sein. Rechte müssen daher sowohl in Django als auch – bei direktem Zugriff auf Supabase – zusätzlich über Datenbankregeln abgesichert werden.

## Warum SQLite nicht die Produktionsdatenbank sein sollte

SQLite eignet sich sehr gut für lokale Tests, einen Solver-Prototyp oder eine einzelne Demo-Installation. Es speichert die Daten in einer Datei und benötigt keinen separaten Datenbankserver.

Für die echte Webanwendung ist SQLite aber nicht die beste Wahl:

- Mehrere Mitarbeitende geben gleichzeitig Verfügbarkeiten ein.
- Die Anwendung braucht eine zentral erreichbare Datenbank.
- Rechte, Backups und der spätere Betrieb sind mit PostgreSQL besser handhabbar.

Deshalb: SQLite für Entwicklung oder Tests, PostgreSQL beziehungsweise Supabase für den laufenden Betrieb.

## Betrachtete Alternativen

| Variante | Vorteile | Nachteile | Bewertung für ShiftPlanner |
| --- | --- | --- | --- |
| **Python + Django + OR-Tools** | Solver, Web-App und Datenmodelle passen zusammen; viele Standardfunktionen vorhanden; eine zentrale Anwendung | Team muss Python und Django lernen | **Empfohlene Lösung** |
| **PHP + Laravel + Python-Solver** | Sehr gutes Web-Framework; sinnvoll bei viel Laravel-Erfahrung | Zwei Sprachen und die Kommunikation zwischen zwei Anwendungen erhöhen die Komplexität | Gute zweite Wahl |
| **Python + FastAPI + React** | Moderne API-Architektur; sehr flexibel | Login, Admin, Formulare und Frontend müssen stärker selbst zusammengesetzt werden | Gut, aber für den MVP unnötig aufwendig |
| **TypeScript + NestJS + Python-Solver** | Einheitliche Sprache im Browser und im Web-Backend; gute Struktur | Der Solver benötigt in der Praxis trotzdem Python; mehr Architekturaufwand | Möglich, aber nicht die einfachste Lösung |
| **Java/Kotlin + Spring Boot** | Sehr robust, gute Typisierung, für große Systeme geeignet | Hoher Einrichtungs- und Lernaufwand für ein Hochschul-MVP | Eher zu schwergewichtig |
| **C# + ASP.NET Core** | Professionelles Web-Framework und gute Werkzeuge | Lohnt sich vor allem bei vorhandener .NET-Erfahrung | Möglich, aber kein Vorteil gegenüber Django |
| **Go** | Schnell und schlank | Weniger passend für die Solver-Logik; meist wäre Python zusätzlich nötig | Nicht empfohlen |

## Grobe interne Struktur

```text
shiftplanner/
├── accounts/       Benutzer, Rollen und Rechte
├── organization/   Betrieb, Mitarbeitende und Verträge
├── scheduling/     Schichtvorlagen, Bedarf und Verfügbarkeiten
├── planner/        OR-Tools-Solver und Regelprüfung
├── fairness/       Historie, Kennzahlen und Begründungen
├── approvals/      Entwurf, Freigabe und Veröffentlichung
└── templates/      Weboberfläche
```

Die Module können unabhängig getestet werden. Insbesondere der Solver sollte viele automatische Tests erhalten, damit harte Regeln niemals versehentlich verletzt werden.

## Schlussfolgerung

Die beste Wahl für ShiftPlanner ist ein **Django-Monolith in Python mit PostgreSQL und OR-Tools**. Das hält den MVP einfach, unterstützt die schwierigste Funktion – die automatische, faire Planung – direkt und lässt genug Raum, die Anwendung später auszubauen.

PHP/Laravel bleibt eine sinnvolle Alternative, falls das Team bereits sicher mit Laravel arbeitet. Mit nur etwas PHP-Erfahrung und ohne feste Sprachvorgabe überwiegen bei diesem Produkt aber die Vorteile von Python und Django.
