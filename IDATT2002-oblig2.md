## Oblig 2
### Oppgave 1

#### a)
Bruk SQL til å finne fornavn og etternavn til alle forfatterne sortert alfabetisk på etternavn. Hvilken eller hvilke operasjoner fra relasjonsalgebraen brukte du?

SELECT - velger kolonner (brukes i omtrent alle spørringene)
ORDER BY - sorterer basert på etternavn
```sql
SELECT fornavn, etternavn 
FROM forfatter
ORDER BY etternavn
```
(prøvde først med DESC, men da ble det helt omrokkert??)

![image](https://github.com/heleneask/IDATT2002/assets/123962967/420cbb27-d08f-406d-bb17-b49ecbd510f3)


### b)
Bruk SQL til å finne eventuelle forlag (forlag_id er nok) som ikke har gitt ut bøker. Hvilken eller hvilke operasjoner fra relasjonsalgebraen brukte du?

JOIN kartesisk produkt - forlag X bok
WHERE - filtrerer ut radene ut ifra valgt verdi
```sql
SELECT forlag.forlag_navn, forlag.forlag_id
FROM forlag forlag
LEFT JOIN bok bok ON forlag.forlag_id = bok.forlag_id
WHERE bok.forlag_id IS NULL;
```
![image](https://github.com/heleneask/IDATT2002/assets/123962967/cb4756f9-413d-42fc-98e2-d5e7027e5847)


### c)
Bruk SQL til å finne forfattere som er født før 1900. Hvilken eller hvilke operasjoner fra relasjonsalgebraen brukte du?

WHERE - filtrerer basert på fødselsår

```sql
SELECT * (eller SELECT fornavn, etternavn, fode_aar)
FROM forfatter 
WHERE fode_aar < 1900;
```
![image](https://github.com/heleneask/IDATT2002/assets/123962967/08ae6b1a-615b-45a8-834a-804a320cd2ab)

### d)
Bruk SQL til å finne navn og adresse til forlaget som har gitt ut boka 'Sult'. Hvilken eller hvilke operasjoner fra relasjonsalgebraen brukte du?

JOIN - kombinererr forlag og bok
WHERE - filtrerer etter tittel
```sql
SELECT forlag.forlag_navn, forlag.adresse
FROM forlag forlag
JOIN bok bok ON forlag.forlag_id = bok.forlag_id
WHERE bok.tittel = 'Sult';
```
![image](https://github.com/heleneask/IDATT2002/assets/123962967/60057848-4f8e-4ea2-a1a8-683d1d64bb40)

### e)
Bruk SQL til å finne titlene på bøkene som Hamsun har skrevet. Hvilken eller hvilke operasjoner fra relasjonsalgebraen brukte du?

JOIN - bok og forfatter id
```sql
SELECT forfatter.etternavn, forfatter.forfatter_id, bok.tittel
FROM forfatter
JOIN bok_forfatter ON forfatter.forfatter_id = bok_forfatter.forfatter_id
JOIN bok ON bok.bok_id = bok_forfatter.bok_id
WHERE forfatter.etternavn = 'Hamsund';
```
![image](https://github.com/heleneask/IDATT2002/assets/123962967/91891d6c-bc4f-433e-ac4e-58db5fb1c1ab)

### f) 
Bruk SQL til å finne informasjon om bøker og forlagene som har utgitt dem. Én linje i oversikten skal inneholde bokas tittel og utgivelsesår, samt forlagets navn, adresse og telefonnummer. Forlag som ikke har gitt ut noen bøker skal også med i listen. Hvilken eller hvilke operasjoner fra relasjonsalgebraen brukte du?

LEFT JOIN - returnerer alle rader fra forlag som er venstre tabell og tilsv. høyre tabell, vil være NULL dersom det ikke er noen gyldige verdier
```sql
SELECT bok.tittel, bok.utgitt_aar, forlag.forlag_navn, forlag.adresse, forlag.telefon
FROM forlag
LEFT JOIN bok ON forlag.forlag_id = bok.forlag_id;	
```
![image](https://github.com/heleneask/IDATT2002/assets/123962967/9d89da9f-e72d-4154-981e-6ff7b245ac40)




### Oppgave 2
#### 2.1
Skriv ut navnet på spilleren (player), teamid og mdate for hvert av målene til Germany
```sql
SELECT player, teamid, mdate
FROM goal 
LEFT JOIN game ON game.id = goal.matchid
WHERE goal.teamid = 'GER';
```
#### 2.2
Skriv ut antall skårede mål pr. spiller for Germany sortert synkende på antall mål (Tips: Ta utgangspunkt i spørringen fra oppg. 2.1)
```sql
SELECT goal.player, COUNT(goal.player) AS antall_mål
FROM goal
INNER JOIN eteam ON goal.teamid = eteam.id
WHERE goal.teamid = 'GER'
GROUP BY goal.player
ORDER BY antall_mål DESC;
```
#### 2.3
Skriv ut player, teamid, coach, gtime for alle målene skåret de første 10 minuttene av kampene (sjekk på attributtet gtime som sier når et mål ble scoret i en kamp)
```sql
SELECT goal.player, goal.teamid, eteam.coach, goal.gtime
FROM goal
INNER JOIN eteam ON goal.teamid = eteam.id
WHERE goal.gtime <= 10;
```
#### 2.4
Skriv ut datoene på kampene og navnet på laget hvor Fernando Santos var trener for team1
Resultattabellen skal bli slik:
mdate 	teamname 
12 June 2012	Greece
16 June 2012	Greece
```sql
SELECT game.mdate, eteam.teamname
FROM game
JOIN eteam ON game.team1 = eteam.id
WHERE eteam.coach = 'Fernando Santos';
```
#### 2.5
Skriv ut datoene på kampene og navnet på laget hvor Fernando Santos var trener for team1 eller team2
Resultattabellen skal bli slik:
mdate 	teamname 
8 June 2012	Greece
12 June 2012	Greece
16 June 2012	Greece
22 June 2012	Greece
```sql
SELECT game.mdate, eteam.teamname
FROM game
JOIN eteam ON game.team1 = eteam.id
WHERE eteam.coach = 'Fernando Santos'
UNION
SELECT game.mdate, eteam.teamname
FROM game
JOIN eteam ON game.team2 = eteam.id
WHERE eteam.coach = 'Fernando Santos';
```
Enklere løsning:
```sql
SELECT game.mdate, eteam.teamname
FROM game
JOIN eteam ON game.team1 = eteam.id OR game.team2 = eteam.id
WHERE eteam.coach LIKE 'Fernando Santos';
```
