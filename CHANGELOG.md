# Changelog

BatchBridge zaczyna historię release'ów w tym repozytorium od sekcji
`Unreleased`. Oznaczenia V1-V8 w dokumentacji są poziomami dojrzałości i zapisami
technicznych milestone'ów; nie oznaczają, że odpowiadające im tagi istnieją w
tym repozytorium.

Nowe zachowanie trafia najpierw do `Unreleased`. Wersjonowana sekcja pojawia się
dopiero po utworzeniu taga i zebraniu dowodów opisanych w
[`doc/RELEASE_PROCESS.md`](doc/RELEASE_PROCESS.md).

## Unreleased

### Documentation

- Zaktualizowano punkty wejścia dokumentacji na tożsamość BatchBridge.
- Zastąpiono sprzeczne kopie roadmapy jednym kanonicznym widokiem.
- Usunięto przestarzałe snapshoty, backlogi, audyty i plany implementacji bez
  aktualnego dowodu.
- Dodano jednolity proces release'u oddzielający walidację repozytorium od
  dowodu stagingowego.

### Maintained baseline

Repozytorium nadal zawiera pełny starter .NET 9 + React 19, modularny backend,
uwierzytelnianie i autoryzację, projekty i zadania, załączniki, powiadomienia,
PostgreSQL, CI/CD, benchmarki V6, migrację MediatR V7 oraz scaffolding V8.0.

## Release policy

- Tag jest źródłem prawdy dla wydanej wersji.
- Release opisuje dokładny commit, walidację, ryzyko i rollback.
- Dowód CI nie zastępuje testu target runtime.
- Dokumenty measurement-first pozostają w repozytorium tylko wtedy, gdy opisują
  powtarzalną procedurę lub zaakceptowany kontrakt.
