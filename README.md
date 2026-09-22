# Travel_Reimbursement_Agent
Travel_Reimbursement_Agent_HCL

# Travel Reimbursement Approval Agent
 
Stack: Python, LangChain, LangGraph, Groq, FAISS, Pydantic, LangSmith, optional MCP

This notebook implements the supplied assignment as a small, runnable prototype. It includes policy RAG, five tools, deterministic rule evaluation, structured output, guardrails, Manual Review/HITL(Human In The Loop), auditability, dashboard, evaluation.

### Architecture

```text
Claim
  ↓
Input validation / guardrails
  ↓
LangGraph workflow
  ├── Policy RAG → FAISS
  ├── Receipt checker
  ├── Category-limit checker
  ├── Approval-threshold checker
  └── Timeliness checker
  ↓
Deterministic rule evaluation
  ├── clear result → finalizer
  └── exception/uncertainty → Manual Review / HITL
  ↓
Exact structured JSON
  ↓
Dashboard + LangSmith audit/trace
```

**Key design principle:
 LLM for interpretation/orchestration/explanation
 deterministic Python handles arithmetic, limits, receipts, dates and schema validation.

 ### LangGraph:
The workflow has explicit stages, conditional routing and a Manual Review boundary.
We can create complex workflows also.

### RAG:
The decision must be grounded in the supplied policy, and stable `POL-*` IDs make the result auditable.

### FAISS:
Local, free and reproducible for the assignment. Pinecone is a valid alternative behind the retriever interface.

### deterministic tools:
Arithmetic, category limits, dates and receipt requirements are deterministic business rules and should not be delegated to an LLM.

### HITL (HUman in the loop)
The policy explicitly requires Manual Review for missing required receipts, business/first-class airfare, high-value claims, late claims and ambiguity.

### structured output using Pydantic:
Pydantic enforces the exact output contract requested by the assignment.

### LangSmith: 
It provides observability for debugging and demonstration.



### Areas for Improvement / Production-Grade Enhancements

1. **Replace FAISS with a Production-Grade Vector Database**
   Instead of FAISS, we can use **Pinecone** or another managed vector database to provide better scalability, persistence, and production-ready infrastructure.

2. **Use an Alternative LLM for Lower Latency**
   The current implementation uses the **Groq API with a free-tier LLM**. For production deployment, we can evaluate other LLM providers or models to achieve lower latency, higher reliability, and better scalability.

3. **Evaluate Alternative Embedding Models**
   The current implementation uses an **open-source Hugging Face embedding model**. Depending on the application requirements, it can be replaced with other embedding models to improve retrieval accuracy, performance, or scalability.

4. **Implement a Centralized Logging Module**
   A dedicated **logging module** should be implemented to capture application events, errors, warnings, and debugging information. This will make monitoring and troubleshooting easier, particularly in a production environment.

5. **Convert the Jupyter Notebook into Modular Python Code**
   Instead of using a `.ipynb` notebook for the complete implementation, the application should be organized into **modular `.py` files** with clearly separated components for data processing, embedding, retrieval, LLM interaction, API handling, and configuration. This approach improves maintainability, testing, scalability, and production readiness.
