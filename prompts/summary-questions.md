Below are the templates for generating a summary/summaries. Each of them handles a specific purpose/task and should be applied at separate times to ensure the summary covers the explanation of:
1. Domain Map: What is the entire domain doing/researching? Why do we need it? What main modules does it contain? (To build the highest abstraction of nodes and understand the structure)
2. Concept Map: What main concepts does each module cover? How does each concept relate to the others? (To identify important/key nodes and build the first edges)
3. Concept Explanation: For each node:
    - What is the precise definition?
    - Why does it exist? / Why do we need it?
    - What problem does it solve?
    - How should I understand it?
    - Examples and counterexamples?
4. Relationships and Derivations:
    - Why/How is A related to B?
    - How does A affect B?
    - How to deduce C from A and B?
    - What is the deduction pipeline?
5. Application Structure:
    - When do we need this knowledge?
    - How to identify these types of questions?
    - What nodes are needed?
    - Use them in what order?
    - How to check the result/evaluation?

# Task Layer 1: Generate a Learning-Oriented Knowledge Map

I am self-studying **[SUBJECT / TOPIC]** using the provided textbooks and learning materials.

Your task is to create a learning-oriented knowledge map, NOT a conventional compressed summary.

## My learning requirements

- Assume I am a beginner in this specific subject, unless I explicitly provide prior knowledge.
- Prioritize conceptual clarity and logical continuity over information density.
- Help me identify the major knowledge nodes and understand how they connect.
- Do not assume I understand technical terms that have not yet been introduced.
- Use the provided materials as the primary source. Do not silently introduce unsupported claims.

## Required output

### 1. Domain Overview
- What does this subject study?
- What problems does it address?
- What are its major components?
- What should I be able to understand or do after studying it?

### 2. Knowledge Structure
Divide the subject into major modules and their core concepts.

For each module:
- State its purpose.
- List its important concepts.
- Explain why these concepts belong to this module.
- Identify prerequisite concepts from other modules.

### 3. Dependency Map
Show the recommended learning order.

Distinguish:
- Concepts that must be understood first.
- Concepts that can be learned independently.
- Concepts that depend on multiple earlier concepts.

Use a simple text-based graph or hierarchical outline.

### 4. Learning Priorities
Classify concepts into:
- Essential: necessary for understanding the subject.
- Supporting: useful for deeper understanding.
- Advanced: can be postponed without blocking basic learning.

### 5. Knowledge Gaps and Assumptions
Identify:
- Prerequisite knowledge I may need.
- Terms that require clarification.
- Topics that the provided materials do not explain sufficiently.

## Important constraints

- Do not explain every concept in detail yet.
- Do not compress multiple independent concepts into one vague statement.
- Do not create a dependency relationship merely because two concepts appear in the same chapter.
- Distinguish relationships explicitly stated in the materials from relationships you infer.

The goal is to give me a clear map of what I need to learn, why I need to learn it, and in what order.




# Task Layer 2: Explain a Knowledge Module Step by Step

I am currently studying:

**Subject:** [SUBJECT]

**Module:** [MODULE]

**My existing knowledge:** [LIST WHAT I ALREADY UNDERSTAND, OR SAY "BEGINNER"]

Explain this module as a structured learning lesson.

## 1. Module Overview
- What is this module about?
- Why is it necessary?
- What problem does it solve?
- How does it connect to the overall subject?

## 2. Prerequisite Check
Before explaining a concept, identify the knowledge required to understand it.

For each prerequisite:
- Give its definition.
- Explain why it is needed here.
- Do not assume I already understand it unless I have confirmed that.

If an important prerequisite is missing, explain it first.

## 3. Concept-by-Concept Explanation

For each core concept, use the following structure:

1. Name: What is this concept called?
2. Definition: What exactly does it mean?
3. Purpose: Why does it exist?
4. Components: What are its essential elements?
5. Mechanism: How does it work?
6. Example: Give a concrete and easy-to-follow example.
7. Boundary: What does it NOT mean? When does it not apply?
8. Connection: How does it relate to previously introduced concepts?

Introduce only a manageable number of new concepts at a time.

## 4. Explain Every Logical Step

Whenever you derive a conclusion, explain the intermediate steps.

Do not jump directly from A to D if understanding requires B and C.

Whenever a technical term appears for the first time:
- Define it before relying on it.
- Explain it in plain language.
- Give an example if necessary.

If an explanation requires mathematical knowledge, explain the relevant mathematical operation before using it.

## 5. Relationship Construction

After explaining the concepts, identify their relationships.

For each important relationship:
- Which concepts are connected?
- What type of relationship exists?
- Why does the relationship hold?
- What assumptions or conditions does it depend on?

## 6. Summary

End with:
- A concise conceptual summary.
- A text-based knowledge graph.
- The most important concepts I should remember.
- The concepts that require further study.

## Teaching style

Teach me as a technically capable beginner, not as an expert.

Use precise definitions but explain them in accessible language.

Prioritize logical continuity over brevity.

Do not use abstract terminology to explain another abstract term without grounding it in a concrete example.

Do not omit intermediate reasoning merely to make the explanation shorter.


# Task Layer 3: Generate Structured Learning Evaluation Questions

Based on the provided learning materials and the knowledge map we have established, generate a set of exercises for:

**Subject:** [SUBJECT]

**Module:** [MODULE]

Organize questions into six levels:

### Level 1: Identification and Recall
Test definitions, facts, formulas, and key components.

### Level 2: Explanation and Distinction
Test why concepts exist, what problems they solve, and how they differ from similar concepts.

### Level 3: Connections and Understanding
Test relationships between concepts, including causal, hierarchical, parallel, and opposing relationships.

### Level 4: Reasoning and Reconstruction
Test derivation, inference, changing conditions, and combining multiple concepts to produce new conclusions.

### Level 5: Application and Problem Solving
Test whether I can independently identify relevant knowledge and use it to solve standard problems.

### Level 6: Transfer and Extension
Test whether I can recognize underlying structures in unfamiliar situations and transfer knowledge to new contexts.

## Requirements for every question

For each question, specify:

1. Evaluation level.
2. Knowledge nodes being tested.
3. Relationships or reasoning operations being tested.
4. Expected answer or solution.
5. Common mistakes or misconceptions.
6. Scoring criteria.

## Difficulty and progression

- Start with fundamental questions.
- Increase difficulty gradually.
- Do not introduce concepts that have not been covered without explicitly identifying them as additional knowledge.
- Ensure that later questions build on earlier knowledge.
- Include both conceptual and practical questions where appropriate.

## Answer policy

The first half of the generated file should be questions and the second half should be the answers. Do NOT place answers right after each question. 

For the answers that need multiple steps of thinking, provide the full pipeline of thinking instead of just the correct answer itself. 



# Task Layer 4: Repair a Gap in My Understanding (optional, use it if the generated results are still leaping)

I am studying [TOPIC / MODULE].

I encountered the following explanation or concept:

[PASTE THE EXPLANATION OR CONCEPT]

My current understanding is:

[EXPLAIN WHAT I UNDERSTAND]

My difficulty is:

[DESCRIBE WHERE I GET LOST]

Please diagnose and repair my understanding.

## Requirements

1. Identify the exact point where my understanding breaks down.
2. Identify any missing prerequisite concepts.
3. Explain those prerequisites before continuing.
4. Reconstruct the explanation step by step, without skipping logical transitions.
5. Define every important technical term before relying on it.
6. Use a concrete example to demonstrate the reasoning.
7. Explain how the new knowledge connects to my existing knowledge.
8. End with one or two questions that test whether I have actually understood the missing part.

Do not simply rewrite the original explanation in different words.

Do not repeat the entire topic if only one conceptual gap needs to be repaired.

Focus on the smallest missing piece of knowledge or reasoning that prevents me from understanding the rest.