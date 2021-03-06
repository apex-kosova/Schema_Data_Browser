# LIESMICH für den Schema Data Browser  
Anwendung zur Analyse Ihrer DB- und APEX-Anwendungen

## Demo
	Demo mit einem benutzerdefinierten Konto
	https://strack-software.oracleapexservices.com/apex/f?p=2000:1

	Demo mit einem APEX-Konto
	https://apex.oracle.com/pls/apex/f?p=48950:1

## Kompatibilität:
	mindestens Application Express 19.1 
	mindestens Oracle Database Express Edition 18c

	Die Verwendung der Web Browser Firefox and Chrome wird empfohlen.
	Safari ist etwas langsamer bei der Darstellung der dynamischen Diagramme.
	
## Installation:
1. Encoding Einstellung für sqlplus unter Windows/DOS

	set NLS_LANG=GERMAN_GERMANY.AL32UTF8 
	chcp 65001

2. SQLDEVELOPER Settings

	Umgebung / Codierung = UTF-8

3. Installation von Sys Komponenten für Schema Verwaltung (optional)
	
-- In einer shared Server Umgebung
	Wenn Sie die App mit 'APEX Authorisation' in einem Schema verwenden kann dieser Schritt entfallen.

-- in der Oracle ATP Cloud 
	cd Schema_Browser_Release 
	sqlplus /nolog 
	
	connect admin 

	@custom_keys.sql					-- Geschützter Speicher für Crypto-Schlüssel und Hash-Salz
	@custom_ctx.sql						-- Custom Context für spezielle Session Parameter.
	
	@data_browser_schema_sys_package.sql
	exec data_browser_schema.Add_Apex_Workspace_Schema(p_Schema_Name=>'&WORKSPACE.', p_Apex_Workspace_Name=>'&WORKSPACE.');
	-- Verwende den Oracle ATP Cloud Workspace Name.

	-- Die Prozedur data_browser_schema.Add_Apex_Workspace_Schema kann wiederholt ausgeführt werden, 
	-- um weitere Schemas für die Verwendung vorzubereiten. Die Installation der Supporting Objects 
	-- muss dann manuell im Apex Workspace für diese Schemas vorgenommen werden.
	exit
	
-- in einer on Premise DB 
	cd Schema_Browser_Release 
	sqlplus /nolog 
	
	connect sys as sysdba 

	@custom_keys.sql
	@custom_ctx.sql
	@data_browser_schema_sys_package.sql
	@data_browser_sys_add_schema.sql -- Beim ersten Aufruf entspricht der Schema_Name oft dem Workspace_Name. 
									 -- Das Passwort wird nur verwendet wenn das Schema noch nicht vorhanden ist.
									 -- Später können weitere per Button auf der Home page hinzufügt werden.
	exit

## APEX Application importieren und installieren.

1. Verwendung der Custom Authorisation
	Der Installer prüft bei der Installation, ob die zusätzlich benötigten Berechtigungen erteilt wurden,
	die im Schritt 3 eingerichtet wurden.
	
1.2. Datei **Data_Browser_Custom_App.sql** hochladen.
	Supporting Objects : Yes 
	
	Die Installation dauert lokal ca. 2 Minuten, Oracle Cloud ca. 7 Minuten. 
	
1.2. Beim ersten Start kommt eine Seite in der Ihr Admin Name, Passwort und E-Mail Adresse abgefragt wird.
	Am bequemsten ist es, die im Browser gespeicherten Daten als Programm Admin Daten wiederzuverwenden.
	Das Passwort wird als salted hash Wert in der Tabelle App_Users gespeichert. 
	Weitere berechtigte Benutzer werden in die Tabelle App_Users eingetragen.	
	Die anderen Felder können erst einmal leer bleiben.

2. Verwendung der APEX Authorisation
	In shared Server Umgebungen in denen keine zusätzlichen Berechtigungen verfügbar sind,
	muss diese im Funktionsumfang etwas reduzierte Version eingesetzt werden.

2.1. Datei **Data_Browser_Apex_App.sql** hochladen.
	Supporting Objects : Yes 
	
	Die Installation dauert lokal ca. 2 Minuten, Oracle Cloud ca. 7 Minuten. 

3. Probezeit / Lizenz
	Ohne Lizenz läuft das Programm 3 Monate im vollen Funktionsumfang und schaltet danach in einen Read Only Betrieb um. 
	Die verbleibende Probezeit wird auf der Homepage am unteren Rand angezeigt.
	Wenn Ihnen das Programm gefällt, können sie eine Lizenz bei Strack.Software@t-online.de bestellen, bevor die Probezeit abgelaufen ist.

## Konfiguration
1.	Menü Einstellungen
	Wenn die Homepage nun angemeldet ist, empfiehlt es sich über das Menü Einstellungen die 
	globalen Regeln für die  Namens-Konventionen als Suchmuter in die entsprechenden Zellen einzutragen.
	Oft sind Listen von LIKE Pattern als Defaultwerte vorhanden. Die Felder haben Hilfetexte die Sie über die Bedeutung informieren. 
	Beim Sichern der Einstellungen werden Jobs ausgelöst, die einen Data Dictionary der App aktualisieren. 
	Wähle dann eine typische Tabellen und prüfe ob die Regeln an den erwarteten Feldern greifen.
	Die App blendet eigene Tabellen default-mäßig aus. 
	
2. Schema verwalten
	Auf der Seite 'Schema verwalten' werden die immer angezeigten Spalten für Labels, Navigations Links und LOVs verwaltet.
	Mit der Festlegung von Feldlisten der natürlichen eindeutigen Schlüsseln, wird der automatische Schlüssel-Lookup für Importe 
	aber auch die Eindeutigkeit von allen angezeigten Labels garantiert. 
	
3. Client App
	In einigen Projekten kann die App als Backstage für die Datenpflege und eine festprogrammierte Apex App als Fachanwendung kombiniert werden.
	In der Homepage kann eine Client App mit aufgelistet werden, wenn diese über die Einstellungen unter Zugriffskontrolle 
	als Kunden-Anwendung ausgewählt wird. Die App kann ausgewählt werden, wenn sie vorher im gleichen Schema installiert ist. 
	
4. Data Reporter App
	Nach der Installation der Data Reporter App, müssen die 'Supporting Objects' des Data Browsers erneut installiert werden, 
	wenn eine Verbindung über das Package Data_Browser_Reporter hergestellt werden soll.

## Upgrade der APEX Application und 'Supporting Objects'
	Wenn Sie eine neue Version installieren, ersetzen sie die ältere Installation und führen Sie die Installation mit der Option 'Supporting Objects' aus.

## Deinstallation
	Wenn Sie die Anwendung und 'Supporting Objects' deinstallieren, werden einige Konfigurationstabellen, Packages und Funktionen erhalten.
	Diese Objekte unterstützen die Funktionstüchtigkeit erzeugten Trigger und erhalten Ihre Einstellungen für einen Upgrade der Installation.

