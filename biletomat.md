# Aktor: Biletomat

## DIAGRAMY PRZYPADKÓW UŻYCIA

### WYŚWIETLENIE DOSTĘPNYCH BILETÓW

```mermaid
flowchart TD
    Start([Inicjalizacja biletomatu]) --> A[Uruchomienie ekranu powitalnego]
    A --> B[Pobranie listy biletów]
    B --> C[Wyświetlenie biletów]
    C --> D[Oczekiwanie na wybór użytkownika]
    D --> End([Rejestracja wyboru użytkownika])
    
    %% Include relations - Aktualizacja biletów
    A --> E[Aktualizacja biletów]
    E --> B
    
    %% Extend relations - Ostrzeżenie o braku danych
    B --> F[Ostrzeżenie o braku danych]
    F -.->|awaria sieci| G[Wyświetlenie komunikatu błędu]
    G --> H[Użycie lokalnej kopii danych]
    H --> C
    
    style Start fill:#e1f5fe
    style End fill:#f3e5f5
    style E fill:#fff3e0
    style F fill:#ffebee
    style G fill:#ffebee
    style H fill:#f1f8e9
```

### OBSŁUGA WYBORU JĘZYKA

```mermaid
flowchart TD
    Start([Inicjalizacja interfejsu językowego]) --> A[Wyświetlenie opcji językowych]
    A --> B[Rejestracja wyboru języka]
    B --> C[Dostosowanie interfejsu]
    C --> End([Interfejs w wybranym języku])
    
    %% Include relations - Opcje językowe
    Start --> D[Opcje językowe]
    D --> A
    
    %% Extend relations - Powrót do języka domyślnego
    A --> E[Powrót do języka domyślnego]
    E -.->|po braku aktywności użytkownika| F[Reset do języka domyślnego]
    F --> A
    
    style Start fill:#e1f5fe
    style End fill:#f3e5f5
    style D fill:#fff3e0
    style E fill:#f1f8e9
    style F fill:#ffebee
```

## DIAGRAMY SEKWENCJI
### SZYBKI WYBÓR RODZAJU BILETU 
```mermaid
sequenceDiagram
    participant B as Biletomat
    participant SC as System_centralny
    participant U as Uzytkownik

    B->>B: Uruchomienie ekranu powitalnego
    B->>SC: Pobranie listy dostepnych biletow
    SC-->>B: Lista biletow i kategorii

    B-->>U: Wyswietlenie kategorii biletow i szczegolow
    B-->>U: Oczekiwanie na wybor uzytkownika
```
### DIAGRAM SEKWENCJI DLA PRZYPADKU UŻYCIA "OBSŁUGA WYBORU JĘZYKA"

**AKTOR:** BILETOMAT  
**OBIEKTY:** INTERFEJS BILETOMATU, SYSTEM JĘZYKOWY, BAZA KONFIGURACJI, UŻYTKOWNIK

```mermaid
sequenceDiagram
    participant U as Użytkownik
    participant IB as Interfejs Biletomatu
    participant SJ as System Językowy
    participant BK as Baza Konfiguracji
    
    Note over IB: Ekran powitalny z opcjami języków
    IB->>IB: Wyświetlenie opcji językowych
    IB->>U: Prezentacja dostępnych języków
    
    U->>IB: Wybór preferowanego języka
    IB->>IB: Rejestracja wyboru języka
    
    IB->>SJ: Żądanie zmiany języka
    SJ->>BK: Zapytanie o konfigurację języka
    BK-->>SJ: Dane językowe (tłumaczenia, format)
    
    alt Język dostępny
        SJ->>SJ: Ładowanie zasobów językowych
        SJ-->>IB: Potwierdzenie zmiany języka
        IB->>IB: Dostosowanie interfejsu
        IB-->>U: Wyświetlenie interfejsu w nowym języku
        Note over U, IB: Interface działa w wybranym języku
    else Język niedostępny  
        SJ-->>IB: Błąd - język niedostępny
        IB-->>U: Komunikat o błędzie
        IB->>IB: Powrót do poprzedniego języka
    end
    
    Note over SJ: Monitoring aktywności użytkownika
    
    alt Timeout - brak aktywności
        SJ->>SJ: Wykrycie braku aktywności
        SJ->>BK: Żądanie języka domyślnego
        BK-->>SJ: Konfiguracja języka domyślnego
        SJ->>IB: Reset do języka domyślnego  
        IB->>IB: Przywrócenie domyślnego interfejsu
        Note over IB: Powrót do języka domyślnego
    end
```

### DIAGRAMY KLAS

#### ANALIZA PRZYPADKU UŻYCIA I DIAGRAMU SEKWENCJI - OBSŁUGA WYBORU JĘZYKA

Na podstawie diagramu sekwencji dla przypadku użycia "Obsługa wyboru języka" zidentyfikowano następujące klasy odpowiedzialne za realizację funkcjonalności:

## OPIS KLAS

### ZIDENTYFIKOWANE KLASY:

- **INTERFEJSBILETOMATU** - odpowiada za wyświetlanie opcji językowych i interakcję z użytkownikiem
- **SYSTEMJEKOWY** - zarządza zmianami języka i monitoruje aktywność użytkownika  
- **BAZAKONFIGURACJI** - przechowuje dane językowe i konfiguracje
- **KONFIGURACYJEKOWA** - reprezentuje ustawienia pojedynczego języka
- **KONTROLERCZASU** - monitoruje timeout i powrót do języka domyślnego

```mermaid
classDiagram
    class InterfejsBiletomatu {
        -String aktualnyJezyk
        -List~String~ dostepneJezyki  
        -Boolean czyAktywny
        -Int timeoutCzas
        +void wyswietlOpcjeJezykowe()
        +void rejestrujWyborJezyka(String jezyk)
        +void dostosowujInterfejs(String jezyk)
        +void wyswietlKomunikat(String komunikat)
        +void resetDoDomsylnego()
    }
    
    class SystemJezykowy {
        -String domyslnyJezyk
        -Map~String, KonfiguracjaJezykowa~ konfiguracjeJezykowe
        -Long ostatniaAktywnosc
        +void zadanieZmianyJezyka(String jezyk)
        +KonfiguracjaJezykowa pobierzKonfiguracje(String jezyk)
        +void ustawDomyslnyJezyk(String jezyk)
        +void ladujZasobyJezykowe(String jezyk)
        +Boolean wykryjBrakAktywnosci()
        +void monitorujAktywnosc()
    }
    
    class BazaKonfiguracji {
        -Map~String, KonfiguracjaJezykowa~ daneJezykowe
        -Map~String, Integer~ statystykiUzycia
        +KonfiguracjaJezykowa pobierzDaneJezykowe(String jezyk)
        +void zapiszKonfiguracje(String jezyk, KonfiguracjaJezykowa konfiguracja)
        +Boolean sprawdzDostepnosc(String jezyk)
        +List~String~ pobierzWszystkieJezyki()
    }
    
    class KonfiguracjaJezykowa {
        -String kodJezyka
        -String nazwaJezyka
        -Map~String, String~ tlumaczenia
        -String formatDaty
        -String formatLiczb
        +String pobierzTlumaczenie(String klucz)
        +void ustawFormat(String typ, String format)
        +Boolean czyKompletna()
    }
    
    class KontrolerCzasu {
        -Long czasStartu
        -Int limitTimeout
        -Boolean czyAktywny
        +void rozpocznijLiczenie()
        +Boolean sprawdzTimeout()
        +void resetujLicznik()
        +void zatrzymajLiczenie()
    }

    InterfejsBiletomatu --> SystemJezykowy : zarządza językami
    SystemJezykowy --> BazaKonfiguracji : pobiera dane językowe
    BazaKonfiguracji *-- KonfiguracjaJezykowa : zawiera
    SystemJezykowy --> KontrolerCzasu : monitoruje aktywność
    InterfejsBiletomatu --> KontrolerCzasu : powiadamia o działaniach
```
