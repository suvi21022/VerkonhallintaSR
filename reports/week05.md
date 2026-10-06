Johdanto
Tietoturva verkonhallinnan näkökulmasta tarkoittaa strategioita, käytäntöjä ja työkaluja, joilla varmistetaan yrityksen verkkoympäristön luottamuksellisuus, eheys ja käytettävyys.

Kaappaus
Verkkoliikenne kaapattiin web1-palvelimella tcpdump-ohjelmalla käyttäen verkkoliitäntää eth1. Liikenne tallennettiin web1.pcap-tiedostoon. Porttiskannausta varten liikennettä tuotettiin attacker-kontista komennolla nmap -Pn 10.10.20.101.

Porttiskannaus
Porttiskannauksessa attacker-laitteen IP-osoitteeksi havaittiin 10.10.10.200 ja web1-palvelimen IP-osoitteeksi 10.10.20.101. Wiresharkissa skannaus näkyi useina TCP SYN -paketteina attackerilta web1-palvelimelle eri kohdeportteihin.

Esimerkiksi porttiin 22 lähetetty SYN-paketti sai palvelimelta SYN/ACK-vastauksen. Tämä osoittaa, että portti 22 oli avoinna ja siinä kuunteli palvelu. Myös portti 80 vastasi SYN/ACK-paketilla. Porttiin 143 lähetetty SYN-paketti sai puolestaan RST/ACK-vastauksen, mikä osoittaa, että portti oli suljettu.

Porttiskannaus osoitti, että verkkoliikenteestä voidaan tunnistaa palveluiden tiloja TCP-yhteyden muodostamisen perusteella.

kuvat1 ja 2

Wireshark
DNS liikenteessä web1 palvelin 10.10.20.101 käytti DNS palvelinta 10.255.255.254. Havaittiin esimerkiksi PTR-kysely 5.0.0.224.in-addr.arpa, johon tuli vastaus ospf-all.mcast.net. Lisäksi havaittiin PTR-kysely, jonka vastaus oli No such name.

HTTP-suodattimella ei löytynyt varsinaisia HTTP-pyyntöjä tai -vastauksia. Porttiin 80 liittyvää TCP-liikennettä kuitenkin havaittiin. Tämän perusteella liikenteessä näkyi TCP-yhteyksien muodostamiseen liittyviä paketteja, mutta varsinaista HTTP-sovellustason liikennettä ei ollut mukana kaappauksessa.

kuvat dns ja http

Loki-analyysi
Web1-palvelimelta ei löytynyt /var/log/syslog-tiedostoa, eikä journalctl sisältänyt lokimerkintöjä.

Turvallisuusarvio
Ympäristössä on joitakin turvallisuutta tukevia ominaisuuksia. Verkkoliikennettä voidaan seurata ja analysoida Wiresharkilla, ja Nginx tallentaa sekä onnistuneet pyynnöt että virhetilanteet lokitiedostoihin. Näiden avulla epäilyttävää toimintaa voidaan havaita ja tutkia jälkikäteen.

Tietoturvariskinä ovat useat avoimet verkkopalvelut. Web1-palvelimella havaittiin esimerkiksi SSH portti 22, HTTP portti 80, SNMP portti 161 ja node_exporter portti 9100. Jokainen verkosta saavutettava palvelu kasvattaa hyökkäyksen mahdollisuutta. Lisäksi liikennekaappauksessa havaittiin porttiskannausta, jossa toinen laite testasi useita palvelimen portteja.

Ympäristöä voitaisiin parantaa rajoittamalla verkkopalveluiden saavutettavuutta palomuurisäännöillä ja sallimalla vain tarvittavat yhteydet. Erityisesti hallintaan liittyvät palvelut, kuten SSH ja SNMP, tulisi rajoittaa vain niille verkoille tai laitteille, jotka niitä tarvitsevat. Lisäksi Nginxin logissa havaittujen portin 80 bind-virheiden syy tulisi selvittää, jotta palvelun toiminta olisi luotettavampaa.

Yhteenveto
Tehtävässä harjoiteltiin verkkoliikenteen kaappaamista ja analysointia tcpdump ja Wireshark-ohjelmilla. Liikenteestä tunnistettiin porttiskannaus ja tutkittiin TCP pakettien perusteella avoimia ja suljettuja portteja. Lisäksi analysoitiin DNS-liikennettä ja tarkasteltiin, miten kyselyt ja vastaukset näkyvät Wiresharkissa.

Loki-analyysissä tutkittiin Nginxin access ja error logeja ja löydettiin onnistuneita HTTP pyyntöjä ja palvelun käynnistymiseen liittyviä virheitä. Tehtävästä opin, että verkkoliikenteen analysointi ja lokien seuranta ovat hyödyllisiä työkaluja tietoturvan valvonnassa ja ongelmien selvittämisessä.


Aita käytetty auttamaan tehtävässä