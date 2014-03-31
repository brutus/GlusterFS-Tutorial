Übersicht
=========

Zuerst mal eine Übersicht, welche Schritte nötig sind, um ein GlusterFS_
**Volumen** zu erstellen:

1. Server starten

2. Volumes erstellen

3. Volumes auf den Client(s) mounten


Server
------

Daemon Installieren
~~~~~~~~~~~~~~~~~~~

Auf *allen* beteiligten Servern (also Geräten die Speicherplatz zu Verfügung
stellen sollen) muss der **Daemon** ``glusterd`` installiert sein und laufen.

.. note::

  Hier musst du natürlich nur Hand anlegen, wenn auf einem der Gerät noch kein
  ``glusterd`` läuft.

Storage Pool
~~~~~~~~~~~~

Dann erstellst du einen **Storage Pool**.

Das ist ziemlich simple: du machst einfach alle beteiligten Server miteinander
bekannt, damit alle Server die Namen aller beteiligten kenne; dezentral und
so.

.. note::

  Sollten sich die Server schon kennen, kannst du dir diesen Schritt sparen.


Volumes
-------

Laufen die Server und kennen sich untereinander, kannst du loslegen und
**Volumes** erstellen; dass ist das, was die *Clients* später benutzen.

Volumes bestehen aus **Bricks** plus Angaben, wie diese Bricks zusammen
arbeiten sollen:

  ``Volume = Bricks + Options``

Aber zuerst brauchst du ja noch die Bricks...

Bricks
~~~~~~

Die oben erwähnten Bricks sind nichts anderes als Pfadangaben zu
physikalischen Speicherplatz in der Form: ``servername:path-to-dir``, zB:

  ``foo.info:/var/downloads``

Das wäre ein Brick der den Inhalt des Verzeichnisses ``/var/downloads`` auf
dem Server ``foo.info`` referenziert.

Hier ist dir vielleicht gerade eine ziemlich cooles Detail aufgefallen: Da
Bricks einfach auf Verzeichnisse verweisen, ist hier so einiges möglich! Kurz:
*alles was du mounten kannst, kann als Brick herhalten*. ZB: USB Platten und
Sticks, Netzwerk-Mounts über NFS_ oder Samba_, *CrypLoops* per EncFS_ oder
eCryptFS_, etc.

.. note::

  Es scheint am besten, wenn die Verzeichnisse *Anfangs* leer sind.
  Jedenfalls habe ich schon mal Komplikationen mit einem nicht leeren
  Verzeichnis bekommen.

Volumes starten
~~~~~~~~~~~~~~~

Hast du deine Bricks zusammen und mit ihnen ein Volume erstellt, brauchst du
es nur noch starten und das war's!


Mounten
-------

Nun kannst du das **Volume** von *Clients* aus mounten und benutzen.

Das geht per ``mount`` Befehl, allerdings stehen zwei Typen zu Wahl:

  * am besten nutzt du den *GlusterFS Client*, Typ: ``glusterfs``

  * alternativ kannst du Volumes auch per *NFS* einbinden, Typ: ``nfs``

.. note::

   Du kannst gemountete Volumes auch wie alle anderen Verzeichnisse per Samba
   freigeben, um sie unter Windows zu nutzen. Und auch sonst alle Sachen mit
   ihnen anstellen, die man mit Mounts eben so machen kann.
