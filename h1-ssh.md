Parannettu versio: https://github.com/ElenaChudina/linux-course/blob/main/h1-SSH.md

SSH Kotitehtävä

Tehtävässä käytiin läpi SSH:n perusidea, avainkirjautuminen ja Ansible.
SSH on käytännössä tapa kirjautua turvallisesti palvelimelle, ja sitä käytetään monessa työkalussa taustalla. 
Julkisella avaimella kirjautuminen tekee käytöstä helpompaa, koska salasanaa ei tarvitse syöttää joka kerta.

Asensin SSH:n ja testasin yhteyden localhostiin. Sen jälkeen loin avainparin ja kopioin public keyn palvelimelle, jolloin pystyin kirjautumaan ilman salasanaa. 
Tämä on tärkeää erityisesti automaatiossa.

Ansible puolestaan toimii SSH:n päällä ja mahdollistaa useiden koneiden hallinnan koodilla. 
Testasin sitä ajamalla komennon localhostiin ja tein yksinkertaisen “hello world” -tyylisen jutun, jossa Ansible loi tiedoston kansioon.
