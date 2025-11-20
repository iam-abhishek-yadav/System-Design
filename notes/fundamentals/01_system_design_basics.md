## System Design Basics

System design is the process of deciding the overall architecture of a system:
- Identifying the main components and modules
- Defining what each component is responsible for
- Defining how these components interact with each other
- Making sure the whole thing works together to solve a specific problem

In short: What are the building blocks, how do they talk to each other, and why is this a good way to solve the problem?

### How to Approach System Design

```mermaid
flowchart TD
    A[Understand Problem Statement] --> B[Break into Components]
    B --> C[Pick One Component]
    C --> D[Analyze Component Details]
    D --> E{Need Subcomponents?}
    E -->|Yes| F[Create Subcomponents]
    F --> D
    E -->|No| G[Move to Next Component]
    G --> C
    D --> H[Database & Caching]
    D --> I[Scaling & Fault Tolerance]
    D --> J[Asynchronous Processing]
    D --> K[Communication]
```

#### Step 1: Understand the Problem Statement

- What are we trying to build?
- Who is it for?
- What does it need to do (at minimum)?
- What are the constraints (scale, reliability, performance, etc.)?

#### Step 2: Break the Problem into Components

- Split the system into logical parts instead of thinking about it as one big block
- Each component should represent one clear responsibility

#### Step 3: Pick One Component and Go Deeper

- Take one component at a time and figure out how it will actually work
- Do not stay at a high level forever

#### Step 4: Analyze Each Component

For each component, think about the following:

**Database and Caching**
- What data does it store?
- Where is that data stored?
- What needs to be fast and can be cached?

**Scaling and Fault Tolerance**
- What happens when traffic increases?
- What happens when this component fails?

**Asynchronous Processing (Delegation)**
- What work can be handed off instead of done immediately?
- Can slow or heavy work be queued and processed later?

**Communication**
- How does this component talk to other components?
- What data is sent, and in which direction?

#### Step 5: Add Subcomponents if Needed

- While going deeper, you may realize one "component" is actually multiple smaller parts
- Create those smaller parts and repeat the same analysis for each of them

### How to Know You Have Built a Good System

```mermaid
graph TB
    A[System Design Quality Checklist] --> B[Component Breakdown]
    A --> C[Clear Responsibilities]
    A --> D[Detailed Design]
    A --> E[Solid Components]
    
    B --> B1[Not one big block]
    B --> B2[Each piece identifiable]
    
    C --> C1[No random extra work]
    C --> C2[Describable in 1-2 sentences]
    
    D --> D1[Data storage defined]
    D --> D2[Communication defined]
    D --> D3[Load behavior defined]
    D --> D4[Failure handling defined]
    
    E --> E1[Scalable]
    E --> E2[Fault-tolerant]
    E --> E3[Available]
```

#### 1. Component Breakdown

- The system is broken down into components
- The design is not just one big block that "does everything"
- You can point to each piece and say what it is

#### 2. Clear Responsibilities

- Each component has a clear set of responsibilities
- No component is doing random extra work it should not be doing
- You can describe any component in one or two sentences

#### 3. Detailed Design

For each component, details are figured out:
- Where does it store data?
- How does it communicate?
- How does it behave under load?
- What happens if it crashes?

#### 4. Solid Components

Each component, in isolation, is solid:
- **Scalable**: It can handle growth
- **Fault-tolerant**: It can handle failures or recover from them
- **Available**: It can keep working without causing the whole system to go down

If all of the above is true, the system design is on the right track.
