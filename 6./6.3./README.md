# 6.3. Export in digitale Bibliotheken

Goobi bietet die Möglichkeit, Vorgänge samt ihrer digitalisierten Objekte zu digitalen Bibliotheken zu exportieren. Grundsätzlich wird die XML-Datei \(`METS`\) zuletzt exportiert. Vorher erfolgt in der Regel der Export der Bilder und gegebenenfalls der OCR-Ergebnisse oder anderen digitalen Objekten \(z.B. Audiodateien, Videodateien, Born-Digital-Daten\). 

Nachfolgende Systeme wie zum Beispiel der Solr Indexer von intranda überwachen den Hotfolder und beginnen dann zu arbeiten, wenn ein definiertes Ereignis eintritt. Da die XML-Datei zuletzt exportiert wird, ist sichergestellt, dass dem Solr Indexer alle anderen benötigten Dateien bereits zur Verfügung stehen und die Abarbeitung beginnen kann, sobald auch diese Datei vorliegt.

Für den Export gibt es verschiedene Parameter, die in Goobi konfiguriert werden können. Diese befinden sich in den Projekteigenschaften im Reiter `Technische Daten` und `Mets-Parameter`.

