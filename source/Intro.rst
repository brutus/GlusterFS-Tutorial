Intro
=====

Ein **GlusterFS Volume** fast Speicherplatz von beliebigen Servern zusammen.

Du musst dafür *keine Partitionen* auf den Servern anlegen, ein *leeres
Verzeichnis* reicht. Es ist auch egal, ob die Verzeichnisse auf den Servern
gleich heißen, gleich viel Speicherplatz frei haben, usw.

  Hast du zB. ein Verzeichnis mit 80 GB auf einem Server und eins mit 40 GB
  auf einem anderen, kannst du daraus ein 120 GB Volume machen.

Außerdem beherrscht GlusterFS_ auch **Striping** und **Mirroring**.

**Striping** entspricht ~ ``RAID 0``, dadurch wird jede einzelne Datei auf
mehrere Server aufgeteilt, damit sie schneller *geschrieben* und *gelesen*
werden kann (nett zB. bei vielen großen Dateien). **Mirroring** entspricht ~
``RAID 1``, es spiegelt die Dateien über mehrere Server (*Datensicherheit* und
*schnellere Lese-Zugriffe*).

Beispiel
--------

In dem Beispiel in diesem Tutorial gehe ich von 4 Servern aus:
``s{1,2,3,4}.test``, auf denen jeweils 250 GB frei sind (und zwar in dem
Verzeichnis ``/shared``).

Wir basteln daraus ein **Volume**, das jeweils 2 Verzeichnisse untereinander
*spiegelt* und die beiden *Spiegel-Sets* danach zusammen fasst:

* Dh. die Verzeichnisse ``s1.test:/shared`` und ``s2.test:/shared`` sind
  untereinander gespiegelt (ergibt ~ 250 GB für Daten).

* Gleiches gilt für die Verzeichnisse ``s3.test:/shared`` und
  ``s4.test:/shared``.

* Die beiden Sets werden dann zu einem gemeinsamen Volumen mit 500 GB
  zusammengefasst.

.. note::

   Dass die Server und Verzeichnisse gleich heißen und in den
   Verzeichnissen auch noch gleich viel Platz frei ist, soll nur
   den Text einfacher machen; für GlusterFS_ ist es wie gesagt egal.

.. important::

   Solange ein Gerät per URL oder IP erreichbar ist *und* auf dem Gerät der
   **Gluster FS Daemon** läuft, kann es an **Volumes** teilnehmen.
