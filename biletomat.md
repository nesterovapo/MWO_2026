# Aktor: Biletomat

## Historyjki użytkownika

### Historia 1
Jako biletomat, chcę posiadać czytelny ekran dotykowy, aby użytkownik mógł łatwo nawigować po interfejsie.

### Historia 2
Jako biletomat, chcę być wyposażony w różne metody płatności (terminal kart, czytnik gotówki, NFC), aby obsługiwać różnorodne transakcje.

### Historia 3
Jako biletomat, chcę wydawać resztę w gotówce, jeśli użytkownik zapłaci nadmiarowo, aby transakcja była zgodna z oczekiwaniami.

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

## Wspólny diagram przypadków użycia

### DIAGRAM PRZYPADKÓW UŻYCIA - BILETOMAT

```mermaid
flowchart TD
    %% Wspólny start
    MainStart([Uruchomienie biletomatu]) --> Init[Inicjalizacja systemu]
    Init --> Choice{Typ operacji}
    
    %% DIAGRAM1 - Wyświetlenie dostępnych biletów (Osoba 1)
    Choice -->|Wyświetlanie biletów| Start1[Uruchomienie ekranu powitalnego]
    Start1 --> A1[Pobranie listy biletów]
    A1 --> B1[Wyświetlenie biletów]
    B1 --> C1[Oczekiwanie na wybór użytkownika]
    C1 --> End1([Rejestracja wyboru użytkownika])
    
    %% DIAGRAM2 - Obsługa wyboru języka (Osoba 2)
    Choice -->|Obsługa języka| Start2[Wyświetlenie opcji językowych]
    Start2 --> A2[Rejestracja wyboru języka]
    A2 --> B2[Dostosowanie interfejsu]
    B2 --> End2([Interfejs w wybranym języku])
    
    %% Połączenie diagramów
    End2 --> Start1
    
    %% Include relations z DIAGRAM1
    Start1 --> Include1[Aktualizacja biletów]
    Include1 --> A1
    
    %% Include relations z DIAGRAM2
    MainStart --> Include2[Opcje językowe]
    Include2 --> Start2
    
    %% Extend relations z DIAGRAM1
    A1 --> Extend1[Ostrzeżenie o braku danych]
    Extend1 -.->|awaria sieci| Error1[Wyświetlenie komunikatu błędu]
    Error1 --> Fallback1[Użycie lokalnej kopii danych]
    Fallback1 --> B1
    
    %% Extend relations z DIAGRAM2
    Start2 --> Extend2[Powrót do języka domyślnego]
    Extend2 -.->|po braku aktywności użytkownika| Reset[Reset do języka domyślnego]
    Reset --> Start2
    
    style MainStart fill:#e1f5fe
    style Choice fill:#fff3e0
    style End1 fill:#f3e5f5
    style End2 fill:#f3e5f5
    style Include1 fill:#fff3e0
    style Include2 fill:#fff3e0
    style Extend1 fill:#ffebee
    style Extend2 fill:#f1f8e9
    style Error1 fill:#ffebee
    style Fallback1 fill:#f1f8e9
    style Reset fill:#ffebee
```
