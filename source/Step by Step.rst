Step by Step
============

Nach der Übersicht schauen wir uns nun die konkreten Befehle an.

.. note::

   Das ``#`` vor den Befehlen muss *nicht* mit eingegeben werden! Es steht da,
   um eine Root-Shell zu kennzeichnen. Bist du also nicht als ``root``
   unterwegs, musst den den Befehlen ein ``sudo`` voranstellen.


GlusterFS installieren
----------------------

``# apt-get install glusterfs-server``

Willst du den Daemon automatisch bei jeden Boot starten:

``# update-rc.d glusterd defaults``

Diesen Schritt musst du auf *jeden* Server durchführen! Alle weiteren nur auf
einem der Server (egal welcher). Die Konfiguration wird automatisch unter den
Servern verteilt.


Storage Pool erstellen
----------------------

Davon ausgehend, das du auf ``s1.test`` bist, musst du ``s{2,3,4}.test``
bekannt machen:

``# gluster peer probe s2.test``

``# gluster peer probe s3.test``

``# gluster peer probe s4.test``

Nun kennen sich alle Server untereinander und geben Konfigurationsanweisungen,
etc. weiter. Ob alles geklappt hat kannst du sicherheitshalber noch schnell
überprüfen:

``# gluster peer status``


Volume erstellen
----------------

Wir kennen die Servername und die Verzeichnisse, damit haben wir unsere
**Bricks** zusammen:

- ``s1.test:/shared``
- ``s2.test:/shared``
- ``s3.test:/shared``
- ``s4.test:/shared``

Das **Volume** ist schnell erstellt:

``# gluster volume create sharedstuff replica 2 s1.test:/shared s2.test:/shared s3.test:/shared s4.test:/shared``

.. note::

   Der ``replica 2`` part sorgt dafür das jeweils 2 Verzeichnisse gespiegelt
   werden. Mit ``stripe 2`` oder ``stripe 4`` hättest du statt dessen *jede*
   *einzelne* Datei über 2 bzw. alle 4 Verzeichnisse aufgeteilt.

   Wenn du **replica** oder **stripe** benutzt, _musst_ du darauf achten, dass
   du ein Vielfaches der angegebenen Anzahl an Bricks nutzt. Bei ``replica 2``
   zB. also 2, 4, 6, etc. Bricks.

   Lässt du den Part ganz weg, wird weder *striping* noch *mirroring* benutzt.
   Die Dateien werden einfach auf die verschiedenen Platten verteilt (JBOD
   like).

Nun muss das Volume nur noch aktiviert werden:

``# gluster volume start sharedstuff``

Und auch hier checken wir noch schnell ob alles geklappt hat:

``# gluster volume info sharedstuff``

Hat alles geklappt ist die serverseitige Einstellung erledigt.
Nun kannst du loslegen und das Volume benutzen.


Volume mounten
--------------

Auf jeden Client musst du die folgenden Schritte ausführen (statt dem
GlusterFS Client kannst du auch NFS nutzen, das lass ich hier aber mal aus):


- FUSE muss installiert sein:

  - Prüfe das mit ``# dmesg | grep -i fuse``

  - bekommst du *keine* Antwort wie ``... fuse init (API version 7.13)``
    musst du es aktivieren:

  - ``# modprobe fuse``

- Der GlusterFS Client musst installiert sein:

  ``# apt-get install glusterfs-client``

- Nun kannst du das Volume mounten (zB. nach ``/var/shared``):

  ``# mount -t glusterfs s1.test:/sharedstuff /var/shared``

  Das geht natürlich auch über einen Eintrag in ``/etc/fstab``:

  ``s1.test:/sharedstuff /var/shared glusterfs defaults,_netdev 0 0``

  .. note::

     ``_netdev`` sorgt dafür, das es erst nach Initialisierung des Netzwerks
     gemounted wird.

Der beim Mounten angegebene Server (``s1.test``) wird *nur einmal* benutzt, um
die Konfiguration zu holen, danach wird automatisch per Load-Balancing, etc.
einer der am Volume beteiligten Server benutzt. Dh. auch, dass du beim
``mount`` Befehl einen beliebigen Server aus dem Pool angeben kannst, auch
einen, der nicht am zu mountenden Volume beteiligt ist.
