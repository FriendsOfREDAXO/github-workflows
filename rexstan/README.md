# rexstan

https://github.com/FriendsOfREDAXO/rexstan

## Workflow verwenden

Um den Workflow zu verwenden, müssen die beiden Ordner `.github` und `.tools` in das eigene Repository kopiert werden.
Der Workflow startet automatisch, wenn ein neuer Commit oder ein neuer Pull-Request erstellt wird. 
Findet rexstan einen oder mehrere Fehler, wird der Workflow abgebrochen und die Fehler in den Actions im Schritt `Run rexstan` ausgegeben.

### Konfiguration

Unter `.tools/rexstan.php` kann die Konfiguration für rexstan angepasst werden.
Dort können Level, Extensions und PHP-Version angepasst werden.
Werden zu rexstan und dem eigenen Addon noch weitere Addons benötigt, können diese in der `rexstan.yml` unter dem Punkt `Copy and install Addons` hinzugefügt und installiert werden.
