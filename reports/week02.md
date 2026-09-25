Verkonhallinta 2 snmp 

2.8 

1. Johdanto 

Mikä on SNMP? 

Simple Network Management Protocol eli tietoliikenneprotokolla, jota käytetään laitteiden hallintaan, valvontaan ja tietojen keräämiseen. 

2. Asennus 

Miten SNMP-agentti asennettiin? 

SNMP agentti asennettiin kontteihin siirtymällä kontin sisään ja ajamalla komennot: 

apt update 

apt install snmp snmpd –y 

Jonka jälkeen tarkistettiin sen tila, mutta koska se ei ollut kännissä asennuksen jälkeen configuraatio tiedostoa /etc/snmp/snmpd.conf muutettiin. Sen jälkeen se käynnistettiin uudelleen ja varmistettiin, että se on käynnissä. 

service snmpd restart 

ps aux | grep snmpd 

service snmpd status 

3. Kerätyt tiedot 

Snmp käynnissä 

 

2.3 

Järjestelmän nimi 

iso.3.6.1.2.1.1.5.0 = STRING: "web1" 

Käyttöjärjestelmä 

iso.3.6.1.2.1.1.1.0 = STRING: "Linux web1 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86_64" 

Uptime 

iso.3.6.1.2.1.1.3.0 = Timeticks: (28620) 0:04:46.20 

 

2.7 

root@db1:/# service snmpd status 

* snmpd is running 

root@branch-client:/# service snmpd status 

* snmpd is running 

 

 

Laite 

Nimi 

Käyttöjärjestelmä 

Uptime 

web1 

web1 

Linux web1 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86_64 

Timeticks: (231252) 0:38:32.52 

db1 

db1 

inux db1 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86_64 

Timeticks: (44781) 0:07:27.81 

branch-client 

branch-client 

inux branch-client 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86_64 

Timeticks: (22978) 0:03:49.78 

 

4. Verkkorajapinnat 

2.5 

iso.3.6.1.2.1.2.2.1.2.1 = STRING: "lo" 

iso.3.6.1.2.1.2.2.1.2.20 = STRING: "eth0" 

montako verkkorajapintaa löytyi 

2 

mikä rajapinta yhdistää laitteen verkkoon 

Eth0 

5. OID-analyysi 

2.6 

iso.3.6.1.2.1.2.2.1.8.1 = INTEGER: 1 

iso.3.6.1.2.1.2.2.1.8.20 = INTEGER: 1 

OID 

Tarkoitus 

sysName.0 

Laitteen hostname 

sysDescr.0 

Palauttaa teksti kuvauksen verkkolaitteesta 

sysUpTime.0 

Kertoo miten kauan järjestelmän network management osa on ollut ylhäällä 

ifDescr 

Palauttaa teksti kuvauksen network interfacesta 

ifOperStatus 

Kertoo network interfacen operational staten 

 

6. Pohdinta 

Omat havainnot SNMP:n hyödyistä ja rajoituksista. 

Mitä hyötyä SNMP:stä on verkonhallinnassa? 

Sen avulla voidaan kerätä tietoa ja valvoa useita eri valmistajien laitteita samalla työkalulla ja samasta näkymästä. 

Se voi lähettää vikailmoituksia ja helpottaa ongelmien tunnistamista verkossa. 

Mitä tietoa SNMP:n avulla voidaan kerätä? 

Laitteiden tiedot kuten valmistaja, malli, sarjanumero ja nimi, uptime, CPU ja muistin käyttö, lämpötila, siirrettyjen ja vastaanotettujen pakettien ja tavujen määrät, porttien tila, packet loss ja porttien kuormitus. 

Mitä ongelmia yhteisöpohjaisessa SNMPv2:ssa on? 

Huono tietoturva. Salauksen ja autentikaation puute. 

Missä tilanteissa käyttäisit mieluummin SNMPv3:a? 

Kaikissa tilanteissa, joissa hallinta tapahtuu julkisen verkon tai wifin/vierasverkon yli tai jos kyseessä on esimerkiksi yrityksen tai muun vastaavan verkko, jossa tulisi olla käytössä tietyt tietoturvastandardit. Eli lähes kaikissa tilanteissa, jossa hallinta ei tapahdu suljetussa vlan verkossa. 