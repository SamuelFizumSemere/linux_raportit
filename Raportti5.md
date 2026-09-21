# Johdanto:
TLS sertifikaatit 

# Toteutus ja tulokset 

# DNS

## dig-asennus ja DNS-kysely
<img width="903" height="366" alt="image" src="https://github.com/user-attachments/assets/e1fb3c43-8961-4854-8389-1096178a7abf" />

kuvasta nähdään että DNS toimii, kun tämä palauttaa sen ip-osoite: 

<img width="830" height="465" alt="image" src="https://github.com/user-attachments/assets/7c38ba64-ff99-483b-a76c-3f1fe4c5b11b" />



## DNS-liikenteen monitorointi tcpdumpilla

Käytössä oleva verkkoliitäntä oli enp0s3, joten DNS-liikennettä seurattiin

Avasin toisen terminaalin ja suoritin DNS-kyselyn komennolla : dig tls-test008.linuxkurssi.xyz.
Suoritin kyselyn kaksi kertaa ja tarkkailin tcpdumpin näyttämää liikennettä.
eli toinen kysely ei välttämättä aiheuta uutta DNS-verkkoliikennettä. 


<img width="955" height="818" alt="image" src="https://github.com/user-attachments/assets/98cb0764-82a2-4f22-be2e-6b53bf6f6552" />


## DNS-cache


Testasin DNS-kyselyä myös ulkoisella DNS-palvelimella komennolla
dig tls-test008.linuxkurssi.xyz @8.8.8.8.

<img width="942" height="872" alt="image" src="https://github.com/user-attachments/assets/d6a74e05-8fd7-4fcc-bc94-0a3240116aa0" />

# Name-Based VirtualHost

<img width="783" height="123" alt="image" src="https://github.com/user-attachments/assets/0a94fd2d-9a8c-4dfe-90e8-aaa64d472dbc" />


## index.html

## Muut tiedostot

## VirtualHostin käyttöönotto

# TLS Certificate

## Certbotin asennus

## TLS-sertifikaatin luonti

## HTTPS:n testaaminen

## Certificate Transparency / crt.sh

## Certificate Renewal

# Monitoring

## HTTPS-liikenteen monitorointi

## HTTP-liikenteen monitorointi

## HTTP → HTTPS redirectin poistaminen

## Redirectin palauttaminen


# Keskeiset havainnot ja Pohdinta
Opin asentamaan VirtualBoxin ja Debianin virtuaalikoneelle sekä käyttämään Linuxin peruskomentoja. Haastavinta oli Debianin asennuksessa tullut virhe, mutta se ratkesi virtuaalikoneen uudelleenkäynnistyksellä. Muuten asennus onnistui melko helposti ohjeita seuraamalla.

# Yhteenveto:
Tavoite saavutettiin, koska sain toimivan Debian Linux -virtuaalikoneen asennettua omalle tietokoneelleni.

### Lähteet
