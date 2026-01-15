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
