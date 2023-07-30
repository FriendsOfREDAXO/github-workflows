# Minify JS

## Workflow verwenden

Um den Workflow zu verwenden, muss der Ordner `.github` in das eigene Repository kopiert werden.
Der Workflow startet automatisch, wenn ein neuer Commit oder ein neuer Pull-Request auf dem `main`- oder `master`-Branch erstellt wird.
Der Workflow minifiziert alle JS-Dateien im angegebenen Ordner und legt diese im gleichen Ordner mit dem Suffix `.min` an.

### Konfiguration

Die einzige Konfiguration, die vorgenommen werden muss, ist der Ordner, in dem die JS-Dateien liegen.
Dieser kann in der `minify.yml` unter `directory: 'assets'` angepasst werden.
Weitere Einstellungsmöglichkeiten findet ihr in der [Dokumentation von auto-minify](https://github.com/marketplace/actions/auto-minify).