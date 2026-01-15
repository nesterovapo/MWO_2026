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

