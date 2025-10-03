# Model pretnji igrica MMORPG žanra

## 1. Upoznavanje i dekompozicija modula

Analizirani modul predstavlja igricu _Massively multiplayer online role-playing game_ (MMORPG) žanra (igrice kao što su _WOW_, _LOTRO_, _Albion online_ i slične).

### 1.1. Tokovi podataka analiziranog modula


Funkcionalnosti koje igrač ima:
- Login ili registracija profila
- Odabir karaktera (izbor klase, rase, pola, imena i prilagođavanje vizuelnog izgleda lika)
- Odabir servera (napredak karaktera je specifičan za izabrani server)
- Gameplay igre (misije, borba sa neprijateljima (PvE), borba sa drugim igračima (PvP), rangiranje igrača, in-game trgovina, upravljanje guild-ovima i grupama, 
komunikacija sa drugim igračima kroz četove, napredovanje kroz XP sistem i nivoe, učešće u sezonskim događajima)
- Pretplate ili kupovina određenog sadržaja igre

Slika ispod prikazuje tok podataka analiziranog modula.

![img](./data-threat-flow-model.png)


**Igrač (Player)** je eksterni entitet koji komunicira sa klijentom. Igrač šalje ulazne komande/zahtjeve koristeći klijenta. 

**Klijent** je komponenta koju igrači koriste za povezivanje sa serverom i interakciju sa igrom. Klijent se pokreće lokalno (na računaru/mobilnom telefonu/konzoli) i predstavlja osnovni interfejs između igrača i servera. Glavna uloga je da prikuplja korisničke komande i da ih šalje serveru na obradu. Kao odgovor od servera klijent dobija ažurirano stanje igre i vrši prikazivanje tog virtuelnog svijeta igraču.
Klijent komunicira sa dva skladišta podataka: **Client Configuration** i **Game Resources**.

**Client Configuration**: skladište podataka koje čuva podešavanje korisničkog okruženja poput rezolucije, glasnoće zvuka, nivoa detalja i ostalih opcija za podešavanje klijenta.

**Game Resources**: skladište podataka koje čuva asete u okviru igre (mape, teksture, audio sadržaj i drugo).

---

**Account Managment Service (AMS)** je komponenta koja je zadužena za upravljanje nalozima i autentifikaciju. Povezana je sa eksternim entitetom za izvršenje transakcija (**Payment**). Komunicira sa skladištem **Account DB**.

**Account DB**: skladište podataka koje čuva podatke o korisničkim profilima i privilegijama.

---

**Realm Serveri** su komponente koje upravljaju logikom igre, ažuriraju stanje i ažurirano stanje šalju nazad klijentima na prikaz. Svaki server obično pripada određenom geografskog regionu (EU, NA, Asia i slično). Svaki server komunicira sa svoja dva skladišta podataka: **World Configuration** i **Players Data**.

**World Configuration**: skladište podataka koje skladišti podatke o resursima u svijetu, aktivnim događajima servera, ekonomiji servera.

**Players Data**: skladište podataka koje sadrži podatke o svakom karakteru servera: inventar, nivo, status misija, pozicija u svijetu, spisak prijatelja i slično.


### 1.2. Resursi i prijetnje

Tabela ispod identifikuje resurse i pruža kratak opis za svaki resurs, a na slici ispod tabele je prikazan tok podataka dopunjen identifikovanim resursima.

| Oznaka | Resurs                     | Opis                                                                 
|--------|----------------------------|----------------------------------------------------------------------|
| R1     | Korisnički nalozi          | Profili vezani za AMS.       |
| R2     | Konfiguracija     | Postavke za klijenta i za servere. | 
| R3     | Aseti     | Resursi vezani za klijenta.                | 
| R4     | Podaci o karakterima       | Podaci igrača vezani za jedan server.              | 
| R5     | Resursi svijeta              | Stanje svijeta nekog servera.                | 
| R6    | Logovi i telemetrija       | Evidencija akcija igrača.             | 
| R7     | Sistem plaćanja   | Transakcije i povezani payment nalozi.                       | 

![img](./data-threat-flow-resources.png)


Tabela u nastavku navodi pretnje za identifikovane resurse, kao i bezbjednosno svojstvo koje se narušava pretnjom.


| **Resurs**              | **Pretnja**                                          | **STRIDE tip**            |  
|--------------------------|-----------------------------------------------------|---------------------------|  
| R1. Korisnički nalozi    | P11. Phishing i krađa kredencijala                                  |  Spoofing, Information Disclosure |  
|         | P12. Aktiviranje pretplate                               | Elevation of Privilege, Tampering |  
|      | P13. Narušavanje reputacije                                  | Spoofing                  |  
| R2. Konfiguracija            | P21. Rušenje aplikacije                                  | Denial of Service         | 
| R3. Aseti           | P31. Prikaz skrivenih stvari sa mape                    | Information Disclosure    |  
| R4. Podaci o karakterima         | P41. Manipulacija podacima radi bržeg napredovanja             | Tampering, Elevation of Privilege                 |  
|             | P42. Dobavljanje privatnih četova radi rušenja reputacije| Information Disclosure    |   
| R5. Resursi svijeta       | P51. Kreiranje beskonačne zalihe određenih resursa       | Tampering                 |   
| R6. Logovi i telemtrija         | P61. Brisanje logova da se prikriju tragovi napada            | Repudiation, Tampering    | 
|   | P62. Prisluškivanje logova radi narušavanja reputacije    |  Information Disclosure    |   
| R7. Sistem plaćanja           | P71. Dupliranje transkacija            | Repudiation, Tampering    |  

Glavna motivacija napadača i varanja u MMORPG žanru:
- Takmičarska prednost: želja igrača da dominiraju nad drugima i budu percipirani kao vješti ili moćni unutar zajednice igre.
- Ekonomska dobit: igrači varaju da bi stekli rijetke predmete ili valutu koje mogu prodati za profit.
- Ušteda vremena: da zaobiđu ponavljajuće ili vremenski zahtjevne zadatke.

## 2. Identifikacija pretnji i napada

U nastavku analizirana je pretnja P41. - Manipulacija podacima radi bržeg napredovanja. Slika ispod daje prikaz pretnji niskog nivoa, napada i odbrambenih menanizama za odabranu pretnju.

![img](./napadi_mitigacije.png)
                           
                                                
## 3. Dubinska analiza odabranih napada i mitigacija