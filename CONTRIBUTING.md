# Unsere Bibliothek Webseite

## Installation und lokales Bauen

1. Hugo installieren: `brew install hugo` (MacOS) oder per Download von https://github.com/gohugoio/hugo/releases
   
1. Als Theme verwenden wir Docsy von Google, welches als Submodul eingebunden ist. Dieses muss initial _geholt_ werden: 
   ```
   git submodule update --init --recursive
   ```

   Um das Theme bzw. die Submodule im Laufe der Zeit zu aktualisieren wird folgender Befehl genutzt:
    ```shell
    git submodule update --recursive
    ```

   Docsy verwendet zusätzliche Libraries, die mit NPM installiert werden 
    ```shell
    npm install
    ``` 

1. `hugo server`
1. Die Website ist dann unter `http://localhost:1313` erreichbar

## _Build_ und _Deployment_

Der Build und das Deployment wird automatisch mittels Github Action durchgeführt, wenn eine Änderung im `main`-Branch erfolgt. Das Ergebnis wird im Branch `gh-pages` angelegt.

Ein lokales Deployment wird durch den Aufruf `hugo` erzeugt. Danach findet man die übersetzte Seite im `public`-Verzeichnis.
 
## Theme
Das aktuelle Theme ist [Docsy](https://github.com/google/docsy)

Dokumentation zu diesem Theme gibt es [hier](https://www.docsy.dev/).
