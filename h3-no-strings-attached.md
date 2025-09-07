# h3 No strings attached

Tehtävät on tehty 7.9.2025.

Harjoituksessa käytetty kone:

Acer Nitro V16.

Käyttöjärjestelmä Windows 11 64-bit.

Prosessori AMD Ryzen 7 8845HS

Näytönohjain Radeon 780M Graphics 3.80 GHz.

Asennettu RAM 16GB.

Virtuaalikoneena Oracle VirtualBoxiin asennettu Debian.

<img width="659" height="596" alt="image" src="https://github.com/user-attachments/assets/ad496b89-e9df-4c54-8008-2c5b70cd61e7" />

## a) Strings
14:30. Tehtävänä oli löytää passtr-ohjelmasta salasana katsomalla binääriä stringsin avulla.
Aloitin lataamalla ja unzippaamalla tehtäväkansion.

<img width="479" height="85" alt="image" src="https://github.com/user-attachments/assets/a2c7a591-7814-4717-99e6-3580a2f29ecb" />

<img width="900" height="526" alt="image" src="https://github.com/user-attachments/assets/3d0010b5-b065-41c9-8407-ee5490048697" />

Unzippauksen jälkeen pääsin ajamaan ohjelmaa komennolla

``` $ ./passtr ```

Salasanaa en vielä tässä vaiheessa tiennyt.

<img width="889" height="238" alt="image" src="https://github.com/user-attachments/assets/0a90c842-a448-4d7d-b979-0a3b71c27271" />

Kokeilin strings-komentoa passtr-ohjelmaan:

``` $ strings passtr ```

<img width="940" height="48" alt="image" src="https://github.com/user-attachments/assets/7e2d4410-e9aa-4e22-918d-ff65a88290ed" />
 
Tuloksena sain tekstiä, josta löytyi mielenkiintoa herättänyt kohta:

<img width="940" height="136" alt="image" src="https://github.com/user-attachments/assets/21141269-5a0a-464a-82a1-f66590f9f012" />
 
Kokeillaan löytynyttä salasanaa sala-hakkeri-321:

<img width="940" height="123" alt="image" src="https://github.com/user-attachments/assets/3252e689-076c-4299-b9c8-1eb74d9ded5f" />
 
14:45 Lippu löytyi!

(Karvinen, 2025)
  
## b) Passtr-haavoittuvuuden korjaaminen
Seuraavaksi haavoittuvuus piti korjata obfuskoimalla löytynyt stringi. Aloitin googlettamalla “obfuscation string C”, koska aihe oli itselleni ihan uusi. Kysymys on siis salasanan muuttamisesta sellaiseen muotoon, että sitä ei pysty saamaan esille strings-komennon avulla ohjelman binääristä. 
Löysin StackOverflowsta Neuhausin postauksen “Hide string in binary at compile time?”, jossa haettiin samantyyppiseen ongelmaan ratkaisua. Löysin seuraavan ratkaisun, jossa jokainen salasanan kirjain piilotetaan erikseen, tässä ’secret’:

```
#define HIDE_LETTER(a)   (a) + 0x50
#define UNHIDE_STRING(str)  do { char * ptr = str ; while (*ptr) *ptr++ -= 0x50; } while(0)
char str1[] = { HIDE_LETTER('s'), HIDE_LETTER('e'), HIDE_LETTER('c'), HIDE_LETTER('r'), HIDE_LETTER('e'), HIDE_LETTER('t'), '\0' };
UNHIDE_STRING(str1);
```

15:30. Oma C:n osaaminen ei ihan riittänyt koodin laittamiseen oikealle kohdalle, joten en saanut haavoittuvuutta korjattua ja päätin siirtyä seuraavan tehtävän pariin. Logiikka varmasti aukeaa seuraavalla oppikerralla.

(Karvinen, StackOverflow)

## c) Packd
15:45 Tehtävänä oli paljastaa packd-ohjelman salasana.
Kokeilin strings-komentoa packdin binääriin, ja sieltä löytyi seuraava kohta:

<img width="483" height="288" alt="image" src="https://github.com/user-attachments/assets/503e566d-4d47-427f-bd27-194cb605a55e" />

Kokeilin löytynyttä ’piilos-An’-stringiä salasanaksi packdiin, mutta se ei ollut oikea:

<img width="859" height="159" alt="image" src="https://github.com/user-attachments/assets/1f4b5289-7207-499a-819a-84647822aa5a" />

Moodle-materiaaleissa oli vinkki binäärin pakkaamisesta ja tähän käytettävästä pakkausohjelmasta. Tämän perusteella myös seuraava kohta kiinnitti huomion:

<img width="940" height="57" alt="image" src="https://github.com/user-attachments/assets/e1f2ec80-294d-4bbb-94bc-9e59fde85b60" />

16:00 Päätin googlettaa mikä UPX on ja miten sitä käytetään. Löysin manuaalin osoitteesta https://linux.die.net/man/1/upx josta luin, että UPX on ohjelmien pakkaukseen käytetty ohjelma, jota voidaan käyttää muun muassa myös niiden purkamiseen. Päätin ladata itselleni UPX:n:

```
$ sudo apt-get install upx
```

Käytin UPX:ää ohjelman purkamiseen manuaalista löytyneen ohjeen mukaisesti:

```
$ upx -d packd
```

<img width="940" height="273" alt="image" src="https://github.com/user-attachments/assets/1403ad92-aa42-48aa-bea0-a377b43b31e3" />

Ohjelma oli nyt purettu ja kokeilin strings-komentoa uudelleen:

```
$ strings packd
```

Nyt komento palautti enemmän tietoa:

<img width="940" height="152" alt="image" src="https://github.com/user-attachments/assets/515df672-9d46-42fd-8836-33dfb6a9e9dc" />
 
Kokeilin nyt löytynyttä stringiä salasanaksi:

<img width="940" height="127" alt="image" src="https://github.com/user-attachments/assets/d2c431f0-b319-4443-afc7-ab36ff7fd682" />

16:15 Lippu löytyi!

(Die.net)


## Lähteet

Tehtävänanto: Karvinen T. 2025. Sovellusten Hakkerointi - 2025 Syksy. Luettavissa: https://terokarvinen.com/sovellusten-hakkerointi/. Luettu 7.9.2025.

Oberhumer M., Molnar L., Reiser J., Medoch J. 2010. Upx(1) - Linux man page. Luettavissa: https://linux.die.net/man/1/upx. Luettu 7.9.2025.

Neuman, 2021. Hide string in binary at compile time?, foorumipostaus. Luettavissa: https://stackoverflow.com/questions/69927341/hide-string-in-binary-at-compile-time. Luettu 7.9.2025.

