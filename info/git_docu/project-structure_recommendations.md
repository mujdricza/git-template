In dieser Datei werden Empfehlungen zur Projekt-Dateistruktur gesammelt.
Die Empfehlungen sind i.d.R. nicht verbindlich, dienen jedoch als eine allgemeine (Alternativ)grundlage für das Projekt.

# Inhaltsverzeichnis

1. [Unterordner und wichtige Dateien](#folders)
   1. [Beispielstruktur](#folders_exp)
   1. [Paketstruktur und relative Imports in Python 3](#relative_imports_P3)

---

<a name="folders"></a>
# Unterordner und wichtige Dateien

Eine klare Ordnerstruktur erleichtert das Handhaben des Projekts. Die meisten Aspekte kann man in der Gruppe gerne selbst wählen und ausarbeiten.
 
Lediglich einige Punkte werden als Kriterien genannt:
- Die Ein- und Ausgabedateien sollten von der Implementierung getrennt werden.
- Es soll für die Berichte (Statusbericht und endgültige Ausarbeitung) einen eigenen Ordner/Unterordner geben.
- Für die ARFF-Extraktionsaufgabe soll ein eigener Ordner erstellt werden (im Beispiel `src/weka_HA` mit je einen Unterordner für jedes Gruppenmitglied).
- Es soll nachvollziehbar sein, wer welche Dateien/Dateienteile erstellt/bearbeitet hat (vor Allem bei der Implementierung). Hierfür genügt eine Auflistung der Namen an geeigneten stellen.

<a name="folders_exp"></a>
## Beispielstruktur

Eine Ordnerstruktur für das GitLab-Projekt als Beispiel mit Kommentaren:

```
+ data/
  +- inputs/         -- Eingabedateien -- wenn sehr groß, bitte hier nur einen Auszug laden
  +- models/         -- z.B. wenn Modelle gelernt werden, können diese hier gespeichert werden
  +- outputs/        -- Ausgabedateien wie Dateien mit der automatischen Annotation, ggf. log-Dateien
  +- evaluation/     -- z.B. für Evaluierungsergebnisse (alternativ als Teil des experiments-Ordners)
+ documentation/
  +- discussion/ o. meetings/  -- z.B. für Festhalten der Ideen, Notizen über zu Besprechendes oder Besprochenes
  +- final_report/             -- für die endgültige Ausarbeitung
  +- status_report/            -- für den Statusbericht
+ experiments/       -- z.B. für Festhalten der verschiedenen Experiment-Parameter, -Ausgaben und -Ergebnisse
+ README.txt oder README.md  -- Beschreibt das Projekt
+ related_work/                -- z.B. relevante Artikel, Notizen über ähnliche Arbeiten
+ resources/         -- z.B. externe Wissensbasen, Word Embeddings, ggf. verwendete Bibliotheken
+ src/               -- alle Implementierungen
  +- evaluation/             -- wenn man Evaluierung eigens implementiert
  +- features/ oder attributes/   -- für Attributextraktion
  main.py                   -- Einstiegspunkt für das Projekt; ggf. mehrere Dateien mit spezifischen Namen wie preprocessing.py, train.py, test.py, evaluation.py
  +- models/ o. lerners/    -- Implementierung für Modelle/Lernalgorithmen und ihre Ausführung, wenn notwendig
  +- readers/        -- Einlese-Module
  +- utils/          -- so oder spezifisch z.B. extraction_utils oder data_helper: kleinere Module mit Hilfe-Funktionen
  +- weka_HA/        -- es wird eine Hausaufgabe mit Extraktion eines Features aus euren Eingabedateien ins ARFF-Format geben
     +- xy/          -- für jedes Gruppenmitglied soll einen Unterordner gestaltet werden
     +- ab/
     +- no/
+ tmp/               -- für temporäre Inhalte wie experimentelle Ausgaben, Notizen über zeitweise auftretende Fehler
```

Diese Ordnerstruktur ist ggf. redundant und enthält auch Alternativen. Wenn man mehr mit der Weka-Oberfläche arbeitet, wird man eine andere Struktur bevorzugen, als würde man lieber Weka in der Implementierung einbinden, oder Klassifizierer aus weiteren Tools/Bibliotheken wie z.B. sklearn, nltk, ... zur Hilfe nehmen. 

Die Ordner sind im Beispiel nur grob strukturiert, es könnten durchaus mehrere Ebenen der Ordnereinbettungen geben (z.B. Ordner für verschiedene Experimentreihen). Wenn man die Dateien sinnvoll benennt, könnte auch eine weniger komplizierte Ordnerstruktur ausreichen.

Es ist sinnvoll, für alle Namen eine Sprache zu verwenden -- z.B. nur englische oder nur deutsche Benennungen wählen. In den Dateien selbst kann man durchaus trotzdem eine andere Sprache für Beschreibungen oder Kommentare wählen.

<a name="relative_imports_P3"></a>
## Paketstruktur und relative Imports in Python 3

Wenn man in Python programmiert, ist das Importieren von Paketen und Modulen aus anderen Ordnern mühsam.

Meine Empfehlung, damit man die Problematik mit den Imports schnell erledigen kann, ist die folgende Herangehensweise:

Ich bevorzuge die Implementierungen in einem Ordner `src` zusammenzufassen. Dieser Ordner bedeutet für das Projekt dann die 'Root-Ebene'.

Damit ich alle (!) Import-Anweisungen relativ zu dieser Root-Ebene angeben kann, modifiziere ich in allen Unterordner die (oft automatisch erstellte) Datei `__init__.py`. 

Nehme man an, man möchte aus `readers/conllU_reader.py` eine Methode namens `check_conllU_format()` in `utils/checkers/conll_format_checker.py` verwenden. 

```
#Minimalbeispiel
src
+- readers/
   +- __init__.py
   +- conllU_reader.py              # <-- aufrufendes Modul
+- utils/
   +- checkers/
      +- conll_format_checker.py    # <-- aufgerufenes Modul
```

1) In allen `__init__.py` der eingebetteten Ordnerebenen füge ich folgende Anweisungen ein:

```
#!/usr/bin/env python3

'''
Example for importing python scripts from an other folder.
Source of idea: http://stackoverflow.com/questions/16981921/relative-imports-in-python-3
'''

import os, sys

PACKAGE_ROOT = '..' # adjust for each folder level ("..", "../..", etc.)
SCRIPT_DIR = os.path.dirname(os.path.realpath(os.path.join(os.getcwd(), os.path.expanduser(__file__))))
sys.path.append(os.path.normpath(os.path.join(SCRIPT_DIR, PACKAGE_ROOT)))

# --> Example relative imports in calling modules: 
#       import mypackage.mymodule as mymdl
#       from mypackage.mymodule import mymethod
```

2) In den Skripten, in denen aus anderen Pfaden unter Root etwas importiert werden soll, importiere ich am Anfang des Importbereichs explizit die Initialisierungsinhalte. Danach kann man das Modul im anderen Pfad (unterhalb `src`) einfach mit dem relativen Pfad ansprechen:

```
# within conllU_reader.py

import __init__

import utils.checker.conll_format_checker as cfc
# similarly with 'from ... import':
# from utils.checker import conll_format_checker as cfc

...
cfc.check_conllU_format(...)

```


