# 2.1.3.  Unterverzeichnis ‚metadata’

Das Verzeichnis `metadata` ist das zentrale Verzeichnis für die Speicherung von Metadaten und digitalen Inhalten von Goobi. Innerhalb dieses Verzeichnisses befindet sich für jeden Goobi-Vorgang ein Verzeichnis mit dem Namen der Vorgangs-ID von Goobi. Die Verzeichnisse der einzelnen Goobi-Vorgänge sind dabei folgendermaßen aufgebaut:

```bash
1234/
1234/meta.xml
1234/images/
1234/ocr/
1234/import/
1234/validation/
1234/taskmanager/
1234/thumbs/
```

Neben der zentralen Metadatendatei `meta.xml` befinden sich je nach Konfiguration gegebenenfalls noch weitere Backupdateien wie z.B. `meta.xml.1`, `meta.xml.2`, `meta.xml.3`

Das Verzeichnis `images` wird innerhalb des Workflows verschiedenen Nutzern temporär zur Verfügung gestellt und hat dabei den folgenden Aufbau:

```bash
1234/images/abc_media/
1234/images/abc_jpg/
1234/images/abc_source/
1234/images/master_abc_media/
```

Der für die Arbeit mit den Inhalten wichtigste Ordner ist derjenige, der auf `_media` endet. Das Verzeichnis, das mit `master_` beginnt, wird üblicherweise dafür verwendet, dort alle Masterimages in unmanipuliertem Status zu speichern. Die weiteren Verzeichnisse dienen zur freien Verwendung innerhalb der Workflows und können beliebig ergänzt werden. Das Verzeichnis, das auf `source` endet, wird ebenso wie das Verzeichnis, das auf `_media` endet, innerhalb des Exports zum Präsentationssystem \(z.B. intranda viewer\) kopiert.

Neben dem Verzeichnis `images` kann ebenfalls ein Verzeichnis `ocr` vorhanden sein. Hierin befinden sich sämtliche OCR-Ergebnisse, die innerhalb des Workflows erzeugt und zu dem Vorgang hinzugefügt wurden. Für jedes vorliegende Format der OCR-Ergebnisse besteht ein eigenes Verzeichnis.

```bash
1234/ocr/abc_alto/
1234/ocr/abc_doc/
1234/ocr/abc_pdf/
1234/ocr/abc_wc/
1234/ocr/abc_xml/
```

Im Ordner `thumbs` können kleinere Versionen der in `images` liegenden Bilder gespeichert werden, die Goobi verwendet um die Bilder in geringer Auflösung darzustellen. Dies erhöht die Geschwindigkeit der Bildanzeige bei größeren Bildern erheblich. Für jeden Unterordner von `images` können ein oder mehrere Unterordner in `thumbs` angelegt werden, die den selben Namen tragen wie der `images` Unterordner, erweitert um einen zusätzlichen Unterstrich \_ und eine Größenangabe in Pixeln. Diese Größenangabe muss dem Maximum von Höhe und Breite der Bilder im jeweiligen Unterordner entsprechen. Die Dateinamen der Bilder im `thumbs` Unterordner müssen denen der Bilder im passenden `images` Unterordner entsprechen, jedoch mit der Dateiendung `.jpg`.

```bash
1234/thumbs/abc_media_800/
1234/thumbs/abc_media_3000/
1234/thumbs/master_abc_media_800/
1234/thumbs/master_abc_media_3000/
```

Wenn zu einer Bilddatei in `images` passende Bilder in `thumbs` vorhanden sind, dann werden diese in Goobi automatisch verwendet für die Anzeige von Thumbnails und zoombaren Bildern im herausgezoomten Zustand.

Sollte auf dem Goobi-Server und auch in den verwendeten Workflows eine automatische Validierung z.B. der Images stattfinden, existiert das Verzeichnis `validation`.

Innerhalb dieses Verzeichnisses wird pro Validierungsdurchführung jeweils ein Unterordner erzeugt, um auch ältere Validierungsergebnisse vorzuhalten. Die Benennung der erzeugten Unterordner erfolgt dabei stets so, dass der Ordnername sowohl Datum und Uhrzeit sowie den Typ der Validierung aufführen. Dies sieht z.B. wie folgt aus:

```bash
1234/validation/2012-11-20_11-20-01_jpylyzer/
1234/validation/2012-11-20_12-02-13_jhove/
1234/validation/2012-11-23_08-12-56_jpylyzer/
```

Findet eine Zusammenarbeit mit dem intranda TaskManager statt, so existiert innerhalb des Ordners ebenso das Verzeichnis `taskmanager`. Binnen dieses Verzeichnisses speichert der TaskManager temporäre Daten für die Durchführung langlaufender Aufgaben. Außerdem werden an dieser Stelle je nach Konfiguration ebenfalls sämtliche Ticket- bzw. Templatedateien dauerhaft gespeichert und vorgehalten, die für die einzelnen Aufrufe des TaskManagers verwendet wurden. Der Inhalt dieses Verzeichnisses sieht folgendermaßen aus:

```bash
1234/taskmanager/2012-11-23_08-12-56_jp2validate/
1234/taskmanager/2012-11-25_14-38-15_iii-create_jpeg/
```

Auch abhängig von der individuellen Installation liegt pro Goobi-Vorgang gleichermaßen ein Ordner `import` vor. Dieser Ordner wird von Import-Plugins dafür verwendet originale Quelldateien zum jeweiligen Vorgang zugehörig mitzuführen. Manuell eingespielte und importierte Katalogdatensatzdateien oder andere Ursprungsdateien können hier aufbewahrt und innerhalb von Scripten im Rahmen der Workflowverarbeitung verwendet werden. Der Inhalt des Ordners könnte z.B. folgendermaßen aussehen:

```bash
1234/import/abc.mrc
1234/import/abc.original.pdf
1234/import/eod/
```

