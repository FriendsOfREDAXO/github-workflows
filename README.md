# Github-Workflows für REDAXO 5 Addons

In diesem Repository findet ihr Workflows für eure REDAXO-Addons.
Diese sollen euch als Hilfe für den Einstieg dienen.
Informationen rund um Workflows und Actions könnt ihr in der offiziellen Dokumentation finden: https://docs.github.com/en/actions.

## Workflows verwenden

Um die Workflows in eurem Github Repository zu verwenden, müsst ihr alle Dateien (außer README.md) aus diesem Repository in eures Kopieren. Die Ordnerstruktur muss bestehen bleiben. Weiter müssen die Actions in eurem Repository aktiviert sein. Die Einstellungen findet ihr unter: _Settings > Actions > General > `Allow all actions and reusable workflows`_.

Ein Workflow ist über eine `.yml` definiert, diese befinden sich im Ordner `.github/workflows`.

Weitere Informationen könnt ihr in den Kommentaren des jeweiligen Workflow finden.

Um PHPUnit oder PestPHP zu nutzen, müssen vorher natürlich noch die Tests definiert werden. Diese müssen sich im Ordner `tests` befinden. Ein Test muss mit `_test` enden z.B. `addon_test.php`.
Ein einfaches Beispiel findet ihr im jeweiligen Ordner.
Viele weitere Beispiele gibt es im REDAXO Repository: https://github.com/redaxo/redaxo/tree/main/redaxo/src/core/tests.

### Was im Workflow passiert

Schaut man sich den Workflow `phpunit/phpunit.yml` an, kann man relativ einfach herausfinden was in diesem passiert.

Neben Namen definiert man zunächst wann dieser aufgerufen werden soll. In diesem Fall bei einem Push oder Pull request in den Branch `master` oder `main`. Danach wird eine virtuelle Maschine mit der neuesten Ubuntu Version gestartet.
Möchte man einen Workflow überspringen, fügt man der Commit-Message einfach eine der folgenden Befehle hinzu: `[skip ci]`, `[ci skip]`, `[no ci]`, `[skip actions]`, `[actions skip]`.

Nun folgen die Schritte die für den eigentlichen Workflow relevant sind:

1. PHP (8.0) aufsetzten, diverse Erweiterungen installieren.
2. Den aktuellen REDAXO release runterladen und in den Ordner `redaxo_cms` entpacken. Dieser wird im Addon selbst angelegt.
3. MySQL starten und eine Datenbank mit dem Namen `redaxo5` anlegen.
4. REDAXO über den Konsolenbefehl installieren.
5. Das Addon in den redaxo_cms-Ordner verschieben und installieren. In diesem Schritt könnt ihr noch weitere Addons installieren, falls diese benötigt werden.
6. Abhängigkeiten aus der composer.json installieren, aktuell nur `phpunit`.
7. PHPUnit ausführen. PHPUnit wird über ein Script aufgerufen, das in der composer.json definiert ist.

## PHPUnit lokal ausführen

PHPUnit kann auch lokal ausgeführt werden. Voraussetzung ist, dass das Addon in der regulären REDAXO Struktur liegt (`redaxo\src\addons\mein_addon`).
Im Addon-Ordner müssen dann die entsprechenden Composer-Packages installiert werden. Das passiert über ein Terminal mit `composer install`. Danach kann PHPUnit mit `composer test` aufgerufen werden.

## TODO

- [ ] Workflow für E2E Tests anlegen
- [ ] Workflow für REXStan anlegen
- [x] Workflow für Pest anlegen (als alternative zu PHPUnit)
- [x] Beispiel im jeweiligen Workflow anlegen
