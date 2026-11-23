# Mono Launcher

Minimalista, fekete (OLED) Android launcher Kotlinban és Jetpack Compose-ban.
Cél: Android 15+ (minSdk/targetSdk 35), Samsung Galaxy S25 FE.

Nincs benne internet-engedély, helyadat, hirdetés, analitika vagy előfizetés.
A manifestben **egyetlen `uses-permission` sincs**.

## APK készítése

### 1. Android Studio (ajánlott)
1. Nyisd meg a `MonoLauncher` mappát (File → Open). Az Android Studio letölti az SDK-t és a függőségeket.
2. Build → Build APK(s). A fájl: `app/build/outputs/apk/debug/app-debug.apk`.
3. Vagy csatlakoztasd a telefont (USB-hibakeresés), és nyomj Run-t.

### 2. Parancssor
```
./gradlew assembleDebug      # debug APK
./gradlew assembleRelease    # release APK (a debug kulccsal aláírva, így telepíthető)
```
JDK 17 és Android SDK 35 kell hozzá (`ANDROID_HOME` vagy `local.properties`: `sdk.dir=...`).

### 3. Felhő (GitHub Actions)
Töltsd fel a projektet egy GitHub-repóba. A `.github/workflows/build.yml` minden pushra
lefordítja az APK-t; az Actions → futás → Artifacts alatt töltheted le.

## Beállítás telefonon
Telepítés után: Beállítások → Alkalmazások → Alapértelmezett alkalmazások → Kezdőképernyő alkalmazás → Mono Launcher.
(Vagy nyomd meg a Home gombot, a rendszer rákérdez.)

## Használat
| Művelet | Hatás |
|---|---|
| Húzás felfelé a főképernyőn | Összes alkalmazás (A–Z) |
| Húzás lefelé | Keresés |
| Húzás balra / jobbra | Gyors alkalmazás (alapból Kamera / Telefon, átállítható) |
| Jobb oldali A–Z sáv | Koppintás vagy húzás: ugrás a betűhöz; ☆ = vissza a kedvencekhez |
| Alkalmazás hosszú nyomása | Kedvenc, feljebb, elrejtés, alkalmazás infó |
| Óra / üres hely hosszú nyomása | Beállítások (óra stílusa, vastagság, méret, 24h, dátum, akku, ikonok, gesztusok) |
| Home gomb | Vissza a főképernyőre |

## Ikonok
Az ikonok az alkalmazások valódi ikonjai. "Egyszínű ikonok" módban a telepített app
témázott (monochrome) ikonrétegét festi fehérre, ha nincs ilyen, szürkeárnyalatos ikont mutat.
Kikapcsolva az eredeti színes ikonok jelennek meg.

## Betűtípus
A referencia Lexend-szerű betűt használ. Hozzáadáshoz tedd a `.ttf` fájlokat az `app/src/main/res/font/`
mappába, és a `Theme.kt`-ben állíts be `FontFamily`-t a `MaterialTheme` typography-jához / a Text-ekhez.
Alapból a rendszerbetű van használva.

## Szerkezet
```
app/src/main/java/app/monolauncher/
  MainActivity.kt        HOME activity, edge-to-edge
  LauncherViewModel.kt   alkalmazáslista, kedvencek, rejtettek, beállítások (SharedPreferences)
  IconLoader.kt          valódi ikonok, egyszínű feldolgozás, gyorsítótár
  Screens.kt             gyökér, főképernyő, összes alkalmazás, gesztusok
  Clock.kt               óra, dátum, akkumulátor (rendszer-broadcastokból)
  AlphabetRail.kt        A–Z sáv
  AppRow.kt              egy lista-sor
  Search.kt              keresés
  Dialogs.kt, Settings.kt  menük és beállítások
```
