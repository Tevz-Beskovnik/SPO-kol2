# SPO predavanja

od 45 strani zadnjega PDFa ne pride vec zraven snov v upostev za 2.kol

Narejeno:
- Objektni moduli, ELF,
- Programske knjižnice,

# Vprašanja

Driverji:
- Kaj dela strategijska rutina gonilnika? Razvršča glave zahtevk pri vhodni vrsti zahtev, le lastna vsakemu gonilniku
- Kaj je glave zahtev? Gonilnik jih dostopa dvakrat, ko gre po skaldu dol po gonilnikah in ko gre po skladu gor
- Kaj so preknitveni vektroji? So naslovi do rotun, ki se kličejo ob prekinitviji in obdelajo prekinitev, pri pametnih V/I napravah dobijo prekinitveni vektorji dodatne funkcionalnosti (kaže na naslov ISRja)
- Kaj delajo gonilniki vodila? skrbijo za plug & play, skrbijo za hibernacijo, vodi seznam naprav na vodilu, sharnjuje seznam vseh naprav na vodilu
- Ali lahko prekinitve prekinjajo druge prekinitve? Odnisno od arhitekture procesorja (8-bitni ne) in pomembnosti prekinitve (ali so uporabniske ali sistemske (sistemske ja))
- Zakaj imamo locene podatkovne vmesnike? Zaradi zagotavljanja varnosti
- Kaj se zgodi ce se sistemski vmesnik zapoln? PO strategiji Round robin se prepisuje
- Kaj je vloga preklopne tabele? za iskanje rutin gonilnika, ko se isce po major stevilki
- Kaj je razlika med monilitnimi in mikro jedri, katero je bolj varno? Monolitna jedra ima več sistemskih serisov v kernel modu, mikro jedra 
- Kaj je problematika kontekstnih preklokov?
- Kdaj nastane napaka SEGFAULT?
- Problematika sistemskih klicov? V tesni povezavi s kontekstnimi preklopi
- Kako delimo gonilnike? Gonilnika razreda, naprave, vodila, blocni, znakovni, network
- Memory mapping naprave?
- Kaksna je razlika med blokirajocimi in neblokirajocimi in asinhronimi klici?
- Kaksna je razlika med tipanjem in prekinitvami?
- Zgornja spodnja polovica prekinitvene rutine?
- vloga /dev direktorija in preklopne tabele?
- Kaj je VID in PID? 
- Vloga registra v Windows sistemu?
- Kaj je hibernacija, kaj je plug & play
- Problem VDM modela, kako sta to resila Windowsova frameworka?
- Zakaj microsoft zeli cimvec userspace driverjov? Zeli cim vec gonilnikov izven kernel spaca
- Kako je sestavljena glava zahtev? fiksni del, gonilniski del
- Kaj je sklad gonilnikov? Minimalno sodelujeta gonilnik naprave in gonilnik vodila
- Bistveni koraki namestitve gonilnikov? datoteka gonilnika (.sys), VID & PID .INF datoteka
Knjiznice:
- Verzioniranje?
- Zgodnjo in pozno povezovanje? Zgodi se ob izvajanju, zgodnje - ko se program zazene, pozno - ob prvem klicu
- Eksplicitno implicitno povezovanje?
- Verzioniranje in dll hell?
Java:
- Nadgradnja dinamicnega povezovanja?
- Sintakticno popolna simbolicna imena?
- Interpretacija vmesne kode v domorodno kodo?
- Ahead of time compiling?
- Problematika ohranjanja simbolnih imen?
- Problematika prevajanja, interpretiranja in pretvarjanja kode v domorodno kodo?
- Koda se pojavi v kopici navdezenega stroja, torej jo lahko spreminjamo
- Kaj vse sestavlja sintakticno popolno ime rutine? Ime razreda, ime metode, vhodni parametri, tipi in st parametrov, tipi izhodnega parametra
- Kaj je nabor konstant pri javi, zakaj je tako ime? SImbolna tabela, tabela simbolov je konstantna
- Problem poznega povezovanja v smilsu razresevanja hierhije? Hierarhicno je rekurzivno potrebno razresevati povezave med razredi
- Razredno nalaganje?
- DVM vs JVM?
- Kaj je refleksija, metapodatki, manifest?
.NET:
- PE format v .NET?
- metapodatki, refleksija?
- .NET zbir?
- Kaj je strong name?
- Kaksna je razlikam med stack based in register based interpreterom?
Nalaganje:
- BIOS vs UEFI, prednosti slabosti?
- Kaj je secure boot? Deluje s pomocjo certificiranjem gonilnikov, preverjanje certifikatov gonilnikov
- Veriga zaupanja, vrh verige zaupanja?
- Vloga TPMja?

## Binarani izvedljivi formati

- ELF, PE - PE je samo windowsova verzija ELF, oboje narejeno na osnovi coff formata

## Statično povezovanje

- Postopek statičnega povezovanja:
    koda --(prevajanje)-> objektni modul --(statično povezovanje)-> izvedljiv modul --(nalaganje)-> naložen program --> izvajanje

- Med statičnim povezovanjem se:
    - Dedljuje pomnilniški prostor
    - Povezovanje medsebojnih posameznih prevedenih modulov, ter simbolov, ki so uporabljena med njimi
    - Prenaslavljanje vseh prenaslovjivih operandov oz. naslovov.
    - Dopolnjevanje izvedljivih programov s sistemskimi rutinami

- Povezovalnik lahko skupaj poveže več objektnih modulov v: 
    - en prenaslovljiv izvedljivi modul (Takšen se lahko naloži na poljuben naslov)
    - en absolutni izvedljivi modul (Ta se lahko naloži samo na točno določen naslov)
    - en objekni modul (ta se lahko naprej povezuje ni izvedljiv)
- Če imamo v dveh modulah dve sekciji z enakim imenom potem se ti lahko naložita na enak naslov, če se vsebinsko delno ujemata, tudi če je ena izmed njiju daljša, problem nasatane kadar se ne ujemata sekciji in jih ne moramo združiti potem povezovalnik javi napako.
- Prenaslovljive sekcije se skupaj združijo v enotno prenaslovljivo sekcijo, ki ima prenaslovljive simbole v skupnji prenaslovljivi tabeli.
- vstopna (import) in zunanja (export) tabeli pa vsebujeta relativne odmike znotraj zunanjih sekcij

## Programske knjižnice

Kaj je?
- zbirka programskih modulov istega tipa, z dogovorjenim dostopom do posameznih modulov/vsebin
- so lahko statično ali dinamično povezane s našim programom
- pogosto več knjižnic za ista opravila (se križam uporabljajo druge knjižnice, ker želimo lastne implementacije, iste funkcionalnosti za različne namene, različne tehnologije, optimizacije, apiji, zalični ekosistemi, licence, itd...)
- delitev na sistemske in uporabniške
- knjižnice za sistemsko pomoč

Zgradba:
- Ponavadi organizirane v manjše logične enote
- vsebina sklopa se nahaja na istem logičnem naslovu
Pri objektnih modulih:
- če je sestavljena iz večih objektnih modulov se ponovijo vse globalne tabele posameznih, torej se imena ne smejo prekrivati oz. ponavljati.

### Dinamično povezljive knjižnice

- Dinamična knjižnica ločena od izvedljivega programa
- Omogoča ločeno vzdrževanje od programa
- Naloži se ob prvem klicu nato dostopna za vse Programe
- Ima fizično, logično ime in ime za povezovanje (v imenu tudi vključena verzija knjižnice)

Ustvarjanje:
- gcc -shared -W1,-soname,libctest.so.1 \ -o libctest.so.1.0 ctest1.o ctest2.o -lc
- Prestavimo v /usr/lib/
Povezivanje:
- Za povezovanje s knjižnico se uporabi zastavica -lctest
- dinamično povezovanje programatično preko knjižnice dlfcn.h

Kako se nalaga knjižnica ob prvem klicu?
- Ko se kliče funkcija npr. name1 in knjižnica ni naložena se:
    1. Skoči v linking table, ki preusmeri v global offset table
    2. Če knjižnica ni naložena ta vsebuje naslov do naslednjega ukaza
    3. Ta ukaz potisne na sklad odmi do name1 v realokacijski tabeli (ta trenutno vsebuje odmik do globalne offset tabele)
    4. za tem se skoči do značke ki potisne inicializacijski parameter za povezivalnik
    5. nato se skoči do dinamičnega povezovalnika znotraj GOT, ki naloži knjžnico
    6. nato se zamenja vstop za name1 v GOT z pravim naslovom name1

### Dinamične knjižnice v Windows

- Imenujejo se DLL
- koda je deljena med vse procese
- podatki so ponavadi privatnim, lahko tudi deljeni kar omogoča IPC, predstavlja pa tudi varnostno riziko
- Vsako funkcijo, ki jo DLL izvaža lahko identificiramo po **zaporedni števki** ali **po imenu**
- Main lahko funckije uvaza preko imena ali stevke
- Starjesi Windowsi so vnaprej dolocili naslove kjer se sys knjiznice nalozijo, kar je pomenilo da so v naprej bile dolocene nekatere sekcije navideznega pomonilnika, kar je bila omejitev za 32 bit arch ki je imela samo 4 gigs rama
- Ce so naslovi knjižnic v naprej določeni se lahko v naprej razrešijo naslovi.
Povezovanje:
- impkicitno ob prevajanju:
    - Po implicitnem povezovanju vsebuje program IAT (import adress table), ki vsebuje vse klicane funckije
    - Med izvajanjem se ta tabela napolni z dejanskimi polnilniskimi naslovi.
- eksplicitno z klicom funckije loadLibrary ali loadLibraryEx




