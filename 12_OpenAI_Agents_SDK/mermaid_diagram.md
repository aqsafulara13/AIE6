# Agent System Flow Diagram

```mermaid
graph TD
    A[User Query] --> B[ResearchManager]
    B --> C[Planner Agent]
    C -->|Generates Search Plan| D[WebSearchPlan]
    D -->|List of Search Items| E[Search Agent]
    E -->|Uses| F[WebSearchTool]
    F -->|Search Results| E
    E -->|Summarized Results| G[Writer Agent]
    G -->|Final Report| H[Output]
    H -->|Contains| I[Short Summary]
    H -->|Contains| J[Markdown Report]
    H -->|Contains| K[Follow-up Questions]

    subgraph "Agent System"
        C
        E
        G
    end

    subgraph "Tools"
        F
    end

    subgraph "Output Components"
        I
        J
        K
    end

    style A fill:#f9f,stroke:#333,stroke-width:2px
    style B fill:#bbf,stroke:#333,stroke-width:2px
    style C fill:#bfb,stroke:#333,stroke-width:2px
    style E fill:#bfb,stroke:#333,stroke-width:2px
    style G fill:#bfb,stroke:#333,stroke-width:2px
    style F fill:#fbb,stroke:#333,stroke-width:2px
    style H fill:#fbf,stroke:#333,stroke-width:2px
```

## Diagram Explanation

The diagram shows:

1. **Flow Direction**: Top to bottom showing the progression from user query to final output
2. **Components**:
   - User Query (pink)
   - ResearchManager (blue)
   - Agents (green)
   - Tools (red)
   - Output (purple)
3. **Subgraphs** to group related components:
   - Agent System (contains all three agents)
   - Tools (contains the WebSearchTool)
   - Output Components (contains the three parts of the final report)

The arrows show the data flow between components, with labels indicating what's being passed between them. 