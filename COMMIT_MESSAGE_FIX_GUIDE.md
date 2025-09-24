# Przewodnik naprawy commit messages dla PR #4366

## Szybkie rozwiązanie problemu

Twój PR nie został zmergowany głównie z powodu **niepoprawnych commit messages**. Oto jak to naprawić:

## Krok 1: Sprawdź obecne commit messages

```bash
git log --oneline -3
```

## Krok 2: Rozpocznij interactive rebase

```bash
git rebase -i HEAD~3
```

## Krok 3: Zmień wszystkie commit messages

W edytorze zmień `pick` na `reword` dla wszystkich commitów:

```
reword 1234567 obecny commit message 1
reword 2345678 obecny commit message 2  
reword 3456789 obecny commit message 3
```

## Krok 4: Popraw każdy commit message

Gdy git poprosi o każdy commit message, użyj tego wzorca:

### Template dla commit messages:

```
Added: Implement text scale disabling feature in terminal view

- Added isTerminalViewScalingDisabled() method to TerminalViewClient interface
- Modified TerminalView.onScale() to respect scaling disable setting
- Added KEY_DISABLE_TERMINAL_VIEW_SCALING configuration property
- Implemented property retrieval in TermuxSharedProperties  
- Added default implementation in TermuxTerminalViewClientBase

This feature allows users to disable terminal view scaling gestures,
providing better control over text display and accessibility options.
Users can now prevent accidental zoom changes during terminal usage.
```

### Wymagania Termux dla commit messages:

✅ **POPRAWNE:**
- `Added: Implement text scale disabling feature`
- `Fixed: Resolve terminal scaling issue`
- `Changed: Update scaling behavior in terminal view`

❌ **NIEPOPRAWNE:**
- `add text scaling feature` (mała litera, brak dwukropka)
- `implement scaling` (niepełny opis)
- `fixes scaling bug` (mała litera na początku)

## Krok 5: Wypchnij zmiany

```bash
git push --force-with-lease origin master
```

⚠️ **UWAGA**: `--force-with-lease` jest bezpieczniejsze niż `--force`

## Krok 6: Sprawdź PR na GitHub

Po wypchnieciu PR powinien zostać automatycznie zaktualizowany z nowymi commit messages.

## Dodatkowe ulepszenia PR

### Rozszerz opis PR o:

1. **Problem**: Dlaczego ta funkcja jest potrzebna?
   ```
   ## Problem
   Users accidentally trigger scaling gestures while using the terminal,
   disrupting their workflow and text readability.
   ```

2. **Rozwiązanie**: Jak Twoja zmiana to rozwiązuje?
   ```
   ## Solution  
   This PR adds a configuration option to disable terminal view scaling
   through the `disable_terminal_view_scaling` property.
   ```

3. **Testowanie**: Jak przetestowałeś?
   ```
   ## Testing
   - Tested on Android 12+ devices
   - Verified scaling disable/enable functionality
   - Confirmed backward compatibility
   ```

## Częste błędy do uniknięcia

❌ **Nie rób tego:**
- `git push --force` (użyj `--force-with-lease`)
- Używaj małych liter na początku typu
- Zapomnij o dwukropku po typie
- Używaj przeszłego czasu w opisie

✅ **Rób to:**
- Używaj dokładnie tych typów: `Added`, `Fixed`, `Changed`, `Removed`, `Deprecated`, `Security`
- Pisz opisy w czasie teraźniejszym
- Dodawaj scope w nawiasach jeśli potrzebne: `Fixed(terminal): ...`
- Używaj `!` przed dwukropkiem dla breaking changes: `Changed!: ...`

## Po naprawie commit messages

1. **Czekaj cierpliwie** - maintainerzy to wolontariusze
2. **Odpowiadaj szybko** na feedback
3. **Bądź gotowy na poprawki** - mogą poprosić o zmiany
4. **Nie "bumped" PR** zbyt często - max raz na 2-3 tygodnie

## Jeśli nadal masz problemy

1. Sprawdź inne PR w repozytorium termux/termux-app jako przykłady
2. Poczytaj pełne wytyczne: https://www.conventionalcommits.org
3. Sprawdź czy wszystkie pliki używają `TermuxConstants` zamiast hardkodowanych wartości

Powodzenia! 🎯