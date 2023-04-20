# Flowchart
- [Matt Eland's Flow example](https://newdevsguide.com/2023/04/15/mermaid-flowcharts/)

> building out a simple flowchart illustrating a REST request that flows from the client to the server, is fulfilled by the database, and then returns back to the client.

### Relationships
```
    Client --> Server
    Server --> Database
```

**Connection Shapes**
- `-->` solid line
- `-.->` to represent a dotted arrow

**Left to Right**: `flowchart LR`

# multiple connections to each shape

```mermaid
flowchart LR
    Client --> Server
    Server --> Database
    Database -.-> Server
    Server -.-> Client
```

# Aliases and Labels
- `c[Client]`
- `c -- HTTP GET --> s`

```mermaid
flowchart LR
    c[Client]
    s[Server]
    db[Database]
    
    c -- HTTP GET --> s
    s -- SQL Query --> db
    db -. Result Set .-> s
    s -. JSON .-> c
```

# Additional Connector Types
- Arrow, Heavy, Doted, Line
-  Also, **multiple relationships** in a single line via **chaining**

```mermaid
flowchart LR
    Base --> Arrow
    Base ==> Heavy
    Base -.-> Dotted
    Base --- Line
    Base --> You --> Can --> Chain --> Relations --> On --> One --- Line
```

# Flowchart Shapes
```mermaid
flowchart
    a[Default]
    b([Rounded])
    c[(Database)]
    d[[Subroutine]]
    e((Circle))
    f>Note]
    g{Decision}
    h{{Hexagon}}
    i[/Parallelogram/]
    j(((Double Circle)))
```

# Subgraphs
- with two font-awesome icons, `fa:fa-code Server` & `fa:fa-user Client`
```mermaid
flowchart LR
    subgraph Azure
        s[fa:fa-code Server]
        db[(fa:fa-table Database)]
    end
    subgraph Netlify
        c[fa:fa-user Client]
    end

    subgraph Netlify
    end
    subgraph Azure
        direction LR
    end
    
    c -- HTTP GET --> s
    s -- SQL Query --> db
    db -. Result Set .-> s
    s -. JSON .-> c
```

