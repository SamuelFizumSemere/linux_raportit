# Johdanto:

Linux Server Basic Configuration

HUOM! jouduin tämä tehtävän tekemään kaksi kertaa kun unohdin painaa commit...  :)


# Toteutus ja tulokset

## Järjestelmän päivittäminen

Ensinnäkin asensin saatavilla olevat päivitykset komennolla:

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

<img width="920" height="381" alt="image" src="https://github.com/user-attachments/assets/7daaebbd-52a1-40e9-b1d5-255fee4eeaad" />


---

## Reititystaulun tarkistaminen

Seuraavaksi tarkistin palvelimen routing-taulun komennolla:

<img width="762" height="73" alt="image" src="https://github.com/user-attachments/assets/46bbb801-fe2c-4ff4-b34b-0902e58b33c3" />


## Ping

Testasin verkkoyhteyden toimivuutta komennolla:

<img width="707" height="348" alt="image" src="https://github.com/user-attachments/assets/b34baf21-e9b3-40a5-a84c-586b11a9798c" />




### Lähteet

Kurssin teoria materiaali.
