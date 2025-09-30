```mermaid
graph TD
    Start([Initial Request]) --> Atomizer{Atomizer:<br/>Is task atomic?}
    
    %% Atomic Path
    Atomizer -->|Yes: Atomic| Executor[Executor:<br/>Execute atomic task]
    Executor --> Return1([Return Result])
    
    %% Non-Atomic Path
    Atomizer -->|No: Complex| Planner[Planner:<br/>Break into subtasks]
    Planner --> SubtaskList[Generate Subtasks<br/>subtask1, subtask2, ..., subtaskN]
    SubtaskList --> Recursive[Recursive Solve Loop]
    
    %% Recursive Processing
    Recursive --> SubAtomizer1{Atomizer:<br/>subtask1 atomic?}
    Recursive --> SubAtomizer2{Atomizer:<br/>subtask2 atomic?}
    Recursive --> SubAtomizerN{Atomizer:<br/>subtaskN atomic?}
    
    %% Subtask 1 - Atomic
    SubAtomizer1 -->|Yes| SubExecutor1[Executor:<br/>Execute subtask1]
    SubExecutor1 --> Result1[Result 1]
    
    %% Subtask 1 - Non-Atomic
    SubAtomizer1 -->|No| SubPlanner1[Planner:<br/>Break subtask1]
    SubPlanner1 --> Recursive1[Recursive<br/>solve...]
    Recursive1 -.->|Further recursion| Result1
    
    %% Subtask 2 - Atomic
    SubAtomizer2 -->|Yes| SubExecutor2[Executor:<br/>Execute subtask2]
    SubExecutor2 --> Result2[Result 2]
    
    %% Subtask 2 - Non-Atomic
    SubAtomizer2 -->|No| SubPlanner2[Planner:<br/>Break subtask2]
    SubPlanner2 --> Recursive2[Recursive<br/>solve...]
    Recursive2 -.->|Further recursion| Result2
    
    %% Subtask N - Atomic
    SubAtomizerN -->|Yes| SubExecutorN[Executor:<br/>Execute subtaskN]
    SubExecutorN --> ResultN[Result N]
    
    %% Subtask N - Non-Atomic
    SubAtomizerN -->|No| SubPlannerN[Planner:<br/>Break subtaskN]
    SubPlannerN --> RecursiveN[Recursive<br/>solve...]
    RecursiveN -.->|Further recursion| ResultN
    
    %% Dependency Management
    Result1 --> DependencyCheck{Check<br/>Dependencies}
    DependencyCheck -->|subtask2 depends on subtask1| Result2
    Result2 --> DependencyCheck2{Check<br/>Dependencies}
    DependencyCheck2 --> ResultN
    
    %% Aggregation
    ResultN --> Aggregator[Aggregator:<br/>Combine all results]
    Aggregator --> ParentResult([Return Parent Task Result])
    
    %% Styling
    classDef atomicClass fill:#90EE90,stroke:#006400,stroke-width:2px
    classDef nonAtomicClass fill:#FFB6C1,stroke:#8B0000,stroke-width:2px
    classDef decisionClass fill:#87CEEB,stroke:#00008B,stroke-width:2px
    classDef processClass fill:#FFE4B5,stroke:#FF8C00,stroke-width:2px
    classDef resultClass fill:#E6E6FA,stroke:#4B0082,stroke-width:2px
    
    class Executor,SubExecutor1,SubExecutor2,SubExecutorN atomicClass
    class Planner,SubPlanner1,SubPlanner2,SubPlannerN nonAtomicClass
    class Atomizer,SubAtomizer1,SubAtomizer2,SubAtomizerN,DependencyCheck,DependencyCheck2 decisionClass
    class Aggregator,Recursive,Recursive1,Recursive2,RecursiveN processClass
    class Result1,Result2,ResultN,ParentResult,Return1 resultClass
