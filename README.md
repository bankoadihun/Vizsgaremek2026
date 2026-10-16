# Vizsgaremek2026

Projekt fejlsztői:

```Székely Levente, Bankó Ádám```

---

# VendTrack – Okos Automata Hálózat és Készletmenedzsment Rendszer
**Szakmai vizsgaremek összefoglaló dokumentáció**

---

### 1. Projekt célja és funkcionális áttekintése
A **VendTrack** célja egy fizikai étel- és italautomata-hálózat digitális transzformációja egy központosított, felhőalapú RESTful architektúrán keresztül. A rendszer valós időben szinkronizálja a készletet, megakadályozza a felesleges kiszállásokat a feltöltők számára, és érintésmentes előrendelési lehetőséget biztosít a vásárlóknak.

* **Fő funkciók:**
  * Valós idejű készlet- és rekeszkapacitás-figyelés automatánként.
  * Webes vásárlói felület: digitális egyenleg, kosár, PIN/QR-kódos átvételi token generálása.
  * Natív asztali adminisztrációs és diszpécser szoftver: automaták állapota, készletfeltöltési listák, hibajelzések.
  * Automata szimulációs felület: digitális átvételi kód beváltása és termékkiadás szimulációja.

---

### 2. Rendszerarchitektúra és technológiai stack

A rendszer tiszta, többrétegű architektúrára épül, biztosítva a komponensek laza csatolását és független tesztelhetőségét.

| Komponens | Alkalmazott technológia | Felelősségi kör / Szerep |
| :--- | :--- | :--- |
| **Backend API** | ASP.NET Core Web API (C#) | Üzleti logika, JWT autentikáció, tranzakciókezelés |
| **Adatbázis** | MySQL / PostgreSQL | Relációs adatmodell, integritási megszorítások, triggerek |
| **Webes Kliens** | HTML5, CSS3/Tailwind, JavaScript | Reszponzív vásárlói felület asztali és mobil eszközökre |
| **Asztali Kliens** | C# .NET WPF | Diszpécseri dashboard, automata szimulátor, offline naplózás |
| **Tesztelés & QA** | xUnit, Postman, Playwright | Egységtesztek, API integrációs tesztek, E2E UI folyamatok |

---

### 3. Főbb adatbázis entitások és kapcsolatok

```
[Machines] 1 ───< N [Slots] >─── 1 [Products]
   │                   │
   1                   N
   │                   │
[Alerts]         [Transactions] >─── 1 [Users]
```

* **Machines:** Automata adatai (azonosító, fizikai helyszín koordinátákkal, hálózati státusz).
* **Slots:** Rekeszek elrendelése automatánként (gép ID, rekeszszám, termék ID, aktuális db, maximum db).
* **Products:** Terméktörzs (név, kategória, egységár, allergének).
* **Transactions:** Vásárlási napló (felhasználó, gép, termék, átvételi PIN, státusz: `Foglalt` | `Kiadva` | `Lejárt`).

---

### 4. Munkamegosztás a 2 fős csapatban

A projekt feladatait a szakterületi követelményekkel összhangban, fejlesztői és tesztelői/kliensoldali modulokra bontva valósítottuk meg:

```
+------------------------------------+    +---------------------------------------+
| 1. Fejlesztő (Backend & Desktop)   |    | 2. Fejlesztő (Web UI & QA)            |
+------------------------------------+    +---------------------------------------+
| • REST API tervezése és logikája   |    | • Reszponzív webes kliens (Frontend)  |
| • Adatbázis tervezés és migrációk  |    | • Végponti & Postman integrációs teszt|
| • Asztali üzemeltetői felület (WPF)|    | • Rendszer- és felhasználói doksi     |
| • Szerveroldali xUnit egységtesztek|    | • Adatbázismodell-diagram készítése   |
+------------------------------------+    +---------------------------------------+
```

---

### 5. Tesztelési stratégia és kódminőség

A *Clean Code* elveket követve a projekt szigorú rétegzettséggel (Controller – Service – Repository) készült.

1. **Egységtesztek (Unit Tests):** A kiadási és levonási logika tesztelése xUnit segítségével (pl. negatív készletre futás megelőzése párhuzamos vásárlási kísérlet esetén).
2. **API Integrációs Tesztek:** Automatizált Postman gyűjtemények a végpontok válaszidejének, HTTP státuszkódjainak és a JWT jogosultságkezelésnek a validálására.
3. **E2E Tesztelés:** A vásárlási folyamat ellenőrzése: termékkiválasztás weben $\to$ tokenképzés a backend által $\to$ tokenbeváltás és állapotfrissítés a szimulátorban.
