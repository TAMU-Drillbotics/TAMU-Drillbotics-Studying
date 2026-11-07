This file stores the fundamental idea of self-studying. It is mainly a rule of thumb, so PLEASE provide feedback and suggestions.

# 1. Idea of Self-studying
## 1.1 How we think about learning (ground model)

All knowledge can be seen as a graph. Each **node** is a fact, an opinion, or a model, and nodes link to one another through four basic relations:

- **Cause and effect**: A leads to B.
- **Hierarchy**: A is a part or special case of B.
- **Parallel**: A and B sit at the same level and coexist.
- **Opposition**: A and B conflict or trade off against each other.
- **Similar**: A (or the structure contains A) is similar to B (or the structure contains B) 
- **Instance**: (Theory) A has B as an instance/example

To learn something is to pull out its key nodes, link them to nodes you already have, and be able to use them in the future. When a new node conflicts with an old one, use reality as the reference and update the graph, either with a small patch or a full rebuild.

### 1.1.1 **Strengthening nodes.** A new node fades unless you revisit it. There are two ways to revisit:

- **Passive review** (re-reading): cheap, but weak. It often creates a false sense of understanding.
- **Active recall** (retrieving the node from memory and linking it to other nodes again): more effort, but much stronger. It also shows which nodes are missing.

### 1.1.2 **Using nodes.** Working memory holds only a few nodes at once. When solving a problem, start from its highest-level abstraction and work downward until you reach nodes you can apply directly. Then combine those nodes to solve it. Doing this over and over builds procedural skill.

### 1.1.3 **Judging how well a node is learned**:

1. Do you still remember it?
2. How many other nodes is it linked to?
3. Can you use it, with other nodes, on real problems?

### 1.1.4 **Build the graph you need, not the perfect one.** Remembering everything forever, with every possible link, costs too much time and effort. Let the goal decide the shape and size of the graph:

- An exam on concepts calls for a graph centered on how concepts relate.
- An exam on calculation calls for practice problems and fluency first.
- A real engineering task calls for application and transfer.

Deeper understanding is always a bonus. The boundary is not a fixed threshold, so grow the graph as far as your goals and time allow. This is also why some exams take far more preparation than others. Some only search a graph you already built, while others need a new way of linking nodes.


# 2. Idea of practicing for self-studying: Strengthening nodes and form better edge connections through feedbacks/practicing. 
## 2.1 The study loop

1. **Input**: Read the textbook, attend lectures, or study worked examples to form the first nodes.
2. **Question**: Use the six levels to write or pick test questions.
3. **Answer on your own**: Close the material, then recall, explain, derive, or solve.
4. **Evaluate**: Check the answers for correctness, completeness, and whether you needed help.
5. **Diagnose**: Decide whether the gap is in a node (recall), a link (explain + connect), the reasoning (derive), or the execution (apply + transfer).
6. **Update**: Fill in, fix, or rebuild that part of the graph, then test again.

## 2.2 The six levels of questions

Practice questions follow these six levels. The test of learning is not "how much did I read and understand?" It is "can I answer, connect, and do this on my own?" Not every topic needs all six levels.

| Level | Key question | Goal | Sample questions | Passes when |
| --- | --- | --- | --- | --- |
| **1. Recall** | What is it? | The node exists | What is the definition? What are its key parts? Can I write it out with the book closed? | You can recall it on your own, not just recognize it |
| **2. Explain** | Why does it exist? What is it not? | The meaning is clear | What problem does it solve? How does it differ from similar ideas? When does it not apply? Give one example and one counterexample. | You can explain it in your own words without mixing it up with other ideas |
| **3. Connect** | What is it related to? | Basic links are correct | Which concepts does it relate to, and how (cause, hierarchy, parallel, opposition)? If one condition changes, what happens to the other? | You can explain the relationships, not just each definition on its own |
| **4. Derive** | What follows? | New conclusions come from existing knowledge | Given A and B, can I derive C? Does the result still hold if a condition changes? Can I reason from effect back to cause? What assumptions does the argument rely on? | You can reason forward and backward, not just repeat the textbook |
| **5. Apply** | How do I use it? | Knowledge is organized for a task | Which nodes does this problem need, and in what order? Which method or tool fits? If I get it wrong, where did it go wrong? | You can solve a standard problem without a worked example to follow |
| **6. Transfer** | What if things change? | Knowledge works in new settings | Can I spot the same structure when the problem is worded differently? Does it carry over to another field? What are its limits? Can I pose a new question? | You can find and use the underlying structure instead of copying examples |

## 2.3 How deep to go

| Goal | Levels | Typical use |
| --- | --- | --- |
| Recognition and memory | 1 | Terms, facts, formulas |
| Concept understanding | 1–3 | Basic concepts and how they connect |
| Independent problem solving | 1–5 | Math, engineering, and programming tasks |
| Mastery | 1–6 | Research, complex problems, long-term skill |

This is a starting point, not a hard rule. Adjust it for the exam or task, what you already know, and how much time you have.


## 2.4 Six core self-check questions

1. **What is it?** Do I know its definition and main content?
2. **Why does it exist?** Do I know what problem it solves?
3. **What is it related to?** Do I know how it connects to other knowledge?
4. **What can I derive from it?** Can I use it to reach new conclusions?
5. **How do I use it?** Can I solve problems with it on my own?
6. **What if conditions change?** Can I still use it in a different situation?


## 2.5 Scoring

Score each question on its own, not the topic as a whole. Judge each level separately instead of looking only at the total.

| Score | Level | Meaning |
| --- | --- | --- |
| 0 | Unable | Cannot answer or complete it |
| 1 | Partial | Needs hints or the source material |
| 2 | Independent | Answers correctly without help |
| 3 | Flexible | Can explain, derive, handle changes, and transfer to new situations |

Being able to explain a concept is not the same as being able to apply it. Solving a familiar problem is not the same as solving a new one.


## 2.6 Diagnosing mistakes

| Symptom | Likely gap | What to do |
| --- | --- | --- |
| Can't recall the definition | The node is not yet formed | Active recall, spaced review |
| Know the definition but can't explain it | The meaning is unclear | Re-study it with examples and counterexamples |
| Know two concepts but can't link them | Missing nodes/links between nodes | Compare them, derive one from the other, explain the relationship |
| Can explain but can't derive conclusions | Weak reasoning | Practice derivations, changed conditions, and working backward |
| Can do the examples but not new problems | Weak application | Practice different wordings and problem types |
| Know the steps but make execution errors | Weak procedural skill | Repeat on your own and target the specific errors |

## 2.7 Ground rules

- **Answer first, then check the material.** Answering with the book open tests familiarity, not recall.
- **Every test should lead to a next step.** Don't reread the whole chapter after a wrong answer. Find the specific gap and fix that.
- **Use delayed testing.** Test again after a day, a few days, and a week. Adjust the gaps based on what you actually forget.
- **Measure progress by what you can do.** Track the tasks you can complete on your own, not pages read or hours spent.

