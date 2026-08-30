# 05 — System Architecture

## 1. Architecture Overview

The Intelligent Black-Box Fuzzer is designed as a modular security-testing framework.

The system accepts an authorized AI target, generates security test cases, executes them through a target adapter, analyzes the responses, identifies potential vulnerabilities, and produces structured security reports.

The architecture is designed so that the core fuzzing engine is independent of a particular AI provider or application.

---

# 2. High-Level Architecture

```text
                         +----------------------+
                         |        USER          |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         |      FRONTEND        |
                         | Dashboard / Reports  |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         |   TARGET MANAGER     |
                         | Target Configuration |
                         +----------+-----------+
                                    |
                                    v
                    +---------------+---------------+
                    |                               |
                    v                               v
          +-------------------+           +-------------------+
          |  FUZZING ENGINE   |           | SCAN MANAGER     |
          +---------+---------+           +-------------------+
                    |
          +---------+---------+
          |                   |
          v                   v
+-------------------+  +-------------------+
| Prompt Generator  |  | Prompt Mutation   |
|                   |  | Engine            |
+---------+---------+  +---------+---------+
          |                     |
          +----------+----------+
                     |
                     v
          +----------------------+
          |  TEST EXECUTION      |
          |      ENGINE          |
          +----------+-----------+
                     |
                     v
          +----------------------+
          |   TARGET ADAPTER     |
          +----------+-----------+
                     |
          +----------+----------+
          |          |           |
          v          v           v
       +-----+    +-----+     +-------+
       | LLM |    | RAG |     | Agent |
       | API |    | App |     |       |
       +--+--+    +--+--+     +---+---+
          |          |             |
          +----------+-------------+
                     |
                     v
          +----------------------+
          | RESPONSE COLLECTOR   |
          +----------+-----------+
                     |
                     v
          +----------------------+
          | RESPONSE ANALYZER    |
          +----------+-----------+
                     |
          +----------+----------+
          |          |           |
          v          v           v
      Rule-Based  Canary     Optional
      Analysis    Detection  LLM Judge
          |          |           |
          +----------+-----------+
                     |
                     v
          +----------------------+
          | VULNERABILITY ENGINE |
          +----------+-----------+
                     |
          +----------+----------+
          |          |           |
          v          v           v
       Finding   Severity     Confidence
          |          |           |
          +----------+-----------+
                     |
                     v
          +----------------------+
          |   EVIDENCE STORE     |
          +----------+-----------+
                     |
              +------+------+
              |             |
              v             v
       +-------------+ +-------------+
       |  DATABASE   | |   REPORT    |
       |             | |  GENERATOR  |
       +-------------+ +------+------+
                              |
                    +---------+---------+
                    |                   |
                    v                   v
                 JSON/PDF           DASHBOARD
