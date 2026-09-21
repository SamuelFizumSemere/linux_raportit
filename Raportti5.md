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

Loin tiedostoja ja index.html. Muokkasin näitä sitten nanolla linuxuser-käyttäjänä. 

<img width="817" height="517" alt="image" src="https://github.com/user-attachments/assets/e6ebd570-5351-45f3-a90c-8d575347c269" />


<img width="812" height="515" alt="image" src="https://github.com/user-attachments/assets/c8a8bd08-2b35-4e95-9963-40d75ec10564" />

## VirtualHostin käyttöönotto

Konfiguraatio tehty
<img width="953" height="626" alt="image" src="https://github.com/user-attachments/assets/d5d0f796-a1f9-4d19-92f7-d692204bcdeb" />

tarkistetaan apache: 

<img width="791" height="315" alt="image" src="https://github.com/user-attachments/assets/881eefd2-d265-4eaf-a1af-f0cdff822489" />

syntax ok !!!

## Certbotin asennus ja TSL sertifikaatien luonti

Asennetaan certbotin: 

<img width="846" height="477" alt="image" src="https://github.com/user-attachments/assets/12636f74-e364-40af-9bad-765e46ca0d78" />

aktivoitu/haettu kai :)

<img width="820" height="335" alt="image" src="https://github.com/user-attachments/assets/818ceac1-d110-497c-9ca9-68261fe3aa83" />


## HTTPS:n testaaminen

 Testasin sivustoa selaimella HTTPS-osoitteella sekä komennolla muttei mennyt nappin :) 
 vaikka apache on kunnossa. 

<img width="818" height="185" alt="image" src="https://github.com/user-attachments/assets/401e0314-ac9b-4613-ae2d-2f6a84450824" />



## Certificate Transparency / crt.sh

Jotain tässä meni pieleen kun en löytänyt mitään julkaistuja sertifikaatit. (... 


## Certificate Renewal

<img width="836" height="191" alt="image" src="https://github.com/user-attachments/assets/73537779-5ec3-497c-adba-7596acd19b07" />




# Keskeiset havainnot ja Pohdinta
Opin juttuja kyllä mutta tuli niin paljon haasteita :)

# Yhteenveto:
Tavoite saavutettiin, koska sain tehtyä melkein kaikki onnistuneesti.

### Lähteet
