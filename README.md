* Format d'une trame Ethernet


  01234567 01234567 01234567 01234567 01234567 01234567
 +--------+--------+--------+--------+--------+--------+
 | Adresse MAC Destination (6 octets)                  |
 +--------+--------+--------+--------+--------+--------+
 | Adresse MAC Source      (6 octets)                  |
 +--------+--------+--------+--------+--------+--------+
 | Type de trame(*)|.................................. .
 +--------+--------+.................................. .
 . ................................................... .
 |............. Données (de 46 a 1500 octets)......... .
 . ................................................... .
 . .........................+--------+--------+--------+
 |..........................| FCS  (**)                |
 +--------+--------+--------+--------+--------+--------+

(*) Valeur pour le type de trame :
(hexadécimal)
0800 : IP (Internet protocol)
0806 : ARP (Address Resolution Protocol)
8035 : RARP (Reverse Address Resolution Protocol)

(**) FCS : Frame Check Sequence - Somme de contrôle de la trame
Ce champs est normalement généré par la carte ethernet.



* Format d'un paquet ARP (RFC 826)

    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |           Matériel            |    Protocole                  |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |Taille AdrPhys |Taille AdrProto|    Opération                  |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |--Adresse MAC source-------------------------------------------|
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |--Adresse MAC source (suite)---|*****Adresse IP source*********|
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |**Adresse IP source (suite)****|--Adresse MAC destination------|
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |---Adresse MAC destination (suite)-----------------------------|
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |**Adresse IP destination***************************************|
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

Remarque : "Type de trame" dans l'entête ethernet (pas représentée ici) : 0806

Matériel : 1 pour ethernet
Protocole : 0800 pour IP
Taille AdrPhys : Taille de l'adresse physique, normalement 6 
car une adresse MAC a 6 octets de long
Taille AdrProto : Taille de l'adresse protocole, normalement 4, 
car une adresse IP fait 4 octets

Opération : prend les valeurs suivantes :
1 Requête ARP
2 Réponse ARP




* Format de l'entête IP (RFC 791)

    0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 3 3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |0  Ver |  IHL  |1    TOS       |2         Total 3  Length      |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |4        Ident  5              |6 Flg|          7  Frag Offset |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |8       TTL    |9   Protocol   |10       Header 11  Chksum     |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |12              13     Src      14    Addr      15             |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |16              17     Dst      18    Addr      19             |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |20              21  Options     22             |23  Padding    |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+

* Version:  (4 bits) Normalement égal à 4 
* IHL:  (4 bits) Internet Header Length. Longueur de l'entête en nombre
  de mots de 32 bits; au minimum égal à 5. La taille de l'entête est
  variable à cause des options.
* TOS : Type of Service:  (1 octet)
* Total Length:  (2 octets) Longueur totale du datagramme IP en octets, qui
  inclus l'entête et les données. Si il y a plusieurs fragments, cette
  taille ne concerne que le fragment courant.
* Identification:  (2 octets) Une valeur permettant de réassembler
  les fragments s'il y a lieu.
* Flags:  (3 bits)
    Various Control Flags.
      Bit 0: reserved, must be zero
      Bit 1:  0 = May Fragment,  1 = Don't Fragment.
      Bit 2:  0 = Last Fragment, 1 = More Fragments.
* Fragment Offset:  (13 bits) Position du fragment. Mesuré en bloc de
  8 octets. Le premier fragment a un offset à 0.
* TTL: Time to Live:  (1 octet) Indique le temps maximum qu'un
  datagramme IP est autorisé à rester sur le réseau. Il est décrémenté
  à chaque passage dans un routeur. Lorsqu'il vaut 0, le datagramme
  est détruit.
* Protocol:  (1 octet) Type de protocole transporté :
  6 pour TCP, 17 pour UDP, 1 pour ICMP
* Header Checksum:  (2 octets) Somme de contrôle calculée sur l'entête. 
* Source Address:  (4 octets) Adresse IP source
* Destination Address:  (4 octets) Adresse IP destination
* Options :  documentées dans la rfc 791



* Format d'un paquet ICMP (RFC 792)

Remarque : un paquet ICMP possède une entête IP

    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |   TYPE ICMP   |     Code      |          Checksum             |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |         utilisé en fonction du message                        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |      Données propres à un message ICMP                     ....
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

Dans l'entête IP :
Version:  (4 bits) Normalement égal à 4 
IHL:  (4 bits) Internet Header Length. (cf IP)
Type of Service:  (1 octet) égal 0
Protocol:  (1 octet) Type de protocole transporté : égal à 1 pour ICMP
Source Address:  (4 octets) Adresse IP source du message ICMP
Destination Address:  (4 octets) Adresse IP destination du message ICMP

TYPE ICMP :

8  : echo = PING
0  : echo reply = Réponse au ping
3  : Destination Unreachable 
11 : Time Exceeded 
12 : Parameter Problem Message
4  : Source Quench Message
5  : Redirect Message
13 : timestamp message;
14 : timestamp reply message.

Code : dans certains cas, précise la raison 


* Format de l'entête TCP (RFC 793)

    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |0       Src    ,1   Port       |2   Dst        ,3     Port     |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |4              ,5 Sequence     ,6  Number      ,7              |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |8              ,9   ACK        ,10   Number    ,11             |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |  Data |           |U|A|P|R|S|F|                               |
   | Offset| Reserved  |R|C|S|S|Y|I|            Window             |
   |12     |       ,13 |G|K|H|T|N|N|14             ,15             |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |16         Chk ,17   Sum       |18       Urgent,19  Pointer    |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |20             ,21  Options    ,22             |23  Padding    |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+
   |24             ,25             ,26    DATA     ,27             |
   +-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-=-+-+-+-+-+-+-+-+

* Source Port:  (2 octets)
* Destination Port:  (2 octets)
* Sequence Number:  (4 octets) Numéro de séquence du premier octet
  de données
* Acknowledgment Number:  (4 octets) Numéro de séquence du premier octet
  de données attendu)
* Data Offset:  (4 bits) Taille de l'entête en nombre de mot de 4 octets.
* Reserved:  (6 bits) Réservé ; doit être à 0
* Control Bits:  (6 bits) (de gauche à droite)
    URG:  Urgent Pointer 
    ACK:  Acknowledgment
    PSH:  Push Function
    RST:  Reset the connection
    SYN:  Synchronize sequence numbers
    FIN:  No more data from sender
* Window:  (2 octets) taille de la fenêtre ; nombre d'octets de données
  (à partir du numéro de séquence indiqué dans "Acknowledgment Number")
  que l'emetteur est disposé à accepter.
  The number of data octets beginning with the one indicated in the
  acknowledgment field which the sender of this segment is willing to
  accept.
* Checksum:  (2 octets) Somme de contrôle. Permet de vérifier l'intégrité
  des données.
* Urgent Pointer:  (2 octets) documenté dans la rfc793
* Padding:  bourrage ; pour s'assurer que l'entête s'arrête bien sur un 
  multiple de 32 octets.




* Format de l'entête UDP (RFC 768)

                  0      7 8     15 16    23 24    31
                 +--------+--------+--------+--------+
                 |     Source      |   Destination   |
                 |      Port       |      Port       |
                 +--------+--------+--------+--------+
                 |                 |                 |
                 |     Length      |    Checksum     |
                 +--------+--------+--------+--------+
                 |
                 |          data ...
                 +---------------- ...



Length : taille de l'entête et des données
Checksum:  (2 octets) Somme de contrôle. Permet de vérifier l'intégrité des
données.

