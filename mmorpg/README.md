# Model pretnji igrica MMORPG žanra

## 1. Tokovi podataka analiziranog modula

Analizirani modul predstavlja igricu _Massively multiplayer online role-playing game_ (MMORPG) žanra (igrice kao što su _WOW_, _LOTRO_, _Albion online_ i slične).

Funkcionalnosti koje igrač ima:
- Login ili registracija profila
- Odabir karaktera (izbor klase, rase, pola, imena i prilagođavanje vizuelnog izgleda lika)
- Odabir servera (napredak karaktera je specifičan za izabrani server)
- Gameplay igre (misije, borba sa neprijateljima (PvE), borba sa drugim igračima (PvP), rangiranje igrača, in-game trgovina, upravljanje guild-ovima i grupama, 
komunikacija sa drugim igračima kroz četove, napredovanje kroz XP sistem i nivoe, učešće u sezonskim događajima)
- Pretplate ili kupovina određenog sadržaja igre

Slika ispod prikazuje tok podataka analiziranog modula.

![img](./data-threat-flow-model.png)

**Igrač (Player)** je eksterni entitet koji komunicira sa klijentom. Igrač šalje ulazne komande/zahtjeve preko klijenta. 

**Klijent** je komponenta koju igrači koriste za povezivanje sa serverom i interakciju sa igrom. 

**Account Managment Service (AMS)** je komponenta koja je zadužena za upravljanje nalozima i autentifikaciju. Povezana je sa eksternim entitetom za izvršenje transakcija.

**Realm Serveri** su komponente koje upravljaju logikom igre, ažuriraju stanje i ažurirano stanje šalju nazad klijentima na prikaz. Svaki server obično pripada određenom geografskog regionu (EU, NA, Asia i slično).

## 2. Resursi i prijetnje

U nastavku su definisani resursi za svako skladište podataka iz prikazanog dijagrama podataka.

**Client Configuration**: omogućava podešavanje rezolucije, glasnoće zvuka, nivoa detalja i ostalog vezanog za podešavanja klijenta.

**Game Resources**: aseti u okviru igre (mape, teksture, audio sadržaj i drugo).

**Account DB**: podaci o korisničkim profilima i privilegijama.

**World Configuration**: podaci o resursima u svijetu, aktivnim događajima servera, ekonomiji servera.

**Players Data**: podaci o svakom karakteru servera (inventar, nivo, status misija, pozicija u svijetu, spisak prijatelja i slično).

Tabela ispod prikazuje pretnje za identifikovane resurse i bezbjednosno svojstvo koje se narušava.


| **Resurs**              | **Pretnja**                                          | **STRIDE tip**            |  
|--------------------------|-----------------------------------------------------|---------------------------|  
| R1. Korisnički nalozi        | P11. Narušavanje reputacije                                  | Spoofing                  |  
|         | P12. Aktiviranje pretplate                               | Elevation of Privilege    |  
| R2. Resursi svijeta servera       | P21. Kreiranje beskonačne zalihe određenih resursa       | Tampering                 |  
| R3. Konfiguracija            | P31. Rušenje aplikacije                                  | Denial of Service         |  
| R4. Podaci karaktera         | P41. Izmena podataka radi bržeg napredovanja             | Tampering                 |  
|             | P42. Dobavljanje privatnih četova radi rušenja reputacije| Information Disclosure    |  
| R5. Inventar karaktera       | P51. Dobavljanje aseta                                   | Tampering                 |  
| R6. Game Resources           | P61. Prikaz skrivenih stvari sa mape                    | Information Disclosure    |  
| R7. Sistem za plaćanje           | P71. Krađa novca             | Repudiation    |  

Glavna motivacija napadača odnosno varanja u MMORPG žanru:
- Takmičarska prednost: želja igrača da dominiraju nad drugima i budu percipirani kao vješti ili moćni unutar zajednice igre.
- Ekonomska dobit: igrači varaju da bi stekli rijetke predmete ili valutu koje mogu prodati za profit.
- Ušteda vremena: da zaobiđu ponavljajuće ili vremenski zahtjevne zadatke.


