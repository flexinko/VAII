# Folklórny portál
 
Jakub Lech
5ZYI32
 
## Stručný opis projektu
 
Folklórny portál je webová aplikácia pre folklórny súbor, ktorá spája verejný web súboru a interný portál pre jeho členov. Rieši problém, že organizácia súboru dnes prebieha cez skupinové chaty, tabuľky a zdieľané priečinky. Noty, nahrávky a materiály ku choreografiám sú roztrúsené, program vystúpenia sa posiela ako správa a vedúci musí každého zvlášť zisťovať, či na vystúpenie alebo skúšku príde.
 
Pre verejnosť (fanúšikov súboru) aplikácia funguje ako web súboru: návštevník si pozrie aktuálny repertoár (choreografie) a plánované vystúpenia. Po prihlásení sa mení na portál člena s kalendárom vystúpení a skúšok, potvrdzovaním účasti a knižnicou materiálov. Súbor sa delí na hudobnú a tanečnú zložku a každý člen vidí materiály svojej zložky: hudobník noty a nahrávky, tanečník materiály ku choreografiám. Kto chce byť členom, zaregistruje sa a požiada o členstvo v zložke. Do portálu sa dostane až po schválení vedúcim.
 
Aplikácia je navrhnutá ako produkt, ktorý si môže nasadiť ľubovoľný súbor: jedna inštalácia slúži jednému súboru a jeho identitu (názov, logo, popis, kontakt) si nastaví hlavný vedúci. Ukážkové dáta obsahujú fiktívny súbor.
 
## Role v projekte
 
Rola, zložka a stav účtu sú stĺpce v tabuľke používateľov (`role`, `section`, `status`), nie samostatné tabuľky.
 
| Rola | Oprávnenia a zodpovednosť |
|---|---|
| **Návštevník** (neprihlásený) | Vidí verejnú časť: úvod, zverejnený repertoár a verejné vystúpenia. Môže sa zaregistrovať a prihlásiť. K interným dátam nemá prístup. |
| **Čakateľ** (registrovaný, neschválený) | Zaregistroval sa a požiadal o členstvo v zložke. Kým ho vedúci neschváli, vidí iba verejnú časť a stav svojej žiadosti. |
| **Člen** (hudobník / tanečník) | Vidí portál člena: nástenku, kalendár vystúpení a skúšok, potvrdzuje svoju účasť. Má prístup ku knižnici materiálov iba svojej zložky a materiály si môže stiahnuť. Nemôže meniť repertoár ani udalosti. |
| **Vedúci zložky** (vedúci hudby / vedúci tanca) | Má práva člena. Navyše schvaľuje žiadosti o členstvo do svojej zložky, spravuje materiály svojej zložky, vytvára skúšky svojej zložky a vidí prehľad účasti. |
| **Hlavný vedúci** (správca) | Spravuje celý súbor: choreografie, vystúpenia a ich program, všetkých členov (rola, zložka, deaktivácia), materiály oboch zložiek, regióny a nastavenia súboru. |
 
## Prípady použitia podľa rolí
 
**Návštevník**
- návštevník si prezrie zverejnený repertoár súboru a vyfiltruje choreografie podľa regiónu,
- návštevník si prezrie plánované verejné vystúpenia a ich detail,
- návštevník sa zaregistruje (meno, e-mail s priebežnou kontrolou dostupnosti, heslo 2×) a vyberie zložku, do ktorej žiada o členstvo,
- návštevník sa prihlási e-mailom a heslom.
- 
**Čakateľ**
- čakateľ sa prihlási a vidí stav svojej žiadosti (čaká na schválenie / zamietnutá).
- 
**Člen — udalosti**
- člen vidí na nástenke najbližšie vystúpenia a skúšky so stavom svojej účasti,
- člen potvrdí alebo zruší účasť na vystúpení či skúške jedným kliknutím,
- člen si zobrazí detail vystúpenia s programom choreografií.
- 
**Člen — knižnica**
- člen vyhľadá a vyfiltruje choreografiu v knižnici svojej zložky,
- hudobník si stiahne noty a prehrá nahrávku, tanečník si pozrie materiály ku choreografii,
- člen si upraví profil (meno, nástroj alebo poznámka) a zmení heslo.
- 
**Vedúci zložky**
- vedúci schváli alebo zamietne žiadosť o členstvo do svojej zložky,
- vedúci nahrá materiál ku choreografii pre svoju zložku, upraví jeho názov a popis alebo ho zmaže,
- vedúci vytvorí, upraví a zruší skúšku svojej zložky,
- vedúci si prezrie, kto potvrdil účasť na udalosti.
- 
**Hlavný vedúci — repertoár a udalosti**
- hlavný vedúci vytvorí, zobrazí, upraví a zmaže choreografiu a určí, či je zverejnená,
- hlavný vedúci vytvorí, zobrazí, upraví a zmaže vystúpenie,
- hlavný vedúci zostaví program vystúpenia: pridá choreografie, zmení ich poradie a odoberie ich,
- hlavný vedúci spravuje regióny.
- 
**Hlavný vedúci — súbor a členovia**
- hlavný vedúci zmení členovi rolu alebo zložku a deaktivuje jeho účet,
- hlavný vedúci upraví nastavenia súboru (názov, popis, kontakt) a nahrá logo.
## Plánované entity
 
| Entita (tabuľka) | Účel | Najdôležitejšie atribúty | Vzťahy |
|---|---|---|---|
| **Používateľ** | účet člena, vedúceho alebo čakateľa | `first_name`, `last_name`, `email` (jedinečný), `password_hash`, `role` (`member` / `leader` / `admin`), `section` (`music` / `dance`), `status` (`pending` / `active` / `rejected` / `inactive`), `note` | 1:N k materiálom (nahral), M:N k udalostiam cez účasť |
| **Región** | folklórny región, z ktorého choreografia pochádza | `name`, `description` | 1:N k choreografiám |
| **Choreografia** | číslo repertoáru, ktoré súbor uvádza | `region_id`, `name`, `author`, `duration_min`, `public_description`, `internal_note`, `photo`, `is_public` | patrí regiónu, 1:N k materiálom, M:N k udalostiam cez program |
| **Materiál** | súbor v knižnici zložky | `choreography_id`, `section` (`music` / `dance`), `type` (`sheet` / `audio` / `video` / `document`), `title`, `description`, `stored_filename`, `size`, `uploaded_by`, `created_at` | patrí choreografii a používateľovi, ktorý ho nahral |
| **Udalosť** | vystúpenie alebo skúška | `type` (`performance` / `rehearsal`), `title`, `starts_at`, `place`, `description`, `section` (pri skúške; prázdne = spoločná), `is_public` | M:N k choreografiám cez program, M:N k používateľom cez účasť |
| **Program vystúpenia** | zaradenie choreografie do programu udalosti | `event_id`, `choreography_id` (jedinečná dvojica), `position` | patrí udalosti a choreografii |
| **Účasť** | odpoveď člena na udalosť | `user_id`, `event_id` (jedinečná dvojica), `status` (`yes` / `no`), `updated_at` | patrí používateľovi a udalosti |
| **Nastavenia súboru** | identita súboru, ktorý aplikáciu používa (jeden záznam) | `name`, `description`, `logo`, `contact_email`, `city` | — |
 
Do povinných entít sa rátajú Región, Choreografia, Materiál a Udalosť. Používateľ, spojovacie tabuľky (Program, Účasť) a Nastavenia súboru sú navyše. Súbory materiálov sa ukladajú na disk pod vygenerovaným menom, v databáze je len cesta a metadáta.
 
## Vzťahy medzi entitami
 
- **Región 1:N Choreografia** — choreografia patrí do jedného regiónu, región má viac choreografií.
- **Choreografia 1:N Materiál** — k jednej choreografii patria materiály pre hudbu aj tanec, zložku určuje atribút `section`; pri zmazaní choreografie sa zmažú aj jej materiály (záznamy aj súbory).
- **Používateľ 1:N Materiál** — evidencia, kto materiál nahral.
- **Udalosť M:N Choreografia** (cez Program vystúpenia) — vystúpenie má program z viacerých choreografií, tá istá choreografia sa uvádza na viacerých vystúpeniach; tabuľka uchováva poradie.
- **Používateľ M:N Udalosť** (cez Účasť) — člen odpovedá na viacero udalostí a na udalosť odpovedá viac členov; jeden člen má k udalosti najviac jednu odpoveď.
## Hlavné stránky aplikácie
 
| ID | Stránka | Účel |
|---|---|---|
| S0 | Úvod | verejná stránka súboru (z nastavení) a najbližšie verejné vystúpenia |
| S1 | Repertoár | zverejnené choreografie s filtrom podľa regiónu |
| S2 | Vystúpenia | zoznam plánovaných verejných vystúpení a ich detail |
| S3 | Registrácia | založenie účtu s výberom zložky a živou kontrolou dostupnosti e-mailu |
| S4 | Prihlásenie | prihlasovací formulár s odkazom na registráciu |
| S5 | Nástenka | najbližšie udalosti so stavom mojej účasti a nové materiály mojej zložky; čakateľ tu vidí stav žiadosti |
| S6 | Knižnica | choreografie s materiálmi mojej zložky, vyhľadávanie a filter bez obnovenia stránky |
| S7 | Detail choreografie | popis, materiály mojej zložky na stiahnutie, prehrávanie nahrávok; vedúcemu upload a mazanie materiálov |
| S8 | Kalendár udalostí | vystúpenia a skúšky s potvrdzovaním účasti |
| S9 | Detail udalosti | informácie, program vystúpenia a prehľad účasti (pre vedúcich) |
| S10 | Môj profil | úprava údajov a zmena hesla |
| S11 | Žiadosti a členovia | schvaľovanie a zamietanie žiadostí, zoznam členov; hlavnému vedúcemu zmena roly a zložky, deaktivácia |
| S12 | Správa choreografií | zoznam a formuláre choreografií, zverejnenie, správa regiónov |
| S13 | Správa udalostí | formuláre vystúpení a skúšok, zostavenie programu a poradia choreografií |
| S14 | Nastavenia súboru | názov, popis, kontakt a logo súboru |
 
## Rozdelenie funkcionality
 
**Základné funkcie**
- registrácia so žiadosťou o členstvo, prihlásenie a odhlásenie, heslá cez `password_hash`,
- schvaľovanie členov vedúcim zložky; do portálu sa dostane iba schválený používateľ,
- odlišný obsah pre návštevníka a člena, knižnica obmedzená podľa zložky; oprávnenia sa kontrolujú na serveri pri každej akcii,
- plné CRUD nad choreografiami, materiálmi a udalosťami, správa regiónov,
- upload materiálov (PDF, MP3, obrázky) a loga s kontrolou typu a veľkosti na serveri, stiahnutie a mazanie,
- zostavenie programu vystúpenia s poradím,
- AJAX: filtrovanie choreografií, potvrdzovanie účasti, schvaľovanie žiadostí, kontrola dostupnosti e-mailu,
- kontrola vstupov na klientovi aj serveri, dopyty cez prepared statements,
- responzívny dizajn s vlastnými CSS pravidlami,
- spustenie v Dockeri s ukážkovými dátami (fiktívny súbor, regióny, choreografie, udalosti, testovacie účty pre každú rolu).
- 
**Rozširujúce funkcie**
1. piesne a harmonizácie: choreografia sa rozdelí na piesne (Choreografia 1:N Pieseň) a k piesňam sa uložia akordy s transpozíciou do inej tóniny v JavaScripte,
2. prehrávač nahrávok na cvičenie (HTML5 Audio API): spomalenie a opakovanie úseku,
3. zmena poradia choreografií v programe pomocou drag & drop,
4. upload materiálov pretiahnutím súboru s náhľadom pred odoslaním,
5. mapa miesta vystúpenia (Leaflet + OpenStreetMap),
6. štýly písané v SASS,
7. tlačová verzia programu vystúpenia,
8. viac súborov v jednej inštalácii (multi-tenant).
