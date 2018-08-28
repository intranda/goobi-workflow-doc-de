# 6.1.1. Globale Verzeichnisstruktur

Da Goobi als Webapplikation in Betrieb genommen wird, verfügt es selbst über einen vorgegebenen Aufbau und befindet sich abhängig vom verwendeten Servlet Container an einem definierten Pfad im Dateisystem. In diesem Abschnitt soll jedoch darauf eingegangen werden, wie die Verzeichnisstrukturen aussehen sollten, innerhalb derer Goobi seine Daten sowie die unterschiedlichen Konfigurationsdateien verwaltet.

Der Basispfad für sämtliche Digitalisierungssoftware im Kontext von Goobi lautet:

```bash
/opt/digiverso/
```

Im Bereich dieses Basispfades bestehen im Allgemeinen folgende Verzeichnisse:

```bash
/opt/digiverso/goobi/
/opt/digiverso/logs/
/opt/digiverso/itm/
/opt/digiverso/viewer/
```

Das Verzeichnis logs ist hierbei das zentrale Verzeichnis für Logdateien. Auch die Goobi-Log-Dateien werden bei korrekter Konfiguration an dieser Stelle gespeichert. Die anderen hier aufgeführten Verzeichnisse stehen jeweils für unterschiedliche Applikationen, wie sie häufig Verwendung finden \(z.B. viewer für intranda viewer, itm für intranda Task Manager und goobi für Goobi.

Der Basispfad für Goobi lautet entsprechend:

```bash
/opt/digiverso/goobi/
```

Unterhalb dieses Basispfades findet sich üblicherweise die folgende Ordnerstruktur, die im Folgenden auch detailliert beschrieben wird:

```bash
/opt/digiverso/goobi/config/
/opt/digiverso/goobi/import/
/opt/digiverso/goobi/metadata/
/opt/digiverso/goobi/plugins/
/opt/digiverso/goobi/rulesets/
/opt/digiverso/goobi/scripts/
/opt/digiverso/goobi/xslt/
```

