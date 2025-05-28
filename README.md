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

# Nalaganje

### Nalagalnik

- Nalozi izvedljiv program v pomnilnik in incializira izvajanje
    - Dodeli se pomnilniski prostor
    - Opravi se prenaslavljanje izvedljivega modula

- Vrste: prevedi in poveži, absolutni nalagalnik (nalozi na fiksen naslov, za absolutne sekcije), nalagalnik s prenaslvaljanjem (prenaslovitvena tabela)

### Absolutni nalagalnik

- Nalozi absolutne sekcije iz zunanjega spomina v RAM
- **začetni nalagalnik** (stalno nalozen v delu RAMa)
- Nalagalnik je del interne procesorjeve logike
- Vklopi samopreizkusanje (BIOS)

### Zacetni nalagalnik 

- Master boot record MBR
- Sektor 0 na disku je MBR (512B), sestavljen iz vecih delov:
    - Koda zacetnega nalagalnika (440 bajtov)
    - vstop v particijski tabeli (16 bajtov en dolg so pa 4x)
    - Podpis MBR: 0xAA55 (2 bajta cisto na koncu prvi 512 bajtov)

Particijski nalagalni sektor:
- Je patricijska tabela nalagalne naprave (v BMR): ima lahko 4 vstope
- en je dolg 16 bajtov
- primarne particije (v MBR)
- razsirjene particije, vsebujejo v svoje nalagalnem sektorju novo particijsko tabelo
    - notranje razsirjene particije
    - logicne particije: notranje particije ki niso razsirjene

Particijski nalagalni sektor:
- Ima lastnosti nalagalne sekcije, je FAT32 in NTFS

Zacetni nalagalnik pri osebnem racunalniku:
- ob zagonu se nalozi ukaz v na BIOS naslovu v CS:IP  na 0xF000:0xFFFF0
    - to je naslov do skoka ki gre na vklop in samopreizkusanje (Power On and Self Test), ki preveri in incializira V/I naprave
- BIOS preveri priklopljene medije v dolocenem vrstnem redu, dokler ne najde magic bajtov (0xAA55) v prvem sektroju (prvih 512 bajtov)
- BIOS nato iz tega sektroja prebere kodo in jo nalozi v izvajanje
- Koda MBR v particijski tabeli nato poisce aktivno particijo in jo nalozi v pomnilniski
- Ta koda nato zacne nalagati jedro os, ki preklopi delovanje CPE iz realnega v zavarovano (protected)

Zacetni nalagalnik GRUB:
- Nalozi MBR tam kjer je koda 0xAA55
- Ta koda je zacetni del GRUB nalagalnika, ki potem se nalozi ostale dele gruba (v naslednjih sekcijah za mbr) 
- Drugi del gruba nato prikaze graficni meni z izbiro razlicnih OSov oz. razlicnih verzij jedra
- Po izbiri GRUB zacne nalagat jedro tega OSa in mu preda kontrolo.
- Ce nalagamo windows, ki ne podpirajo multiboota, si GRUB v naprej shrani zacetni nalagalnik windowsa, ki ga klice potem in zgleda kot da bi se windows zagnal iz standardnega MBRja

### BIOS

- Ima svoje gonilnike, ker se vedno potrebuje interaktirati z perifernimi naprvami ce zelimo spreminjat nastavitve
- Sklad tehnologije: HW -> HAL -> Kernel space -> User space
- Jedro ima HAL zato da ni potrebno spreminjati jedra za vsako novo napravo, ki se priključi

### Slabosti BIOSa

- Bios je bil izdelan za 16 in 32 bitne sisteme
- 16 bitni nacin delovanja procesorja, imamo samo 1mb pomnilnika na voljo
- Maximalna dolžina particij je bila 2TB zaradi 32 bitne arhitekture, najvecji podprt disk je bil 4TB

- Možne rešitve za problem BIOSa:
    - UEFI - unified extensible firmware interface
    - Coreboot
    - openBIOS
    - unificiran HAL
    - Linux kernel kot glavni bootlaoder

### UEFI

- Razvil ga je Intel, uporabljajo ga prakticno vsi zdaj
- Posreduje services tako zacetnemu nalagalniku kot OSu,
    - npr. V/I gonilniki za periferne naprave
- UEFI se zacel uporabljat zaradi varnostnih razlogov, omejitev zunanjih pomnilnih medijev, itd..,
- Omogoca 128 particij, podpira ogromne particije
- omogoca secure boot, ki deluje na osnovi certifikatov
- UEFI shell - bogata podpora, ima Unix ukaze; md, cd, rm, copy, del, dir
- Za zaganjanje OSov se uporablja uefi bytecode, ki se shranjuje v .EFI datotekah, ki povedo kako se os boota nahaja se v \EFI\BOOT\BOOT{neko ime}.EFI
- Omogca nalaganje preko mreze
- Ima podporo za CMS (compatibility support module) backward compatibility - emulacija BIOSa za legacy ose
- GOP - graphical output protocol - odstrani odvisnost na VGA
- UEFI ne predpisuje graficnega vmesnika, to ga naredi proizvajalec
- uporablja standardno particijsko shemo in GUID

### Secure boot

- Temelji na podpisovanju nalagalnikov z kljuci in preverjanje podpisov
- TPM trusted platform module - cip ki shranjuje sifrirne kljuce, uporablja SHA, RSA, AEF. UEFI ga uporablja za kljuc platforme
- Podpisovanje gonilnikov - gonilnik se poslej MSju, ki ga preveer in podpise, to se naredi tak da se program hasha, ter skriptira z kljucem, ta se potem prilozi kot cert, ki gal ahko ob nalaganju odkripitramo z MSjevim javnim kljucom -> iz tega dobimo hash in ga preverimo z nasim gonilnikov, ce se ujemata je gud, ce ne je bila koda gonilnika spremenjena
- Isot kot z gonilniki pri MS se dela z zagonskimi particijami pri secure bootu

### Kako je zgledal prehod in BIOS na UEFI

1. BIOS -> 2. UEFI compatibility za BIOS -> 3. UEFI in CSM boot -> 4. samo UEFI

### GUID 

-
