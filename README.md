# Tutorial: Laravel unter WSL2 einrichten
Dieser Leitfaden dient zur Einrichtung einer Laravel App unter Windows (WSL2).

## Speed Up Laravel unter Windows
Unter Berücksichtigung der App-Geschwindigkeit sollten die Laravel-Quelldateien innerhalb des WSL-Filesystems installiert werden und nicht ausserhalb im Windows-Filesystem.

## Vorraussetzungen
- Windows mit WSL2
- Docker mit aktivierter WSL-Unterstützung
- VS Code Editor

## WSL workspace Ordner einrichten 
Zuerst legen wir einen Workspace-Ordner innerhalb des WSL-Filesystems an, dieses finden wir über die Win-Exploreradresszeile unter:
```
# WSL-Stammverzeichnis
\\wsl$

# Home-Ordner
\\wsl$\Ubuntu\home
```
Hier navigieren wir uns bis zum Benutzerordner (hier "max") und erstellen dort den Workspace für unsere Laravel-Projekte
```
\\wsl$\Ubuntu\home\max\workspace
```
Von diesem Ordner kann man sich jetzt eine Verknüpfung auf den Desktop ziehen.

## Laravel installieren
Öffne dazu ein Terminal, ausgehend vom erstellten Workspace-Ordner. Dort führen wir wie folgt die Laravel-Installation aus:
```
# in WSL anmelden
wsl

# Neues Laravel-Projekt erstellen
curl -s "https://laravel.build/example-app?with=pgsql&devcontainer" | bash
```

## Sail alias in WSL registrieren
Um den Befehl 'sail' ständig nutzen zu können, muss dieser in die .bashrc Datei als alias registriert werden. Dazu im Win-Explorer zum WSL-Benutzerordner (../home/max) wechseln und der .bashrc Datei folgende Zeile hinzufügen:
```
alias sail='[ -f sail ] && sh sail || sh vendor/bin/sail'
```

## Projekt in VS Code öffnen
Der Editor kann hier kostenlos heruntergeladen werden: https://code.visualstudio.com/

Starte den VS Code Editor und öffne ein WSL-Fenster, dazu im Editor die Taste F1 drücken und im Textfeld "new wsl window" eingeben bzw. auswählen. Links unten sollte danach "WSL:Ubuntu" zu sehen sein.

Jetzt nur noch den eben erstellten Projektordner öffnen, unter "Ordner öffnen".

## Forward DB-Port ändern
Um unabhängig von weiteren installierten Datenbanksystemen auf die Laravel-Daten zugreifen zu können, sollte der Forward-Port geändert werden. Dazu folgende Zeile mit dem gewünschten Port, der .env Datei hinzufügen:
```
FORWARD_DB_PORT=25062
```

## Laravel App testen
Dazu innerhalb des Editors ein Terminal öffnen, mit ALT+T. Jetzt sollten wir automatisch innerhalb unseres Projektordners Befehlsgewalt haben. Dort starten wir nun unsere Testumgebung via sail:
```
# starten
sail up -d

# stoppen
sail stop
```
Und können uns das Ergebnis unter http://localhost im Browser anschauen.

Somit ist die Basisanwendung startklar! 

# Gimmicks
## Laravel-Jetstream installieren
https://jetstream.laravel.com/2.x/features/api.html#enabling-api-support
```
sail composer require laravel/jetstream
sail artisan jetstream:install livewire
sail npm install
sail npm run build
sail artisan migrate
```





