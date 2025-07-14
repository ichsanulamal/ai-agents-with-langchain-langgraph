# Project Instructions

Create a LangGraph workflow that consists of nodes and edges that allow for the following functionality:

* [ ] Ask the patient what health topic or medical condition they'd like to learn about.
* [ ] Have the agent search Tavily using the LangChain community tool for Tavily, focusing on reputable medical sources.
* [ ] Have the agent summarize the Tavily results in patient-friendly language.
* [ ] Present the summarization to the patient and allow them to read it.
* [ ] Prompt the patient to indicate they are ready to take a comprehension check.
* [ ] Have the agent create a one question quiz based on the summarization of the health information.
* [ ] Present the quiz question to the patient.
* [ ] Prompt the patient to enter the answer to the quiz question.
* [ ] Have the agent grade the quiz question. The agent should respond with a grade, as well as a justification for why the grade was given, which should include citations from the health information summary.
* [ ] Present the grade and feedback to the patient.
* [ ] Ask the patient if they'd like to learn about a new health topic or if they want to exit the HealthBot session.
* [ ] Start the flow over again or exit. If the flow is started over again, you will need to reset the state so previous health information isn't carried over to maintain patient privacy and accuracy.

For patient input, you should use Jupyter notebook's `input` function, which will display a modal input with a description and text input to the patient. For displaying output to the patient, you can use the `print` function.


## **Conceptual Level DFD (Level 0)**

```mermaid
flowchart TD
    %% External Entities
    Patient[Patient/User]
    
    %% Main Process
    HealthBot[HealthBot AI Education System]
    
    %% External Data Sources
    WebKnowledge[(Web Knowledge Base)]
    
    %% Data Flows
    Patient -->|Learning Topic Request| HealthBot
    HealthBot -->|Search Query| WebKnowledge
    WebKnowledge -->|Search Results| HealthBot
    HealthBot -->|Educational Summary| Patient
    HealthBot -->|Quiz Question| Patient
    Patient -->|Quiz Answer| HealthBot
    HealthBot -->|Grade & Feedback| Patient
    
    class Patient entity
    class HealthBot process
    class WebKnowledge datastore
```

This high-level view shows:
- **Patient/User** as the external entity
- **HealthBot AI Education System** as the single main process
- **Web Knowledge Base** as the external data source
- Basic data flows showing the overall system interaction

## **Logical Level DFD (Level 1)**

```mermaid
flowchart TD
    %% External Entities
    Patient[Patient/User]
    
    %% Level 1 Processes
    P1["1.0 Search & Retrieve Information"]
    P2["2.0 Generate Educational Summary"]
    P3["3.0 Create Quiz Questions"]
    P4["4.0 Evaluate User Response"]
    P5["5.0 Manage Learning Session"]
    
    %% Data Stores
    D1[(D1: Session State)]
    D2[(D2: Learning Content)]
    
    %% External Systems
    TavilyAPI[Tavily Search API]
    OpenAI[OpenAI API]
    
    %% Data Flows
    Patient -->|1. Learning Topic| P5
    P5 -->|2. Search Request| P1
    P1 -->|3. Search Query| TavilyAPI
    TavilyAPI -->|4. Raw Results| P1
    P1 -->|5. Search Results| D2
    P1 -->|6. Retrieved Data| P2
    
    P2 -->|7. Summarization Request| OpenAI
    OpenAI -->|8. Generated Summary| P2
    P2 -->|9. Educational Summary| D2
    P2 -->|10. Summary| Patient
    
    P5 -->|11. Quiz Request| P3
    D2 -->|12. Summary Content| P3
    P3 -->|13. Quiz Generation Request| OpenAI
    OpenAI -->|14. Quiz Question| P3
    P3 -->|15. Quiz Question| D1
    P3 -->|16. Quiz Question| Patient
    
    Patient -->|17. Quiz Answer| P4
    D1 -->|18. Quiz Context| P4
    D2 -->|19. Reference Content| P4
    P4 -->|20. Grading Request| OpenAI
    OpenAI -->|21. Grade & Feedback| P4
    P4 -->|22. Evaluation Results| D1
    P4 -->|23. Grade & Feedback| Patient
    
    Patient -->|24. Session Choice| P5
    D1 -->|25. Session Data| P5
    P5 -->|26. Session Control| P1
    P5 -->|27. Session Control| P2
    P5 -->|28. Session Control| P3
    P5 -->|29. Session Control| P4
    
    %% Styling
    classDef entity fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef process fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    classDef datastore fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    classDef external fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    
    class Patient entity
    class P1,P2,P3,P4,P5 process
    class D1,D2 datastore
    class TavilyAPI,OpenAI external
```

This detailed view breaks down the main process into five core processes:

### **Core Processes:**
1. **Search & Retrieve Information (1.0)** - Handles Tavily API calls and data retrieval
2. **Generate Educational Summary (2.0)** - Uses OpenAI to create patient-friendly summaries
3. **Create Quiz Questions (3.0)** - Generates assessment questions based on summaries
4. **Evaluate User Response (4.0)** - Grades answers and provides feedback
5. **Manage Learning Session (5.0)** - Controls workflow and user choices

### **Data Stores:**
- **D1: Session State** - Maintains current session data, quiz context, and user progress
- **D2: Learning Content** - Stores retrieved information and generated summaries

### **External Systems:**
- **Tavily Search API** - Provides web search capabilities
- **OpenAI API** - Handles AI-powered text generation and analysis

### **Key Data Flows:**
- 29 numbered data flows showing the complete information exchange
- Clear separation between user interactions, API calls, and internal data management
- Feedback loops for session management and workflow control

This logical level demonstrates how the LangGraph nodes, state management, and API integrations work together to deliver the educational experience described in your project requirements.