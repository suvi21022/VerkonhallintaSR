Verkonhallinta T3 
1. Johdanto
Monitoroinnilla tarkoitetaan järjestelmien ja verkkojen toiminnan jatkuvaa seuraamista. Monitoroinnin avulla voidaan kerätä tietoa esimerkiksi palvelinten suorituskyvystä, verkkoliikenteestä, muistin ja levytilan käytöstä sekä laitteiden toimintatilasta.

Monitorointi on tärkeää palveluiden ja verkkojen ylläpidossa, koska sen avulla ongelmia voidaan havaita ja paikantaa nopeammin. Monitoroinnin avulla voidaan myös huomata resurssien käytön kasvaminen ennen kuin se aiheuttaa varsinaisen ongelman palvelulle.

Prometheus on monitorointijärjestelmä, joka kerää mittareita valvottavista kohteista ja tallentaa ne aikasarjoina. Prometheus käyttää PromQL-kyselykieltä mittareiden hakemiseen ja analysointiin. Tässä tehtävässä Prometheus keräsi tietoa Node Exporterilta ja Grafanaa käytettiin tietojen visualisointiin.

2. Node Exporterin käyttöönotto
3.3 
Prometheus näkee palvelimen

3. Prometheus
Targets-sivun tarkastelu ja havainnot.
3.4 
Grafanan tietolähde

4. Dashboard
Kuvakaappaus koko dashboardista.
3.5 
Luo dashboard

5. Kuormitustesti
T3.6 

Miten cpu käyrä muuttui?: 

root@web1:/# stress-ng --cpu 4 --timeout 60s 

Nousi 1,47 -> 6.82 

Miten levytilan käyttö muuttui? 
6.12 -> 6.14 %
 

Näkyikö verkkoliikenteessä muutoksia? 

Network Transmit -käyrä nousi noin arvosta 0 noin arvoon 550 ja Network Receive -käyrä noin arvosta 0 noin arvoon 150000

3.7 

6. SNMP vs Prometheus

Ominaisuus 

SNMP 

Prometheus 

Tiedonkeruu 

Push ja pull malli 

Pääasiassa pull malli 

Käyttöönotto 

Helppoa, vaatii yleensä vain kytkemisen päälle 

Vaatii valvontainfrastruktuurin 

Mittarien määrä 

Kymmenistä satoihin/laite. Rajallinen rakenne rajoittaa mittarien määrää. 

Huomattavasti enemmän ja tarkempia. Dynaaminen ja skaalautuva. 

Visualisointi 

Graafit luodaan yleensä automaattisesti. 

Grafanalla luotava oma dashboard, mahdollisuus kustomoida miten ja mitä dataa esitetään. 

Hälytysmahdollisuudet 

Yksinkertaiset push ja pull hälytykset joko laitteen tai hallintapalvelimen puolelta. 

Ei ota vastaan laitteiden omia hälytyksiä vaan pull  ja alertmanager rakenne huomaa ongelman oman sääntöpohjaisen järjestelmänsä kautta 

Soveltuvuus pilviympäristöihin 

Se voi toimia, mutta se on suunniteltu verkkolaitteiden hallintaan 

Toimii hyvin ja se on suunniteltu moderneja pilvirakenteita ajatellen 

 

3.8 


Mitä hyötyä Prometheuksesta on verrattuna SNMP:hen? 

SNMP sopii hyvin siihen jos halutaan tietää esimerksikis missä laitteessa on ongelma, mutta jos se halutaan paikantaa tarkemmin Prometheuksen ja node exporterin avulla voidaan saada tietää hyvin tarkasti tietoa prosesseista joita laitteessa on. Eli prometheus tarjoaa tietoa paljon tarkemmin ja se voidaan visualisoida ja esittää monipuolisemmin. 

Millaisia mittareita ylläpitäjän kannattaa seurata jatkuvasti? 

Porttien kautta kulkevan datan määrä. Cpu, muisti ja levyn käyttö. Laitteen tila up/down ja  uptime. 

Mitä tietoa dashboardisi tarjoaa ylläpitäjälle? 

CPU käyttö, muistin käyttö, levyn käyttö, network receive ja network transmit. 

Mitä uusia mittareita lisäisit dashboardiin? 

Uptime ja laitteen tila, sekä packet loss seuranta. 

Miten monitorointitiedosta voisi olla hyötyä vianetsinnässä? 

Niiden avulla voisi selvittää tai rajata ongelmaa. Voi nähdä johtuuko vika esimerkiksi internetin vai laitteen ongelmasta tai siitä, että jokin laite on yhtäkkiä pois verkosta, kaatunut tai hajonnut.

7. Yhteenveto
Mitä opit harjoituksesta?
Opin verkon monitoroinnista, sekä mikä on prometheus ja grafana ja mihin niitä käytetään. opin myös ottamaan ne käyttöön ja käyttämään niitä.