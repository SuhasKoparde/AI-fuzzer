# System Architecture

```text
USER
  |
  v
FRONTEND
  |
  v
TARGET MANAGER
  |
  v
FUZZING ENGINE
  |-- Prompt Generator
  |-- Prompt Mutation Engine
  `-- Test Execution Engine
  |
  v
TARGET ADAPTER
  |
  v
AUTHORIZED AI TARGET
  |-- LLM API
  |-- RAG Application
  `-- AI Agent
  |
  v
RESPONSE ANALYZER
  |-- Rules / Patterns
  |-- Canary Detection
  `-- Optional LLM Judge
  |
  v
VULNERABILITY ENGINE
  |-- Classification
  |-- Confidence
  |-- Severity
  `-- Evidence
  |
  +------------------+
  |                  |
  v                  v
DATABASE       REPORT GENERATOR
                    |
                    v
              JSON / PDF / DASHBOARD
```

## Design Principle
The core fuzzing engine should be independent of any one AI provider. Target-specific communication is isolated behind adapters.
