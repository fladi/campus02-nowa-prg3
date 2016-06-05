:data-transition-duration: 2000
:skip-help: true
:css: css/campus02.css

.. title: Network Input / Output (I/O) in Java

.. _World Wide Web Consortium (W3C): http://www.w3.org/
.. _IPv6 Verbreitung: http://www.google.ch/ipv6/statistics.html
.. _The Java Tutorials\: Custom Networking: http://docs.oracle.com/javase/tutorial/networking/index.html
.. _Java Platform SE 8\: java.net: http://docs.oracle.com/javase/8/docs/api/index.html?java/net/package-summary.html

.. role:: java(code)
   :language: java

----

Multithreading in Java
======================


----

Agenda
------

 * Einführung in Multithreading

  * Was sind Threads
  * Warum Threads

 * Implemetierungen in Java
 * Verwalten von Threads
 * Synchronisation
 * Deadlocks

----

Was sind Threads?
=================

* Parallele Ausführungsstränge innerhalb eines Prozesses
* Nutzen Daten eines Prozesses
* Sind "leichtgewichtiger" und somit einfacher zu erzeugen als ein Prozess
* Thread-Wechsel ist nicht vorhersehbar

----

Warum Threads?
==============

* Hintergrundprozesse
* Usability
* Ausnutzung von CPUs/ Kernen

----

Threads in Java
===============

Von :java:`Thread` ableiten und :java:`run()` implementieren:

.. code:: java

  public class MyThread extends Thread {

    @Override
    public void run() {
      /* Code */
    }

  }

.. code:: java

  public static void main(String[] args) {
    MyThread t = new MyThread();
    t.start();
  }

----

Threads in Java
===============

Interface :java:`Runnable` implementieren und an :java:`Thread` übergeben:

.. code:: java

  public class MyRunnable implements Runnable {

    @Override
    public void run() {
      /* Code */
    }

  }

.. code:: java

  public static void main(String[] args) {
    Thread t = new Thread(new MyRunnable());
    t.start();
  }

----

Übung: Zwei Threads
===================

Implementieren Sie einen :java:`Thread`, der alle 200 Millisekunden einen
Buchstaben ausgibt.

Benutzen Sie für das Timing :java:`Thread.sleep(200)`. Diese Methode pausiert
die Ausführung eines Threads für die angegebene Anzahl an Millisekunden.

----

Threads verwalten
=================

Threads stoppen sich selbst, wenn ihre :java:`run()` Methode beendet ist. Sollte
ein vorzeitiger Abbruch des Threads gewünscht sein, so muss dies im Ablauf von
:java:`run()` berücksichtigt werden.

----

Beispiel: "Höfliches" Stoppen
=============================

.. code:: java

  public class Worker implements Runnable {

    private boolean isRunning = true;

    public void requestShutDown() {
      isRunning = false;
    }

    @Override
    public void run() {
      while( isRunning ) {
        /* Code */
      }
    }
  }

----

Beispiel: "Unhöfliches" Stoppen
===============================

.. code:: java

  Thread t = new MyThread();
  t.start();
  Thread.sleep(1000);
  t.stop();

* Beendet den :java:`Thread` *gewaltsam*
* Ist **deprecated**: Kann in künftigen Java-Versionen entfallen
* Allerdings: Keine wirkliche Alternative

----

Beispiel: Kombiniertes Stoppen
==============================

.. code:: java

  Thread t = new MyThread();
  t.start();
  t.requestShutDown();
  t.join(5000); // 5 Sekunden
  t.stop();

----

Synchronisation
===============

Wird für **kritische Sektionen** benötigt. Dies sind Codebereiche, die **nicht
gleichzeitig von mehreren Threads** ausgeführt werden dürfen, weil sich die
Threads "ansonsten in die Quere kommen würden".

----

Was passiert hier?
==================

.. figure:: figures/threads-parallel-problem.svg
   :alt: Probleme beim parallelen Lesen/Schreiben mit Threads

----

Beispiel: Synchronized
======================

.. code:: java

  private static Object lock = new Object();

  public void run() {
    /* Unkritischer Code */
    synchronized(lock) {
      /* Kritischer Codde */
    }
    /* Unkritischer Code */
  }

Währende der :java:`synchronized`-Block ausgeführt wird, darf kein anderer
Thread, der sich auf das selbe **Sperr-Objekt** bezieht, diesen betreten.

Jedes Objekt kann ein Sperr-Objekt sein.

----

Beispiel: Synchronisierte Methoden
==================================

Eine Methode mit dem :java:`synchronized`-Schlüssewort ...

.. code:: java

  public synchronized void doStuff() {
    /* Kritische Sektion = Ganze Methode */
  }

... ist das selbe wie eine Methode, bei der der gesamte Code in einem
:java:`synchronized`-Block verpackt ist.

.. code:: java

  public void doStuff() {
    synchronized(this) {
      /* Kritische Sektion = Ganze Methode */
    }
  }

