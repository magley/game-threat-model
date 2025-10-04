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
*Slika 1. Dijagram toka podataka igrice MMORPG žanra*

---

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

---

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

*Tabela 1. Identifikovani resursi*


![img](./data-threat-flow-resources.png)

*Slika 2. Dijagram toka podataka sa prikazom identifikovanih resursa*

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

*Tabela 2. Prikaz potencijalnih pretnji visokog nivoa za identifikovane resurse*

Glavna motivacija napadača i varanja u MMORPG žanru:
- Takmičarska prednost: želja igrača da dominiraju nad drugima i budu percipirani kao vješti ili moćni unutar zajednice igre.
- Ekonomska dobit: igrači varaju da bi stekli rijetke predmete ili valutu koje mogu prodati za profit.
- Ušteda vremena: da zaobiđu ponavljajuće ili vremenski zahtjevne zadatke.

## 2. Identifikacija pretnji i napada

U nastavku analizirana je pretnja P41. - Manipulacija podacima radi bržeg napredovanja. Slika ispod daje prikaz pretnji niskog nivoa, napada i odbrambenih menanizama za odabranu pretnju.

![img](./napadi_mitigacije.png)

*Slika 3. Stablo napada za pretnju P41*   

### Botovi za Farming
Botovi su programi ili skripte koji emuliraju akcije igrača i ponavljaju ih beskonačno dugo. U MMORPG igrama, botovi se koriste da bi se brzo generisao XP kroz repetitivne zadatke, kao što je ubijanje slabih protivnika ili skupljanje resursa. Pošto botovi mogu raditi 24/7, igrač koji ih koristi stiče ogromnu prednost nad ostalim igračima.

Mitigacije:
- **Analiza logova i ponašanja**: server prikuplja telemetriju o akcijama igrača i ritmu komandi. Ako se detektuje nerealno ponašanje (npr. igrač koji 16 sati bez prestanka ubija istog NPC-a), sistem označava nalog kao sumnjiv. Ovo je ključna mitigacija jer botovi koji imitiraju inpute često uspijevaju da prevare anti-cheat softvere.
- **HoneyPots**: namjerno postavljena zamka koja je dizajnirana da privuče napadače ili botove. Ideja je da igrač koji igra normalno neće nikad reagovati na honeypot, ali bot koji izvršava skripte hoće. Na primjer, server kreira NPC-a koji je nevidljiv ljudima, ali postoji u kodu. Botovi će mehanički napasti takav resurs, dok ljudski igrači ne.
- **Anti-cheat softveri**: Mogu da detektuju one botove koji vrše manipulaciju samog procesa igre. Ako bot radi izvan procesa igre (npr. emulator miša) odnosno ako ga ništa ne povezuje sa igrom u memoriji, tu softver ne pomaže. Zbog toga se tretira kao dopunska, a ne glavna mitigacija.


### Client Hook
Ovaj napad se odnosi na modifikaciju klijentske aplikacije igre ili umetanje dodatnog koda koji presreće i mijenja podatke koji se šalju serveru. Na primjer, napadač može manipulisati vrijednošću promjenljive koja predstavlja količinu XP-a, pa zatim poslati zahtijev serveru u kome stoji da je igrač stekao više XP-a nego što zapravo jeste.

Mitigacije: 
- **Anti-cheat softver (npr. BattleEye)**: radi na strani klijenta, prati integritet klijentskih fajlova i detektuje neautorizovane modifikacije. Ako detektuje manipulaciju, može blokirati igru ili prijaviti serveru.
- **Authoritative server model**: server preuzima potpunu kontrolu nad obračunom XP-a,, dok klijent šalje serveru samo akciju. Na primjer, klijent neće slati "zaradio sam 30XP poena završavanjem misije" nego „završio sam misiju“, a server provjerava validnost događaja i sam računa koliko XP treba da se dodijeli.


### Webhook replay prilikom kupovine opreme
Napadač šalje iste zahtjeve za kupovinu (putem webhooka) kako bi duplirao ili ilegalno generisao opremu u svom inventaru.
Kupovina itema se često obavlja putem eksternih payment provajdera. Kada kupovina prođe, provajder šalje serveru igre webhook (HTTP zahtjev sa potvrdom o uplati i detaljima transakcije). Ako server ne implementira zaštitu, napadač može snimiti jedan webhook i kasnije ga više puta poslati, čime bi za jednu uplatu dobio više itema.

Mitigacije: 
- **HMAC/Signature verification**: svaki zahtjev mora imati validan digitalni potpis kojeg server provjerava. To garantuje da zahtjev nije izmjenjen i da dolazi od legitimnog klijenta. Npr. payment provajder dodaje digitalni potpis (npr. HMAC SHA256) u zaglavlje svakog webhooka. Server provjerava potpis i odbacuje zahtjev ako je falsifikovan. Međutim, HMAC sam po sebi ne sprječava ponovno slanje istog zahtjeva, već je osnovni sloj zaštite koji sprječava falsifikovane ili izmijenjene zahtjeve. Zato se često koristi uz tkz. **Replay Window**. Ovo podrazumijeva da webhook mora sadržati timestamp trenutka slanja, koji je uključen u potpis (HMAC). Server provjerava da je timestamp unutar malog vremenskog prozora. Ako je poruka starija od toga, automatski se odbacuje. Ova mjera sprječava napadača da snimi webhook i pošalje ga ponovo kasnije.
- **Idempotency**: osigurava da ponovljeni zahtjevi ne proizvode višestruke efekte. Svaki webhook ima jedinstveni ID transakcije. Server čuva sve procesirane ID-jeve i odbacuje duplikate. Time se sprječava višestruka obrada iste kupovine.


### Race condition
Napad se javlja kada dva ili više procesa istovremeno pokušaju da promijene isti resurs (npr. dodavanje nekog rijetkog predmeta u inventar), a sistem ne kontroliše redoslijed tih promjena tj ne sinhronizuje pravilno izvršavanje transakcija. Ovo rezultira dupliranjem resursa i neispravnim ažuriranjem inventara.

Mitigacije: 
- **Idempotency**: isto kao prethodno. Svaki zahtjev dobija jedinstveni identifikator. Ako server primi dva ista ID-ja, obrađuje ga samo jednom. Ovaj mehanizam eliminiše efekat dupliranja i greške.  
- **ACID transakcije (Atomicity, Consistency, Isolation, Durability)**: na serverskoj strani garantuju da se sve operacije izvršavaju u potpunosti ili uopšte ne, čime se eliminišu posljedice race condition-a. Npr. kada igrač kupuje item, server u jednoj transakciji će radi: provjerava da li igrač ima dovoljno zlata, oduzima zlato, dodaje item u inventar. Pošto je sve ovo jedna operacija, time se sprječava da za istu količinu zlata napadač dobavi više item-a.


### Packet Injection: 
Napadač presreće i mijenja mrežne pakete koje klijent šalje serveru ili može zaobići klijenta i direktno slati konstruisane mrežne pakete serveru. Ako server slijepo veruje svakom paketu, napadač može na primjer ubacivati predmete za inventar bez ograničenja. Ovakav napad je posebno opasan jer se mnogo akcija oslanja na mrežnu komunikaciju između klijenta i servera.

Mitigacije: 
- **Validacija paketa na serverskoj strani**: svaki zahtjev mora proći provjeru autentifikacije, autorizacije, validacije vezane za samu logiku događaja i konzistentnosti. Ovo osigurava da svaki paket sadrži samo dozvoljene i očekivane podatke. 
- **Rate limiting**: ograničava broj zahtjeva po klijentu ili IP adresi u određenom periodu, što pomaže da se napad ublaži i detektuje.


### Lag Switch

Napadač manipuliše svojom mrežom kako bi namjerno stvorio kašnjenja u komunikaciji između klijenta i servera. U PvP borbama to može omogućiti prednost, jer napadač može zamrznuti poziciju, a kada vrati vezu, server mu odjednom upisuje sve akcije. 

Mitigacije:
- **Anti-cheat softver (FairFight)**: sistem prati obrasce latencije i označava naloge koji često imaju sumnjive skokove u mrežnom kašnjenju praćene prednošću u igri.

- **Banovanje igrača**: kada se zloupotreba potvrdi, igrač se suspenduje ili trajno banuje, što predstavlja društveni i tehnički mehanizam odvraćanja od ovakvih napada.


### DDoS (Distributed Denial of Service)
Masovno slanje zahtjeva ka serveru radi preopterećenja i obaranja usluge, čime se drugim igračima onemogućava normalna igra.

Mitigacije:
- **Load balancing**: raspoređuje saobraćaj ravnomjerno između servera, povećavajući otpornost infrastrukture i omogućavajući kontinuitet usluge čak i u slučaju napada.

- **Rate limiting**: graničava broj zahtjeva po klijentu ili IP adresi, sprječavajući preopterećenje servera. 




## 3. Dubinska analiza odabranih napada i mitigacija