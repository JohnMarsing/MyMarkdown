# Entity Relationship Diagram
- [Matt Eland's example](https://newdevsguide.com/2023/04/08/creating-erds-with-mermaid/)

```mermaid
erDiagram
    CITY {
        int city_id PK
        string name
        string state_abbreviation FK
    }
    STATE {
        string state_abbreviation PK
        string name
        int country_id FK
    }
    COUNTRY {
        int country_id PK
        string name
    }
    COUNTRY ||--|{ STATE : "Has"
    STATE ||--|{ CITY : "Has"
    CITY ||--o| STATE : "Is capital of"
    CITY ||--o| COUNTRY : "Is capital of"
```
