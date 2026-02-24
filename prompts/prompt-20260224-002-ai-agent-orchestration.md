# Premium Prompt: Multi-Agent Orchestration Architecture Designer

**Category:** AI / Agent Systems / System Design  
**Estimated Price:** $6.99  
**Difficulty Level:** Expert  
**Target Audience:** AI Engineers, Product Managers, Startup CTOs building agent platforms

---

## Description

This prompt specializes in designing sophisticated multi-agent systems where multiple AI agents coordinate to solve complex problems. It moves beyond single-agent systems into orchestration patterns: hierarchical agents, market-based agents, consensus mechanisms, state management across agents, and failure handling. Users describe their problem domain, agent types needed, and scalability requirements—the prompt returns a complete architecture with patterns, Python pseudocode, scaling strategies, and production pitfalls.

**Use Case:** A SaaS company building an autonomous research platform needs 5 agents (researcher, fact-checker, analyst, editor, publisher) that coordinate asynchronously while maintaining consistency. This prompt generates the complete orchestration architecture: message protocols, state machines, conflict resolution, retry logic, and deployment topology.

---

## Prompt Text

```
You are an expert in distributed systems and AI agent design with deep experience building
production multi-agent systems. Your specialty is translating complex coordination problems
into clean, scalable agent orchestration architectures.

## Your Mission

Design a complete multi-agent orchestration system that:
1. Solves a specific business problem through coordinated agents
2. Scales gracefully (from 2 agents to 100+)
3. Handles failures & disagreements intelligently
4. Maintains consistency & auditability
5. Minimizes latency and computational waste

## Input: Problem Definition

**Business Problem:**
[Describe the complex task that requires multiple specialized perspectives/agents]

**Agent Types Needed:**
For each agent, specify:
- Name & role
- Input (what data it consumes)
- Output (what decisions/artifacts it produces)
- Failure modes (what could go wrong)
- Knowledge requirements
- Cost constraints

**Non-Functional Requirements:**
- Latency target (response time)
- Throughput (tasks/second)
- Reliability (uptime SLA)
- Auditability (can we replay decisions?)
- Cost constraints (total spend per task)

**Constraints:**
- Budget for inference ($ per task)
- Preferred LLM providers (OpenAI, Anthropic, Groq, local)
- Existing infrastructure (Kubernetes, Lambda, VMs)
- Data governance (PII sensitivity, retention)
- Compliance requirements (if any)

---

## Output Architecture

### 1. PROBLEM DECOMPOSITION (250-300 words)

**Identify the core coordination challenges:**
- Is this primarily **sequential** (Agent A → B → C pipeline)?
- Or **parallel + consensus** (Multiple agents vote on outcomes)?
- Or **hierarchical** (Manager agent delegates to workers)?
- Or **market-based** (Agents compete/bid for subtasks)?
- Or **hybrid** (Mix of the above)?

**Map dependencies:**
```
[ASCII diagram showing agent relationships and data flow]
```

**Identify critical paths:**
- Which bottlenecks will limit end-to-end latency?
- Where do we need caching or parallelization?
- What's the longest dependency chain?

---

### 2. AGENT DESIGN SPECIFICATIONS (400-500 words)

For each agent, specify:

#### Agent: [Name]
**Purpose:** [One sentence clarity]

**Input Schema:**
```json
{
  "field_name": "description & type",
  "required": true
}
```

**Processing Logic:**
- Task: [What does this agent specifically do?]
- Model choice: [Which LLM or model class? Why?]
- Prompt patterns: [Few-shot examples? Chain of thought? Tools?]
- Token budget: [Estimated input + output tokens]
- Fallback behavior: [If API fails, what happens?]

**Output Schema:**
```json
{
  "decision_or_artifact": "description & type",
  "confidence_score": "0.0-1.0, how certain is this agent?",
  "reasoning": "brief explanation for auditability"
}
```

**Success Criteria:**
- Accuracy metric (how to measure this agent's output quality)
- Latency target (max seconds)
- Cost target (max $ per invocation)

**Failure Modes & Handling:**
- Mode 1: [What could fail] → [Recovery strategy]
- Mode 2: [What could fail] → [Recovery strategy]

---

### 3. ORCHESTRATION PROTOCOL (400-450 words)

**Coordination Model:**

Choose the right pattern for your problem:

#### Sequential Pipeline
```
Agent A (researcher) → Agent B (validator) → Agent C (formatter) → Output
```
- **Advantages:** Simple, deterministic, low latency
- **Disadvantages:** Bottlenecks (slowest agent blocks all others)
- **When to use:** Linear workflows (research → fact-check → publish)
- **Python pseudocode:**

```python
def sequential_orchestration(task):
    result_a = agent_researcher(task)
    if result_a.confidence < 0.7:
        return handle_low_confidence(result_a)
    
    result_b = agent_validator(result_a)
    if not result_b.approved:
        return request_revision(result_a)
    
    result_c = agent_formatter(result_b)
    return result_c
```

#### Parallel + Consensus
```
Agent A ─┐
Agent B ├→ Consensus Engine → Output
Agent C ─┘
```
- **Advantages:** Leverages diverse perspectives, robust to single failures
- **Disadvantages:** Consensus overhead, handling disagreement
- **When to use:** Critical decisions where multiple viewpoints matter (hiring, diagnosis)
- **Python pseudocode:**

```python
def consensus_orchestration(task):
    results = concurrent_map([
        lambda: agent_analyst_1(task),
        lambda: agent_analyst_2(task),
        lambda: agent_analyst_3(task)
    ])
    
    consensus = merge_opinions(results)
    if consensus.agreement < 0.8:
        # Escalate to human or request agent refinement
        return escalate_for_review(results)
    
    return consensus.output
```

#### Hierarchical / Supervisor Pattern
```
        [Supervisor Agent]
       /         |         \
  [Worker1]  [Worker2]  [Worker3]
```
- **Advantages:** Scales to many agents, clear authority
- **Disadvantages:** Single point of failure (supervisor)
- **When to use:** Large-scale parallel work with a "project manager" agent
- **Python pseudocode:**

```python
def hierarchical_orchestration(task):
    supervisor = SupervisorAgent(task)
    subtasks = supervisor.decompose()
    
    # Parallelism at this level
    results = {
        f"subtask_{i}": worker_agent(subtask)
        for i, subtask in enumerate(subtasks)
    }
    
    # Supervisor synthesizes
    output = supervisor.synthesize(results)
    return output
```

#### Market-Based / Auction Pattern
```
[Task Broker] → Agents bid on subtasks → [Allocator] → Results
```
- **Advantages:** Dynamic load balancing, optimal resource allocation
- **Disadvantages:** Overhead of bidding, gaming the system
- **When to use:** Heterogeneous agent capabilities, resource-constrained
- **Python pseudocode:**

```python
def market_orchestration(task):
    subtasks = decompose_task(task)
    broker = TaskBroker(subtasks)
    
    for subtask in subtasks:
        bids = broadcast_request(subtask)  # Agents bid: cost + latency + confidence
        winner = select_best_bid(bids)     # Optimize for speed/cost/quality
        result = winner.execute(subtask)
        broker.record(subtask, result)
    
    return broker.aggregate()
```

**Choose pattern:** [Recommend 1-2 patterns with rationale for this specific problem]

---

### 4. STATE MANAGEMENT & CONSISTENCY (300-350 words)

**State to Track:**
- Task state: (pending → processing → completed → failed → cached)
- Agent outputs: (cache for reuse?)
- Decisions & reasoning: (audit trail)
- Conflicts: (where agents disagreed, how resolved)

**Consistency Guarantees:**
- **Strong consistency:** All agents see latest state before acting (slower, safer)
- **Eventual consistency:** Agents may work with stale state, reconcile later (faster, risky)
- **Causal consistency:** Agents see effects of their own actions, not others' (middle ground)
- **Recommendation for this system:** [Which and why]

**Implementation Pattern:**
```python
class OrchestrationState:
    def __init__(self, task_id):
        self.task_id = task_id
        self.state = "pending"
        self.agent_results = {}
        self.conflicts = []
        self.audit_log = []
    
    def record_agent_output(self, agent_name, output):
        self.agent_results[agent_name] = output
        self.audit_log.append({
            "timestamp": now(),
            "agent": agent_name,
            "output": output
        })
    
    def detect_conflict(self, agent1, agent2):
        # Compare outputs, flag if contradictory
        if self.contradicts(agent1, agent2):
            self.conflicts.append((agent1, agent2))
            return trigger_consensus_protocol()
```

**Durability & Recovery:**
- Persist state to: [Database/File system choice]
- Retry logic: [How many times? Backoff strategy?]
- Resumability: [Can we pick up where we left off?]

---

### 5. CONFLICT RESOLUTION STRATEGIES (250-300 words)

**When agents disagree, how to handle?**

**Strategy 1: Voting / Majority Rules**
- All agents vote, majority wins
- Pros: Simple, democratic, fault-tolerant (survives minority failures)
- Cons: 50/50 ties, ignores agent specialization
- Use when: Equal trust in all agents

**Strategy 2: Expertise Weighting**
- Each agent has a confidence score; weighted vote
- Pros: Respects domain expertise
- Cons: Calibrating weights requires data
- Use when: Agents have different specializations

**Strategy 3: Escalation to Human**
- Unresolved conflicts go to human for decision
- Pros: Maintains safety, captures learning
- Cons: Defeats automation purpose (expensive)
- Use when: High-stakes decisions (hiring, medical)

**Strategy 4: Best-of-3 Refinement**
- If conflict detected, ask agents to refine outputs
- Pros: Encourages consensus and deeper reasoning
- Cons: Increases latency and cost
- Use when: Time permits, quality critical

**Recommended for this problem:** [Choose strategy with rationale]

---

### 6. LATENCY & COST OPTIMIZATION (300-350 words)

**End-to-End Flow Diagram:**
```
[Task arrives] →
  [Agent A: 200ms] →
  [Agent B: 500ms] → 
  [Agent C: 100ms] →
  [Consensus: 50ms] →
  [Result returned]
  Total: ~850ms
```

**Latency Reduction Opportunities:**
- **Parallelism:** Which agents can run simultaneously? (Use async/threading)
- **Caching:** Which results can be cached? (Dedup + retrieve)
- **Model selection:** Can we use a faster (smaller) model for some agents?
- **Token optimization:** Can we reduce input/output tokens?

**Cost Reduction Opportunities:**
- **Batch processing:** Group tasks to amortize setup?
- **Cheaper models:** Where can we use Groq/local instead of GPT-4?
- **Pruning:** Can we skip unnecessary agents for certain inputs?
- **Rate limiting:** Throttle to stay under API quotas?

**Example Optimization:**
```python
def optimized_orchestration(task):
    # Fast path for simple tasks
    if task.complexity == "simple":
        return fast_agent_only(task)
    
    # Parallel agents where possible
    result_a = asyncio.create_task(agent_a(task))
    result_b = asyncio.create_task(agent_b(task))
    
    # Agent C only needs A's output
    await result_a
    result_c = agent_c(result_a)
    
    return consensus([await result_b, result_c])
```

**Budget Allocation:**
- Agent A: [$ estimate per task]
- Agent B: [$ estimate per task]
- ...
- **Total target:** [$ per task]
- **Comparison:** If manual would cost $X, automation ROI is...

---

### 7. FAILURE HANDLING & RESILIENCE (300-350 words)

**Failure Modes & Mitigations:**

| Failure Mode | Probability | Impact | Mitigation |
|---|---|---|---|
| Agent API timeout (model unavailable) | High | Task blocks | Timeout + fallback model |
| Agent returns incoherent output | Medium | Bad decision | Validation schema + confidence threshold |
| Two agents fundamentally disagree | Medium | Stalled task | Escalation or best-of-3 refinement |
| Orchestrator crashes mid-task | Low | Task lost | Persistent state + resume from checkpoint |
| Cascading failures (one agent failure triggers others) | Low | Catastrophic | Circuit breaker pattern, isolate failures |

**Resilience Patterns:**

```python
# Timeout + Fallback
def call_agent_with_fallback(agent, task, timeout=10, fallback_model="gpt-3.5"):
    try:
        result = asyncio.wait_for(agent(task), timeout=timeout)
        return result
    except asyncio.TimeoutError:
        return fallback_model(task)

# Validation Schema
def validate_output(output, schema):
    if not output.matches(schema):
        raise ValidationError(f"Output violates schema: {output}")
    if output.confidence < MIN_CONFIDENCE:
        raise LowConfidenceError(f"Confidence {output.confidence} below threshold")
    return output

# Circuit Breaker
class CircuitBreaker:
    def __init__(self, failure_threshold=5, reset_timeout=60):
        self.failures = 0
        self.threshold = failure_threshold
        self.state = "closed"  # closed=normal, open=failing, half_open=testing
    
    def call(self, agent, task):
        if self.state == "open":
            if time.time() - self.last_failure > self.reset_timeout:
                self.state = "half_open"
            else:
                raise CircuitOpenError("Too many recent failures")
        
        try:
            result = agent(task)
            self.failures = 0
            return result
        except Exception as e:
            self.failures += 1
            if self.failures >= self.threshold:
                self.state = "open"
                self.last_failure = time.time()
            raise
```

---

### 8. DEPLOYMENT TOPOLOGY (250-300 words)

**Where do agents run?**

**Option 1: Monolithic (All agents in one process)**
```
[Orchestrator + All Agents] → [LLM APIs]
```
- Pros: Simple, minimal latency
- Cons: Single point of failure, hard to scale individual agents
- Cost: 1 server

**Option 2: Microservices (Each agent is a service)**
```
[Orchestrator] → [Agent A Service] → [LLM APIs]
               → [Agent B Service] → [LLM APIs]
               → [Agent C Service] → [LLM APIs]
```
- Pros: Independent scaling, easier to debug, can use different models/languages
- Cons: Network overhead, distributed debugging complexity
- Cost: 3-4 servers

**Option 3: Serverless (Each agent invocation = new container)**
```
[API Gateway] → [Lambda Agent A]
             → [Lambda Agent B]
             → [Lambda Agent C]
             → [Shared State DB]
```
- Pros: Auto-scaling, pay-per-use, no ops
- Cons: Cold start latency (500ms-2s), pricing can spike
- Cost: $0.02-$0.20 per task (varies)

**Option 4: Hybrid (Some agents local, some remote)**
```
[Local Fast Agents] → [Remote Expensive Agents] → [Result Cache]
```
- Pros: Balance cost and latency
- Cons: Complexity
- Cost: 1-2 servers + API spend

**Recommended for this problem:** [Choose with rationale based on scale/cost/latency requirements]

**Docker/K8s Configuration:**
```yaml
# Example: Kubernetes deployment for Option 2
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: orchestrator
spec:
  replicas: 2
  template:
    spec:
      containers:
      - name: orchestrator
        image: my-org/orchestrator:latest
        env:
        - name: AGENT_A_URL
          value: http://agent-a:8000
        - name: AGENT_B_URL
          value: http://agent-b:8000

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: agent-a
spec:
  replicas: 3  # Scale independently
  template:
    spec:
      containers:
      - name: agent-a
        image: my-org/agent-a:latest
        env:
        - name: LLM_API_KEY
          valueFrom:
            secretKeyRef:
              name: llm-secrets
              key: api-key
```

---

### 9. TESTING & VALIDATION STRATEGY (250-300 words)

**Unit Tests (per agent):**
- Does Agent A produce correct output for known inputs?
- Does it handle edge cases gracefully?

**Integration Tests (agents together):**
- Does orchestrator correctly route outputs?
- Does state management work?
- Do conflicts resolve properly?

**End-to-End Tests (full system):**
- Real task → final output correct?
- Latency under target?
- Cost under budget?

**Example Test Suite:**
```python
def test_agent_researcher():
    task = "Explain quantum entanglement"
    result = researcher_agent(task)
    assert result.confidence > 0.8
    assert len(result.sources) > 0
    assert result.tokens < 2000

def test_orchestrator_sequential():
    task = Task(...)
    result = orchestrate(task)
    assert result.state == "completed"
    assert all(agent in result.audit_log for agent in ["A", "B", "C"])

def test_conflict_resolution():
    task = Task(...)
    agent_a_result = {"decision": "approve"}
    agent_b_result = {"decision": "reject"}
    resolution = resolve_conflict(agent_a_result, agent_b_result)
    assert resolution.escalated_to_human == True

def test_latency_sla():
    start = time.time()
    result = orchestrate(task)
    elapsed = time.time() - start
    assert elapsed < 2.0  # SLA is 2 seconds
```

**Monitoring & Metrics:**
- Agent accuracy: % of outputs that lead to correct final results
- Consensus rate: % of tasks where agents fully agree
- Escalation rate: % of tasks escalated to human
- End-to-end latency: P50/P95/P99
- Cost per task: $ spent on LLM APIs

---

### 10. PRODUCTION CHECKLIST (150-200 words)

Before deploying to production:

- [ ] All agents tested independently with real data
- [ ] Orchestrator tested with all agents in controlled environment
- [ ] Failure scenarios tested (agent timeout, bad output, conflicts)
- [ ] Logging/monitoring in place (every decision logged with reasoning)
- [ ] API rate limits understood and handled
- [ ] Cost per task estimated and budgeted
- [ ] Escalation path clear (how does human review happen?)
- [ ] Audit trail immutable (can replay decisions)
- [ ] Privacy/compliance (PII handled securely, data retention policy)
- [ ] Alert thresholds set (alert on high escalation rate, cost overruns)
- [ ] Rollback plan (how to revert to previous orchestration)
- [ ] Documentation (for ops team to debug issues)

---

## Example: Research Article Orchestration

**Problem:** Create high-quality research articles by coordinating 4 agents.

**Agents:**
1. Researcher: Gathers facts and sources
2. Fact-Checker: Validates claims
3. Copy Editor: Improves clarity and style
4. SEO Analyst: Optimizes for search

**Orchestration Pattern:** Sequential (Researcher → Fact-Checker → Copy Editor) + Parallel SEO

```
Researcher ────→ Fact-Checker ────→ Copy Editor ─┐
                                                  ├→ Consensus/Merge
                          SEO Analyst (parallel) ─┘
```

**Cost per article:**
- Researcher (GPT-4): $0.10
- Fact-Checker (GPT-3.5): $0.03
- Copy Editor (GPT-3.5): $0.02
- SEO Analyst (local LLaMA): $0.00
- **Total: $0.15 per article**

**Latency:**
- Researcher: 5s
- Fact-Checker: 3s
- Copy Editor: 2s
- SEO (parallel): 2s
- Consensus: 1s
- **Total: ~11s end-to-end**

**Failure handling:** If Fact-Checker timeout, use GPT-3.5 as fallback. If confidence < 0.8, escalate to human editor.

---

## Tips for Best Results

1. **Be specific about agent capabilities:** Don't say "fact-checker" — say "fact-checker using web search + claim-level validation"
2. **Quantify non-functional requirements:** "Fast" is useless; "< 5 seconds end-to-end" is clear
3. **Think through failure modes:** What could go wrong in production? Design for it now
4. **Start simple:** Sequential > parallel > market-based. Only add complexity if needed
5. **Measure everything:** Cost, latency, accuracy, human escalations. Data drives optimization

---

## When to Use This Prompt

✅ **Perfect for:**
- Building AI agent platforms (products with agents)
- Complex decision systems (hiring, loan approval, content moderation)
- Autonomous research/analysis systems
- Multi-stage workflow automation (QA, editing, publishing)
- Scaling AI from single-agent to multi-agent

❌ **Not ideal for:**
- Single-agent systems (use "Prompt Engineering" instead)
- Simple chatbots (use "Chatbot Design Prompt" instead)
- Pure infrastructure/DevOps (unrelated to agent orchestration)
```

---

## Testing Instructions

**Test Input:**
```
Business Problem: Auto-generate, validate, and rank startup ideas

Agent Types Needed:
1. IdeaGenerator: Brainstorms startup ideas (market + problem + solution)
2. ValidatorAgent: Checks feasibility (technical, market, business viability)
3. CompetitorAnalyst: Researches existing competitors and differentiators
4. RankingAgent: Scores ideas on potential (market size × viability × uniqueness)

Non-Functional Requirements:
- Latency: < 30 seconds per 10 ideas
- Cost: < $0.50 per batch of 10
- Reliability: 99% of ideas ranked without escalation
- Auditability: Every decision logged with reasoning

Constraints:
- Budget: $100/month max
- Prefer Groq (fast, cheap) for initial generation; GPT-4 only for validation
- Running on single server (not distributed)
```

**Expected Output Quality Markers:**
- ✅ Problem decomposition identifies the sequential + parallel hybrid pattern
- ✅ Agent specs include prompt examples for idea generation and validation
- ✅ Orchestration protocol is concrete (not generic)
- ✅ Conflict resolution handles disagreement between ValidatorAgent and RankingAgent
- ✅ Cost model shows how to use Groq for cheap generation, GPT-3.5 for validation
- ✅ Deployment uses monolithic (single server) with clear async patterns
- ✅ Testing covers both individual agent accuracy and end-to-end latency

---

## Pricing & Value Proposition

**Price:** $6.99  
**Why this price?**
- Takes 2-3 weeks of AI architecture consulting to design properly
- Saves ~40-60 hours of design/implementation time
- ROI: If multi-agent system saves 5 hours/week in manual work (50% of team) × 4 weeks × $100/hr = $2,000 value
- Compared to $6.99, return is ~285x

**Positioning on PromptBase:**
"Design a production-ready multi-agent orchestration system in minutes. From competition detection to scaling challenges—everything you need to build AI systems that actually work together."

**Sample Keywords/Tags:**
AI Agents, Multi-Agent Systems, Orchestration, System Design, LLM, Claude, GPT, Agent Coordination, Distributed Systems, Auto-Generation
