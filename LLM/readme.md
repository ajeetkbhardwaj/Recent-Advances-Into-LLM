Paper : LeanNavigator

Problem Statement :

ATPs build onto top of LLMs suffers from two challenges

1. Error

   - LLM mostly makes mistakes into the proof generation which invalidate the entire results
   - LLM don't have a build is step-by-step proof verification mechanism
2. Data

   - LEAN4 theorem and proof data corpus too small compared trillions of tokens used to train the general purpose llms. Thus, It limits the ATP model performance and generalization

Previous Paper : ReProver to benchmark results - smaller dataset of theorems and proofs

Hypothesis : A large dataset of formally verified Lean proofs i.e several orders of magnitude larger than existing libraries will significantly improve the accuracy and robustness of ATP models.

Previous Solution(ReProver) : Similar Idea but different approach towards data collection and generation.

Proposed Solution(LeanNavigator) :

LeanNavigator - It automatically generate a large‐scale dataset of novel Lean theorems paired with verified proofs by exploring the state transition graph of existing theorems.

State Transition Graph :

States : Its intermediate proof goals Ex. theorem with current hypotheses

Edges : Lean tactics that transition one state to the next.

Assumption : Traversing directed graph from a theorem’s initial state to a proof‐finished node, new reachable states become new theorems, and the sequence of tactics along any path forms a valid proof.

Dataset Generation Algorithm

1. Initialization of LeanDojo(Interactive Lean Client) by  theorem’s initial state
2. BFS with Priority to explore(traverse) the directed graph upto 30 mins
3. Proon : the Acyclic graph explore by the BFS upto 8 tactics to finish the proof node.
4. Proof selection - For each provable state, choose the shortest proof path.

Limitations

1. They used Ray having 24 processor onto 24 theorems that took 28 days to finish the proof.
2. only bounded within the scope of tactics
3. lack high-level NL planning to control complete proof structure.

---

Assumption - LRM are trained onto the large scale LEAN4 dataset such that they understand the mathematical reasoning for the proof generation of theorems.

Problem :

1. Large Reasoning Model for whole proof generation

   - similar to code generation
   - non-verifiable proofs generation
   - non-deterministic : In one run may generate correct proof but into another may not if yes then may used different approach.
   - Hallucination etc.
2. Tree search for proof generation

   - computationally expensive with proof complexity increases
   - only bounded within the scope of tactics
   - lack high-level NL planning to control complete proof structure.

Gaps

- Natural Language Reasoning for Proof Generation Planning
- LEAN4 formal mathematics translation
- Regorous formal verification feedback to the every generated proof

  Idea : How to take balanced approach to handle them.

Hypothesis : A multi-agentic framework that combines the models natural language planning, lean4 formal mathematics generation and formal verification. So, hypothesis is to make deeper coordinated structured intraction amongs the planning, generation, verification and feebdack agents guided by formal mathematical reasoning that may produce verified structured lean4 proof of theorems.

Solution : Lean4Agent -> a multi-agentic system

1. Structured Coordinated Intraction to achieve common goal by each and every agents.
2. Agents capable of doing formal reasoning from its prompt -> Long Chain of Information
3. Planning -> must be right otherwise proof will be wrong : Overcome Idea -> Feed the Proof Sketch into the Natural Language
4. Generation -> llm must capable of generation of lean4 program from natural language -> mathematical reasoning capability
5. Verification -> Lean4 Verifier
6. Feedback -> capable of handling errors into the generated lean4 program
