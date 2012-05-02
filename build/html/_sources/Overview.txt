Übersicht
=========

Erst mal eine Übersicht, welche Schritte nötig sind um GlusterFS
Volumen zu erstellen.

Daemon Installieren
-------------------

Zuerst einmal musst du auf **allen** beteiligten Servern (also Geräten die
Speicherplatz zu Verfügung stellen sollen) den **Daemon** installieren.

Storage Pool
------------

Dann erstellst du einen **Storage Pool**.

Das ist ziemlich simple, man macht einfach alle beteiligten Server miteinander
bekannt, damit alle Server die Namen aller beteiligten kenne; dezentral und
so.

Sollten sich die Server schon kennen, kannst du dir diesen Schritt sparen.

Volumes
-------

Laufen die Server und kennen sich untereinander, kannst du loslegen und
**Volumes** erstellen; dass ist das, was die Clients später benutzen.

Volumes bestehen aus **Bricks** und dazu Angaben, wie diese Bricks zusammen
arbeiten sollen (zB. Optionen und Raid-Angaben)

Aber zuerst brauchst du ja noch die Bricks...

Bricks
~~~~~~

Die oben erwähnten Bricks sind nichts anderes als Pfadangaben zu
physikalischen Speicherplatz in der Form: ``servername:path-to-dir``

  zB: ``foo.info:/var/downloads``

Das wäre ein Brick der den Inhalt des Verzeichnisses ``/var/downloads`` auf
dem Server ``foo.info`` referenziert.

.. note::

   Es scheint am besten, wenn die Verzeichnisse *Anfangs* leer sind.
   Jedenfalls habe ich schon mal Komplikationen mit einem nicht leeren
   Verzeichnis bekommen.

.. note::

  Hier ist dir vielleicht gerade eine ziemlich cooles Detail aufgefallen: Da
  Bricks einfach auf Verzeichnisse verweisen, ist hier so einiges möglich!
  Kurz: alles was du mounten kannst, kann als Brick herhalten. ZB: CrypLoops
  per EncFS oder ein verschlüsseltes FS per CryptFS, etc. Natürlich auch
  Mounts per Samba, NFS, etc.

Hast du deine Bricks zusammen und mit ihnen ein Volume erstellt, brauchst du
es nur noch starten und das war's!

Mounten
-------

Nun kannst du das Volume von Clients aus mounten und benutzen.

Das geht normal per ``mount`` Befehl, allerdings stehen zwei Typen zu Wahl:

  * am besten nutzt du den *GlusterFS Client*, Typ: ``glusterfs``

  * alternativ kannst du Volumes auch per *NFS* einbinden, Typ: ``nfs``

.. note::

   Du kannst gemountete Volumes auch wie alle anderen Verzeichnisse per Samba
   freigeben, um sie unter Windows zu nutzen.
