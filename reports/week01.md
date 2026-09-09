1
Ympäristön tarkoitus on harjoitella verkon hallintaa, monitorointia ja dokumentointia.

3 laiteluettelo
Laite	nimi	IP-osoite	Tarkoitus
R1 clab-hamk-verkonhallinta-golden-r1 172.20.20.13 Verkon reititin
R2 clab-hamk-verkonhallinta-golden-r2 172.20.20.12 Verkon reititin
R3 clab-hamk-verkonhallinta-golden-r3 172.20.20.8 Verkon reititin
client1	clab-hamk-verkonhallinta-golden-client1	172.20.20.10 Käyttäjän asiakaskone
attacker clab-hamk-verkonhallinta-golden-attacker 172.20.20.9 Tietoturvatestaukseen tarkoitettu kone
web1 clab-hamk-verkonhallinta-golden-web1 172.20.20.3 Web-palvelin
db1 clab-hamk-verkonhallinta-golden-db1	172.20.20.14 Tietokantapalvelin
branch-client clab-hamk-verkonhallinta-golden-branch-client 172.20.20.5 Sivukonttorin asiakaskone
ansible	clab-hamk-verkonhallinta-golden-ansible	172.20.20.11 Verkon hallinta ja automaatio
prometheus clab-hamk-verkonhallinta-golden-prometheus 172.20.20.15 Verkon ja palveluiden monitorointi
grafana	clab-hamk-verkonhallinta-golden-grafana	172.20.20.16 Monitorointidatan visualisointi
zabbix clab-hamk-verkonhallinta-golden-zabbix 172.20.20.17 Verkon ja palveluiden monitorointi

4 IP- suunnitelma

Verkko  Tarkoitus  Yhdyskäytävä 

10.10.10.0/24  Käyttäjän verkko. Sisältää client1 ja attacker r1 10.10.10.1 

10.10.20.0/24 Serverin verkko. Sisältää web1 ja db1 r2 10.10.20.1 

10.10.30.0/24 Branch office network. Sisältää branch-client r3 10.10.30.1 

10.10.99.0/24 Hallintaverkko r2 10.10.99.1 

10.255.12.0/30 r1->r2 

10.255.23.0/30 r2->r3

R1 käyttäjäverkon yhdyskäytävä: 10.10.10.1
R2 palvelinverkon yhdyskäytävä: 10.10.20.1
R3 sivukonttoriverkon yhdyskäytävä: 10.10.30.1
R2 hallintaverkon osoite: 10.10.99.1
R1–R2 point-to-point-verkko: 10.255.12.0/30
R2–R3 point-to-point-verkko: 10.255.23.0/30
client1: 10.10.10.101
branch-client: 10.10.30.101

5 Reitityksen analyysi
ping-testit:
root@client1:/# ping -c 4 10.10.20.101
PING 10.10.20.101 (10.10.20.101) 56(84) bytes of data.
64 bytes from 10.10.20.101: icmp_seq=1 ttl=62 time=0.114 ms
64 bytes from 10.10.20.101: icmp_seq=2 ttl=62 time=0.245 ms
64 bytes from 10.10.20.101: icmp_seq=3 ttl=62 time=0.133 ms
64 bytes from 10.10.20.101: icmp_seq=4 ttl=62 time=0.099 ms

--- 10.10.20.101 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3184ms
rtt min/avg/max/mdev = 0.099/0.147/0.245/0.057 ms

root@client1:/# ping -c 4 10.10.30.101
PING 10.10.30.101 (10.10.30.101) 56(84) bytes of data.
64 bytes from 10.10.30.101: icmp_seq=1 ttl=61 time=0.161 ms
64 bytes from 10.10.30.101: icmp_seq=2 ttl=61 time=0.153 ms
64 bytes from 10.10.30.101: icmp_seq=3 ttl=61 time=0.146 ms
64 bytes from 10.10.30.101: icmp_seq=4 ttl=61 time=0.191 ms

--- 10.10.30.101 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3080ms
rtt min/avg/max/mdev = 0.146/0.162/0.191/0.017 ms

traceroute:
root@client1:/# traceroute 10.10.30.101
traceroute to 10.10.30.101 (10.10.30.101), 30 hops max, 60 byte packets
1  10.10.10.1 (10.10.10.1)  0.724 ms  0.587 ms  0.558 ms
2  10.255.12.2 (10.255.12.2)  0.530 ms  0.455 ms  0.432 ms
3  10.255.23.2 (10.255.23.2)  0.411 ms  0.369 ms  0.341 ms
4  10.10.30.101 (10.10.30.101)  0.317 ms  0.266 ms  0.230 ms

reittitauluanalyysi:
root@client1:/# ip route
default via 10.10.10.1 dev eth1 
10.10.10.0/24 dev eth1 proto kernel scope link src 10.10.10.101 
172.20.20.0/24 dev eth0 proto kernel scope link src 172.20.20.10

yhteenveto:
Eniten aikaa kului verkon ymmärtämiseen ja ip-osoitteiden yhdistämiseen oikeisiin laitteisiin.

Dokumentaatio auttaa it-asiantuntiaa näkemään verkon rakenteen ja sen laitteet selkeämmin. Se voi myös auttaa
ongelmien paikantamisessa ja ratkaisussa.

Käytetty AIta apuna osassa tehtävää.