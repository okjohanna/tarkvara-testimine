## **1. Loe läbi etteantud dokumentatsioon (README - Business Requirements).**
Kas dokumentatsioon on täielik ja kirjeldab selgelt nõutud funktsioone ja omadusi?\
Kas iga dokumentatsioonis kirjeldatud aspekt on piisavalt üksikasjalik, et seda koodis kontrollida?
___

1. Hinna kohta pole piisavalt andmeid

- Kust tuleb alghind? Kas seda mõjutab auto tüüp (Compact, Electric, Cabrio, Racer), auto vanus, mingi muu tunnus?
- Kas pikaajalise rendi soodustus Low seasoni ajal rakendub alates 10. või 11. päevast?
- Kas allahindlus on kogusummale või ainult päevadele, mis tulevad *peale* 10. päeva? (Nt. 12 päevasel rendil tuleb viimase 2 päeva *päevahinnale* 10% soodustus)
- Kui alghind = rentija vanus ja miinimum hind = rentija vanus, siis ei oleks Low seasonil pika rendi soodustusel mõtet, sest miinimum hinnaks on rentija vanus.
- Mis tasud veel hinnale lisanduvad/hinda mõjutavad? Kas neid peab kalkulaator näitama? Või ongi ainult fikseeritud päevahind?

Miinimumhinna määramine vastavalt inimese *vanusele* ei ole väga loogiline ja ilmselt tooks endaga kaasa hulgaliselt kaebusi või peletaks suure osa klientuurist eemale. 

2. Nõuete sõnastus ja järjestus võiks olla võimalikult selge ja kergesti arusaadav

See on vist liigne tähenärimine ja veidi ülesandest mööda, aga näiteks on nende kahe punkti puhul *see, mis määrab millal reegel päriselt kehtib*, sulgudes alles lause lõpus justkui kõrvalmärkusena:

- For Racers, the price is increased by 50% if the driver is 25 years old or younger (except during the low season).
- If renting for more than 10 days, price is decreased by 10% (except during the high season).

High ja Low seasoni ajavahemikud olid nende vahel eraldi punktidena küll välja toodud, aga pigem võiks need kuskil alguses paika panna. (Nt. Seasons: Low season is Nov 1st - March 31st. High season is Apr 1st - Oct 31st.)

3. Mida peaks kalkulaator kuvama, kui kasutaja päring ei vasta dokumentatsioonis olevatele tingimustele?

Dokumentatsioonis pole mainitud midagi veateadete kohta. Pole ka määratud maksimum/miinimum pikkust auto rendile.

4. Mis siis kui kasutaja tahab rentida autot 31. märtsist kuni 1. aprillini?

Või isegi pikemaks ajaks nii, et rendi aeg ei jää ainult ühele perioodile. Kalkulaator võiks sellega osata arvestada.

5. Praegusest dokumentatsioonist järeldan, et kalkulaatoris peab olema:

- funktsioon, mis kontrollib rentija vanust ja kas ta tohib valitud auto tüüpi rentida
- funktsioon, mis tuvastab kasutaja valitud kuupäevade põhjal hooaja (High või Low)
- funktsioon, mis määrab hinnale teguri (nt. `if (season === "high")` vastab tõele, siis peaks hinda korrutama x1.15)
- kontroll, mis ei laseks hinnal langeda alla miinimumi (rentija vanus)

6. Kas iga dokumentatsioonis kirjeldatud aspekt on piisavalt üksikasjalik, et seda koodis kontrollida?

- Alla 18 ei rendi - kontrollitav
- 18-21 ainult Compact - kontrollitav
- High/Low season - osaliselt jah? (see on küsimärgi all, kuidas toimib üle kahe hooaja rentimine või näiteks detsembrist veebruarini rentimine)
- High season +15% - osaliselt jah? protsent ja hooaja ajavahemikud on teada, aga mis on baashind?
- Racer +50% 22-25a. - kontrollida saaks nende põhjal - mis hooajal renditakse ja kas juhi vanus jääb 22-25a. vahemikku (22 ja 25 kaasaarvatud)
- pikk rent -10% Low seasoni ajal - polnud selge, mis päevast alates soodustust rakendada tahetakse
- miinimumhind = vanus - alghind täpsustamata, ma ei oskaks seda praegu kontrollida

## **2. Loe ja analüüsi koodi, et veenduda selle vastavuses dokumentatsioonis toodud nõuetele.**

Kontrolli järgmisi aspekte:

- Kas kood on loogiliselt struktureeritud ja lihtsasti loetav?
  - `price` funktsioonis tehakse enne vanusekontrolli ära `getClazz(type)`, `get_days(pickupDate, dropoffDate)` ja `getSeason(pickupDate, dropoffDate)`arvutused, kuigi liiga noore rentija puhul visatakse tulemused ära. Kõige esimesena peaks kontrollima vanust
  - `price` funktsiooni parameetritest `pickup` ja `dropoff` pole kuskil kasutusel
  - `getSeason` if tingimus on raskesti loetav (3 ||-ga ühendatud osa, mille vahel on ilma selgituseta kuude numbrid)
- Kas funktsioonide ja muutujate nimed on loogilised?
  - Funktsioonide ja muutujate nimedes on segamini kasutatud erinevaid tavasid (camelCase, snake_case või on nimi lihtsaltniimoodikirjutatud).
  - Osad nimed ebamäärased või meelega valesti kirjutatud (nt `clazz`, `days` võiks olla `carClass`, `totalRentalDays`).
- Kas kood vastab dokumentatsioonis kirjeldatud funktsioonidele ja reeglitele?
  - Koodist ei ole ükski dokumentatsioonis kirjeldatud funktsioon või reegel _täielikult puudu_
- Kas mõni dokumentatsioonis kirjeldatud funktsioon on koodist puudu või valesti rakendatud?
  - Hooaja (`getSeason`) määramine on valesti rakendatud, `getMonth()` loendus algab 0-st (praeguse koodi põhjal on High hooaeg maist novembrini)

## **3. Kirjuta üles kõik leitud lahknevused või probleemid koodi ja dokumentatsiooni vahel.**

### rentalPrice.js

1. Hooaja kuud on nihkes (`getSeason`) (start peaks olema 3, end 9, sest `getMonth()` loendus algab 0-st)
2. Aasta muutusega ei arvestata hooaja määramisel (nt rent 2026 märtsist 2027 veebruarini)
3. Miinimum hinda ei rakendata. Peale rendihinna arvutamist `price` funktsioonis peaks toimuma kontroll nt `rentalPrice = Math.max(rentalPrice, age * days)`
4. Hinda ei ümardata kahe komakohani nagu tavaliselt kombeks, praegu tulevad vastused sellisena: `Price: $89.69999999999999 per day`
5. Baashind puudub (kui baashind on `age * days`, siis -10% allahindlus ei rakendu kunagi)
6. Parameetrid `pickup` ja `dropoff` võetakse sisse, aga ei kasutata. Business Requirements fail neid ei maini.

### form.html

1. Autode tüüpide väärtused ei klapi `rentalPrice.js` failis olevatega (`compact` vs `Compact` jne). Ainult Electric töötaks õigesti
2. Tekstiväljadel ei ole min ega max pikkust määratud, sisu ei kontrollita
3. "Driver's age" asemel on praegu "Diver's age"
4. Vanuse väli `type="number"` lubab negatiivseid ja komakohtadega arve
5. Saab valida mineviku kuupäevi

### result.html

1. Kõik 4 autotüüpi näitavad "Price: 0$ per day", ei näe kohta kuhu server saaks päris hinna panna
2. `price()` tagastab kogu rendiperioodi summa, mitte päevahinna, result.html peaks näitama Total Price või siis päevahinda ja kogusummat mõlemat
3. Vormis valib kasutaja ühe autotüübi, aga result.html näitab neljale autole hinda
4. Pole näha, mille kohta hind on (kuupäevad, päevade arv, vanus, asukohad)
5. Nimed pole ühes laadis kirjas: Compact Car, Electric Car, Cabrio, Racer
6. index.js rida 27 `res.send(formHtml + resultHtml.replaceAll('$0', result))` asendab kõik "$0" sama väärtusega. Vea korral on tulemuseks 4 korda "Price: Drivers 21 y/o or less can only rent Compact vehicles per day"
