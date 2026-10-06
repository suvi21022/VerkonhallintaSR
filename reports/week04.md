Johdanto
Infrastructure as Code on koodipohjainen infrastruktuuri, jolla voidaan hallita ja pystyttää IT-infrastruktuuria koneellisesti luettavien määritystiedostojen avulla.

Inventory
4.1 
@ungrouped: 
@user_network: 
client1 
attacker 

@server_network: 
web1 
db1 

@branch_office: 
branch-client 

@network_devices: 
@routers: 
r1 
r2 
r3 

@linux_hosts: 
@clients: 
client1 
attacker 
branch-client 

@servers: 
web1 
db1 

@monitoring: 
prometheus 
grafana 
zabbix 
cadvisor 

@management: 
ansible 

@ubuntu_hosts: 
client1 
web1 
db1 
branch-client 

@node_exporter:
@routers: 
r1 
r2 
r3 

@clients: 
client1 
attacker 
branch-client

@servers: 
web1 
db1

4.5 

attacker | OS=Linux; IP=172.20.20.11; CPU=16; RAM=31969 MB 

web1 | OS=Ubuntu 24.04.4 LTS; IP=172.20.20.8; CPU=16; RAM=31969 MB 

db1 | OS=Ubuntu 24.04.4 LTS; IP=172.20.20.10; CPU=16; RAM=31969 MB 

client1 | OS=Ubuntu 24.04.4 LTS; IP=172.20.20.15; CPU=16; RAM=31969 MB 

branch-client | OS=Ubuntu 24.04.4 LTS; IP=172.20.20.7; CPU=16; RAM=31969 MB 

r1 | OS=Ubuntu 24.04.4 LTS; IP=172.20.20.5; CPU=16; RAM=31969 MB 

r2 | OS=Ubuntu 24.04.4 LTS; IP=172.20.20.5; CPU=16; RAM=31969 MB 

r3 | OS=Ubuntu 24.04.4 LTS; IP=172.20.20.5; CPU=16; RAM=31969 MB 

Esimerkkiplaybookit
SNMP
install-snmp.yml asentaa ja konfiguroi SNMP kaikille ubuntu_hosts ryhmän koneille.

Playbookin Ansible moduuleita:
apt pakettien asentamiseen
copy SNMP-konfiguraation luomiseen
service palvelun käynnistämiseen ja hallintaan
shell SNMP-prosessin tarkistamiseen
debug tulosten näyttämiseen

Node Exporter
install-node-exporter.yml asentaa Prometheus Node Exporterin kaikille ubuntu_hosts ryhmän koneille.

Se asentaa tarvittavat paketit, luo asennushakemiston, lataa Node Exporterin,
purkaa paketin, kopioi Node Exporter -binäärin asennushakemistoon,
käynnistää Node Exporterin ja tarkistaa HTTP-yhteydellä, että exporter vastaa portissa 9100.

Oma playbook
Asensin web palvelimeksi Nginxin koneelle web1.

Koodi:
- name: Install and configure web server
  hosts: web1
  become: true

  vars:
    web_server: nginx

  tasks:

    - name: Update package cache
      apt:
        update_cache: yes

    - name: Install web server
      apt:
        name: "{{ web_server }}"
        state: present

    - name: Create index page
      copy:
        dest: /var/www/html/index.html
        content: |
          <html>
          <head>
            <title>HAMK Ansible Web Server</title>
          </head>
          <body>
            <h1>Web server: {{ inventory_hostname }}</h1>
            <p>This page was created with Ansible.</p>
          </body>
          </html>

    - name: Start web server
      shell:
        cmd: nginx

    - name: Verify web server responds
      uri:
        url: http://localhost
        status_code: 200

    - name: Display verification result
      debug:
        msg: "Web server is running on {{ inventory_hostname }}"

Output:
image 4.4_A_output.png

Vertailu
Käsin tehtävissä asennuksissa jokainen palvelin täytyy konfiguroida erikseen. Tämä vie aikaa ja lisää riskiä sille, että jokin asetus tai asennusvaihe unohtuu. Ansiblella samat asennukset ja asetukset voidaan määritellä playbookiin ja suorittaa usealle palvelimelle samalla tavalla.

Automaation tärkeimpiä hyötyjä ovat ajansäästö, tasalaatuisuus ja virheiden vähentyminen. Kun sama playbook suoritetaan usealle palvelimelle, asetukset pysyvät samanlaisina eri koneissa. Playbook voidaan myös suorittaa uudelleen tarvittaessa, jolloin ympäristöä on helpompi ylläpitää ja palauttaa.

Yhteenveto
Opin, miten inventory määrittelee hallittavat laitteet ja miten playbookeilla voidaan automatisoida palvelimien asennuksia ja konfiguraatioita.

Harjoituksissa opin käyttämään Ansible-moduleita esimerkiksi pakettien asentamiseen, tiedostojen luomiseen, ohjelmien käynnistämiseen ja palveluiden toiminnan tarkistamiseen. Opin myös käyttämään muuttujia ja handlereita playbookeissa.

