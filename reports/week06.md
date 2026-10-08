1. Johdanto
Zabbix on avoimen lähdekoodin valvontajärjestelmä, jota voidaan käyttää palvelimien, verkkolaitteiden, palveluiden ja muiden IT ympäristön osien valvontaan. Zabbix kerää valvontatietoja esimerkiksi Zabbix agentin ja SNMP avulla. Kerättyjen tietojen perusteella voidaan seurata ympäristön tilaa, luoda dashboardeja ja määrittää triggereitä hälyttämään ongelmatilanteista.

2. Hostien lisääminen
web1 – 172.20.20.5
db1 – 172.20.20.9
branch-client – 172.20.20.3

3. Dashboard
Kuvakaappaus dashboardista.

4. Mittarit
CPU utilization	4,35 %	Kertoo, kuinka paljon CPU kapasiteetista käytetään. Korkea käyttö voi kertoa suuresta kuormasta tai resurssien puutteesta.

Memory utilization	13,58 %	Kertoo käytetyn RAM-muistin määrän suhteessa käytettävissä olevaan muistiin. Korkea käyttö voi aiheuttaa suorituskykyongelmia.

System uptime	02:17:03	Kertoo, kuinka kauan järjestelmä on ollut käynnissä. Sen avulla voidaan havaita esimerkiksi odottamattomia uudelleenkäynnistyksiä.

Bits received	3,53 Kbps	Kertoo verkkoliitännän vastaanottamasta liikenteestä. Sen avulla voidaan seurata verkkoliikenteen määrää ja havaita poikkeavaa liikennettä.

Bits sent	3,63 Kbps	Kertoo verkkoliitännän lähettämästä liikenteestä. Sen avulla voidaan seurata ulospäin suuntautuvaa verkkoliikennettä ja sen muutoksia.

5. Triggerit
korkea CPU käyttö trigger:
{web1:system.cpu.util.last()}>80 Warning
Kertoo jos CPU käyttö nousee yli 80%

Alhainen vapaa levytila
{web1:vfs.fs.size[/configs,pused].last()}>80 Warning
Koska mittari kertoo käytetyn tilan määrän, yli 80 % käyttö vastaa alle 20 % vapaata levytilaa.

6. Hälytys- ja häiriötestit
CPU-kuormaa testattiin web1-palvelimella suorittamalla alla oleva komento vcpu määrän vuoksi:
root@web1/#
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 
yes > /dev/null & 

Käyttö nousi nopeasti lähes 100%, joka laukausi Warning ilmoituksen. Ilmoitus myös hävisi itsestään, kun käyttö laski alle 80%.
-kuvat memorytilawan6.8 ja cputriggergraph6.8

7. Vertailu
SNMP
Tiedonkeruu: Kerää tietoa verkkolaitteilta SNMP-protokollalla
Dashboardit: Ei sisäänrakennettuja dashboardeja
Hälytykset: Valvottavat laitteet lähettävät SNMP Trap -viestejä keskitetylle valvontapalvelimelle, kun jokin määritetty kynnysarvo ylittyy tai laitteessa tapahtuu virhe.
Käyttöönotto: Aika yksinkertaista, jos laitteet tukevat SNMP:tä
Skaalautuvuus: Sopii hyvin verkkolaitteiden valvontaan
Yrityskäyttö: Erittäin yleinen verkkolaitteiden ja infrastruktuurin valvonnassa

Prometheus
Tiedonkeruu: Perustuu pääasiassa pull-malliin ja aikasarjatietokantaan. Tekee säännöllisin väliajoin HTTP GET -pyynnön valvottavan kohteen määritettyyn end pointtiin.
Dashboardit: Grafana tarjoaa monipuoliset dashboardit
Hälytykset: Alertmanagerilla voidaan tehdä hälytyksiä
Käyttöönotto: Vaatii Prometheus-konfiguraation ja exporterit
Skaalautuvuus: Erittäin hyvä suurille metrimäärille ja moderniin pilvi- ja container-ympäristöön
Yrityskäyttö: Yleinen pilvi-, Kubernetes- ja DevOps-ympäristöissä

Zabbix
Tiedonkeruu: Kerää tietoa Zabbix-agentilla, SNMPä ja muilla menetelmillä
Dashboardit: Sisäänrakennetut dashboardit ja visualisointi
Hälytykset: Sisäänrakennetut triggerit ja hälytykset
Käyttöönotto: Keskitetty käyttöönotto, mutta vaatii enemmän konfigurointia
Skaalautuvuus: Sopii suurten ympäristöjen keskitettyyn valvontaan
Yrityskäyttö: Laajasti käytetty IT-infrastruktuurin kokonaisvaltaiseen valvontaan


8. Yhteenveto
Opin käyttämään useita verkonhallintatyökaluja, niiden eroista ja yleisimmistä käyttötarkoituksista. Opin myös ymmärtämään verkkorakenteita paremmin.