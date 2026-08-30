# 04 — AI Security Attack Taxonomy

## 1. Purpose

This document defines the initial vulnerability categories that will be tested by the Intelligent Black-Box Fuzzer.

The taxonomy is based on the security areas identified during the literature review.

The initial implementation focuses on four core vulnerability categories:

1. Prompt Injection
2. Jailbreak
3. Context Poisoning
4. Data Leakage

Additional categories will be implemented after the core framework is validated.

---

# 2. Attack Classification

| ID | Vulnerability | Primary Target | Initial Priority |
|----|---------------|-----------------|------------------|
| V01 | Prompt Injection | LLM / Chatbot / Agent | High |
| V02 | Jailbreak | LLM / Chatbot | High |
| V03 | Context Poisoning | RAG / Agent | High |
| V04 | Data Leakage | LLM / RAG / Agent | High |
| V05 | Indirect Prompt Injection | RAG / Agent | Future |
| V06 | RAG Poisoning | RAG | Future |
| V07 | Tool Misuse | AI Agent | Future |
| V08 | Agent Security | AI Agent | Future |

---

# 3. V01 — Prompt Injection

## Description

Prompt injection occurs when attacker-controlled instructions influence the behavior of an AI application in a way that conflicts with its intended instructions or security policy.

The testing objective is to determine whether untrusted input can override or manipulate the application's intended behavior.

## Target

- LLM APIs
- AI chatbots
- RAG applications
- AI agents

## Testing Strategy

The fuzzer will generate controlled prompt-injection test cases.

The generated cases may vary through:

- Instruction restructuring
- Paraphrasing
- Context variation
- Multi-turn interactions
- Encoding variations

## Detection

The response analyzer will compare the target behavior against the expected security policy.

Potential indicators include:

- Following attacker-controlled instructions
- Revealing protected test information
- Ignoring defined restrictions
- Performing an unauthorized test action

## Evidence

The system will record:

- Test case
- Mutated test case
- Target response
- Detection reason
- Timestamp

---

# 4. V02 — Jailbreak

## Description

Jailbreak testing evaluates whether adversarial prompts can cause a safety-aligned model to bypass its intended safety restrictions.

Research reviewed in this project includes automated jailbreak techniques such as PAIR, MASTERKEY, Jailbreak Mimicry, and other adaptive approaches.

## Target

Primarily:

- LLM APIs
- AI chatbots

## Testing Strategy

The fuzzer will use controlled variations of test prompts.

Mutation strategies may include:

- Paraphrasing
- Contextual reframing
- Multi-turn variations
- Instruction transformations
- Other research-inspired test transformations

The project will evaluate whether the target maintains its expected safety behavior.

## Detection

A potential jailbreak finding may be generated when the target produces behavior that violates the defined test policy.

The system should avoid treating every unusual response as a confirmed vulnerability.

Findings should contain confidence information.

---

# 5. V03 — Context Poisoning

## Description

Context poisoning occurs when manipulated or untrusted contextual information influences the AI system's output or behavior.

This is particularly relevant to:

- RAG applications
- AI agents
- Systems processing external documents
- Systems using retrieved information

## Testing Strategy

The initial implementation will use controlled test context rather than unauthorized modification of real-world knowledge bases.

Example test workflow:

    Controlled Context
          ↓
    Add Test Manipulation
          ↓
    Send Context to Target
          ↓
    Observe Response
          ↓
    Compare With Expected Behavior

## Detection

The analyzer will determine whether manipulated context causes:

- Incorrect instruction following
- Unexpected behavior
- Security-policy violations
- Exposure of synthetic test information

---

# 6. V04 — Data Leakage

## Description

Data leakage testing evaluates whether an AI application exposes information that it should not reveal.

The project will primarily use synthetic sensitive information and canary values during development.

## Example Canary

A unique synthetic value may be inserted into a controlled test context.

Example:

    CANARY-AIFUZZER-7F29X

The scanner can then determine whether the value appears unexpectedly in the target response.

## Testing Strategy

The testing workflow is:

    Insert Synthetic Canary
            ↓
    Execute Test
            ↓
    Collect Response
            ↓
    Search for Canary
            ↓
    Determine Exposure
            ↓
    Generate Finding

## Detection

Potential indicators include:

- Canary exposure
- Unexpected reproduction of test data
- Sensitive test-context disclosure
- Unauthorized information disclosure

Real personal information should not be used during normal development testing.

---

# 7. V05 — Indirect Prompt Injection

## Status

Future Extension

## Description

Indirect prompt injection occurs when malicious instructions are embedded in external content consumed by an AI application.

Examples of external content include:

- Web pages
- Documents
- Emails
- Retrieved content

## Target

- RAG applications
- Web agents
- AI assistants

## Planned Testing

The framework may introduce controlled malicious instructions into an authorized test dataset and determine whether the AI follows those instructions.

---

# 8. V06 — RAG Poisoning

## Status

Future Extension

## Description

RAG poisoning involves manipulating information available to a retrieval system so that the generated response is influenced by malicious or incorrect content.

Research such as PoisonedRAG demonstrates the security risks associated with knowledge-base corruption.

## Planned Testing

The project will initially use a controlled local RAG environment.

The scanner will evaluate:

- Retrieval behavior
- Context influence
- Generated output
- Poisoned-content influence

---

# 9. V07 — Tool Misuse

## Status

Future Extension

## Description

AI agents can interact with external tools and APIs.

Tool misuse testing evaluates whether adversarial input can cause an agent to invoke a tool incorrectly or outside its intended purpose.

## Target

- AI agents
- Tool-enabled LLM systems

## Planned Testing

Testing will use controlled mock tools.

Examples:

- Mock database
- Mock email service
- Mock file system
- Mock API

No destructive real-world action should be performed.

---

# 10. V08 — Agent Security

## Status

Future Extension

## Description

Agent security testing evaluates security risks arising from the interaction between:

- LLM
- Memory
- Tools
- External data
- System instructions
- Other agents

Research reviewed in this project includes AgentDojo, Agent Security Bench, MCPTox, and related agent-security work.

## Planned Testing

The project may evaluate:

- Memory manipulation
- Tool selection
- Instruction hierarchy
- Unauthorized actions
- Cross-component influence

Testing will use controlled agent environments.

---

# 11. Test Case Structure

Every generated test case should follow a common structure.

```text
Test Case
│
├── Test ID
├── Vulnerability ID
├── Target ID
├── Original Prompt
├── Mutation Technique
├── Mutated Prompt
├── Expected Behavior
├── Actual Response
├── Detection Result
├── Confidence
└── Evidence
