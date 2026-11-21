# Roadmap BatchBridge

## Cel

To jest kanoniczny widok dalszego rozwoju BatchBridge. Oznaczenia V1-V8 opisują
poziomy dojrzałości, a nie serię automatycznych kalendarzowych release'ów.

BatchBridge pozostaje:

- modularnym monolitem .NET z backendem ASP.NET Core;
- typowanym frontendem React;
- referencyjną bazą do nauki i ponownego użycia;
- projektem, który oddziela stan kodu od dowodu działania na realnym środowisku.

Roadmapa nie jest listą technologii do wdrożenia. Nowa zależność lub infrastruktura
wymaga konkretnego problemu, prostszej alternatywy, kosztu utrzymania i mierzalnego
kryterium sukcesu.

## Aktualny stan

Przegląd aktualny na **2026-11-21**.

| Etap | Stan | Znaczenie dla kolejnego release'u |
| --- | --- | --- |
| V1 Fundament | Zrealizowany | Build, podstawowe workflowy, baza PostgreSQL i testy są częścią startera. |
| V2 Bezpieczeństwo | Zrealizowany w repozytorium | Sesje, rotacja tokenów, 2FA, lockout, rate limiting i konfiguracja mają testy; hosting nadal wymaga walidacji środowiskowej. |
| V3 Domena i VSA | Zrealizowany pilotaż | `Projects`, `ProjectTasks` i `Notifications` używają modułowych slice'ów; starsze obszary migrują się tylko przy realnej zmianie. |
| V4 Kompletność produktu | Częściowo domknięty | Główne workflowy istnieją, ale kontrakt typów Notifications nie jest jeszcze zgodny między backendem i frontendem, a krytyczna macierz E2E wymaga kolejnych scenariuszy. |
| V5 Operacje | Implementacja gotowa, dowód środowiskowy nieukończony | VPS, migracje, backup, restore, rollback i obserwowalność są przygotowane, lecz bez stagingu nie można deklarować produkcyjnego release'u. |
| V6 Niezawodność | Zrealizowany dla udokumentowanego zakresu | Baseline, indeks paginacji, idempotencja, outbox leasing, metryki i odporność frontendu mają zapisaną metodę powtórzenia. |
| V7 Ewolucja architektury | Zrealizowany tor MediatR | Moduły produkcyjne używają `ISender` i `IRequestHandler`; pozostałe kierunki są opcjonalne. |
| V8 Reusable starter | Zrealizowany V8.0 | Warianty template'u i generatory mają proof. Pakiety, automatyczne aktualizacje i większe moduły capability pozostają poza zakresem. |

## Priorytet kolejnego cyklu

1. **Tożsamość i dokumentacja release'u.** README, changelog, release checklist i
   linki mają wskazywać BatchBridge oraz bieżące repozytorium.
2. **Kontrakt Notifications.** Dwanaście typów backendu musi mieć jawne zachowanie
   frontendu i fallback dla nieznanego przyszłego typu.
3. **Autoryzacja i E2E.** Utrzymać macierz uprawnień oraz dodać testy tylko dla
   krytycznych workflowów, których obecne scenariusze nie pokrywają.
4. **Dowody V5.** Staging, backup off-host, restore, alert i rollback są osobną
   bramką środowiskową, nie przeszkodą dla lokalnych zmian funkcjonalnych.
5. **V8.1 tylko po użyciu.** `dotnet new`, pakiety i automatyczne aktualizacje mają
   sens dopiero po zebraniu kosztu realnego użycia startera.

## Reguły planowania

Każda zmiana produktowa potrzebuje:

- konkretnego aktora i problemu;
- właściciela danych oraz reguły;
- granicy transakcji i zachowania przy concurrency;
- publicznego kontraktu oraz meaningful failure paths;
- najtańszego testu odrzucającego hipotezę;
- obserwowalności, rollout i rollback, jeśli dotyczy;
- aktualizacji dokumentacji razem ze zmianą.

Większą propozycję opisz w
[`../FEATURE_PROPOSAL_TEMPLATE.md`](../FEATURE_PROPOSAL_TEMPLATE.md), a implementację
prowadź według [`../ADDING_FEATURES.md`](../ADDING_FEATURES.md).

## Dokumenty milestone'ów

Dokumenty poniżej są archiwami zakresu i decyzji. Nie są dowodem aktualnej
gotowości release'u; ten dowód powstaje według
[`../RELEASE_PROCESS.md`](../RELEASE_PROCESS.md).

| Etap | Dokument |
| --- | --- |
| V1 | [01_V1_JUNIOR_BASELINE.md](01_V1_JUNIOR_BASELINE.md) |
| V2 | [02_V2_STABILIZATION_AND_SECURITY.md](02_V2_STABILIZATION_AND_SECURITY.md) |
| V3 | [03_V3_DOMAIN_TRANSACTIONS_AND_CONCURRENCY.md](03_V3_DOMAIN_TRANSACTIONS_AND_CONCURRENCY.md) |
| V4 | [04_V4_PRODUCT_COMPLETENESS.md](04_V4_PRODUCT_COMPLETENESS.md) |
| V5 | [05_V5_DEPLOYMENT_AND_OPERATIONS.md](05_V5_DEPLOYMENT_AND_OPERATIONS.md) |
| V6 | [06_V6_PERFORMANCE_AND_RELIABILITY.md](06_V6_PERFORMANCE_AND_RELIABILITY.md) |
| V7 | [07_V7_OPTIONAL_EVOLUTION.md](07_V7_OPTIONAL_EVOLUTION.md) |
| V8 | [08_V8_REUSABLE_MODULAR_STARTER.md](08_V8_REUSABLE_MODULAR_STARTER.md) |

## Decyzje zachowane

- [Granica agregatu `ProjectTask`](10_ADR_PROJECT_TASK_AGGREGATE_BOUNDARY.md)
- [Inkrementalna modularizacja VSA](11_ADR_INCREMENTAL_MODULAR_VSA.md)
- [Inkrementalna adopcja MediatR](14_ADR_INCREMENTAL_MEDIATR_ADOPTION.md)
- [Dystrybucja source-template](15_ADR_SOURCE_TEMPLATE_DISTRIBUTION.md)
- [Polityka sesji](09_ADR_AUTH_SESSION_POLICY.md)
- [Ochrona przed brute force](09_ADR_AUTH_BRUTE_FORCE_PROTECTION.md)

Standard wykonawczy dla nowych slice'ów znajduje się w
[`../MODULAR_VSA_MODULE_CHECKLIST.md`](../MODULAR_VSA_MODULE_CHECKLIST.md).
