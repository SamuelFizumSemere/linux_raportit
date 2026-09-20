# Johdanto:

Linux Server Basic Configuration

HUOM! jouduin tämä tehtävän tekemään uudestaan kun unohdin painaa commit joten palautuksen mielessä alustavasi teen ...


# Toteutus ja tulokset

## Järjestelmän päivittäminen

Seuraavaksi asensin saatavilla olevat päivitykset komennolla:

`sudo apt-get upgrade`



---

## SSH-palvelimen tarkistaminen

Seuraavaksi tarkistin SSH-palvelimen tilan komennolla:

`sudo systemctl status ssh`

<img width="911" height="677" alt="image" src="https://github.com/user-attachments/assets/fcbe2e60-b67c-4186-810f-58c1a0c590a1" />


SSH-palvelin oli käynnissä. SSH-palvelimen avulla voidaan muodostaa etäyhteys Linux-palvelimelle. SSH-clientillä puolestaan muodostetaan yhteyksiä muihin koneisiin.

---

## SSH key authentication

Seuraavaksi määritin SSH-avaimilla kirjautumisen. SSH-avaimet koostuvat julkisesta ja yksityisestä avaimesta. Avaimet luodaan omalla paikallisella tietokoneella komennolla:

`ssh-keygen`

<img width="771" height="438" alt="image" src="https://github.com/user-attachments/assets/ef252574-f357-4dff-ad2b-b255424f3821" />



### Julkisen avaimen kopioiminen palvelimelle

Kopioin julkisen SSH-avaimen Azure-palvelimelle komennolla:

<img width="765" height="377" alt="image" src="https://github.com/user-attachments/assets/415c5ac7-73b0-447a-9896-65db536c451d" />


--

<img width="863" height="536" alt="image" src="https://github.com/user-attachments/assets/4b019c83-70a3-4112-9ec9-1f7afb8ebf72" />

salasana vaihdettu: 

<img width="457" height="145" alt="image" src="https://github.com/user-attachments/assets/f0f8cff8-2e85-4131-80a4-684c039b8690" />

Testasin sitten, että se toimii oikein...


---

## Apache2-web-palvelimen asennus

Asensin jo edellisessä tehtävässä apache2.

Tämän jälkeen tarkistin Apache-palvelimen tilan:

<img width="825" height="458" alt="image" src="https://github.com/user-attachments/assets/283487a2-eb2e-44b3-a944-f1f498e9ca39" />

Apache oli käynnissä.


---

## Apache-oletussivun muuttaminen

Muutin myös tämän sisällön edellisessä tehtävässä. 

<img width="897" height="637" alt="image" src="https://github.com/user-attachments/assets/69f6a9e4-6ab5-4504-9eb4-06ff09e6beaa" />


 sekä selaimessa että curl-komennolla

---

## public-sites-hakemiston luominen

Seuraavaksi loin tehtävänannossa määritellyn hakemiston:

<img width="912" height="260" alt="image" src="https://github.com/user-attachments/assets/4d3c59ff-8c07-459b-8560-c35470a3cf7c" />



---

# Palomuuri UFW

## UFW:n asennus

Tämä on jo asennettu edellisessä tehtävässä. 

---

## SSH-liikenteen, HTTP- ja HTTPS-liikenteen  salliminen

<img width="915" height="517" alt="image" src="https://github.com/user-attachments/assets/014546f5-8ec0-47ef-a8fb-b1ac922f031e" />


Lopuksi tarkistin palomuurin tilanteen

Palomuurissa näkyivät sallitut portit. 


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
