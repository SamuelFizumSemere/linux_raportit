# Johdanto:
Apache2

# Toteutus ja tulokset 

## Apache2 asennus
Asennan seuraavasti apache2 seuraavasti tehtävän mukaisesti
<img width="875" height="560" alt="image" src="https://github.com/user-attachments/assets/1cc958c8-a5e0-4e47-8fb6-623c648a6212" />

ja tarkastan statuksen seuraavalla komennolla 
<img width="848" height="543" alt="image" src="https://github.com/user-attachments/assets/a7ed87e1-084e-4534-976c-d0dfb8e64b8b" />

## apache sivu

<img width="1075" height="795" alt="image" src="https://github.com/user-attachments/assets/603bdd24-4475-4979-9815-8c70f04bb224" />

ja komennolla curl http://localhost sain kokonaan html sivusto terminaaliin.

Komennolla echo 'This is the default page of my samu web server' | sudo tee /var/www/html/index.html sain muutettua index.html

<img width="822" height="140" alt="image" src="https://github.com/user-attachments/assets/5d98884d-5a23-4dbf-99b3-7fc8fa3d1447" />

Eli echo tulostaa se merkkijonossa oleva teksti ja pipe antaa echo:n tuloksen ja välittää sen eteenpäin. "tee" muokkaa sen html sudon oikeuksilla. 

<img width="1083" height="492" alt="image" src="https://github.com/user-attachments/assets/d32feb84-be1c-42ca-b568-a63c5783def6" />

### Toinen tapa muuttaa sivuston sisältö 
Käyttämällä 
sudo sh -c "echo 'This is my new web server' > /var/www/html/index.html"

Explain why sudo echo ‘This is…’ > /var/www/html/index.html 
does not work? 

- tää ei toimi koska > jälkeiset komennot suoritetaan ennen kun sudo.

## Konfiguraatio /etc/hosts

<img width="917" height="602" alt="image" src="https://github.com/user-attachments/assets/d6e2fa5f-3520-4451-a2ae-e83d97788429" />

Kun tietokoneessa käytetään nimeä site1.com, yhdistä se IP-osoitteeseen 127.0.0.1. Testataan toimii tehdyt konfiguraatiot
<img width="822" height="527" alt="image" src="https://github.com/user-attachments/assets/fd1f94c7-0c9f-4737-9c6f-a6944568ea15" />

Sain tollei muutettua sen osoitteen site1.com
<img width="1287" height="191" alt="image" src="https://github.com/user-attachments/assets/556c8b30-2c1e-49b0-8ab8-74c77c1bb4a4" />

## Omo sivusto Site 1

Loin site1 kansio kotihakemistossa. 
Loin ja editoin samalla nanolla tiedosto index.html
Laitoin ihan perus html koodia sen sisällä. 

<img width="972" height="582" alt="image" src="https://github.com/user-attachments/assets/554881f0-95c6-49dc-bb67-6e9a2330f642" />

### annan seuraavaksi Apachelle oikeus lukea tiedostoja
Koska Apache toimii erillisenä käyttäjänä, sille täytyy antaa oikeus lukea verkkosivun tiedostoja. Tämän vuoksi käytin chmod-komentoja, joilla annoin tarvittavat luku- ja hakemiston käyttöoikeudet.

<img width="842" height="563" alt="image" src="https://github.com/user-attachments/assets/be464881-becd-4d4d-9e1b-90771c649f15" />

Seuraavaksi määritin Apachelle name-based virtual hostin. Luo­in tiedoston `/etc/apache2/sites-available/site1.com.conf`, jossa määrittelin `site1.com`-osoitteen käyttämään omassa kotihakemistossani sijaitsevaa `site1`-hakemistoa.

`ServerName` määrittää verkkotunnuksen, johon virtual host vastaa, ja `DocumentRoot` määrittää hakemiston, josta Apache hakee verkkosivun tiedostot. Lisäksi määrittelin `<Directory>`-asetuksen ja `Require all granted` -määrityksen, jotta Apache voi tarjota hakemiston sisältöä.

Aktivoin virtual hostin komennolla `a2ensite` ja tarkistin Apache-konfiguraation komennolla `apache2ctl configtest`. Testi palautti `Syntax OK`, minkä jälkeen latasin Apache-konfiguraation uudelleen. Lopuksi testasin sivustoa komennolla `curl http://site1.com` sekä verkkoselaimella. Sivusto toimi odotetulla tavalla.

<img width="941" height="626" alt="image" src="https://github.com/user-attachments/assets/d11c0cf3-192b-429b-bf43-411efaf57dd8" />

Otetaan Virtual Host käyttöön: 
<img width="987" height="587" alt="image" src="https://github.com/user-attachments/assets/4f4391c1-0037-4847-a201-ef836a584c67" />

## Palomuuri UFW

Asensin ja otin käyttöön UFW-palomuurin. Ensin tarkistin palomuurin tilan komennolla sudo ufw status ja sallin HTTP-liikenteen portissa 80 komennolla sudo ufw allow 80/tcp.

<img width="847" height="545" alt="image" src="https://github.com/user-attachments/assets/2090b648-3326-49c8-bfc8-7dc28150e070" />

Näin palomuuri saatiin päälle: 

<img width="715" height="405" alt="image" src="https://github.com/user-attachments/assets/8ad62b2c-ff60-4fe0-9849-2964b88eb0f7" />

## Apache-logeja
<img width="823" height="527" alt="image" src="https://github.com/user-attachments/assets/a3e31caf-617d-436c-ad7f-c4742eb59cb0" />




# Keskeiset havainnot ja Pohdinta
Opin asentamaan VirtualBoxin ja Debianin virtuaalikoneelle sekä käyttämään Linuxin peruskomentoja. Haastavinta oli Debianin asennuksessa tullut virhe, mutta se ratkesi virtuaalikoneen uudelleenkäynnistyksellä. Muuten asennus onnistui melko helposti ohjeita seuraamalla.

# Yhteenveto:
Tavoite saavutettiin, koska sain toimivan Debian Linux -virtuaalikoneen asennettua omalle tietokoneelleni.

### Lähteet

https://medium.com/@cbates255/apache-web-server-on-ubuntu-linux-c0072c995117
