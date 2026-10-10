# MovieTracker

Meno a priezvisko študenta: Maksym Chernikov

Študijná skupina: 5ZYI36

## Stručný opis projektu
MovieTracker je webová aplikácia na evidenciu filmov, ktoré používateľ videl alebo plánuje pozrieť. Rieši bežný problém, keď si človek nepamätá, aké filmy už videl, ako ich hodnotil a čo si chcel pozrieť neskôr. Aplikácia je určená milovníkom filmov, ktorí si chcú viesť osobný prehľad a nájsť nové filmy v spoločnom katalógu.

Hlavným účelom je poskytnúť katalóg filmov, ku ktorému môže každý registrovaný používateľ pridávať filmy do svojho osobného zoznamu, nastavovať im stav (chcem vidieť, pozerám, videl som), hodnotiť ich a písať si poznámky. Aplikácia vznikla ako webová verzia mojej existujúcej mobilnej aplikácie MovieTracker. Backend je vytvorený vo frameworku VAJKO (PHP), pričom frontend je plánovaný v Reacte.

## Role v projekte
- **Návštevník (neprihlásený používateľ)** – môže prehliadať katalóg filmov, vyhľadávať a filtrovať ich, zobraziť detail filmu a verejné recenzie. Nemôže si ukladať filmy ani hodnotiť.
- **Registrovaný používateľ** – okrem možností návštevníka si vedie osobný zoznam filmov so stavom, hodnotením a poznámkami, píše recenzie, spravuje svoj profil (vrátane profilovej fotky) a môže navrhnúť nový film.
- **Správca (administrátor)** – spravuje katalóg filmov a žánrov (pridávanie, úprava, mazanie, nahrávanie plagátov), moderuje recenzie a spravuje používateľské účty vrátane prideľovania rolí.

## Prípady použitia podľa rolí
**Návštevník**
- Prehliada katalóg filmov a strácza sa v ňom pomocou stránkovania.
- Vyhľadá film podľa názvu a prefiltruje katalóg podľa žánru alebo roku vydania.
- Zobrazí detail filmu s popisom, žánrami, priemerným hodnotením a recenziami.
- Zaregistruje sa a prihlási do aplikácie.

**Registrovaný používateľ**
- Pridá film zo katalógu do svojho zoznamu a nastaví mu stav (chcem vidieť, pozerám, videl som).
- Ohodnotí film a pripíše k nemu osobnú poznámku.
- Upraví alebo odstráni záznam vo svojom zozname.
- Napíše, upraví alebo zmaže vlastnú recenziu.
- Zobrazí štatistiky svojho pozerania (počet videných filmov, priemerné hodnotenie, obľúbené žánre).
- Upraví svoj profil a nahrá profilovú fotku.
- Navrhne nový film, ktorý v katalógu chýba.

**Správca**
- Pridá nový film, upraví jeho údaje a nahrá plagát.
- Zmaže film z katalógu.
- Spravuje zoznam žánrov.
- Schváli alebo zamietne film navrhnutý používateľom.
- Moderuje (maže) nevhodné recenzie.
- Spravuje používateľov a mení ich role.

## Plánované entity
- **Používateľ (users)** – registrovaný používateľ aplikácie. Atribúty: id, e-mail, prihlasovacie meno, hash hesla, rola, cesta k profilovej fotke, dátum registrácie.
- **Film (movies)** – film v katalógu. Atribúty: id, názov, popis, rok vydania, dĺžka v minútach, cesta k plagátu, dátum pridania, stav schválenia, id používateľa, ktorý film pridal.
- **Žáner (genres)** – kategória filmu. Atribúty: id, názov.
- **Filmy a žánre (movie_genres)** – prepojovacia tabuľka. Atribúty: id filmu, id žánru.
- **Záznam v zozname (watchlist_entries)** – film v osobnom zozname používateľa. Atribúty: id, id používateľa, id filmu, stav (chcem vidieť / pozerám / videl som), hodnotenie, poznámka, dátum pridania.
- **Recenzia (reviews)** – verejná recenzia filmu. Atribúty: id, id používateľa, id filmu, text, hodnotenie, dátum vytvorenia.

## Vzťahy medzi entitami
- **Žáner – Film (M:N):** jeden film môže mať viac žánrov a jeden žáner patrí viacerým filmom. Vzťah je realizovaný tabuľkou `movie_genres`.
- **Používateľ – Film cez zoznam (M:N):** používateľ môže mať vo svojom zozname viac filmov a film sa môže nachádzať v zoznamoch viacerých používateľov. Vzťah je realizovaný tabuľkou `watchlist_entries`, ktorá nesie aj ďalšie údaje (stav, hodnotenie, poznámka).
- **Používateľ – Recenzia (1:N):** používateľ môže napísať viac recenzií, každá recenzia má jedného autora.
- **Film – Recenzia (1:N):** film môže mať viac recenzií, každá recenzia patrí jednému filmu.
- **Používateľ – Film pridaný používateľom (1:N):** používateľ môže navrhnúť viac filmov, každý film má jedného navrhovateľa (administrátor pridáva filmy rovnakým spôsobom).

## Hlavné stránky aplikácie
1. **Domov / katalóg filmov** – zoznam filmov s vyhľadávaním, filtrovaním podľa žánru a roku a stránkovaním.
2. **Detail filmu** – plagát, popis, žánre, priemerné hodnotenie, recenzie a tlačidlo na pridanie do zoznamu.
3. **Prihlásenie a registrácia** – formuláre na vytvorenie účtu a prihlásenie.
4. **Môj zoznam** – osobný zoznam filmov používateľa so stavmi, hodnotením a poznámkami (úprava priamo v zozname).
5. **Profil a štatistiky** – údaje používateľa, nahratie profilovej fotky a prehľad štatistík pozerania.
6. **Správa filmov (admin)** – pridávanie, úprava a mazanie filmov vrátane nahrávania plagátov a schvaľovania návrhov.
7. **Správa používateľov a žánrov (admin)** – zoznam používateľov, zmena rolí, správa žánrov a moderovanie recenzií.

## Rozdelenie funkcionality
**Základné funkcie**
- Registrácia, prihlásenie a odhlásenie s rolami (návštevník, používateľ, správca) a ochranou stránok aj koncových bodov (endpointov) na serveri.
- Katalóg filmov s vyhľadávaním, filtrovaním a stránkovaním.
- Detail filmu.
- Osobný zoznam filmov (pridanie, zmena stavu, hodnotenie, poznámka, odstránenie).
- Plný CRUD nad filmami (správca) a nad záznamami v zozname (používateľ).
- Nahrávanie a správa súborov (plagáty filmov, profilové fotky).
- Validácia vstupov na strane klienta aj servera.
- Asynchrónne volania (fetch API) pre vyhľadávanie a filtrovanie, zmenu stavu a hodnotenia bez znovunačítania stránky.

**Rozširujúce funkcie**
- Frontend ako SPA v Reacte komunikujúci s backendom (VAJKO) cez JSON API.
- Recenzie filmov a moderovanie recenzií.
- Návrhy nových filmov od používateľov so schvaľovaním správcom.
- Štatistiky pozerania s grafmi (napr. Chart.js).
- Napojenie na externé API s dátami o filmoch (napr. TMDB) na predvyplnenie údajov.
- Odporúčanie filmov podľa obľúbených žánrov.