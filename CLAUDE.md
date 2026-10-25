# CLAUDE.md – kontext projektu (VAII semestrálna práca)

Tento súbor číta Claude Code na začiatku každej session. Obsahuje všetko podstatné, čo sme rozhodli pri návrhu projektu.

## O mne a o spôsobe práce

- Som Jakub Lech, študent informatiky (FRI UNIZA), predmet **VAII – vývoj webových aplikácií**.
- **Kódu musím úplne rozumieť**, na obhajobe sa pýtajú doplňujúce otázky (inak hrozí Fx). Preto:
  - pracuj po **malých krokoch**, každý krok mi stručne vysvetli (čo robí, prečo takto),
  - nepíš veľké bloky kódu naraz, radšej postupne,
  - pri zložitejších veciach mi daj aj krátke „ako to vysvetliť na obhajobe“.
- **Kód vygenerovaný AI musí byť označený** komentárom, napr. `// AI (Claude): ...` alebo blokom `/* AI-generated: ... */`.
- Prevzatý kód (cvičenia, internet) musí mať v komentári **zdroj**.
- **Git**: commituj priebežne s jasnými správami – v repozitári musí byť vidieť postupný progres počas semestra.
- Komunikuj po **slovensky**.
- Commity a push robím sám v PhpStorme.

## Režim učenia (platí vždy)

- **Kód za mňa nepíš.** Projekt musím obhájiť sám.
- Keď chcem niečo spraviť: vysvetli, čo treba urobiť a prečo, ukáž krátky všeobecný príklad (pár riadkov) a nechaj ma napísať to sám.
- Keď napíšem kód: skontroluj ho, povedz čo je dobre, čo zle a prečo. Opravu neprepisuj, daj nápovedu.
- Súbory smieš priamo upraviť **len keď poviem „napíš to ty“**. Taký kód označ ako AI kód (v HTML `<!-- AI (Claude): ... -->`, v CSS `/* AI (Claude): ... */`).
- Po každom kroku mi polož 1–2 kontrolné otázky, ako na obhajobe.

## Aktuálna úloha – 4. týždeň: statické HTML + CSS

- **Bez frameworku, bez servera, bez JavaScriptu.** Stránky sa musia otvoriť dvojklikom v prehliadači.
- Všetko patrí do priečinka **`html/`** v koreni repozitára (HTML, CSS, obrázky). Cesty musia byť **relatívne** (`html/css`, nie `/css/style.css`).
- Treba spraviť:
  1. **Základné rozloženie**: hlavička s logom a názvom, navigácia, priestor pre obsah (placeholder), päta.
  2. **Jednu netriviálnu stránku**: **S6 Knižnica** (vyhľadávanie, filter regiónu, prepínač Všetko/Hudba/Tanec, karty choreografií s materiálmi, tlačidlá na stiahnutie). Ovládacie prvky nemusia fungovať, stačí vizuálne.
- Sémantické elementy (`header`, `nav`, `main`, `section`, `footer`).
- **Responzívne**: desktop aj mobil; na mobile iný počet stĺpcov a upravená navigácia; **žiadny horizontálny scroll**.
- Finálny dizajn nie je cieľ, ale držím sa farieb a písiem nižšie (portál = svetlý režim).
- Framework Vajko, Docker a databáza prídu až v ďalších týždňoch – časti nižšie o nich sú na neskôr.

## Povinné požiadavky zadania (skrátene)

- Server: **PHP + framework VAJKO** (https://github.com/thevajko/vaiicko), MVC, oddelená logika od prezentácie.
- Min. **5 dynamických stránok**, min. **50 riadkov vlastného JS**, min. **20 vlastných CSS pravidiel**.
- Min. **3 zmysluplné DB entity** (bez users a bez asociačných tabuliek), aspoň jeden vzťah **1:N alebo M:N**.
- **CRUD nad 2 entitami**, časť po **prihlásení**, **viac rolí + autorizácia**, **práca so súbormi (upload)**.
- **2× zmysluplný AJAX**, validácia vstupov **na klientovi aj serveri** (typ aj platná hodnota).
- Bezpečnosť: prepared statements (SQL injection), heslá cez `password_hash`, chránené endpointy.
- Responzívny dizajn. Spustiteľné v **Dockeri**, pri prvom spustení import **ukážkových dát**.
- Extra body: vlastný responzívny dizajn, SASS/pokročilé CSS, HTML5 API, externé API, pokročilá validácia na klientovi.
- Analýza/návrhy sa odovzdávajú do priečinka `analyza/` v repozitári (PDF).

## Téma

**Portál pre folklórny súbor** – konkrétne **FS Drienovec, Bánovce nad Bebravou** (jedna inštalácia = jeden súbor; identitu súboru nastavuje hlavný vedúci). Ukážkové dáta (vystúpenia, choreografie, počty) sú zatiaľ vymyslené. Pred zverejnením loga a názvu treba súhlas súboru.

- **Verejná časť**: úvod, repertoár, vystúpenia, registrácia, prihlásenie.
- **Portál člena** (po prihlásení a schválení): nástenka, kalendár udalostí, knižnica materiálov, profil.
- **Správa** (vedúci): schvaľovanie členov, správa choreografií, udalostí, materiálov, nastavenia súboru.

## Roly

Rola, zložka a stav sú stĺpce v `users` (nie samostatné tabuľky):

- `role`: `member` / `leader` (vedúci zložky) / `admin` (hlavný vedúci)
- `section`: `music` / `dance` (pri admin môže byť prázdne)
- `status`: `pending` (čaká na schválenie) / `active` / `rejected` / `inactive`

Návštevník = neprihlásený. Čakateľ = `status = pending` (vidí len verejnú časť a stav žiadosti). Vedúci zložky schvaľuje žiadosti do svojej zložky.

**Rozhodnutie:** knižnica je **spoločná pre všetkých aktívnych členov** (nie je obmedzená zložkou). Zložka slúži len na to, kto koho schvaľuje.

## Dátový model

- `users` – first_name, last_name, email (unique), password_hash, role, section, status, note (nástroj), created_at
- `regions` – name, description
- `choreographies` – region_id (FK), name, author, duration_min, public_description, internal_note, photo, is_public
- `materials` – choreography_id (FK), section (`music`/`dance` = len **kategória Hudba/Tanec**), type (`sheet`/`audio`/`video`/`document`), title, description, stored_filename, size, uploaded_by (FK users), created_at
- `events` – type (`performance`/`rehearsal`), title, starts_at, place, description, section (pri skúške; prázdne = spoločná), is_public
- `event_items` – event_id, choreography_id (unique pár), position → **M:N program vystúpenia**
- `attendances` – user_id, event_id (unique pár), status (`yes`/`no`), updated_at → **M:N účasť**
- `ensemble_settings` – name, description, logo, contact_email, city (jeden záznam)

Povinné entity: regions, choreographies, materials, events. Súbory sa ukladajú na disk pod vygenerovaným menom, v DB je len meno a metadáta.

## Stránky a stav návrhu

| ID | Stránka | Stav |
|---|---|---|
| S0 | Úvod (verejný) | navrhnutý |
| S1 | Repertoár (verejný) – filter regiónov | navrhnutý |
| S2 | Vystúpenia (verejné) – podľa mesiacov, **detail sa rozbalí priamo v zozname** (popis + program) | navrhnutý |
| S3 | Registrácia (žiadosť o členstvo, výber zložky, AJAX kontrola e-mailu) | navrhnutý |
| S4 | Prihlásenie | navrhnutý |
| S5 | Nástenka člena – udalosti s Prídem/Neprídem (AJAX), nové materiály | navrhnutý |
| S6 | Knižnica – vyhľadávanie, región, filter Všetko/Hudba/Tanec, karty choreografií rozdelené na Hudba/Tanec, stiahnutie | navrhnutý |
| S8 | Kalendár udalostí | nenavrhnutý |
| S10 | Môj profil – osobné údaje, zmena hesla (zložku mení vedúci) | navrhnutý |
| S11 | Žiadosti a členovia (vedúci) – schválenie cez AJAX | nenavrhnutý |
| S12/S13 | Správa choreografií / udalostí – možno ako formuláre v okne namiesto samostatných stránok | nenavrhnutý |
| S14 | Nastavenia súboru | nenavrhnutý |

**Vypustené/odložené:** detail choreografie (S7) zatiaľ nerobíme, **prehrávač nahrávok nebude**.

## AJAX (plán)

Filtrovanie repertoáru/knižnice, potvrdenie účasti Prídem/Neprídem, schválenie/zamietnutie žiadosti, kontrola dostupnosti e-mailu pri registrácii.

## Dizajn

Figma: https://www.figma.com/design/B2I5UFigzS8uFsGgPwvqx2/Untitled
Platné návrhy: „v6 tmavá – 1..4“ (úvod), „S1 Repertoár (verejný, v6)“, „S2 Vystúpenia (verejné, v6)“, „S3 Registrácia (v6)“, „S4 Prihlásenie (v6)“, „S5 Nástenka člena“, „S6 Knižnica (spoločná…)“, „S10 Môj profil“. Staršie verzie v2–v5 sú len história.

- **Verejná časť (tmavá):** čiernobiela fotografia tanečníc ako **fixné pozadie** (`background-attachment: fixed`, pre iOS fallback cez fixný prvok), stmavenie čiernou ~60 %, biely text, karty ako matné sklo (biela 8 %, okraj biela 18 %, `backdrop-filter: blur`). Pozadie definovať v spoločnom layoute, telu dať tmavú `background-color`, aby pri preklikávaní neblikalo.
- **Portál (svetlý režim tej istej témy):** tmavá navigácia + pod ňou rybiansky lem, svetlé pozadie, biele karty, bez fotky.
- **Písma:** nadpisy **Amatic SC Bold** (len veľké krátke texty), všetko ostatné **Nunito**.
- **Farby:**
  - tmavá navigácia/päta `#0F0C09`, text `#151105`, biela `#FFFFFF`, jemný svetlý text `#E9E4DC`
  - akcent ružová `#DD8786` (tlačidlá s tmavým textom, aktívne menu), svetloružová `#F4D8D7` (badge), tmavšia ružová na text `#B4544F`, veľké čísla `#C9615C`
  - portál: pozadie `#F5F1EB`, okraje `#E8E1D4`, tlmený text `#6B6256`
  - zelená len pre stav „✓ Prídem“
- **Ornamenty** (vektory, súbory `rybiansky-vzor.svg`, `srdce-a-vtaky.svg`):
  - **rybiansky lem** v pôvodných farbách `#502D19` (obrys), `#DD8786`, `#F4D8D7`, `#F08A3A` – opakovaný vodorovne (`background: url(lem.svg) repeat-x`), na úvode dole, v päte, v portáli pod navigáciou
  - **srdce a vtáky (kohúty)** – malý akcent (prihlásenie/registrácia, nástenka)
- Bez vlnovky (zamietnutá).

## Dokumenty v repozitári

- `README.md` – návrh projektu (odovzdané). **Treba aktualizovať:** knižnica už nie je obmedzená zložkou, prehrávač v rozšíreniach vypustiť.
- `analyza/` – PDF s dizajnom (úvod, knižnica, profil).
