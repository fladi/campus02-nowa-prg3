:data-transition-duration: 2000
:skip-help: true
:css: css/campus02.css

.. title: File Input / Output (I/O) in Java

.. _World Wide Web Consortium (W3C): http://www.w3.org/

.. role:: java(code)
   :language: java

----

File Input / Output (I/O) in Java
=================================


----

Agenda
------

 * Einführung in File I/O mit Java
 * Unterscheidung Binär- und Textdaten
 * Lesen und Schreiben von Dateien

   * mit :java:`InputStream` und :java:`OutputStream`
   * mit :java:`Reader` und :java:`Writer`

 * Behandlung von :java:`IOException` und try-with-resources
 * Zeichenkodierungen

----

Input/Output
============

*Lesen und Schreiben von Daten*

Woher bzw. wohin?

* aus Dateien auf der Festplatte / USB-Stick
* von einem Server im Netzwerk
* aus dem Arbeitsspeicher
* aus einer Datenbank

----

Was wird gelesen bzw. geschrieben?
----------------------------------

* **Binärdaten**: Bytes bzw. "Zahlen"
* **Text**: Characters bzw. Zeichen


----

Beispiel 1
==========

Ansicht im Hexeditor:

.. code:: hexdump

  00000000: 8950 4e47 0d0a 1a0a  .PNG....
  00000008: 0000 000d 4948 4452  ....IHDR
  00000010: 0000 0100 0000 0100  ........
  00000018: 0803 0000 006b ac58  .....k.X
  00000020: 5400 0003 0050 4c54  T....PLT
  00000028: 45af ab9b a8a5 93a3  E.......
  00000030: a193 a49e 8ca0 9c87  ........
  00000038: 9d9b 899b 9b8e 9799  ........
  00000040: 8894 978a 9998 8294  ........
  00000048: 9683 9593 8794 947f  ........

----

Worum handelt es sich?
----------------------

Um Binärdaten, konkret um ein PNG-Bild.

Was ist an dieser Datei von Interesse?
--------------------------------------

Die einzelnen Bytes (`00` bis `ff`).

----


Beispiel 2
==========

Ansicht im Hexeditor:

.. code:: hexdump

  00000000: 6162 6364 6566 6768  abcdefgh
  00000008: 696a 6b6c 6d6e 6f70  ijklmnop
  00000010: 7172 7374 7576 7778  qrstuvwx
  00000018: 797a 4142 4344 4546  yzABCDEF
  00000020: 4748 494a 4b4c 4d4e  GHIJKLMN
  00000028: 4f50 5152 5354 5556  OPQRSTUV
  00000030: 5758 595a 0a         WXYZ.

----

Worum handelt es sich?
----------------------

Um Text, konkret "abcdefg...".

Was ist an dieser Datei von Interesse?
--------------------------------------

Die einzelnen Zeichen aus denen der Text besteht.

----

Datenquellen
============

* Konsole

  * :java:`System.in`
  * :java:`System.out`

* Dateien

  * :java:`File`-Klasse
  * :java:`Stream`-Klassen

* Netzwerk

  * :java:`Socket`-Klasse


----

File: Klasse
============

* Bildet plattformunabhängig eine Datei oder ein Verzeichnis ab
* Stellt Methoden bereit um ...

  * ... Eigenschaften der Datei auszulesen
  * ... Zugriffsberechtigungen zu prüfen
  * ... Dateien zu verwalten


----

File: Konstruktoren
-------------------

.. code:: java

  /* Dateiname als String */
  public void File(String path) { ... };
  /* Verzeichnis und Dateiname separat als String */
  public void File(String dir, String name) { ... };
  /* Verzeichnis als File und Dateiname als String */
  public void File(File dir, String name) { ... };

----

File: Wichtige Methoden
-----------------------

.. code:: java

  /* Prüfen ob Datei gelesen werden kann */
  public boolean canRead() { ... };
  /* Prüfen ob Datei geschrieben werden kann */
  public boolean canWrite() { ... };
  /* Prüfen ob Datei überhaupt existiert */
  public boolean exists() { ... };
  /* Länge der Datei in Byte */
  public long length() { ... };

----

Demo: File
==========

----

Package: java.io
================

.. figure:: figures/java-io-api.svg
   :alt: Aufteilung der java.io API

----

Byte-orientierte Streams
========================

.. figure:: figures/java-stream-api-bytes.svg
   :alt: Byte-orientierte Streams

----

java.io.InputStream
===================

* Ist sehr Low-Level
* Ist Closeable
* Ist eine abstrakte Klasse
* Hat verschiedene konkrete Subklassen

  * Für verschiedene Datenquellen: :java:`FileInputStream`, :java:`ByteArrayInputStream`, :java:`AudioInputStream`
  * Für effektiveren Zugriff: :java:`BufferedInputStream`

----

InputStream: Klassenhierarchie
------------------------------

.. figure:: figures/java-inputstream-api.svg
   :alt: Klassenhierarchie für java.io.InputStream


----

InputStream: Implementierungen
------------------------------

.. csv-table::
  :header: "**Klasse**", "**Beschreibung**"

  ":java:`InputStream`", "Basisklasse zum Lesen für byteorientierte Streams."
  ":java:`FileInputStream`", "Liest aus Dateien."
  ":java:`ObjectInputStream`", "Stellt Methoden zur Verfügung, mit denen Java-Objekte gelesen werden können."
  ":java:`FilterInputStream`", "Lässt direkt beim Einlesen das Bearbeiten (z.B.: Entschlüsselung von Daten) von Dateien zu und dienst als Basisklasse für weitere Stream-Klassen."
  ":java:`BufferedInputStream`", "Klasse, die über einen optimierten Zugriff (Puffer) auf Dateien verfügt."

----

Beispiel: FileInputStream
-------------------------

.. code:: java
  :number-lines: 1

  /* Datei mit File-Konstruktor öffnen */
  File file = new File("pfad/zu/der/datei.txt");
  /* FileInputStream zum Lesen der Datei öffnen */
  FileInputStream fis = new FileInputStream(file);
  int byteRead;
  /* Byte für Byte aud FileInputStream lesen */
  while ((byteRead = fis.read()) != -1) {
    /* Gelesene Zahl (int) in Zeichen umwandeln */
    char[] ch = Character.toChars(byteRead);
    System.out.print(ch[0]);
  }
  /* FileInputStream muss geschlossen werden */
  fis.close();

----

Übung 1: Konsolen-Input
-----------------------

Schreiben Sie ein Programm, welches von :java:`System.in` (Vergleichbar mit
:java:`FileInputStream`) solange Zeichen einließt, bis der Benutzer ein "x" eingibt.

Verwenden Sie das :java:`FileInputStream`-Beispiel als Vorlage.

----

java.io.OutputStream
====================

* Ist wiederum sehr Low-Level
* Ist Closeable und Flushable
* Ist eine abstrakte Klasse
* Hat verschiedene konkrete Subklassen für:

  * Verschiedene Datensenken

    * :java:`FileOutputStream`, :java:`ByteArrayOutputStream`

  * Effektiveren Zugriff

    * :java:`FilterOutputStream`, :java:`PrintStream`, :java:`BufferedOutputStream`


----

OutputStream: Klassenhierarchie
-------------------------------

.. figure:: figures/java-outputstream-api.svg
   :alt: Klassenhierarchie für java.io.OutputStream


----

OutputStream: Implementierungen
-------------------------------

.. csv-table::
  :header: "**Klasse**", "**Beschreibung**"

  ":java:`OutputStream`", "Basisklasse zum Schreiben für byteorientierte Streams."
  ":java:`FileOutputStream`", "Schreibt in Dateien."
  ":java:`ObjectOutputStream`", "Stellt Methoden zur Verfügung, mit denen Java-Objekte geschrieben werden können."
  ":java:`FilterOutputStream`", "Manipuliert direkt beim Schreiben den Output (z.B.: Verschlüsselung) und dient als Basisklasse für weitere Stream-Klassen."
  ":java:`BufferedOutputStream`", "Klasse, die über eine optimierten Zugriff (Puffer) auf Dateien verfügt."
  ":java:`PrintStream`", "Stellt Methoden zur zeilenorientierten Ausgabe (print / println) zur Verfügung."

----

Beispiel: FileOutputStream
--------------------------

.. code:: java
  :number-lines: 1

  /* Datei mit File-Konstruktor öffnen */
  File file = new File("pfad/zu/der/datei.txt");
  /* Datei mit FileOutputStream zum Schreiben öffnen */
  FileOutputStream fos = new FileOutputStream(file);
  /* Zu schreibenden Inhalt festlegen */
  String outputText = "Hallo Datei, dies ist ein Text.";
  /* String in einzelne Zeichen zerlegen */
  for (char c : outputText.toCharArray()) {
    /* Zu (int) umwandeln und in Datei schreiben */
    fos.write((int) c);
  }
  /* Eigentlichen Schreibvorgang durchführen */
  fos.flush();
  /* FileOutputStream schliessen */
  fos.close();

----

Kombinieren von Klassen
=======================

:java:`java.io`-Klassen können miteinander kombiniert werden.

Beispielsweise nimmt :java:`BufferedInputStream` ein :java:`InputStream`-Objekt auf.
Dieses kann ein :java:`FileInputStream` oder auch ein anderer :java:`InputStream`
sein.

----

Beispiel: BufferedInputStream
-----------------------------

.. code:: java

  /* Datei mit File-Konstruktor öffnen */
  File file = new File("pfad/zu/der/datei.txt");
  /* FileInputStream zum Lesen der Datei öffnen */
  FileInputStream fis = new FileInputStream(file);
  /* BufferedInputStream nimmt den FileInputStream auf */
  BufferedInputStream bis = new BufferedInputStream(fis);
  int byteRead;
  /* Byte für Byte aud FileInputStream lesen */
  while ((byteRead = bis.read()) != -1) {
    /* Gelesene Zahl (int) in Zeichen umwandeln */
    char[] ch = Character.toChars(byteRead);
    System.out.print(ch[0]);
  }
  /* FileInputStream muss geschlossen werden */
  bis.close();

----

Übung 2: Objekt schreiben und lesen
-----------------------------------

Schreiben Sie ein Programm, das ein :java:`String`-Objekt mit dem Inhalt "Hallo
Datei, dies ist ein Text." in eine Datei "object.dat" schreibt und anschließend
aus dieser wieder ausliest und auf die Konsole schreibt.

Verwenden Sie die Klassen :java:`FileOutputStream` und
:java:`ObjectOutputStream` sowie :java:`FileInputStream` und
:java:`ObjectInputStream`.

Mit der Methode :java:`writeObject(...)` können Sie ein Objekt in die Datei
schreiben und mit :java:`readObject()` wieder auslesen. Beim Lesen müssen Sie
das Ergebnis in einen :java:`String` casten.

Betrachten Sie die Datei in einem Editor (Notepad, Notepad++).

----

Inhalt im Hexeditor
-------------------

.. code:: hexdump

  00000000: aced 0005 7400 1f48  ....t..H
  00000008: 616c 6c6f 2044 6174  allo Dat
  00000010: 6569 2c20 6469 6573  ei, dies
  00000018: 2069 7374 2065 696e   ist ein
  00000020: 2054 6578 742e        Text.

Die Steuerzeichen vor dem Text werden von :java:`ObjectInputStream` benutzt, um
Klasse und Größe des zu lesenden Objekts zu ermitteln.

----

Zeichenorientierte Streams
==========================

.. figure:: figures/java-stream-api-chars.svg
   :alt: Zeichenorientierte Streams

----

Zeichenorientierte Streams
--------------------------

* Lesen / Schreiben Unicode Zeichen
* Zeichenlänge hängt von der Kodierung des Zeichens (Typ :java:`char`) ab

  * UTF-8
  * UTF-16 (Java intern)
  * Latin-1 (ISO 8859-15)
  * usw.

* Vererbungshierachie ähnelt jener der byteorientierten Streams

  * :java:`Reader` (statt :java:`InputStream`)
  * :java:`Writer` (statt :java:`OutputStream`)

----

java.io.Reader
==============

* Ist sehr Low-Level
* Kennt keine Zeilen
* Ist eine abstrakte Klasse
* Hat verschiedene konkrete Subklassen für

  * Verschiedene Datenquellen

    * :java:`FileReader`, :java:`StringReader`, :java:`InputStreamReader`

  * Effektiveren Zugriff

    * :java:`BufferedReader`

----

Reader: Klassenhierarchie
-------------------------

.. figure:: figures/java-reader-api.svg
   :alt: Klassenhierarchie für java.io.Reader

----

Reader: Implementierungen
-------------------------

.. csv-table::
  :header: "**Klasse**", "**Beschreibung**"

  ":java:`Reader`", "Basisklasse zum Lesen von zeichenorientierten Strömen."
  ":java:`BufferedReader`", "Ein Puffer wird für die Leseoperation verwendet."
  ":java:`InputStreamReader`", "Bietet die Möglichkeit einen byteorientieren Stream in einen zeichenorientieren Stream zu koppeln."
  ":java:`FileReader`", "Leitet von InputStreamReader ab und greift direkt auf Dateien zu."
  ":java:`StringReader`", "Mit dieser Klasse kann auf einen String, wie auf einen Stream zugegriffen werden."

----

Beispiel: BufferedReader
------------------------

.. code:: java
  :number-lines: 1

  /* Datei mit File-Konstruktor öffnen */
  File file = new File("pfad/zu/datei.txt");
  /* Mit FileReader zum Lesen öffnen */
  FileReader fr = new FileReader(file);
  /* BufferedReader zum Lesen von Zeilen */
  BufferedReader br = new BufferedReader(fr);
  String line;
  /* Zeile für Zeile auslesen und ausgeben */
  while ((line = br.readLine()) != null) {
    System.out.println(line);
  }
  /* BufferedReader schliessen */
  br.close();

----

Übung 3: Zeilen lesen
---------------------

Schreiben Sie ein Programm, das zeilenweise Tastatureingaben auf die Konsole
schreibt, bis das Wort "STOP" eingegeben wird.

Verwenden Sie dazu den :java:`InputStream` der über :java:`System.in`
bereitgestellt wird, sowie die Klassen :java:`InputStreamReader` und
:java:`BufferedReader`.

----

java.io.Writer
==============

* Ist wiederum sehr Low-Level
* Kennt keine :java:`print`-Methoden
* Ist eine abstrakte Klasse
* Hat verschiedene konkrete Subklassen für:

  * Verschiedene Datensenken

    * :java:`FileWriter`, :java:`StringWriter`, :java:`OutputStreamWriter`

  * Effektiveren Zugriff

    * :java:`BufferedWriter`, :java:`PrintWriter`

----

Writer: Klassenhierarchie
-------------------------

.. figure:: figures/java-writer-api.svg
   :alt: Klassenhierarchie für java.io.Writer

----

Writer: Implementierungen
-------------------------

.. csv-table::
  :header: "**Klasse**", "**Beschreibung**"

  ":java:`Writer`", "Basisklasse zum Schreiben von zeichenorientierten Strömen."
  ":java:`BufferedWriter`", "Ein Puffer wird für die Schreiboperationen verwendet."
  ":java:`OutputStreamWriter`", "Bietet die Möglichkeit einen byteorientieren Stream in einen zeichenorientieren Stream zu koppeln."
  ":java:`FileWriter`", "Leitet von :java:`OutputStreamWriter` ab und schreibt direkt in Dateien."
  ":java:`StringWriter`", "Mit dieser Klasse kann auf einen String, wie auf einen Stream zugegriffen werden."
  ":java:`PrintWriter`", "Bietet Methoden um einfache Datentypen im Text auszugeben."

----

Beispiel: PrintWriter
---------------------

.. code:: java

  /* Datei mit File-Konstruktor öffnen */
  File file = new File("pfad/zu/datei.txt");
  /* Mit FileWriter zum Schreiben von Zeichen öffnen */
  FileWriter fw = new FileWriter(file);
  /* PrintWriter zum formatierten Schreiben */
  PrintWriter pw = new PrintWriter(fw);
  /* Zwei Zeilen schreiben */
  pw.println("Hallo Welt!");
  pw.println("Dies ist die zweite Zeile.");
  /* Eigentlichen Schreibvorgang durchführen */
  pw.flush();
  /* PrintWriter schliessen */
  pw.close();

----

Übung 4: FileWriter
-------------------

Schreiben Sie ein Programm, das Namen von Studierenden und ihre Noten von der
Konsole einliest und anschließend als "noten.csv"-Datei speichert.

Die Eingabe der Daten soll in folgendem Format erfolgen:

.. code::

    Max Musterman: 1
    Maria Musterfrau: 1
    Joe Sixpack: 5
    ...
    STOP

Die Eingabe endet, wenn das Wort "STOP" eingegeben wird. Verwenden Sie die
Klassen :java:`BufferedReader`, :java:`InputStreamReader` und
:java:`FileWriter`.

----

Exceptions
==========

Problem bei I/O: es kann potentiell etwas schief gehen

* Datei existiert nicht
* Festplatte voll/defekt
* Netzwerkverbindung bricht ab
* ...

----

Exceptions
----------

Versuchen Sie herauszufinden, welche Exceptions in den bisherigen Beispielen
geworfen werden können.

Stellen Sie Ihre Beispiele so um, dass eventuell auftretende Exceptions direkt
behandelt und nicht nach außen geworfen werden.

----

Beispiel: Exceptions
--------------------

.. code:: java

  File file = new File("pfad/zu/datei.txt");
  BufferedReader br = null;
  try {
    br = new BufferedReader(new FileReader(file));
  } catch (FileNotFoundException e) {
    e.printStackTrace();
  } finally {
    try {
      br.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }

----

Java 7: try-with-resources
--------------------------

* Alle :java:`AutoCloseables` können bereits im :java:`try`-Block geöffnet
  werden.
* Diese werden am Ende des Blocks automatisch geschlossen, somit is kein
  :java:`finally` nötig.
* Gibt es keinen :java:`catch`-Block und treten im :java:`try`-Block und beim Schließen
  Exceptions auf, so wird die erste nach außen geworfen, die zweite wird
  unterdrückt.
* Nach dem Schlüsselwort :java:`try` wird in runden Klammern die kritische
  Ressource deklariert und initialisiert.
* Der :java:`finally`-Block zum Schließen kann entfallen.

----

Beispiel: try-with-resources
----------------------------

.. code:: java

  File file = new File("pfad/zu/datei.txt");
  try (BufferedReader br = new BufferedReader(new FileReader(file))) {
    /* Daten einlesen */
  } catch (IOException e) {
    e.printStackTrace();
  }

----

Character Encoding
==================

Bei Textdateien werden Zeichen (Buchstaben) bestimmten Zahlen zugeordnet, z.B.:

* A = 65 = 0x41
* B = 66 = 0x42
* ...

Diese Zuweisungen werden "Zeichenkodierung" genannt. In Java sind die
verschiedenen Zeichenkodierungen als :java:`Charset` bekannt.

----

Beispiel: Alphabet
------------------

Zeichenkodierung mit deutschem Alphabet

.. code::

  aäbcdefghijklmnoöpqrsßtuüvwxyz
  AÄBCDEFGHIJKLMNOÖPQRSßTUÜVWXYZ

----

ISO-8859-15
-----------

.. code:: hexdump

  00000000: 61e4 6263 6465 6667  a.bcdefg
  00000008: 6869 6a6b 6c6d 6e6f  hijklmno
  00000010: f670 7172 73df 7475  .pqrs.tu
  00000018: fc76 7778 797a 0a41  .vwxyz.A
  00000020: c442 4344 4546 4748  .BCDEFGH
  00000028: 494a 4b4c 4d4e 4fd6  IJKLMNO.
  00000030: 5051 5253 df54 55dc  PQRS.TU.
  00000038: 5657 5859 5a0a       VWXYZ.

Größe: **62** Bytes

----

UTF-16
------

.. code:: hexdump

  00000000: 6100 e400 6200 6300  a...b.c.
  00000008: 6400 6500 6600 6700  d.e.f.g.
  00000010: 6800 6900 6a00 6b00  h.i.j.k.
  00000018: 6c00 6d00 6e00 6f00  l.m.n.o.
  00000020: f600 7000 7100 7200  ..p.q.r.
  00000028: 7300 df00 7400 7500  s...t.u.
  00000030: fc00 7600 7700 7800  ..v.w.x.
  00000038: 7900 7a00 0a00 4100  y.z...A.
  00000040: c400 4200 4300 4400  ..B.C.D.
  00000048: 4500 4600 4700 4800  E.F.G.H.
  00000050: 4900 4a00 4b00 4c00  I.J.K.L.
  00000058: 4d00 4e00 4f00 d600  M.N.O...
  00000060: 5000 5100 5200 5300  P.Q.R.S.
  00000068: df00 5400 5500 dc00  ..T.U...
  00000070: 5600 5700 5800 5900  V.W.X.Y.
  00000078: 5a00 0a00

Größe: **124** Bytes

----

UTF-8
-----

.. code:: hexdump

  00000000: 61c3 a462 6364 6566  a..bcdef
  00000008: 6768 696a 6b6c 6d6e  ghijklmn
  00000010: 6fc3 b670 7172 73c3  o..pqrs.
  00000018: 9f74 75c3 bc76 7778  .tu..vwx
  00000020: 797a 0a41 c384 4243  yz.A..BC
  00000028: 4445 4647 4849 4a4b  DEFGHIJK
  00000030: 4c4d 4e4f c396 5051  LMNO..PQ
  00000038: 5253 c39f 5455 c39c  RS..TU..
  00000040: 5657 5859 5a0a       VWXYZ.

Größe: **70** Bytes

----

Zeichenkodierung: Unterschiede
------------------------------

* Die letzten drei Beispiele:

  * einmal 62, einmal 124, einmal 70 Bytes
  * beinhalten den gleichen Text

* Worin unterscheiden sie sich?

  * in der Darstellung der Zeichen

    * ein Byte pro Zeichen
    * zwei Bytes pro Zeichen
    * ein Byte für gewisse Zeichen, zwei Bytes für andere

----

Ein Byte pro Zeichen
--------------------

* 256 verschiedene Zeichen darstellbar
* Beispiele:

  * ASCII
  * ISO 8859-15 (auch Latin 1)
  * Windows-1252 (Westeuropäische Sprachen)
  * Windows-1250 (Zentral- und Osteuropa)
  * ...

----

Unicode
-------

* Deckt den Großteil der weltweit verwendeten Schriftzeichen ab.
* Internationaler Standard
* Seit 1991
* Derzeit Unicode 8.0.0 (Juni 2015)

----

Unicode Transformation Format
-----------------------------

* UTF-16

  * (meist) zwei Bytes pro Zeichen
  * verwendet für z.B. Java :java:`String`

* UTF-8

  * variable Länge
  * ein Byte für ASCII Zeichen
  * bis zu 4 Bytes für andere Zeichen
  * Sehr häufig im WWW verwendet

----

Beispiel: UTF-8
---------------

.. code:: java

  File file = new File("umlaute.txt");
  try (PrintWriter pr = new PrintWriter(new OutputStreamWriter(new FileOutputStream(file), StandardCharsets.UTF_8))) {
    pr.println("Köche machen Müsli mit Äpfel");
  } catch (IOException e) {
    e.printStackTrace();
  }

.. code:: hexdump

  00000000: 4bc3 b663 6865 206d  K..che m
  00000008: 6163 6865 6e20 4dc3  achen M.
  00000010: bc73 6c69 206d 6974  .sli mit
  00000018: 20c3 8470 6665 6c0a   ..pfel.
