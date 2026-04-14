## x) Tiivistelmät  

- **Apache installed with Ansible - quick notes**  
  - Apache voidaan asentaa helposti Ansiblella apt-moduulilla  
  - Web-palvelin pitää käynnistää ja usein myös ottaa käyttöön bootissa  
  - Tiedostojen hallinta (esim. index.html) voidaan tehdä Ansiblella  
  Oma huomio: aika suoraviivaista, mutta pitää muistaa myös palvelun käynnistys  

- **Handlers: running operations on change**  
  - Handler suoritetaan vain jos jokin muuttuu  
  - Esim. jos config muuttuu → restart palvelu  
  - Ei turhia restartteja  
  Oma ajatus: tämä vaikuttaa hyödylliseltä isommissa projekteissa  

- **Notifying handlers**  
  - Tehtävä voi “notify” handleria  
  - Handler ajetaan vasta lopuksi  
  - Parantaa tehokkuutta  
  Kysymys: mitä tapahtuu jos monta taskia kutsuu samaa handleria?  

- **ansible-doc service**  
  - service-moduulilla hallitaan palveluita  
  - tärkeät: name, state, enabled  
  - state: started, stopped, restarted  
  - enabled: käynnistyy bootissa  
  Oma huomio: tätä tarvitaan melkein aina palvelinten kanssa  


## a) Apassi  

Tässä asensin Apache2-palvelimen käsin. Asensin sen apt:lla ja käynnistin palvelun. Tämän jälkeen tarkistin selaimella, että oletussivu toimii.

Tämän jälkeen muokkasin web-sivua. Annoin tavalliselle käyttäjälle oikeudet muokata tiedostoa, jotta ei tarvitse käyttää sudoa.

sudo chown user:user /var/www/html/index.html
nano /var/www/html/index.html

Kun muokkasin tiedostoa, sivu näkyi selaimessa osoitteessa:
http://localhost

b) Moottorix

Tässä asensin Nginx-palvelimen. Ensin piti pysäyttää Apache, koska molemmat käyttävät samaa porttia.

bash
sudo systemctl stop apache2
sudo apt-get install nginx
sudo systemctl start nginx

Tämän jälkeen tarkistin selaimella, että Nginx toimii.

Muokkasin sivua samalla tavalla kuin aiemmin:

sudo chown user:user /var/www/html/index.nginx-debian.html
nano /var/www/html/index.nginx-debian.html

Sivu näkyi selaimessa normaalisti.

c) Automoottorix

Tässä automatisoin Nginx-asennuksen Ansiblella. Tein playbookin, jossa asensin paketin ja käynnistin palvelun.
bash
micro nginx.yml

sisältö:

YAML

- hosts: all
  become: true
  tasks:
    - name: Asenna nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Käynnistä nginx
      service:
        name: nginx
        state: started
        enabled: true

  ansible-playbook nginx.yml
  
Tämän jälkeen nginx oli asennettu ja käynnissä automaattisesti. HTML-sivun tein käsin kuten aiemmin.

Yhteenveto

Tässä tehtävässä tuli hyvin esille, miten palvelimia voidaan asentaa käsin ja sitten automatisoida Ansiblella. 
Apache ja Nginx toimivat aika samalla tavalla, mutta tärkeä huomio oli se, että ne eivät voi olla samaan aikaan päällä samalla portilla.
Ansible helpotti erityisesti asennusta ja palvelun hallintaa.
