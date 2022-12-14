# Tutorial: Laravel unter WSL2 einrichten
Dieser Leitfaden dient zur Einrichtung einer Laravel App unter Windows (WSL2).

## Speed Up Laravel unter Windows
Unter Berücksichtigung der App-Geschwindigkeit sollten die Laravel-Quelldateien innerhalb des WSL-Filesystems installiert werden und nicht ausserhalb im Windows-Filesystem.

## Vorraussetzungen
- Windows mit WSL2
- Docker mit aktivierter WSL-Unterstützung - https://www.docker.com/products/docker-desktop/
- VS Code Editor - https://code.visualstudio.com/

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
Weitere Build-Konfigurationen sind hier zu finden:

https://laravel.com/docs/9.x/installation#getting-started-on-linux


## Projekt in VS Code öffnen
Starte den VS Code Editor und öffne ein WSL-Fenster, dazu im Editor die Taste F1 drücken und im Textfeld "new wsl window" eingeben bzw. auswählen. Unten links sollte nun "WSL:Ubuntu" zu sehen sein.

Jetzt nur noch den eben erstellten Projektordner öffnen, unter "Ordner öffnen".

## Forward DB-Port ändern
Um unabhängig von weiteren installierten Datenbanksystemen auf die Laravel-Daten zugreifen zu können, sollte der Forward-Port geändert werden. Dazu folgende Zeile mit dem gewünschten Port, der .env Datei hinzufügen:
```
FORWARD_DB_PORT=25062
```

## Laravel App via Dev Container starten
Im VS Code Editor sollte die Meldung "Reopen in Dev Container" erscheinen, dort den entsprechenden Button drücken (ansonsten via F1 nach diesen Befehl suchen). Somit findet die Entwicklungsumgebung direkt in Linux statt und alle Terminal-Befehle (ALT+T) entsprechend auch (sail commands sind somit hinfällig).

Nun kann die App unter http://localhost im Browser geöffnet werden.

Somit ist die Basisanwendung startklar!

## Laravel UI (Bootstrap) installieren
Gute Grundausstattung mit nützlichen Features: https://github.com/laravel/ui

```
# registrieren
composer require laravel/ui

# bootstrap (vue) scaffolding
php artisan ui vue
php artisan ui vue --auth

# install
npm install

# Database vorbereiten
php artisan migrate
```

## Vite konfiguirieren
Wichtig ist die hmr Einstellung, da sonst über den Dev Container leider auf 0.0.0.0 anstatt auf localhost gezeigt wird:
```js
export default defineConfig({
    server: {
        hmr: {
            host: 'localhost'
        }
    },
    plugins: [
        laravel({
            input: ['resources/css/app.css', 'resources/js/app.js'],
            refresh: true,
        }),
    ],
});
```

App ausführen
```
npm run dev
```

# Clone from Github and Init

Zuerst via VS-Code seine Laravel-Repo klonen, dann innerhalb des Terminals folgende Befehle ausführen:

https://laravel.com/docs/9.x/sail#installing-composer-dependencies-for-existing-projects

```
wsl
docker run --rm --name laravel-pgsql \
    -u "$(id -u):$(id -g)" \
    -v "$(pwd):/var/www/html" \
    -w /var/www/html \
    laravelsail/php81-composer:latest \
    composer install --ignore-platform-reqs
```

Nicht vergessen node_modules zu installieren, innerhalb des DevContainers

```
npm install
```
