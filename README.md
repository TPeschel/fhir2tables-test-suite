# Fhir2Tables

## Test
### Wie erstellt man einen Test?
Das Erstellen eines Testfalles ist im Wesentlichen durch das Schreiben einer Spezifikation in Form eines R-Skriptes erledigt.  
Das Spezifikationsskript muss 4 Elemente enthalten.
1. Der baseR4-Endpoint des FHIR-Servers:  
```endpoint```
2. Die FHIR-Suchanfrage:  
```fhir.search```
3. Die Struktur der aus dem Bundle zu erstellenden Tabellen:  
```tables.design```
4. Eine Funktion, die die Daten wie gewünscht filtert:  
```filter.data```  

Beispiel einer Spezifikation zum Abfragen aller männlichen Patienten mit deren ID, Geschlecht und Geburtsdatum:  

```
###
# Endpunkt des fhir r4 Servers
###
#endpoint <- "https://hapi.fhir.org/baseR4"
endpoint <-  "https://vonk.fire.ly/R4/"

###
# fhir search ohne Endpunktangabe
###
fhir.search <- "Patient?_format=xml&gender=male"

###
# Welche Daten aus den Pages sollen wie in welchen Tabellen erzeugt werden
# Hier nur eine Tabelle Patient mit den Einträgen PID, Geschlecht und Geburtsdatum
###
tables.design <- list(
	Patient = list(
		entry   = ".//Patient",
		items = list(
			PID       = "id/@value",
			GENDER    = "gender/@value",
			BIRTHDATE = "birthDate/@value"
		)
	)
)

###
# filtere Daten in Tabellen vor dem Export ins Ausgabeverzeichnis
###
filter.data <- function( list.of.tables ) {

  ###
  # filter here whatever you want!
  ###

  ###
  # nur komplette Datensaetze erwuenscht
  ###
  list.of.tables <- lapply( list.of.tables, na.omit )

  ###
  # gib gefilterte Daten zurueck
  ###
  list.of.tables
}
```
### Wie startet man einen Test?
Aus dem Ordner **api**, indem sich das R-Skript **runTest.R** befindet, startet man einen Test mit folgender Eingabe in die Kommandozeile:  
```Rscript runTest.R -s specification-file -o output-directory```  
Hierbei sind:  
```specification-file```: der Name des R-Skriptes, das den Test spezifiziert (in der Regel spec.R)  
und  
```output-directory```: der Name des Verzeichnisses, in dem die Resultate gespeichert werden sollen (z.B. result).

### 4 vorbereitete Tests
Die spec.R Dateien von 4 vorbereiteten Testfälle befinden sich im Ordner tests.  
```
.
├── api
│   ├── fhir2tables.Rproj
│   ├── runExample.R
│   ├── rxml4.R
│   ├── spec.R
│   └── workflow
│       └── exampleObsWithPatEnc00.R
└── tests
    ├── 1
    │   ├── result
    │   │   ├── Patient.csv
    │   │   └── tables.RData
    │   └── spec.R
    ├── 2
    │   └── spec.R
    ├── 3
    │   └── spec.R
    └── 4
        └── spec.R
```  
Der erste Test wurde hier durch folgenden Befehl, der im Ordner **api** ausgeführt wurde, bereits als Beispiel erzeugt:  
```
.../fhir2tables/api$ Rscript runTest.R -s ../tests/1/spec.R -o ../tests/1/result
```

### Erreichbare Endpoints  
  - "http://demo.oridashi.com.au:8305/"  
  - "http://test.fhir.org/r4/"  
  - "https://vonk.fire.ly/R4/"  
    - vonk scheint die besten Daten zu haben, doch nur wenige  
  - "https://hapi.fhir.org/baseR4/"  
    - hapi hat viele aber auch viele unsinnige Daten und scheint bereits ueberlastet zu sein
	- ab 21:30 wird es besser  

## Polar Use Cases Tests ... will be continued
