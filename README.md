<div align="center">

# 🥤 VendTrack
### Okos Automata Hálózat és Készletmenedzsment Rendszer
**Szakmai vizsgaremek összefoglaló dokumentáció (2026)**

[![.NET Core](https://img.shields.io/badge/.NET_8.0-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)](#)
[![C#](https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=csharp&logoColor=white)](#)
[![WPF](https://img.shields.io/badge/WPF-Desktop-0078D7?style=for-the-badge&logo=windows&logoColor=white)](#)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)](#)
[![MySQL](https://img.shields.io/badge/MySQL-005C84?style=for-the-badge&logo=mysql&logoColor=white)](#)
[![xUnit](https://img.shields.io/badge/Tested_with-xUnit-blueviolet?style=for-the-badge)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](#)

<p align="center">
  <b>Készítették:</b> 
  <a href="https://github.com/">Székely Levente</a> &bull; 
  <a href="https://github.com/">Bankó Ádám</a>
</p>

</div>

---

## 📌 Tartalomjegyzék
- [Áttekintés](#-1-projekt-célja-és-funkcionális-áttekintése)
- [Rendszerarchitektúra és Stack](#-2-rendszerarchitektúra-és-technológiai-stack)
- [Adatbázis Modell](#-3-főbb-adatbázis-entitások-és-kapcsolatok)
- [Munkamegosztás](#-4-munkamegosztás-a-2-fős-csapatban)
- [Tesztelési Stratégia](#-5-tesztelési-stratégia-és-kódminőség)
- [Telepítés és Futtatás](#-6-telepítés-és-futtatás)

---

## 🚀 1. Projekt célja és funkcionális áttekintése

A **VendTrack** célja egy fizikai étel- és italautomata-hálózat digitális transzformációja egy központosított, felhőalapú RESTful architektúrán keresztül. A rendszer valós időben szinkronizálja a készletet, megakadályozza a felesleges kiszállásokat a feltöltők számára, és érintésmentes előrendelési lehetőséget biztosít a vásárlóknak.

### 🌟 Főbb funkciók
- 📦 **Valós idejű készletfigyelés:** Gépenkénti és rekeszenkénti kapacitás és fogyás monitorozása.
- 🛒 **Webes vásárlói felület:** Reszponzív felület digitális egyenleggel, kosárkezeléssel és azonnali PIN/QR-kódos átvételi token generálással.
- 🖥️ **Natív asztali adminisztráció (WPF):** Áttekintő diszpécser dashboard, dinamikus feltöltési listák és valós idejű telemetria/hibajelzések kezelése.
- ⚙️ **Automata szimulátor:** Dedikált modul a helyszíni hardver működésének, kódbeváltásának és termékkiadásának tesztelésére.

---

## 🛠️ 2. Rendszerarchitektúra és technológiai stack

A rendszer tiszta, többrétegű (*Controller &rarr; Service &rarr; Repository*) architektúrára épül, amely garantálja a komponensek laza csatolását és független skálázhatóságát, tesztelhetőségét.

| Réteg / Komponens | Alkalmazott technológia | Felelősségi kör / Szerep |
| :--- | :--- | :--- |
| **Backend API** | ![ASP.NET Core](https://img.shields.io/badge/ASP.NET_Core-512BD4?style=flat-square&logo=dotnet&logoColor=white) | Üzleti logika, JWT autentikáció, tranzakcióbiztos raktárkezelés |
| **Adatbázis** | ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white) | Relációs séma, integritási feltételek, triggerek és audit naplózás |
| **Webes Kliens** | ![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white) ![TailwindCSS](https://img.shields.io/badge/Tailwind-38B2AC?style=flat-square&logo=tailwind-css&logoColor=white) ![JS](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black) | Reszponzív vásárlói felület asztali és mobil eszközökre |
| **Asztali Kliens** | ![WPF](https://img.shields.io/badge/.NET_WPF-0078D7?style=flat-square&logo=windows&logoColor=white) | Diszpécseri vezérlőpult, automata szimulátor, hálózati diagnosztika |
| **Tesztelés & QA** | ![xUnit](https://img.shields.io/badge/xUnit-blueviolet?style=flat-square) ![Postman](https://img.shields.io/badge/Postman-FF6C37?style=flat-square&logo=postman&logoColor=white) ![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat-square&logo=playwright&logoColor=white) | Egységtesztek, API integrációs tesztek, E2E vásárlási folyamatok |

---

## 🗄️ 3. Főbb adatbázis entitások és kapcsolatok

```text
[Machines] 1 ───< N [Slots] >─────── 1 [Products]
    │                   │
    1                   N
    │                   │
[Alerts]          [Transactions] >─── 1 [Users]
```

### Entitás leírások:
* **`Machines`**: Automata törzsadatok (egyedi azonosító, GPS koordináták, telephely, hálózati állapot).
* **`Slots`**: Rekeszkiosztás gépenként (`MachineId`, `SlotNumber`, `ProductId`, aktuális darabszám, max kapacitás).
* **`Products`**: Termékkatalógus (megnevezés, kategória, fogyasztói ár, allergének, tápértékek).
* **`Transactions`**: Tranzakciós napló (`UserId`, `MachineId`, `ProductId`, átvételi PIN/QR kód hash, státusz: `Foglalt` | `Kiadva` | `Lejárt`).
* **`Alerts`**: Gépi riasztások (hőmérséklet hiba, elakadás, kiürülésveszély).

---

## 👥 4. Munkamegosztás a 2 fős csapatban

| Szerepkör | Felelős | Főbb feladatok és modulok |
| :--- | :--- | :--- |
| **Backend & Desktop** | **Székely Levente** | &bull; REST API tervezés és végrehajtás (.NET)<br>&bull; Adatbázis sématervezés, indexelés és migrációk<br>&bull; Asztali üzemeltetői felület és hardverszimuláció (WPF)<br>&bull; Szerveroldali xUnit egység- és szerviztesztek |
| **Web UI & QA** | **Bankó Ádám** | &bull; Reszponzív webes vásárlói frontend felület kialakítása<br>&bull; Végponti Postman integrációs tesztcsomagok készítése<br>&bull; Teljes körű rendszer- és végfelhasználói dokumentáció<br>&bull; Adatbázis architektúra és kapcsolati diagramok elkészítése |

---

## 🧪 5. Tesztelési stratégia és kódminőség

A fejlesztés során a **Clean Code** és **SOLID** alapelveket követtük, szigorú rétegzettséget fenntartva.

1. **Egységtesztek (Unit Tests - xUnit):**
   - Raktárlevonási és versenyhelyzet (race condition) vizsgálatok párhuzamos vásárlások esetén.
   - Tranzakciós határidők és PIN generáló algoritmusok lefedettsége.
2. **API Integrációs Tesztek (Postman Collection):**
   - HTTP státuszkódok és strukturált hibaválaszok validálása.
   - JWT jogosultságok (User / Dispatcher / Admin) szigorú ellenőrzése.
3. **End-to-End (E2E) Tesztelés:**
   - Teljes vásárlási életciklus ellenőrzése:
     $$\text{Termékkiválasztás (Web)} \longrightarrow \text{Token generálás (Backend)} \longrightarrow \text{Kiadás szimuláció (Desktop Client)}$$

---

## 💻 6. Telepítés és Futtatás

### Előfeltételek
- [.NET 8.0 SDK](https://dotnet.microsoft.com/download)
- [MySQL Server 8.0+](https://dev.mysql.com/downloads/)
- Modern webböngésző (Chrome, Edge, Firefox)

### Lépések

1. **Repository klónozása:**
   ```bash
   git clone https://github.com/felhasznalonev/VendTrack.git
   cd VendTrack
   ```

2. **Backend beállítása:**
   ```bash
   cd src/VendTrack.Api
   # Állítsd be a kapcsolati sztringet az appsettings.json fájlban!
   dotnet restore
   dotnet ef database update
   dotnet run
   ```

3. **Asztali kliens indítása:**
   ```bash
   cd src/VendTrack.Desktop
   dotnet run
   ```

---

<div align="center">
  <sub>VendTrack &copy; 2026 - Szakmai Érettségi / Technikusi Vizsgaremek</sub>
</div>
