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

### PHP CS Fixer

Untersucht ob der Code sich an die definierten Regeln hält **und formatiert den Code ggf. direkt um**.

### Rexstan

Prüft den Code mit Rexstan nach den definierten Regeln und gibt eventuelle Fehler aus.

### PHPUnit

Stellt die Möglichkeit für eigene Unit-Test bereit. ([Anleitung](#phpunit-1))

### Publish to Redaxo

AddOn durch ein GitHub Release automatisch in den Redaxo Installer eintragen.

# Konfiguration und weitere Infos

Einige der Tests benötigen individuelle Einstellungen die im Folgenden aufgeführt sind. Außerdem sind hier tiefere
Informationen zu den einzelnen Tests vermerkt.



## PHP CS Fixer

CS Fixer benötigt keine weiteren Konfigurationen. Das Script öffnet die `composer.json` installiert die benötigten
Packages (`require-dev`) und führt die dort bereitgestellten Scripte aus (`cs-dry` und `cs-fix`).
In der `.php-cs-fixer.dist.php` wird
das [Redaxo Regelset](https://github.com/redaxo/php-cs-fixer-config/blob/main/src/Config.php) integriert.

### PHP CS Fixer lokal ausführen

PHP CS Fixer kann auch lokal ausgeführt werden. Hierzu muss auf dem System einmalig
global [Composer](https://getcomposer.org/download) installiert werden.
Anschließend führt man in der Console einmalig `composer install` hiermit werden die Packages aus `require-dev` lokal
installiert.
Nun kann man jederzeit mit `composer cs-dry` aktuelle Fehler anzeigen lassen, und/oder mit `composer cs-fix` gefundene
Fehler beheben lassen.

## Rexstan

Werden für ein AddOn weitere AddOns benötigt müssen diese in der rexstan.yml eingetragen werden. Im Bereich - name: Copy
and install AddOns wird das aktuelle Repo kopiert, Rexstan heruntergeladen und anschließend beides installiert.

```yaml
      - name: Copy and install AddOns
        run: |
          rsync -av --exclude='./vendor' --exclude='.github' --exclude='.git' --exclude='redaxo_cms' './' 'redaxo_cms/redaxo/src/addons/${{ github.event.repository.name }}'
          redaxo_cms/redaxo/bin/console install:download 'rexstan' '1.*'
          redaxo_cms/redaxo/bin/console package:install 'rexstan'
          redaxo_cms/redaxo/bin/console package:install '${{ github.event.repository.name }}'
```

Eigene Abhängigkeiten werden hier eingefügt. Hat das Abhängige AddOn weitere Abhängigkeiten müssen diese ebenfalls
eingetragen werden. **Wichtig, Abhängigkeiten müssen immer zuerst installiert werden, sonst kommt es zu einem Fehler
während der Installation.
Hier ein Beispiel für ein AddOn, das YRewrite benötigt (Yrewrite wiederum benötigt YForm):

```yaml
      - name: Copy and install AddOns
        run: |
          rsync -av --exclude='./vendor' --exclude='.github' --exclude='.git' --exclude='redaxo_cms' './' 'redaxo_cms/redaxo/src/addons/${{ github.event.repository.name }}'
          redaxo_cms/redaxo/bin/console install:download 'rexstan' '1.*'
          redaxo_cms/redaxo/bin/console install:download 'yrewrite' '2.*'
          redaxo_cms/redaxo/bin/console install:download 'yform' '4.*'
          redaxo_cms/redaxo/bin/console package:install 'yform'
          redaxo_cms/redaxo/bin/console package:install 'yrewrite'
          redaxo_cms/redaxo/bin/console package:install 'rexstan'
          redaxo_cms/redaxo/bin/console package:install '${{ github.event.repository.name }}'
```

In .tools/rexstan.php sind die Regeln für Rexstan definiert.

# PHPUnit

Genauso wie bei ([Rexstan](#rexstan-konfigurieren)) müssen bei PHP Unit Abhängigkeiten hinterlegt werden.

Danach ist der Workflow für eigene Tests vorbereitet. In .tools/bootstrap.php sind Variablen für PHPUnit festgelegt. In
phpunit.xml.dist sind die Regeln für PHPUnit definiert.
In der Composer.json ist ein Script `unit-test` hinterlegt, das die Tests ausführt. Es werden automatisch alle Tests
ausgeführt die im Ordner `tests/unit`liegen und mit `_test.php` enden.

## PHPUnit lokal ausführen

Auch PHPUnit kann lokal ausgeführt werden. Wie bei [PHP CS Fixer](#php-cs-fixer-lokal-ausführen) müssen einmalig
Composer und die benötigten Packages installiert werden.
Danach können die Tests über die Konsole mit `composer unit-test` ausgeführt werden.

## Eigene Unit-Tests schreiben

Mit einem Unit Test kann man eigene PHP Klassen automatisiert Prüfen lassen und damit festlegen, ob die Klasse auch nach
Änderungen wie gewünscht funktioniert.

1. Im AddOn im Ordner `tests/unit` eine neue Datei mit einem aussagekräftigen Namen anlegen. Der Name muss
   mit `_test.php` enden. (Empfehlung: `addonname_test.php`)
2. Eine Testklasse für die zu prüfende Klasse anlegen. Die Klasse muss mit `TEST` enden und erweitert die
   Klasse `TestCase`
3. Für jeden Test der Klasse eine Funktion anlegen, die mit `test` beginnt. (Empfehlung: `testFunktionName()`)
4. In der Funktion wird dann definiert, was die Testklasse tun soll.
5. Abschließend wird das Ergebnis (zum Beispiel mit einer Assert Funktion) überprüft. `TestCase` liefert etliche Assert
   Funktionen. Man kann auf true oder false prüfen, ob ein Ordner oder eine Datei existiert, zwei identische Werte
   vergleichen und noch vieles mehr ([Dokumentation](https://docs.phpunit.de/en/10.0/assertions.html)).

> Langfristig sollte das Ziel sein, für jede Klasse und jede Funktion entsprechende Tests zu schreiben. Da dass jedoch
> viel Fleißarbeit ist, ist es sinnvoll, zunächst nur die wichtigsten Funktionen zu testen und ggf. neue Erweiterungen mit
> Tests zu versehen.

### Beispiel Werte abgleichen

Hier wird die Ausgabe der eigenen Funktion mit einem erwarteten Wert abgeglichen.

```php
class meineKlasseTEST extends TestCase {
    public function testFunktionName() {
        $actual = meineKlasse::funktionName();
        $expected = 'Erwarteter Wert';
        static::assertEquals($expected, $actual);
}
```

### Beispiel rex_view::addCssFile

Dieser Test ist aus dem Redaxo Core. Hier wird addCssFile aus der Klasse rex_view getestet. Es wird geprüft, ob die
Datei am Ende der Liste der CSS Dateien auftaucht. Außerdem wird geprüft, ob eine CSS Datei mit einem definierten Key
auch wieder mit diesem Key aufrufbar ist.

```php
class meineKlasseTEST extends TestCase {
    public function testAddGetCss(): void
    {
        rex_view::addCssFile('my.css');
        $files = rex_view::getCssFiles()['all'];
        static::assertTrue('my.css' == end($files));

        rex_view::addCssFile('print.css', 'print');
        $files = rex_view::getCssFiles()['print'];
        static::assertTrue('print.css' == end($files));
    }
}
```
