# PHP CS Fixer 

Fixed Code-Styles und pushed diese direkt in das Repository. 

Möchte man vermeiden, dass Dateien verändert werden kann der Auto-Commit entfernt und das Composer-Command verändert werden:
```
-   uses: stefanzweifel/git-auto-commit-action@v4
    with:
        commit_message: Apply php-cs-fixer changes
```

Statt `run: composer cs-fix` muss `run: composer cs-dry` genutzt werden.

---

https://github.com/PHP-CS-Fixer/PHP-CS-Fixer

https://github.com/redaxo/php-cs-fixer-config