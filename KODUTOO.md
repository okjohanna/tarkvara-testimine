## **1. Loe läbi etteantud dokumentatsioon (README - Business Requirements).**
Kas dokumentatsioon on täielik ja kirjeldab selgelt nõutud funktsioone ja omadusi?\
Kas iga dokumentatsioonis kirjeldatud aspekt on piisavalt üksikasjalik, et seda koodis kontrollida?
___

Tunnen, et dokumentatsioonis olevad nõuded võiksid olla täpsemad.
Panen siia kirja asjad, mis tekitasid küsimusi või jäid mulle praegu segaseks. 

1. **Hinna kohta pole piisavalt andmeid.**

- Kust tuleb alghind? Kas seda mõjutab auto tüüp (Compact, Electric, Cabrio, Racer), auto vanus, mingi muu tunnus?
- Kas pikaajalise rendi soodustus Low seasoni ajal rakendub alates 10. või 11. päevast?
- Kas allahindlus on kogusummale või ainult päevadele, mis tulevad *peale* 10. päeva? (Nt. 12 päevasel rendil tuleb viimase 2 päeva *päevahinnale* 10% soodustus)
- Kui alghind = rentija vanus ja miinimum hind = rentija vanus, siis ei oleks Low seasonil pika rendi soodustusel mõtet, sest miinimum hinnaks on rentija vanus.
- Mis tasud veel hinnale lisanduvad/hinda mõjutavad? Kas neid peab kalkulaator näitama? Või ongi ainult fikseeritud päevahind?
- Valuuta on puudu.

Miinimumhinna määramine vastavalt inimese *vanusele* ei ole väga loogiline ja ilmselt tooks endaga kaasa hulgaliselt kaebusi või peletaks suure osa klientuurist eemale. 

2. **Nõuete sõnastus ja järjestus võiks olla võimalikult selge ja kergesti arusaadav.**

See on vist liigne tähenärimine ja veidi ülesandest mööda, aga näiteks on nende kahe punkti puhul *see*, mis *määrab millal reegel päriselt kehtib*, sulgudes alles lause lõpus justkui kõrvalmärkusena:
- For Racers, the price is increased by 50% if the driver is 25 years old or younger (except during the low season).
- If renting for more than 10 days, price is decreased by 10% (except during the high season).

High ja Low seasoni ajavahemikud olid nende vahel eraldi punktidena küll välja toodud, aga pigem võiks need kuskil alguses paika panna. (Nt. Seasons: Low season is Nov 1st - March 31st. High season is Apr 1st - Oct 31st.)

3. **Mida peaks kalkulaator kuvama, kui kasutaja päring ei vasta dokumentatsioonis olevatele tingimustele?**

Dokumentatsioonis pole mainitud midagi veateadete kohta. Pole ka määratud maksimum/miinimum pikkust auto rendile. 

1. **Mis siis kui kasutaja tahab rentida autot 31. märtsist kuni 1. aprillini?**

Või isegi pikemaks ajaks nii, et rendi aeg ei jää ainult ühele perioodile. Kalkulaator võiks sellega osata arvestada.

5. **Praegusest dokumentatsioonist järeldan, et kalkulaatoris peab olema:**

- funktsioon, mis kontrollib rentija vanust ja kas ta tohib valitud auto tüüpi rentida
- funktsioon, mis määrab kasutaja valitud kuupäevade põhjal, kas tegemist on High või Low hooajaga
- funktsioon, mis määrab hinnale teguri (nt. `if (season === "high")` vastab tõele, siis peaks hinda korrutama x1.15)
- funktsioon, mis tagab, et hind alla miinimumi ei läheks

6. **Kas iga dokumentatsioonis kirjeldatud aspekt on piisavalt üksikasjalik, et seda koodis kontrollida?**

- Alla 18 ei rendi - kontrollitav
- 18-21 ainult Compact - kontrollitav
- High/Low season - osaliselt jah (see on küsimärgi all, kuidas toimib üle kahe hooaja rentimine)
- High season +15% - osaliselt jah? protsent on teada ja hooaja saab määrata, aga mis on siiski baashind?
- Racer +50% 22-25a. - kontrollida saaks nende põhjal - mis hooajal renditakse ja kas juhi vanus jääb 22-25a. vahemikku (22 ja 25 kaasaarvatud)
- pikk rent -10% Low seasoni ajal - polnud selge, mis päevast alates soodustust rakendada tahetakse
- miinimumhind = vanus - alghind täpsustamata, ma ei oskaks seda praegu kontrollida