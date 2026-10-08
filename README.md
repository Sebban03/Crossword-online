# Krzyżówka Duel

Turowa krzyżówka panoramiczna dla dwóch osób. Każdy gra na swoim urządzeniu (telefon, laptop), na wspólnej planszy. Sześć poziomów, od rozgrzewki do eksperta.

Cała gra to jeden plik `index.html`. Wspólny stan gry trzyma Firebase Firestore (darmowy plan wystarczy z zapasem).

## 1. Firebase (ok. 5 minut)

1. Wejdź na https://console.firebase.google.com i utwórz nowy projekt (Google Analytics możesz wyłączyć).
2. W menu po lewej wybierz **Build → Firestore Database → Create database**. Lokalizacja `eur3 (Europe)`, tryb **production**.
3. W zakładce **Rules** zastąp całą treść zawartością pliku `firestore.rules` z tego repozytorium i kliknij **Publish**.
4. Wejdź w **Project settings** (koło zębate) → sekcja **Your apps** → ikona `</>` (Web). Podaj dowolną nazwę, hostingu Firebase nie zaznaczaj.
5. Firebase pokaże obiekt `firebaseConfig`. Skopiuj go i wklej w `index.html` w miejsce `null` w linijce

```js
const FIREBASE_CONFIG = null;
```

tak żeby wyglądało to mniej więcej tak

```js
const FIREBASE_CONFIG = {
  apiKey: "AIza...",
  authDomain: "twoj-projekt.firebaseapp.com",
  projectId: "twoj-projekt",
  storageBucket: "twoj-projekt.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

Klucz `apiKey` w publicznym kodzie to w Firebase normalna rzecz. Dostęp do danych ograniczają reguły z punktu 3.

## 2. GitHub Pages

1. Utwórz nowe publiczne repozytorium, np. `krzyzowka-duel`.
2. **Add file → Upload files** i wgraj `index.html`, `README.md` oraz `firestore.rules`.
3. **Settings → Pages → Build and deployment**. Source **Deploy from a branch**, branch `main`, folder `/ (root)`, potem **Save**.
4. Po minucie gra będzie pod adresem `https://TWOJ-LOGIN.github.io/krzyzowka-duel/`.

## Jak grać

1. Obie osoby otwierają ten sam adres.
2. Pierwsza wpisuje imię, wybiera poziom i klika **Utwórz grę i pokaż kod**.
3. Druga wpisuje imię i ten 4 literowy kod, potem **Dołącz**. Gra startuje sama, losując kto zaczyna.

Zasady w skrócie

* Tura trwa 2 minuty, gracz dostaje 5 losowych liter z tych, których brakuje na całej planszy.
* Klikasz kratkę, potem literę. Na komputerze można pisać z klawiatury, Enter zatwierdza.
* Dobra litera +1, zła −1 (zła nie zostaje na planszy).
* Dokończenie hasła daje premię 2, 4, 7 lub 10 punktów zależnie od długości.
* Wszystkie 5 liter bez błędu dwie tury z rzędu daje +3, kolejne tury +5, +7, maksymalnie +9.
* Po 4 turach z rzędu, w których trafiono najwyżej jedną literę, gra pokazuje obu graczom jedno hasło jako podpowiedź.
* Jeśli rywal zniknie, jego tura przepada po 2 minutach i 15 sekundach.

Wszystkie liczby da się zmienić w obiekcie `SCORE` na górze skryptu.

## Testowanie bez Firebase

Bez konfiguracji gra działa w trybie testowym. Możesz otworzyć ją w dwóch kartach tej samej przeglądarki (jedna tworzy grę, druga dołącza) albo wybrać **Gra na jednym urządzeniu**, gdzie gracze podają sobie telefon.

## Dodawanie poziomów

Plansze siedzą w tablicy `LEVELS` w `index.html`. Każde hasło to `[kierunek, wiersz, kolumna, "HASŁO", "Definicja"]`, gdzie kierunek `0` oznacza poziomo, a `1` pionowo. Pole z definicją stoi zawsze tuż przed hasłem (z lewej albo nad nim). Wiersz 0 i kolumna 0 to zawsze pola definicji.
