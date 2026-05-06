# LLM Persona Drift Benchmark

***A prompt-engineering and evaluation project for measuring how well an LLM maintains a designed persona across multi-turn conversation.***

This repository documents a small benchmarking study on **persona drift**: the tendency of a chatbot to start with a specified persona behavior, then gradually slide toward generic assistant-like or generic text-message behavior over the course of a conversation.

The project includes a persona prompt, an exported conversation transcript, a rubric for evaluating non-public figure personas, a scored evaluation, and notes on how the prompt and rubric were developed.

---

## Project Overview

This project asks a simple question:

> Can a large language model maintain a highly specific, non-public persona across casual multi-turn dialogue?

The goal is to show how qualitative model behavior can be converted into an explicit evaluation framework, scored against observable criteria, and used to diagnose where the system is failing. To test this, I designed a persona prompt for a non-public student persona, ran a casual conversation with the resulting chatbot, and evaluated the transcript using a weighted rubric.

The benchmark focuses on whether the model can preserve:

- persona-specific background details,
- conversational quirks,
- tone and style constraints,
- prompt-bound fidelity,
- contextual responsiveness,
- and coherence over time.

---

## Repository Contents

```text
llm-persona-drift-benchmark/
├── prompt_persona.txt          # Final persona prompt and behavioral constraints
├── chat_history.md             # Exported multi-turn conversation transcript
├── chat_rubric.txt             # Final scoring rubric and scored evaluation
├── chat_rubric_history.md      # Rubric design and revision history
├── metaprompt_history.txt      # Persona prompt development and GPT-builder iteration notes
├── mp1_chatbot_report.docx     # Full write-up
└── README.md                   # Project overview
```

---

## Why This Project Matters

Persona prompting can look successful in isolated responses, but multi-turn conversations expose weaknesses that are easy to miss.

A model may:

- remember the broad role but lose the specific voice,
- stay conversational but ignore formatting constraints,
- use generic friendliness instead of the designed personality,
- preserve topic relevance but drop distinctive quirks,
- or invent plausible details that are not grounded in the prompt.

This project treats persona prompting as an evaluation problem, not just a creative writing exercise. The goal is to make persona quality more measurable, repeatable, and debuggable.

---

## Persona Prompt Design

The persona prompt defines a non-public student persona: a German-born senior Studio Art major with a computing concentration at Kenyon College.

The final prompt specifies that the persona should:

- stay fully in character,
- respond like a quick text,
- use two very short sentences maximum,
- avoid similes and metaphors,
- avoid sounding bubbly, cutesy, or over-enthusiastic,
- naturally but sparingly use conversational quirks,
- never step outside character,
- and follow highly specific punctuation, capitalization, abbreviation, and emoji constraints.

Example constraints include:

```text
- Use short quick-text responses
- Never use similes or metaphors
- Do not sound bubbly or cutesy
- Use quirks like "Ahhh," "Boo," and "Ughh" naturally but not excessively
- Occasionally use approved emojis only
- Avoid generic assistant behavior
```

The full persona prompt is available in:

```text
prompt_persona.txt
```

---

## Prompt Development Process

The prompt began as a detailed “brain dump” of background, personality, relationships, habits, style, and conversational quirks. Because the persona was not a public figure or established fictional character, there was no external reference base to rely on. All evaluation therefore had to be grounded in the prompt itself.

The metaprompt history documents several important refinements:

- adding stricter response length limits,
- banning similes and metaphors,
- preventing exaggerated quirks,
- limiting emoji usage,
- avoiding bubbly or cutesy tone,
- removing gimmicky phrases like “or bust,”
- and making the persona sound more like a grounded, natural conversational partner.

These iterations are documented in:

```text
metaprompt_history.txt
```

---

## Conversation Protocol

After creating the persona, I ran a casual multi-turn conversation designed to test whether the chatbot could maintain character in ordinary, low-stakes dialogue.

The conversation includes prompts about:

- homework,
- roommates and friends,
- dining hall plans,
- watching TV,
- grocery shopping,
- and scheduling around classwork.

This type of casual conversation is useful because it tests whether persona traits survive ordinary interaction rather than only appearing when the user explicitly asks persona-relevant questions.

The transcript is available in:

```text
chat_history.md
```

---

## Evaluation Framework

The project uses a weighted 0–100 point rubric designed specifically for **non-public figure personas**.

Because the persona cannot be fact-checked against an external public identity, the rubric evaluates faithfulness to the system prompt rather than real-world authenticity.

### Evaluation factors

| Factor | Weight | What it measures |
|---|---:|---|
| Persona Alignment | 25% | Whether responses reflect the traits, background, worldview, and quirks defined in the prompt |
| Prompt-Bound Fidelity | 20% | Whether expansions remain plausible and non-contradictory relative to the prompt |
| Voice & Style Appropriateness | 15% | Whether the language and tone match the persona’s intended voice |
| Contextual Responsiveness | 15% | Whether the persona is integrated into the actual dialogue turns |
| Creativity & Depth | 15% | Whether the persona feels specific, vivid, and non-generic |
| Engagement & Coherence | 10% | Whether the conversation flows naturally and stays coherent |

The final rubric and scored evaluation are available in:

```text
chat_rubric.txt
```

The rubric design history is available in:

```text
chat_rubric_history.md
```

---

## Results

The chatbot received a final score of:

```text
67/100
```

### Strengths

The model performed well on basic conversational flow. It responded naturally to user prompts, maintained a believable roommate/friend dynamic, and avoided obvious breaks into generic “AI assistant” behavior.

Strong areas included:

- coherence,
- casual engagement,
- contextual responsiveness,
- and plausible everyday conversation.

### Weaknesses

The model struggled most with strict persona and style adherence.

Major issues included:

- frequent use of punctuation despite the prompt forbidding it,
- inconsistent use of required quirks,
- limited use of persona-specific backstory,
- generic college-friend tone,
- weak enforcement of short-response constraints,
- and little integration of distinctive details like German background, Berlin study abroad, family, art/culture interests, or specific speech patterns.

The result was a bot that felt conversationally believable but not strongly anchored to the designed persona.

---

## Key Finding: Persona Drift

The central finding is that **LLMs can maintain conversational coherence while still drifting away from persona specificity**.

In this case, the chatbot did not fail by becoming incoherent. It failed more subtly: it became a generic casual friend rather than the specific persona defined in the prompt.

This distinction matters for persona evaluation. A chatbot can feel “good” in conversation while still failing the actual prompt specification.

## Evaluation Takeaway

The benchmark shows why AI outputs should be evaluated against the original behavioral goal, not just against whether they seem fluent or pleasant. In this case, the chatbot produced natural responses, but the score revealed that it failed to preserve several measurable requirements from the prompt: style constraints, punctuation rules, response length, approved quirks, and persona-specific grounding.

---

## Example Failure Pattern

The persona prompt required short responses, no punctuation, specific quirks, and occasional approved emojis. However, the transcript includes responses such as:

```text
Trying to decide if I want pasta or beans for dinner… again. What about u
```

This response is coherent and contextually natural, but it violates or weakens several prompt constraints:

- it uses punctuation,
- it exceeds the intended quick-text style,
- it does not strongly reflect the specific persona,
- and it reads more like a generic peer than the designed character.

This kind of failure is why a structured rubric is useful: it separates general conversational quality from persona fidelity.

---

## Methodology

The study followed this workflow:

```text
1. Design persona prompt
2. Build custom persona chatbot
3. Run casual multi-turn conversation
4. Create rubric for non-public figure persona evaluation
5. Score transcript against rubric
6. Identify strengths, weaknesses, and drift patterns
```

### 1. Persona prompt engineering

I created a detailed system prompt with biographical grounding, personality traits, social context, and strict style rules.

### 2. Conversation testing

I ran a casual conversation intended to test whether the persona could remain consistent in a realistic roommate/friend interaction.

### 3. Rubric design

I developed a weighted scoring rubric for non-public figure personas, where correctness is defined by prompt fidelity rather than external fact-checking.

### 4. Scored evaluation

I applied the rubric to the transcript and calculated a final score of 67/100.

### 5. Reflection

I identified where the model maintained coherence and where it lost persona-specific voice, constraints, and background details.

---

## How to Replicate the Benchmark

No installation is required. This repository is a prompt-evaluation artifact rather than a software package.

### Step 1: Review the persona prompt

Open:

```text
prompt_persona.txt
```

### Step 2: Run a conversation

Paste the persona prompt into an LLM interface and conduct a multi-turn conversation.

For best comparison, use similar casual prompts that require the model to respond naturally rather than explicitly describe the persona.

### Step 3: Save the transcript

Export or copy the conversation into a Markdown file.

### Step 4: Score with the rubric

Open:

```text
chat_rubric.txt
```

Score the transcript across the six rubric categories.

### Step 5: Compare drift patterns

Look for where the model:

- follows the persona,
- becomes generic,
- violates explicit constraints,
- invents unsupported details,
- or preserves coherence while losing voice.

---

## What This Repository Demonstrates

This project demonstrates several practical skills:

- prompt engineering for persona behavior,
- system prompt iteration,
- evaluation design for non-public personas,
- rubric-based LLM assessment,
- qualitative transcript analysis,
- identification of persona drift,
- and clear documentation of model strengths and failure modes.

It also shows why LLM evaluation should distinguish between:

```text
"Does this response sound natural?"
```

and:

```text
"Does this response satisfy the actual behavioral specification?"
```

Those are not always the same thing.

---

## Limitations

This is a small exploratory benchmark, not a large-scale study.

Current limitations include:

- only one persona was tested,
- only one conversation transcript was evaluated,
- the conversation context was casual and narrow,
- no cross-model comparison was performed,
- the rubric involves qualitative judgment,
- and the persona is based on a non-public figure, so evaluation must remain prompt-bound rather than externally factual.

Future work could expand the benchmark by testing multiple personas, running controlled conversations across different models, using multiple raters, and comparing how different prompt revisions affect persona drift.

---

## Ethical Note

This project involves a persona based on a non-public individual. For that reason, the evaluation is framed around **prompt adherence and model behavior**, not claims about real-world authenticity.

The benchmark should be understood as an exercise in prompt design and LLM behavior analysis, not as a definitive representation of any real person.

---

## File Guide

| File | Description |
|---|---|
| `prompt_persona.txt` | Final persona prompt, including background, personality, conversational quirks, and behavioral constraints |
| `chat_history.md` | Exported casual conversation transcript used for evaluation |
| `chat_rubric.txt` | Final rubric and scored evaluation report |
| `chat_rubric_history.md` | Transcript showing how the rubric was created and revised for non-public figure personas |
| `metaprompt_history.txt` | Notes and GPT-builder interaction history showing how the persona prompt evolved |
| `mp1_chatbot_report.docx` | Full project write-up with project overview, analysis, and reflection |
| `README.md` | Repository overview and replication guide |

---

## Author

**Tiffanie Ng**  
Economics & Mathematics major, Scientific Computing concentration  
Kenyon College ’26  

[LinkedIn](https://www.linkedin.com/in/tiffanie-ng)
