# Aktor: Użytkownik

## Historyjki użytkownika

### Historia 1
Jako użytkownik, chcę płacić za bilet kartą, gotówką lub telefonem, aby mieć większą elastyczność w wyborze metody płatności.

### Historia 2
Jako użytkownik, chcę otrzymać wyraźne instrukcje na ekranie, aby wiedzieć, jak dokonać zakupu krok po kroku.

### Historia 3
Jako użytkownik, chcę widzieć czas pozostały na decyzję (np. wyświetlany licznik czasu), aby móc szybko podjąć działanie.

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

## Wspólny diagram przypadków użycia

### DIAGRAM PRZYPADKÓW UŻYCIA - UŻYTKOWNIK

```mermaid
flowchart TD
    %% Wspólny start
    MainStart([Użytkownik podchodzi do biletomatu]) --> Choice{Wybór akcji}
    
    %% DIAGRAM1 - Szybki wybór rodzaju biletu (Osoba 1)
    Choice -->|Zakup biletu| Start1[Rozpoczęcie interakcji - zakup]
    Start1 --> A1[Wybór kategorii]
    A1 --> B1[Wybór biletu]
    B1 --> C1[Wyświetlenie podsumowania]
    C1 --> D1[Potwierdzenie wyboru]
    D1 --> E1[Realizacja transakcji]
    E1 --> End1([Zakończenie procesu zakupu])
    
    %% DIAGRAM2 - Wybór języka (Osoba 2)
    Choice -->|Zmiana języka| Start2[Rozpoczęcie interakcji - język]
    Start2 --> A2[Wyświetlenie opcji języka]
    A2 --> B2[Wybór języka]
    B2 --> C2[Dostosowanie interfejsu]
    C2 --> End2([Kontynuacja z wybranym językiem])
    
    %% Połączenie diagramów
    End2 --> Choice
    
    %% Include relations z DIAGRAM1
    A1 --> Include1[Sprawdzenie biletów]
    Include1 --> B1
    
    %% Include relations z DIAGRAM2
    Start2 --> Include2[Domyślny język]
    Include2 --> A2
    
    %% Wspólne anulowanie transakcji (występuje w obu)
    Start1 --> Cancel[Anulowanie transakcji]
    A1 --> Cancel
    B1 --> Cancel
    C1 --> Cancel
    Start2 --> Cancel
    A2 --> Cancel
    B2 --> Cancel
    C2 --> Cancel
    Cancel --> MainStart
    
    %% Extend relations z DIAGRAM1
    Start1 --> Extend1[Podpowiedź interfejsu]
    Extend1 -.->|jeśli użytkownik nie wybiera przez określony czas| A1
    
    %% Extend relations z DIAGRAM2
    A2 --> Extend2[Lista popularnych języków]
    Extend2 -.->|opcjonalnie| B2
    
    style MainStart fill:#e1f5fe
    style Choice fill:#fff3e0
    style End1 fill:#f3e5f5
    style End2 fill:#f3e5f5
    style Cancel fill:#ffebee
    style Include1 fill:#fff3e0
    style Include2 fill:#fff3e0
    style Extend1 fill:#f1f8e9
    style Extend2 fill:#f1f8e9
```
