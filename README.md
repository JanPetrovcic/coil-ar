# Coil AR — sledenje geometriji (eksperimentalni prototip)

Ta prototip uporablja spletno kamero + OpenCV.js, **brez MindAR in brez targets.mind**.
Zažene se na GitHub Pages (HTTPS). Ne potrebuje namestitve aplikacije.

## Namestitev

1. Odpri svojo GitHub zbirko `janpetrovcic/coil-ar`.
2. Pred spremembo lahko staro `index.html` preimenuješ v `index-v6.html` oziroma jo shraniš na računalnik.
3. V koren repozitorija naloži **dve datoteki**: `index.html` in `guide.jpg`.
4. Če uporabljaš `Add file → Upload files`, na GitHubu zamenjaj obstoječi `index.html` in potrdi commit na veji `main`.
5. Po uspešni objavi odpri `https://janpetrovcic.github.io/coil-ar/?v=geometry1`.

## Testiranje

1. Odpri povezavo **neposredno v Chrome**.
2. Pritisni **Zaženi kamero**; počakaj, da OpenCV naloži knjižnico (potreben je internet).
3. Cel leseni četrtkrog (sprednji kos s tremi črtami) naj bo čim bolj v kadru in ne preveč pod kotom.
4. Pritisni **Označi 4 točke**.
5. Tapni 4 točke v pravilnem vrstnem redu po `guide.jpg`:
   - 1 spodnji levi kot lesa,
   - 2 spodnje presečišče radialne črte in krožnega roba,
   - 3 srednje presečišče,
   - 4 zgornje presečišče.
6. Po zadnji označeni točki bi se morale pojaviti osi X/Y/Z in turkizni obris kosa.
7. Telefon premikaj počasi. Če se prikaz izgubi, tapni **Reset** in ponovno označi 4 točke.
8. Gumb **Prikaži črte** prikazuje samodejno zaznane ravne črte (rumeno) z OpenCV HoughLinesP, vendar **jih še ne uporablja za avtomatsko inicializacijo**.

## Omejitve

- **Začetno označevanje je ročno**. Samodejno zaznavanje črt je diagnostična funkcija. Ta različica še ne prepozna druge tuljave sama od sebe.
- Sledenje uporablja Lucas–Kanade optical flow in robustno homografijo (RANSAC) iz značilnic notranjosti izbranega kosa. Pri zamenjavi kosa ali izgubi sledenja se ponovno označijo točke.
- Koordinate geometrijskega modela so ocenjene iz fotografije: krožni lok R=150 mm, radialne črte pri 21°, 51°, 76°. Točne številke se lahko razlikujejo od resnične konstrukcije.
- XYZ osi temeljijo na približni oceni položaja iz ravninske homografije s predpostavljenimi parametri kamere; niso še natančno kalibrirane za fizikalni prikaz polja.
- Odprta kamera potrebuje HTTPS in uporabniško dovoljenje. OpenCV.js se prvič prenese prek interneta.
- Lahko zmanjka značilnic v lesu pri močni spremembi perspektive/odsevu; v tem primeru je potrebna ponovna kalibracija. Zamenjava tuljave zahteva ponovno označitev.

## Naslednji korak

Po preverjanju osnovnega delovanja: kalibracija točnih kotov/velikosti, avtomatsko povezovanje zaznanih črt z lokom, samodejna inicializacija brez 4 dotikov, in šele nato Biot–Savart magnetno polje.
