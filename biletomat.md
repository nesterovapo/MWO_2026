# Aktor: Biletomat

## DIAGRAMY PRZYPADKÓW UŻYCIA

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
