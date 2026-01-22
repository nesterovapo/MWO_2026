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
