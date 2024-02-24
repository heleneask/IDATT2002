### Oblig 2

#### a)
Bruk SQL til å finne fornavn og etternavn til alle forfatterne sortert alfabetisk på etternavn. Hvilken eller hvilke operasjoner fra relasjonsalgebraen brukte du?

SELECT - velger kolonner (brukes i omtrent alle spørringene)
ORDER BY - sorterer basert på etternavn

SELECT fornavn, etternavn 
FROM forfatter
ORDER BY etternavn
(prøvde først med DESC, men da ble det helt omrokkert??)
![image](https://github.com/heleneask/IDATT2002/assets/123962967/420cbb27-d08f-406d-bb17-b49ecbd510f3)


### b)
Bruk SQL til å finne eventuelle forlag (forlag_id er nok) som ikke har gitt ut bøker. Hvilken eller hvilke operasjoner fra relasjonsalgebraen brukte du?

JOIN kartesisk produkt - forlag X bok
WHERE - filtrerer ut radene ut ifra valgt verdi

SELECT forlag.forlag_navn, forlag.forlag_id
FROM forlag forlag
LEFT JOIN bok bok ON forlag.forlag_id = bok.forlag_id
WHERE bok.forlag_id IS NULL;

![image](https://github.com/heleneask/IDATT2002/assets/123962967/cb4756f9-413d-42fc-98e2-d5e7027e5847)


### c)
Bruk SQL til å finne forfattere som er født før 1900. Hvilken eller hvilke operasjoner fra relasjonsalgebraen brukte du?

WHERE - filtrerer basert på fødselsår

Bruker SELECT og WHERE
SELECT * (eller SELECT fornavn, etternavn, fode_aar)
FROM forfatter 
WHERE fode_aar < 1900;

![image](https://github.com/heleneask/IDATT2002/assets/123962967/08ae6b1a-615b-45a8-834a-804a320cd2ab)

### d)
Bruk SQL til å finne navn og adresse til forlaget som har gitt ut boka 'Sult'. Hvilken eller hvilke operasjoner fra relasjonsalgebraen brukte du?

JOIN - kombinererr forlag og bok
WHERE - filtrerer etter tittel

SELECT forlag.forlag_navn, forlag.adresse
FROM forlag forlag
JOIN bok bok ON forlag.forlag_id = bok.forlag_id
WHERE bok.tittel = 'Sult';

![image](https://github.com/heleneask/IDATT2002/assets/123962967/60057848-4f8e-4ea2-a1a8-683d1d64bb40)

### e)
Bruk SQL til å finne titlene på bøkene som Hamsun har skrevet. Hvilken eller hvilke operasjoner fra relasjonsalgebraen brukte du?

JOIN - bok og forfatter id

SELECT forfatter.etternavn, forfatter.forfatter_id, bok.tittel
FROM forfatter
JOIN bok_forfatter ON forfatter.forfatter_id = bok_forfatter.forfatter_id
JOIN bok ON bok.bok_id = bok_forfatter.bok_id
WHERE forfatter.etternavn = 'Hamsund';

![image](https://github.com/heleneask/IDATT2002/assets/123962967/91891d6c-bc4f-433e-ac4e-58db5fb1c1ab)

### f) 
Bruk SQL til å finne informasjon om bøker og forlagene som har utgitt dem. Én linje i oversikten skal inneholde bokas tittel og utgivelsesår, samt forlagets navn, adresse og telefonnummer. Forlag som ikke har gitt ut noen bøker skal også med i listen. Hvilken eller hvilke operasjoner fra relasjonsalgebraen brukte du?

LEFT JOIN - returnerer alle rader fra forlag som er venstre tabell og tilsv. høyre tabell, vil være NULL dersom det ikke er noen gyldige verdier

SELECT bok.tittel, bok.utgitt_aar, forlag.forlag_navn, forlag.adresse, forlag.telefon
FROM forlag
LEFT JOIN bok ON forlag.forlag_id = bok.forlag_id;	

![image](https://github.com/heleneask/IDATT2002/assets/123962967/9d89da9f-e72d-4154-981e-6ff7b245ac40)
