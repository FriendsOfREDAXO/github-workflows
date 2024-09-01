# Standard GitHub Workflows für Redaxo Addons

Die hier angelegten Workflows sind der aktuelle Standard für FOR AddOns
Die Integration in ein AddOn ist relativ einfach und hilft, die Codequalität des jeweiligen AddOns zu verbessern und
einen gemeinsamen Standard zu schaffen.

## Github Workflows im eigenen Projekt einrichten?

1. Github Actions im Repository aktivieren (Settings > Actions > General > Allow all actions and reusable workflows)
   - https://docs.github.com/en/actions
2. Alle Dateien und Ordner aus diesem Repo (außer _example und README.md) in das AddOn kopieren.
3. Ggf. Abhängigkeiten zu anderen AddOns (und deren Abhängigkeiten) in phpunit.yml und rexstan.yml (.github/workflows)
   eintragen ([Anleitung](#rexstan-1))
4. Optional: Eigene Unit-Tests schreiben ([Anleitung](#phpunit-1)
5. Fertig 🚀

## Was sind GitHub Workflows?

GitHub Workflows sind `.yml` Dateien die in `.github/workflows` liegen und automatisch bei bestimmten Aktionen im
Repository (z.B. einem Push oder einem Pull Request) aufgerufen werden.
Im Hintergrund wird dann innerhalb von Sekunden eine virtuelle Maschine gestartet, Redaxo samt AddOns installiert und
die Tests ausgeführt.

Hängt man `[skip ci]` an den Commit, werden die Tests nicht ausgeführt.

> Sobald die Tests in ein AddOn integriert sind laufen sie automatisch bei jedem Push und Pull Request. Verändert wird
> der Code allerdings nur durch den CS Fixer. Die beiden anderen Tests geben lediglich Informationen über das Testergebnis
> aus. **Dass AddOn kann auch bei fehlgeschlagenen Tests hochgeladen und genutzt werden**

## Welche Tests gibt es?

<https://github.com/FriendsOfREDAXO/github-workflows/wiki>

### PHP CS Fixer

Untersucht ob der Code sich an die definierten Regeln hält **und formatiert den Code ggf. direkt um**.
<https://github.com/FriendsOfREDAXO/github-workflows/wiki/PHP-CS-Fixer>

### Rexstan

Prüft den Code mit Rexstan nach den definierten Regeln und gibt eventuelle Fehler aus.
<https://github.com/FriendsOfREDAXO/github-workflows/wiki/rexstan-(phpstan-f%C3%BCr-REDAXO)>

### PHPUnit

Stellt die Möglichkeit für eigene Unit-Test bereit. ([Anleitung](#phpunit-1))
<https://github.com/FriendsOfREDAXO/github-workflows/wiki/PHP-Unit>

### Publish to Redaxo

AddOn durch ein GitHub Release automatisch in den Redaxo Installer eintragen.
<https://github.com/FriendsOfREDAXO/github-workflows/wiki/Installer%E2%80%90Action>

# Konfiguration und weitere Infos

Einige der Tests benötigen individuelle Einstellungen die im Folgenden aufgeführt sind. Außerdem sind hier tiefere
Informationen zu den einzelnen Tests vermerkt.
