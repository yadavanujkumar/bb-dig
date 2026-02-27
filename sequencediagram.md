---
id: project-sequence-diagram
---

```mermaid
sequenceDiagram
    participant User
    participant System
    participant PDFLoader
    participant AI_Agent
    participant OutputStore

    User->>System: Upload PDF or payload
    System->>PDFLoader: Validate and load PDF
    PDFLoader-->>System: PDF bytes
    System->>AI_Agent: Extract text, invoke model
    AI_Agent-->>System: Extraction result
    System->>System: Validate extraction
    alt Extraction valid
        System->>OutputStore: Save output as JSON
    else Extraction invalid
        System->>System: Retry extraction
        System->>AI_Agent: Retry extraction
        AI_Agent-->>System: Retry result
        System->>System: Validate retry
        alt Retry valid
            System->>OutputStore: Save output as JSON
        else Still invalid
            System->>OutputStore: Flag for manual review
        end
    end
    OutputStore-->>User: Output/flagged result
```
