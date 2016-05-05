# Programmieren 3

DI Michael Fladischer

Folien im reStructuredText-Format für die LV **Programmieren 3** an der [FH
CAMPUS02](https://www.campus02.at/) für den Lehrgang NOWA.

## reStructuredText zu HTML

Um aus den reStructuredText-Dateien HTML-Foliensätze zu erzeugen, wird
[hovercraft](https://pypi.python.org/pypi/hovercraft) benötigt.

Der Aufruf für `mdpress` ist für jede Markdown-Datei wie folgt:

    hovercraft <name>.rst <directory>

This will create the HTML in the folder `<directory>`.

To install `hovercraft` on Debian using `apt`:

    sudo apt install hovercraft
