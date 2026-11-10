# Coil AR — Marker v8

Spletni prototip za spremljanje fizične tuljave z AR markerjem in prikaz tridimenzionalnega koordinatnega sistema. Končni cilj je vizualizacija magnetnega polja okoli tuljave; izračun magnetnega polja **še ni implementiran**.

**Spletna aplikacija:** https://janpetrovcic.github.io/coil-ar/  
**Izvorna koda:** https://github.com/janpetrovcic/coil-ar

## Trenutno delovanje

- Delovanje neposredno v mobilnem brskalniku prek GitHub Pages (HTTPS), brez namestitve aplikacije.
- **AR.js 3.4.5 + A-Frame 1.6.0**, sledenje markerju **Barcode 3 × 3, ID 5**.
- Fizični marker meri **35 × 35 mm** in je nameščen na **zadnji strani tuljave**.
- Barvne osi: **X rdeča, Y zelena (pravokotno na marker), Z modra (v ravnini markerja)**.
- Izbira kamere med napravami, ki jih ponuja brskalnik; primarni objektiv 1× ni vedno izrecno na voljo. Ob neuspešni izbiri je predviden povratek na privzeto kamero.
- Zahtevana kamera **1280 × 960**, obdelava markerja **960 × 720**, do **30 zaznav/s**; dejanske vrednosti so odvisne od telefona.
- Nastavljivi odmiki **X, Y in Z v cm**, povečava koordinatnih osi in dodatna korekcija merila. Nastavitve se shranijo lokalno v brskalniku.
- Pretvorba, preverjena s praktičnim preizkusom: **1 enota AR prizora ≈ širina markerja = 3,5 cm**; dodatna korekcija je okvirna, ne nadomešča kalibracije objektiva.
- **Rumena 10-centimetrska referenčna črta je odstranjena iz prikaza** po zaključku preizkusa merila.

## Zagon

1. Odpri zgornjo povezavo v Chromu oziroma primernem mobilnem brskalniku.
2. Dovoli uporabo kamere ter jo usmeri na marker **ID 5** na zadnji strani tuljave.
3. Odpri **Nastavitve / kalibracija** za izbiro kamere, odmike X/Y/Z in velikost osi.
4. Če menjaš kamero, izberi napravo in potrdi ponovni zagon strani.
5. Prilagodi odmike, da je belo izhodišče koordinatnega sistema v želeni točki tuljave.

## Nastavitve v kodi

V `index.html`:

```js
const DEFAULTS = { x: 0, y: 0, z: -20, cal: 1, scale: 10 };
const MARKER_SIZE_CM = 3.5;
const SCENE_UNITS_PER_CM = 1 / MARKER_SIZE_CM;
```

- `x`, `y`, `z`: začetni odmiki v centimetrih, izraženi v lokalnem koordinatnem sistemu markerja.
- `cal`: dodatni faktor korekcije merila (privzeto 1).
- `scale`: vizualna povečava XYZ osi (v trenutnem vmesniku **10–30×**); **ne spreminja odmikov**.
- Izbrana kamera se shrani v `sessionStorage`, nastavitve drsnikov pa v `localStorage` pod ključem `coilARv8`. Shranjene nastavitve lahko preglasijo nove privzete vrednosti v kodi; uporabi gumb **Ponastavi kalibracijo**, če želiš privzete vrednosti.
- Označba `size="0.035"` na elementu markerja predstavlja fizično širino markerja v metrih.

## Pomembno: odstranitev rumene črte

Če iz `index.html` odstraniš HTML element `<a-entity id="ruler"> ... </a-entity>`, je treba odstraniti **ali zaščititi tudi JavaScript**, ki dostopa do `ruler`:

```js
const ruler = document.getElementById('ruler');
// Če reference ni več, sme ostala koda teči naprej:
if (ruler) {
  ruler.setAttribute('scale', { x: REFERENCE_LENGTH_CM * k, y: REFERENCE_LENGTH_CM * k, z: REFERENCE_LENGTH_CM * k });
  if (ruler.object3D) ruler.object3D.scale.setScalar(REFERENCE_LENGTH_CM * k);
}
```

V zgornjem primeru sta `k` in `REFERENCE_LENGTH_CM` že določena v obstoječem `apply()`; gre za prikaz varnega pogoja, ne za samostojno zamenjavo cele funkcije. Brez te spremembe lahko skripta vrže napako in odmiki oziroma drsniki prenehajo delovati. Po želji odstrani tudi besedila v nastavitvah, ki še omenjajo rumeno črto.

## Omejitve in nadaljnje delo

- Sledenje se izgubi, ko marker ni viden; če bo potreben pogled z druge strani, bo potreben dodaten marker ali drugačen način sledenja.
- Na stabilnost vplivajo svetloba, razdalja, zorni kot, popačenje objektiva in kalibracija kamere.
- Naslednji koraki: dokončna določitev koordinatnega izhodišča v središču tuljave, preverjanje poravnave s fizičnimi merami in nato prikaz magnetnih silnic.

## Posodobitev na GitHubu

Repozitorij je statična stran: zamenjaj `index.html` in po potrebi `README.md` ter potrdi **Commit changes**. GitHub Pages nato objavi novo različico. Za preizkus nove datoteke lahko uporabiš dodatek k URL-ju, na primer `?v=marker8` (to samo pomaga pri osvežitvi predpomnilnika, ne namesti kode).
