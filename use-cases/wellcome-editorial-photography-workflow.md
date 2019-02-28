---
description: Beschreibung des Workflows der Wellcome Library für den Upload zu Tandem Vault
---

# Wellcome Editorial Photography Workflow

## Einführung

Der Editorial Photography Workflow liest aus einem Hotfolder sogenannte "shoots" aus und bearbeitet sie in einem vollautomatischen Workflow. Ergebnis ist das Speichern der master-Bilder in Preservica, das Hochladen von Derivaten zu Tandem Vault und das Speichern einer exportierten METS-Datei in einem Amazon S3 bucket.

## Installation von Programmen und Scripten

Es werden folgende Programme und Scripte für den Betrieb benötigt, alle sollten in der aktuellen Version kompiliert werden:

* Goobi \(im Moment aus dem Branch `feature_aws_sdk`\)
* TaskManager
* TaskClient
* REST-Hotfolder-Import-Plugin \(git: `goobi-plugin-rest-wellcome-photos`\)
* TaskManager TifValidation Plugin \(git: `taskmanager-plugin-job-tifvalidation`\)
* Template für das TifValidation Plugin
* embedEditorialMeta.sh \(git: `intrandascripts` unter `scripts/wellcome/`\)
* jpegConversionEditorial.py \(git: `intrandascripts` unter `scripts/wellcome/`\)
* TaskManager Plugin SDB\(Preservica\) \(git: `taskmanager-plugin-job-sdb`\)
* TaskManager Plugin TandemVault \(git: `taskmanager-plugin-job-tandemvault`\)
* Goobi Export Plugin für diesen workflow \(git: `goobi-plugin-export-wellcome-photos`\)
* AWSMetsUploadPlugin \(git: `goobi-plugin-step-wellcome-aws-metsupload`\)

## Konfiguration in Goobi

Das REST-Hotfolder-Import-Plugin muss im Goobi-Classpath liegen `WEB-INF/lib` und die anderen Plugins an den Standard-Orten, damit sie von Goobi bzw. dem TaskManager gefunden werden. Die Scripte können frei platziert werden. Die Pfade müssen allerdings in der Konfiguration angepasst werden. Es bietet sich der folgende Ordner an:

```text
/opt/digiverso/goobi/scripts/
```

Zusätzlich muss der Aufruf für die REST-Schnittstelle in der WebAPI-Konfiguration `/opt/digiverso/goobi/config/goobi_webapi.xml` freigegeben werden:

```markup
<credentials ip="0:0:0:0:0:0:0:1" password="CHANGEME">                      
    <command name="setErrorStep"/>                                              
    <command name="closeStep"/>                                                 
    <command name="addToProcessLog"/>                                           
    <command name="/wellcome/createeditorials"/>                                                 
</credentials>
```

Außerdem muss ein Projekt für den neuen Workflow angelegt werden. Hier werden zwei FileGroups benötigt:

### FileGroup TANDEMVAULT:

```text
Path: file:///opt/digiverso/viewer/media/$(meta.CatalogIDDigital)/ 
Mime Type: image/jpeg
Suffix: jpg
```

### FileGroup SDB

```text
Path: file:///opt/digiverso/viewer/media/$(meta.CatalogIDDigital)/ 
Mime Type: image/tiff
Suffix: tif
```

Als letztes wird noch ein eigener Regelsatz sowie die Tif-Konfiguration benötigt, wie unten aufgeführt.

## Einrichtung des Workflows

Der Workflow besteht aus 7 Schritten, alle sind automatische Schritte:

### Create process

Dieser Schritt ist lediglich ein Platzhalter

### Embed metadata

Script-Schritt mit folgendem Kommando:

```text
/bin/bash "/opt/digiverso/goobi/scripts/embedEditorialMeta.sh" "_{process.archiveStatus}" "_{origpath}" "_{meta.PlaceOfPublication}" "_{process.Creators}" "_{meta.Contains}" "_{meta.Description}" "_{meta.Usage}" "_{meta.AccessLicense}"
```

### 3. TIFF Validation

Script-Schritt mit folgendem Kommando:

```text
/usr/bin/java -jar /opt/digiverso/itm/bin/TaskClient.jar -itm http://localhost:8080/itm/service -s {origpath} -d {processpath} -e -i {stepid} -T {processtitle} -t TIFVALIDATION -n tiff-template.xml -gid {processid}
```

### 4. Create JPEG derivatives

Script-Schritt mit folgendem Kommando:

```text
/usr/bin/env "python" "/opt/digiverso/goobi/scripts/jpegConversionEditorial.py" "{origpath}" "{imagepath}/{processtitle}_jpg"
```

### 5. Upload to preservica

Standard-SDB-Aufruf

### 6. Upload to Tandem Vault

Script-Schritt mit folgendem Kommando:

```text
/usr/bin/java "-jar" "/opt/digiverso/itm/bin/TaskClient.jar" "-itm" "http://localhost:8080/itm/service" "-s" "{imagepath}/{processtitle}_jpg" "-d" "{imagepath}/source/" "-n" "template" "-e" "-i" "{stepid}" "-T" "{processtitle}" "-gid" "{processid}" "-t" "TandemVault" "-tv_r" "_{process.b-number}" "-tv_t" "_{meta.TitleDocMain}" "-tv_n" "_{meta.Usage}" "-tv_ut" "_{meta.AccessLicense}" "-tv_st" "_{meta.ShootType}"
```

### 7. Export Step plugin

Konfiguriertes Plugin `WellcomeExport`

### 8. Step plugin:

Konfiguriertes Plugin `AWSMetsUploadPlugin`

## CRON-Job

Um die Vorgänge anzustoßen, muss noch regelmäßig ein curl-Aufruf stattfinden, der das Import-Plugin zur Arbeit auffordert. Dieser sollte stündlich aufgerufen werden und sieht folgendermaßen aus:

```text
/usr/bin/curl -X POST -H "token: CHANGEME" -H "templateid: CHANGEME" -H "hotfolder: /opt/digiverso/goobi/editorial_hotfolder" http://localhost:8080/goobi/api/wellcome/createeditorials
```

Dabei müssen die `templateid`, das `token` und der `hotfolder` noch angepasst werden.

## Benötigte Dateien

```markup
<tifValidationConfig>
    <namespace uri="http://www.loc.gov/mix/v20" name="mix" />
    <namespace uri="http://hul.harvard.edu/ois/xml/ns/jhove" name="jhove" />
    <check>
        <xpath>
            string(//jhove:repInfo/jhove:format)
        </xpath>
        <wanted>
            TIFF
        </wanted>
        <error_message>
            Check for image format: Image: "${image}" Wanted value: "${wanted}"\, found value: "${found}".
        </error_message>
    </check>
    <!--Check if the image is well-formed and valid-->
    <check>
        <xpath>
            string(//jhove:repInfo/jhove:status)
        </xpath>
        <wanted>
            Well-Formed and valid
        </wanted>
        <error_message>
            Check for image status: Image: "${image}" Wanted value: "${wanted}"\, found value: "${found}".
        </error_message>
    </check>
</tifValidationConfig>
```

