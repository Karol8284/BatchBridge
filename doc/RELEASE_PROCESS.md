# Proces release'u

Ten dokument jest pojedynczym źródłem prawdy dla przygotowania release'u
BatchBridge. Dokumentacja milestone'ów V1-V8 opisuje zakres; nie zastępuje
aktualnego dowodu dla release candidate.

## Obowiązkowa kolejność

1. Czysty, przewidywalny stan roboczy i jeden zaakceptowany zakres release'u.
2. `Unreleased` w [`CHANGELOG.md`](../CHANGELOG.md) opisuje zmiany oraz ryzyko.
3. Dokumentacja publiczna wskazuje aktualne repozytorium i działające ścieżki.
4. Wszystkie decyzje odbiegające od architektury mają ADR.
5. Buildy i testy wykonują się na dokładnym commicie kandydackim.
6. Dowody stagingowe są wymagane przed deklaracją produkcji.

## Bramka repozytorium

Uruchom od katalogu głównego repozytorium:

```powershell
dotnet restore backend\backend.slnx
dotnet build backend\backend.slnx --configuration Release
dotnet test backend\UnitTests\UnitTests.csproj --configuration Release
dotnet test backend\IntegrationTests\IntegrationTests.csproj --configuration Release
```

```powershell
Set-Location frontend
npm ci
npm run test:once
npm run build
Set-Location ..
```

Testy PostgreSQL i pełny smoke wymagają działającego Docker Engine. Pełna lokalna
bramka:

```powershell
docker info
.\scripts\Invoke-E2ETests.ps1
```

Walidacja dokumentacji:

```bash
git diff --check
```

Wynik builda nie oznacza wyniku runtime. Test jednostkowy nie oznacza przejścia
PostgreSQL, a CI lub lokalny Compose nie oznacza działania logowania, storage ani
alertów na realnym hoście.

## Bramka środowiskowa

Przed release'em produkcyjnym muszą istnieć dowody dla:

- protected staging i deployu obrazu z pełnym SHA;
- kontrolowanej migracji;
- publicznego smoke przez HTTPS;
- backupu skopiowanego poza host;
- restore i health checks po odtworzeniu;
- dostarczenia testowego alertu;
- rollbacku do poprzedniego niezmiennego obrazu.

Procedura znajduje się w
[`V5_RELEASE_GATE.md`](V5_RELEASE_GATE.md) i
[`V5_DEPLOYMENT_RUNBOOK.md`](V5_DEPLOYMENT_RUNBOOK.md).

## Wymagany rekord wydania

Rekord w GitHub Release lub innym zatwierdzonym artefakcie powinien zawierać:

| Pole | Zawartość |
| --- | --- |
| Commit | Pełny SHA kandydata |
| Zakres | Linki do wpisów changeloga i ADR-ów |
| Repozytorium | Wyniki faktycznie uruchomionych buildów, testów i Compose |
| Środowisko | Wynik stagingu, backupu, restore, alertu i rollbacku albo jawne `N/A` |
| Ryzyko | Zmiany kontraktu, migracje, sekrety i operacyjny rollback |
| Artefakty | Obrazy GHCR, raporty CI, evidence i poprzednia wersja rollbacku |

Nie twórz repozytoryjnych plików z `TBD`, gdy brak danych. Brak dowodu należy
oznaczyć jako `NOT RUN` lub `BLOCKED`, a nie jako pozytywny wynik.

## Tag i changelog

- Nazwa taga: `vMAJOR.MINOR.PATCH`.
- Zmiany trafiają najpierw do `Unreleased`.
- Sekcja wersji powstaje po przejściu bramek i utworzeniu taga.
- Linki do release'ów i porównań używają repozytorium `Karol8284/BatchBridge`.
- Nie przenoś starej historii tagów z innego repozytorium bez osobnego dowodu.
