# Analiza dlaczego Pull Request nie został zmergowany

## Podsumowanie

Twój pull request [#4366 "Added: Implement text scale disabling feature in terminal view"](https://github.com/termux/termux-app/pull/4366) w repozytorium `termux/termux-app` jest nadal otwarty i nie został zmergowany z następujących powodów:

## Status Pull Request

- **Status**: Otwarty od 23 stycznia 2025
- **Zmiany**: 3 commity, +26 dodań, -2 usunięć w 6 plikach
- **Mergeable**: Tak (brak konfliktów)
- **Stan**: "pending" - oczekuje na przegląd/akcję

## Główne przyczyny braku merge'a

### 1. Naruszenie wytycznych commit messages

Projekt Termux ma **bardzo ścisłe** wymagania dotyczące commit messages zgodnie z [Conventional Commits](https://www.conventionalcommits.org). Z dokumentacji README.md:

> **Commit messages must use the Conventional Commits spec**
> **The first letter for `type` and `description` must be capital and description should be in the present tense.**

**Dozwolone typy (dokładnie jak w nawiasach):**
- **Added** for new features
- **Changed** for changes in existing functionality  
- **Deprecated** for soon-to-be removed features
- **Removed** for now removed features
- **Fixed** for any bug fixes
- **Security** in case of vulnerabilities

**Wymagany format:**
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### 2. Brak formalnego review od maintainerów

Pull request nie ma jeszcze oficjalnego review od maintainerów projektu Termux. Jedynym komentarzem jest:
- Review od `yashthattebito` z komentarzem "Bito is crafting review details..." (prawdopodobnie automatyczny bot)
- Jeden nieistotny komentarz "1 v.l" od innego użytkownika

### 3. Możliwe problemy z CI/CD

Status pull requesta to "pending", co może oznaczać, że:
- Testy automatyczne jeszcze się nie uruchomiły
- Są problemy z workflows GitHub Actions
- Czeka na weryfikację maintainerów

## Twoje zmiany - analiza techniczna

Implementujesz funkcję wyłączania skalowania terminala poprzez:

1. **Nową metodę interfejsu**: `isTerminalViewScalingDisabled()` w `TerminalViewClient`
2. **Implementację w klasie bazowej**: `TermuxTerminalViewClientBase`
3. **Logikę w widoku terminala**: Modyfikacja metody `onScale()` w `TerminalView`
4. **Nowe właściwości konfiguracyjne**: `KEY_DISABLE_TERMINAL_VIEW_SCALING`
5. **Obsługę w ustawieniach**: `TermuxSharedProperties.isTerminalViewScalingDisabled()`

Zmiany wydają się technicznie poprawne i zgodne z architekturą projektu.

## Co zrobić aby PR został zmergowany

### 1. Popraw commit messages (KRYTYCZNE)

Twoje commit messages muszą być przepisane zgodnie z wytycznymi. Przykład:

**Zamiast:**
```
implement text scale disabling feature
```

**Powinno być:**
```
Added: Implement text scale disabling feature in terminal view

This commit introduces a new configuration option to disable terminal view
scaling and includes the necessary interface and implementation changes
to support this feature across the Termux application.
```

### 2. Wykonaj git rebase z poprawionymi commit messages

```bash
git rebase -i HEAD~3  # dla 3 commitów
# Zmień pick na reword dla każdego commita
# Popraw każdy commit message zgodnie z wytycznymi
git push --force-with-lease origin master
```

### 3. Dodaj więcej szczegółów w opisie PR

Rozszerz opis PR o:
- **Uzasadnienie**: Dlaczego ta funkcja jest potrzebna?
- **Przypadki użycia**: Kiedy użytkownicy będą z tego korzystać?
- **Testy**: Jak testowałeś zmiany?
- **Backward compatibility**: Czy zmiany są wstecznie kompatybilne?

### 4. Upewnij się o zgodności z wytycznymi projektu

Z README.md projektu:
> "Pull requests using hardcoded values **will/should not** be accepted"

Sprawdź czy wszystkie nowe stałe używają `TermuxConstants` zamiast hardkodowanych wartości.

### 5. Bądź cierpliwy i proaktywny

- Maintainerzy Termux to wolontariusze
- Projekt ma 467 otwartych issues - duże obciążenie
- Możesz delikatnie "bumped" PR po kilku tygodniach

## Template poprawionego commit message

```
Added: Implement text scale disabling feature in terminal view

- Added isTerminalViewScalingDisabled() method to TerminalViewClient interface
- Modified TerminalView.onScale() to respect the new scaling disable setting  
- Added KEY_DISABLE_TERMINAL_VIEW_SCALING property to TermuxPropertyConstants
- Implemented isTerminalViewScalingDisabled() in TermuxSharedProperties
- Added default implementation in TermuxTerminalViewClientBase

This feature allows users to disable terminal view scaling for better
accessibility and user experience when precise text sizing is required.

Fixes: #[issue-number-if-exists]
```

## Dalsze kroki

1. **Natychmiast**: Popraw commit messages zgodnie z Conventional Commits
2. **Następnie**: Rozszerz opis PR o więcej szczegółów
3. **Opcjonalnie**: Stwórz issue opisujące problem, który rozwiązuje Twoja funkcja
4. **Monitoruj**: Śledź feedback od maintainerów i odpowiadaj szybko

## Przydatne linki

- [Conventional Commits](https://www.conventionalcommits.org)
- [Keep a Changelog](https://github.com/olivierlacan/keep-a-changelog)
- [Termux Contributing Guidelines](https://github.com/termux/termux-app#for-maintainers-and-contributors)
- [Twój PR #4366](https://github.com/termux/termux-app/pull/4366)

Powodzenia z merge'em! 🚀