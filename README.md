# Github-Workflows für REDAXO 5 Addons

In diesem Repository findet ihr Workflows für eure REDAXO-Addons.
Diese sollen euch als Hilfe für den Einstieg dienen.
Informationen rund um Workflows und Actions könnt ihr in der offiziellen Dokumentation
finden: https://docs.github.com/en/actions.

## Workflows verwenden

Um die Workflows in eurem Github Repository zu verwenden, müsst ihr alle Dateien (außer README.md) aus diesem Repository
in eures Kopieren. Die Ordnerstruktur muss bestehen bleiben. Weiter müssen die Actions in eurem Repository aktiviert
sein. Die Einstellungen findet ihr unter: _Settings > Actions > General > `Allow all actions and reusable workflows`_.

Ein Workflow ist über eine `.yml` definiert, diese befinden sich im Ordner `.github/workflows`.

Weitere Informationen könnt ihr in den Kommentaren des jeweiligen Workflow finden.

Um PHPUnit, PestPHP oder Nightwatchjs zu nutzen, müssen vorher natürlich noch die Tests definiert werden. Alle Tests
müssen sich im Ordner `tests` befinden. Tests für PHPUnit oder PestPHP zusätzlich im Unterordner `unit`, für
Nightwatchjs im Unterordner `e2e`. Unit-Tests müssen mit `_test` enden, z.B. `addon_test.php`. Diese Struktur kann frei
angepasst werden. Für PHPUnit und PestPHP in `phpunit.xml.dist`, für Nightwatchjs in `nightwatch.conf.js`.

### Was im Workflow passiert

Schaut man sich den Workflow `phpunit/phpunit.yml` an, kann man relativ einfach herausfinden was in diesem passiert.

Neben Namen definiert man zunächst wann dieser aufgerufen werden soll. In diesem Fall bei einem Push oder Pull request
in den Branch `master` oder `main`. Danach wird eine virtuelle Maschine mit der neuesten Ubuntu Version gestartet.
Möchte man einen Workflow überspringen, fügt man der Commit-Message einfach eine der folgenden Befehle
hinzu: `[skip ci]`, `[ci skip]`, `[no ci]`, `[skip actions]`, `[actions skip]`.

Nun folgen die Schritte die für den eigentlichen Workflow relevant sind:

1. PHP (8.0) aufsetzten, diverse Erweiterungen installieren.
2. Den aktuellen REDAXO release runterladen und in den Ordner `redaxo_cms` entpacken. Dieser wird im Addon selbst
   angelegt.
3. MySQL starten und eine Datenbank mit dem Namen `redaxo5` anlegen.
4. REDAXO über den Konsolenbefehl installieren.
5. Das Addon in den redaxo_cms-Ordner verschieben und installieren. In diesem Schritt könnt ihr noch weitere Addons
   installieren, falls diese benötigt werden.
6. Abhängigkeiten aus der composer.json installieren, aktuell nur `phpunit`.
7. PHPUnit ausführen. PHPUnit wird über ein Script aufgerufen, das in der composer.json definiert ist.

## PHPUnit/PestPHP lokal ausführen

PHPUnit/PestPHP kann auch lokal ausgeführt werden. Voraussetzung ist, dass das Addon in der regulären REDAXO Struktur
liegt (`redaxo\src\addons\mein_addon`).
Im Addon-Ordner müssen dann die entsprechenden Composer-Packages installiert werden. Das passiert über ein Terminal
mit `composer install`. Danach kann PHPUnit/PestPHP mit `composer test` aufgerufen werden.

## Nightwatchjs/Playwright lokal ausführen

Auch Nightwatchjs/Playwright kann lokal ausgeführt werden. Wie auch bei den Unit-Tests ist Voraussetzung, dass das Addon in der
regulären REDAXO Struktur liegt (`redaxo\src\addons\mein_addon`).
Im Addon-Ordner müssen dann die entsprechenden Node-Module installiert werden. Das passiert über ein Terminal
mit `npm install`. Weiter muss eine `.env`-Datei angelegt werden. In dieser muss die `LAUNCH_URL` (URL der REDAXO-Seite) eingefügt werden,
z.B.: `LAUNCH_URL=http://local.dev/`. Danach kann Nightwatchjs mit `npm test` aufgerufen werden.
