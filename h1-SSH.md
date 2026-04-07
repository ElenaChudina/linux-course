a) Sshecrets – SSH:n asennus ja testaus

Tässä vaiheessa tarkoituksena oli asentaa SSH-palvelin ja varmistaa, että etäyhteys toimii.
Aloitin päivittämällä pakettilistan komennolla sudo apt-get update ja asensin OpenSSH-serverin komennolla sudo apt-get -y install ssh. 
Asennuksen jälkeen käynnistin SSH-palvelun ja laitoin sen käynnistymään automaattisesti komennolla sudo systemctl enable --now ssh.
Kun palvelu oli käynnissä, testasin yhteyden toimivuuden. Koska tein tämän omalla koneella, käytin komentoa ssh localhost.
Yhteys muodostui normaalisti, mikä kertoi että SSH toimii oikein. Tämä on tärkeä perusvaihe, koska kaikki myöhemmät tehtävät, 
kuten Ansible, perustuvat toimivaan SSH-yhteyteen.

b) Pubkey – kirjautuminen ilman salasanaa

Seuraavaksi otin käyttöön SSH-avainkirjautumisen, joka helpottaa kirjautumista ja on käytännössä välttämätön automaatiossa. 
Loin avainparin komennolla ssh-keygen ja hyväksyin oletusasetukset, jolloin avaimet tallentuivat .ssh-hakemistoon.
Tämän jälkeen kopioin julkisen avaimen palvelimelle komennolla ssh-copy-id localhost. Tämä lisäsi avaimen automaattisesti authorized_keys-tiedostoon, 
jolloin se hyväksytään kirjautumisessa. Testasin tämän kirjautumalla uudelleen komennolla ssh localhost, ja tällä kertaa salasanaa ei enää kysytty.
Tämä tekee työskentelystä huomattavasti sujuvampaa, ja lisäksi se on turvallisempi vaihtoehto kuin pelkkä salasana, kunhan yksityinen avain pysyy turvassa.

c) Hei Ansible

Viimeisessä osassa tutustuin Ansibleen, joka on työkalu useiden koneiden hallintaan koodin avulla. 
Asensin Ansiblen komennolla sudo apt-get install ansible ja loin sille oman hakemiston. Tämän jälkeen tein hosts.ini-tiedoston, johon lisäsin localhostin. 
Tämä tiedosto toimii ikään kuin listana koneista, joita Ansible hallitsee.
Ensimmäiseksi testasin yhteyden komennolla ansible all -a 'uptime' -i hosts.ini. Komento toimi, mikä tarkoitti että Ansible pystyy käyttämään SSH-yhteyttä oikein. 
Tämän jälkeen loin yksinkertaisen playbookin ja roolin, 
jonka tehtävänä oli luoda tiedosto /tmp/hello-ansible.
Kun ajoin playbookin komennolla ansible-playbook site.yml, Ansible suoritti tehtävän ja loi tiedoston koneelle.
Tarkistin lopuksi komennolla ssh localhost 'cat /tmp/hello-ansible', että tiedosto oli oikeasti luotu.
Tämä harjoitus havainnollisti hyvin, miten Ansible käyttää SSH:ta taustalla ja miten tärkeää toimiva ja automatisoitu kirjautuminen on. Kun SSH ja avaimet ovat kunnossa, Ansible toimii todella helposti ja tehokkaasti.
