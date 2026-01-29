
# Aktor: Użytkownik

## DIAGRAMY PRZYPADKÓW UŻYCIA

### SZYBKI WYBÓR RODZAJU BILETU

```mermaid
flowchart TD
    Start([Użytkownik podchodzi do biletomatu]) --> A[Rozpoczęcie interakcji]
    A --> B[Wybór kategorii]
    B --> C[Wybór biletu]
    C --> D[Wyświetlenie podsumowania]
    D --> E[Potwierdzenie wyboru]
    E --> F[Realizacja transakcji]
    F --> End([Zakończenie procesu])
    
    %% Include relations - Sprawdzenie biletów
    B --> G[Sprawdzenie biletów]
    G --> C
    
    %% Include relations - Anulowanie transakcji (dostępne w dowolnym momencie)
    A --> H[Anulowanie transakcji]
    B --> H
    C --> H
    D --> H
    E --> H
    H --> End
    
    %% Extend relations - Podpowiedź interfejsu
    A --> I[Podpowiedź interfejsu]
    I -.->|jeśli użytkownik nie wybiera przez określony czas| B
    
    style Start fill:#e1f5fe
    style End fill:#f3e5f5
    style G fill:#fff3e0
    style H fill:#ffebee
    style I fill:#f1f8e9
```

### WYBÓR JĘZYKA

```mermaid
flowchart TD
    Start([Użytkownik uruchamia biletomat]) --> A[Rozpoczęcie interakcji]
    A --> B[Wyświetlenie opcji języka]
    B --> C[Wybór języka]
    C --> D[Dostosowanie interfejsu]
    D --> End([Kontynuacja z wybranym językiem])
    
    %% Include relations - Domyślny język
    A --> E[Domyślny język]
    E --> B
    
    %% Include relations - Anulowanie transakcji (dostępne w dowolnym momencie)
    A --> F[Anulowanie transakcji]
    B --> F
    C --> F
    D --> F
    F --> End
    
    %% Extend relations - Lista popularnych języków
    B --> G[Lista popularnych języków]
    G -.->|opcjonalnie| C
    
    style Start fill:#e1f5fe
    style End fill:#f3e5f5
    style E fill:#fff3e0
    style F fill:#ffebee
    style G fill:#f1f8e9
```

## DIAGRAMY SEKWENCJI

### SZYBKI WYBÓR RODZAJU BILETU
```mermaid
sequenceDiagram
    participant U as Uzytkownik
    participant S as System_Biletomat

    U->>S: Rozpoczecie interakcji
    S-->>U: Wyswietlenie ekranu startowego

    U->>S: Wybor kategorii biletu
    S-->>U: Wyswietlenie listy biletow

    U->>S: Wybor konkretnego biletu
    S-->>U: Wyswietlenie podsumowania

    U->>S: Potwierdzenie wyboru
    S-->>U: Rozpoczecie realizacji transakcji

    alt Anulowanie transakcji
        U->>S: Anuluj proces
        S-->>U: Zakonczenie interakcji
    end
```

### DIAGRAM SEKWENCJI DLA PRZYPADKU UŻYCIA "WYBÓR JĘZYKA"

**AKTOR:** UŻYTKOWNIK  
**OBIEKTY:** INTERFEJS BILETOMATU, SYSTEM JĘZYKOWY, BAZA JĘZYKÓW, KONTROLER ANULOWANIA

```mermaid
sequenceDiagram
    participant U as Użytkownik
    participant IB as Interfejs Biletomatu
    participant SJ as System Językowy  
    participant BJ as Baza Języków
    participant KA as Kontroler Anulowania
    
    U->>IB: Uruchomienie biletomatu (dotknięcie ekranu)
    IB->>SJ: Żądanie ekranu powitalnego
    SJ->>BJ: Pobranie domyślnego języka
    BJ-->>SJ: Konfiguracja języka domyślnego
    SJ-->>IB: Dane ekranu powitalnego
    IB-->>U: Wyświetlenie ekranu powitalnego z opcjami języka
    
    Note over IB: Dostępne opcje: języki + anuluj
    
    alt Wybór języka
        U->>IB: Wybór preferowanego języka
        IB->>SJ: Żądanie zmiany na wybrany język
        SJ->>BJ: Weryfikacja dostępności języka
        
        alt Język dostępny
            BJ-->>SJ: Potwierdzenie + dane językowe
            SJ->>SJ: Ustawienie domyślnego języka
            SJ-->>IB: Konfiguracja nowego interfejsu
            IB->>IB: Dostosowanie interfejsu do wybranego języka
            IB-->>U: Wyświetlenie interfejsu w nowym języku
            Note over U, IB: Użytkownik może kontynuować w wybranym języku
            
        else Język niedostępny
            BJ-->>SJ: Błąd - język niedostępny  
            SJ-->>IB: Komunikat o błędzie
            IB-->>U: Wyświetlenie komunikatu błędu
            IB->>IB: Powrót do listy języków
        end
        
    else Anulowanie transakcji
        U->>IB: Wybór opcji "Anuluj"
        IB->>KA: Żądanie anulowania procesu
        KA->>KA: Przetworzenie żądania anulowania
        KA-->>IB: Potwierdzenie anulowania
        IB-->>U: Wyświetlenie komunikatu o anulowaniu
        IB->>IB: Reset interfejsu do ekranu głównego
        Note over IB: Powrót do stanu początkowego
        
    else Lista popularnych języków (extend)
        IB->>SJ: Żądanie listy popularnych języków
        SJ->>BJ: Pobranie statystyk użycia języków
        BJ-->>SJ: Lista posortowana według popularności
        SJ-->>IB: Top 5 najpopularniejszych języków
        IB-->>U: Wyświetlenie popularnych języków na górze
        Note over U: Opcja "Zobacz więcej języków" dostępna
        
        opt Rozwinięcie pełnej listy
            U->>IB: "Zobacz więcej języków"
            IB->>SJ: Żądanie pełnej listy języków
            SJ->>BJ: Pobranie wszystkich dostępnych języków
            BJ-->>SJ: Kompletna lista języków
            SJ-->>IB: Pełna lista języków
            IB-->>U: Wyświetlenie wszystkich dostępnych języków
        end
    end
```

### DIAGRAMY KLAS
Na podstawie diagramu sekwencji dla przypadku użycia "Wybór języka" zidentyfikowano następujące klasy odpowiedzialne za realizację funkcjonalności:

### ZIDENTYFIKOWANE KLASY:

- **UZYTKOWNIK** - reprezentuje osobę korzystającą z biletomatu i jej preferencje językowe
- **INTERFEJSBILETOMATU** - obsługuje wyświetlanie ekranu powitalnego i opcji językowych
- **SYSTEMJEKOWY** - zarządza dostępnymi językami i ich konfiguracjami
- **BAZAJEZYKOW** - przechowuje wszystkie dane językowe i statystyki popularności
- **KONTROLERANULOWANIA** - obsługuje proces anulowania transakcji
- **KONFIGURACYJEKOWA** - zawiera ustawienia i tłumaczenia dla konkretnego języka

```mermaid
classDiagram
    class Uzytkownik {
        -String id
        -String preferencjeJezykowe
        -Boolean czyAutentykowany
        -List~String~ historiaWyborow
        +void uruchomBiletomat()
        +String wybierzJezyk(List~String~ dostepneJezyki)
        +void wybierzAnuluj()
        +void zapiszuPreferencje(String jezyk)
        +Boolean potwierdzWybor(String jezyk)
    }
    
    class InterfejsBiletomatu {
        -String stanInterfejsu
        -String aktualnyJezyk
        -List~String~ wyswietlaneJezyki
        -Boolean czyEkranPowitalny
        +void wyswietlEkranPowitalny()
        +void wyswietlOpcjeJezykowe(List~String~ jezyki)
        +void wyswietlKomunikat(String komunikat)
        +void dostosowujInterfejs(String jezyk)
        +void resetDoGlownego()
        +void obslugaNulowania()
    }
    
    class SystemJezykowy {
        -String domyslnyJezyk
        -Map~String, KonfiguracjaJezykowa~ dostepneJezyki
        -List~String~ popularneJezyki
        +List~String~ pobierzEkranPowitalny()
        +Boolean weryfikujDostepnosc(String jezyk)
        +void ustawDomyslnyJezyk(String jezyk)
        +KonfiguracjaJezykowa pobierzKonfiguracje(String jezyk)
        +List~String~ pobierzPopularneJezyki()
    }
    
    class BazaJezykow {
        -Map~String, KonfiguracjaJezykowa~ konfiguracjeJezykowe
        -Map~String, Integer~ statystykiPopularnosci
        -List~String~ wspieraneJezyki
        +KonfiguracjaJezykowa pobierzDaneJezykowe(String jezyk)
        +Boolean sprawdzDostepnosc(String jezyk)
        +List~String~ pobierzWszystkieJezyki()
        +List~String~ pobierzPopularne(Int limit)
        +void aktualizujStatystyki(String jezyk)
    }
    
    class KontrolerAnulowania {
        -Boolean czyAktywny
        -String stanProcesu
        -List~String~ dozwoloneAkcje
        +void przetworzZadanieAnulowania()
        +void potwierdzAnulowanie()
        +void resetInterfejsu()
        +Boolean sprawdzStanProcesu()
        +void wyswietlKomunikatAnulowania()
    }
    
    class KonfiguracjaJezykowa {
        -String kodJezyka
        -String nazwaJezyka
        -Map~String, String~ tlumaczenia
        -Boolean czyDostepny
        +String pobierzTlumaczenie(String klucz)
        +Boolean czyKompletna()
        +void ustawDostepnosc(Boolean status)
    }

    Uzytkownik --> InterfejsBiletomatu : współdziała
    InterfejsBiletomatu --> SystemJezykowy : zarządza językami
    SystemJezykowy --> BazaJezykow : pobiera dane językowe
    BazaJezykow *-- KonfiguracjaJezykowa : zawiera
    InterfejsBiletomatu --> KontrolerAnulowania : obsługuje anulowanie
    Uzytkownik ..> KontrolerAnulowania : może aktywować
```
