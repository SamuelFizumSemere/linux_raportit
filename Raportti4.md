# Johdanto:

Linux Server Basic Configuration

HUOM! jouduin tämä tehtävän tekemään uudestaan kun unohdin painaa commit joten palautuksen mielessä alustavasi teen ...


# Toteutus ja tulokset

## Järjestelmän päivittäminen

Seuraavaksi asensin saatavilla olevat päivitykset komennolla:

`sudo apt-get upgrade`

Tämä päivittää järjestelmään asennettujen pakettien uusimmat versiot.


Päivitysten jälkeen järjestelmä oli ajan tasalla.

---

## SSH-palvelimen tarkistaminen

Seuraavaksi tarkistin SSH-palvelimen tilan komennolla:

`sudo systemctl status ssh`

--

SSH-palvelin oli käynnissä. SSH-palvelimen avulla voidaan muodostaa etäyhteys Linux-palvelimelle. SSH-clientillä puolestaan muodostetaan yhteyksiä muihin koneisiin.

Teorian mukaan pilvipalveluiden virtuaalikoneissa SSH on yleensä valmiiksi käytössä, koska se on tärkeä tapa muodostaa etäyhteys palvelimelle. SSH-palvelimen toimintaa voidaan hallita `systemctl`-komennoilla.

---

## SSH key authentication

Seuraavaksi määritin SSH-avaimilla kirjautumisen. SSH-avaimet koostuvat julkisesta ja yksityisestä avaimesta. Avaimet luodaan omalla paikallisella tietokoneella komennolla:

`ssh-keygen`

--

Komento kysyi, mihin avain tallennetaan ja haluanko käyttää passphrasea. Käytin tässä tehtävässä oletusasetuksia.

Avainpari tallentui `.ssh`-hakemistoon. Yksityinen avain on:

`~/.ssh/id_rsa`

ja julkinen avain:

`~/.ssh/id_rsa.pub`

Yksityistä avainta ei saa jakaa muille tai kopioida palvelimelle. Julkinen avain voidaan puolestaan kopioida palvelimelle. Teorian mukaan SSH käyttää yksityistä avainta todistamaan käyttäjän henkilöllisyyden ja palvelin tarkistaa sen julkisen avaimen avulla.

### Julkisen avaimen kopioiminen palvelimelle

Kopioin julkisen SSH-avaimen Azure-palvelimelle komennolla:

`ssh-copy-id linuxuser@<public_ip>`

--

Tämän jälkeen kokeilin kirjautumista uudelleen:

`ssh linuxuser@<public_ip>`

--

Kirjautuminen onnistui ilman, että palvelimen salasanaa tarvitsi kirjoittaa.

Tämä tarkoittaa, että SSH-avainkirjautuminen toimii oikein.

---

## Apache2-web-palvelimen asennus

Seuraavaksi asensin Apache2-web-palvelimen komennolla:

`sudo apt-get install apache2`

--

Tämän jälkeen tarkistin Apache-palvelimen tilan:

`sudo systemctl status apache2`

--

Apache oli käynnissä.

Seuraavaksi avasin selaimella osoitteen:

`http://<public_ip>`

--

Selaimessa näkyi Apache-palvelimen oletussivu, joten web-palvelin toimii.

Testasin saman asian myös terminaalissa komennolla:

`curl http://<public_ip>`

[kuva]

`curl` haki Apache-palvelimen palauttaman HTML-sisällön suoraan terminaaliin.

---

## Apache-oletussivun muuttaminen

Seuraavaksi muutin Apache-palvelimen oletussivun sisällön tehtävänannon mukaiseksi.

Käytin komentoa:

`echo 'This is my test default page' | sudo tee /var/www/html/index.html`

--

Komennossa `echo` tuottaa tekstin ja pipe `|` välittää sen `tee`-komennolle. `tee` kirjoittaa tekstin `index.html`-tiedostoon sudo-oikeuksilla.

Testasin tämän jälkeen sivun uudelleen:

`curl http://<public_ip>`

--

Sivun sisältö muuttui tekstiksi:

`This is my test default page`

Testasin sivun myös selaimella.

--

---

## public-sites-hakemiston luominen

Seuraavaksi loin tehtävänannossa määritellyn hakemiston:

`sudo mkdir -p /home/linuxuser/public-sites`

--

Hakemistoa tullaan käyttämään myöhemmin web-sisällön ylläpitämiseen ja seuraavissa tehtävissä TLS-varmenteiden kanssa.

---

# Palomuuri UFW

## UFW:n asennus

Seuraavaksi asensin UFW-palomuurin komennolla:

`sudo apt-get install ufw`

--

UFW eli Uncomplicated Firewall on Linuxin palomuurityökalu. Sen avulla voidaan määrittää, mitä verkkoyhteyksiä palvelimelle sallitaan.

---

## SSH-liikenteen salliminen

Ennen kuin otin palomuurin käyttöön, sallin SSH-liikenteen portissa 22:

`sudo ufw allow 22/tcp`

--

Tämä on tärkeää, koska olen muodostanut yhteyden palvelimelle SSH:n avulla. Jos UFW otettaisiin käyttöön ennen SSH-portin sallimista, voisin lukita itseni ulos palvelimelta.

Teorian mukaan SSH-portti pitää avata ennen UFW:n käyttöönottoa, jotta etäyhteys ei katkea.

---

## HTTP- ja HTTPS-liikenteen salliminen

Seuraavaksi sallin HTTP-liikenteen portissa 80:

`sudo ufw allow 80/tcp`

--

Sallin myös HTTPS-liikenteen portissa 443:

`sudo ufw allow 443/tcp`

--

Tämän jälkeen otin UFW-palomuurin käyttöön:

`sudo ufw enable`

--

Lopuksi tarkistin palomuurin tilanteen:

`sudo ufw status verbose`

--

Palomuurissa näkyivät sallitut portit 22, 80 ja 443.

Azure-ympäristössä liikenne voi kulkea kahden palomuurikerroksen läpi. Ensimmäinen on pilvipalveluntarjoajan security group ja toinen on Linux-palvelimen oma UFW-palomuurI.

---

# Networking

## IP-osoitteen tarkistaminen

Seuraavaksi tarkistin virtuaalikoneen verkkoliitännät ja IP-osoitteet komennolla:

`ip addr`

--

Komennon tuloksessa näkyi muun muassa loopback-liitäntä `lo`, jonka IP-osoite on `127.0.0.1`.

Lisäksi näkyi palvelimen verkkoliitäntä, jolla on yksityinen IP-osoite.

Azure-virtuaalikoneessa `ip addr` ei näytä virtuaalikoneen julkista IP-osoitetta. Tämä johtuu siitä, että julkinen IP käsitellään Azure-verkon puolella eikä sitä ole suoraan määritetty Linux-käyttöjärjestelmän verkkoliitäntään.

Julkisen IP-osoitteen pystyy tarkistamaan palvelimelta esimerkiksi komennolla:

`curl ifconfig.me`

--

---

## Reititystaulun tarkistaminen

Seuraavaksi tarkistin palvelimen routing-taulun komennolla:

`ip route`

-- 

Tuloksessa näkyi oletusreitti `default via`, joka kertoo, minkä yhdyskäytävän kautta palvelin lähettää liikennettä muihin verkkoihin.

Lisäksi näkyi palvelimen oman verkon reitti.

---

## Ping

Testasin verkkoyhteyden toimivuutta komennolla:

`ping 8.8.8.8`

--

Ping onnistui ja palvelin sai vastauksia osoitteesta `8.8.8.8`.

Pingillä voidaan testata, pääsevätkö paketit kohteeseen ja takaisin. Teorian mukaan onnistunut ping kertoo, että perusverkkoyhteys kohteeseen toimii.

---

# Packet inspection

Seuraavaksi tutkin verkkoliikennettä `ngrep`-ohjelmalla. `ngrep` näyttää verkkopaketteja reaaliajassa ja sen avulla voidaan tarkastella esimerkiksi HTTP-, SSH- ja ICMP-liikennettä.

Asensin ngrep-ohjelman komennolla:

`sudo apt-get install ngrep`

--

Tässä tehtävässä käytin kahta SSH-terminaalia. Ensimmäisessä tarkkailin verkkoliikennettä ja toisessa generoin liikennettä esimerkiksi `curl`- tai `ping`-komennolla.

---

# HTTP-liikenteen tarkastaminen ngrepillä

Ensimmäisessä terminaalissa suoritin:

`sudo ngrep -d eth0 -W byline "" host <ip-address> and port 80`

--

Toisessa terminaalissa generoin HTTP-liikennettä komennolla:

`curl http://<ip-address>`

--

### Mitä HTTP-viestejä näkyi?

Ngrepissä näkyi HTTP-liikennettä palvelimen ja asiakkaan välillä. HTTP-pyynnössä näkyi esimerkiksi `GET`-pyyntö, jolla selain tai `curl` pyytää palvelimelta verkkosivun sisältöä.

### Näkyikö default web page -sivun sisältö?

Kyllä, HTTP-liikenteessä pystyi näkemään myös palvelimen lähettämää sisältöä, koska tässä käytettiin tavallista HTTP-yhteyttä eikä HTTPS-yhteyttä.

Tämä tarkoittaa, että HTTP-liikenne kulkee salaamattomana ja pakettien sisältöä voidaan tarkastella verkossa.

[kuva]

### Ngrep-komennon parametrien selitys

Komennossa:

`sudo ngrep -d eth0 -W byline "" host <ip-address> and port 80`

`sudo` antaa tarvittavat oikeudet verkkoliikenteen tarkkailuun.

`ngrep` käynnistää verkkopakettien tarkkailun.

`-d eth0` määrittää verkkoliitännän, jota tarkkaillaan.

`-W byline` näyttää pakettien sisältöä selkeämmin rivi kerrallaan.

`""` tarkoittaa, ettei liikenteestä suodateta tiettyä tekstihakua.

`host <ip-address>` rajoittaa tarkkailun määritettyyn IP-osoitteeseen.

`port 80` rajoittaa liikenteen HTTP-porttiin 80.

---

# SSH-liikenteen tarkastaminen

Seuraavaksi tarkastelin SSH-liikennettä komennolla:

`sudo ngrep -d eth0 -W byline "" port 22`

[kuva]

Tämän jälkeen muodostin SSH-yhteyden toisesta terminaalista tai generoin muuta SSH-liikennettä.

[kuva]

SSH-liikenteessä näkyi verkkopaketteja, mutta itse kirjautumiseen tai komentoihin liittyvä sisältö ei näkynyt samalla tavalla selkokielisenä kuin HTTP-liikenteessä.

Tämä kertoo, että SSH on salattu protokolla. SSH:n tarkoituksena on mahdollistaa turvallinen etäkäyttö niin, että esimerkiksi käyttäjän salasanaa ja palvelimella annettuja komentoja ei voida lukea suoraan verkkoliikenteestä.

---

# ICMP-liikenteen tarkastaminen ngrepillä

Seuraavaksi tarkastelin ICMP-liikennettä komennolla:

`sudo ngrep -d eth0 -W byline "" icmp`

[kuva]

Toisessa terminaalissa suoritin:

`ping 8.8.8.8`

---

Ngrep näytti ping-komennon aiheuttamaa ICMP-liikennettä.

ICMP eli Internet Control Message Protocol on verkkoprotokolla, jota käytetään esimerkiksi verkkoyhteyden testaamiseen ja erilaisiin verkon virhe- ja tilaviestintään liittyviin tarkoituksiin.

Ping käyttää ICMP:tä lähettämällä kohteelle pyyntöjä ja odottamalla vastauksia. Tämän avulla voidaan tarkistaa, onko kohteeseen toimiva verkkoyhteys.

ICMP on tärkeä verkkoyhteyksien vianmäärityksessä, koska esimerkiksi `ping`-komennolla voidaan nopeasti tarkistaa, pääsevätkö paketit tiettyyn kohteeseen.

---

---

# ICMP-liikenteen tarkastaminen tcpdumpilla

Seuraavaksi tein saman ICMP-liikenteen tarkastelun `tcpdump`-ohjelmalla:

`sudo tcpdump -i eth0 icmp`

 ---

Vertailin tulosta ngrepin kanssa.

Ngrep näyttää liikennettä enemmän tekstimuodossa ja pystyy näyttämään myös pakettien sisältöä. Tcpdump puolestaan näyttää verkkopakettien tietoja tiiviimmin, kuten pakettien lähde- ja kohdeosoitteita sekä protokollan.

Molemmilla voidaan tarkastella verkkoliikennettä, mutta tässä tehtävässä tcpdump oli mielestäni selkeämpi ICMP-pakettien perusrakenteen tarkasteluun, kun taas ngrep sopii paremmin tietyn sisällön etsimiseen paketeista.




# Keskeiset havainnot ja Pohdinta

--

# Yhteenveto:


### Lähteet

Linux Server Setup - Basic Configurations -kurssin teori materiaali.
