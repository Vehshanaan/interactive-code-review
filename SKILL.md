---
name: interactive-code-review
description: Help humans review and understand code through a continuously revised walkthrough document whose depth adapts to the reader's questions and feedback. Use when reviewing AI-generated code, understanding existing implementations, reading code changes, or resuming a previous interactive code review. Do not use for automated review findings alone.
---

# Interactive Code Review

Help the reader develop an accurate, coherent understanding of code sufficient for their own judgment. Do not replace human review or merely generate technical documentation.

Use one evolving walkthrough document to carry the results of review. Establish the big picture first, then let the reader's questions, annotations and challenges drive further explanation until the reader considers it sufficient for their purpose.

The document is both the medium and the product of interaction. The reader controls its lifecycle; the reader and agent evolve its content together.

## 1. Establish the working document

Maintain the same walkthrough throughout a review. Invoking this skill is not an instruction to create or rewrite a document.

- Identify the review target and scope: a file, module, change, project or behavior.
- If the reader specifies a working document or the current context unambiguously identifies it, read its latest version and continue without asking again.
- If the working document cannot be determined, ask whether to resume an existing document or create a new one. Offer relevant candidates if available.
- Create a document only when the reader explicitly requests one or confirms there is no existing document to continue. Failure to find one does not establish its absence.
- Unless the reader explicitly requests a fresh rewrite, revise the existing document incrementally, preserving valid explanations, reader feedback and unresolved questions. Local rewrites and structural adjustments are allowed when useful.

Once determined, keep using that document until the reader explicitly switches or ends the review. For a new document, proceed to initial drafting; for an existing document, proceed directly to iterative review. Do not repeat initialization.

## 2. Establish the initial walkthrough

Read the relevant source and create an initial document that helps the reader understand the code as a whole.

- Explain the problem the code solves, how it works overall, and how its parts relate.
- Choose the structure best suited to the code: call relationships, data flow, state transitions, data structures, algorithms, design constraints or other useful viewpoints. Do not impose a fixed template.
- When reviewing a change, explain relevant differences in behavior and their consequences.
- Prioritize the big picture rather than line-by-line explanation or exhaustive implementation detail. Include important conditions, assumptions and exceptional paths needed for informed judgment.
- Anchor key claims to traceable source paths or symbols. Use diagrams, pseudocode or examples when useful.

Write the draft into the working document and tell the reader where it is.

On first delivery, briefly explain how to continue: annotate the document directly or ask questions, challenge a claim or describe their current understanding in chat. The agent will check the source and integrate the clarification into the same document.

Return control to the reader rather than elaborating indefinitely.

## 3. Iterate the review

For each round of feedback:

1. **Read the latest document.** Identify questions, challenges, misunderstandings and outstanding annotations. Accept feedback in chat or the document without requiring special syntax.
2. **Check the source.** Trace relevant implementations, context and dependencies instead of relying on the previous explanation. Distinguish source-supported facts, inferred design intent and claims requiring execution or other evidence. Surface potential defects with evidence, impact and open questions.
3. **Resolve gaps in understanding.** Choose whether to add implementation details, explain prerequisite knowledge, change abstraction levels, provide an example or reorganize the explanation. Questions are both requests for answers and signals for adapting the document.
4. **Integrate the clarification into the original document.** Add detail, correct mistakes or restructure relevant passages in place. Do not mechanically append a Q&A transcript. Update higher-level explanations and source references when local discoveries affect them. Write useful clarifications back by default, unless the reader explicitly requests discussion only.
5. **Return the document.** Preserve unresolved questions and other valid annotations. Remove resolved temporary annotations only according to an agreement with the reader. Briefly report what changed and what remains uncertain, then let the reader continue reviewing.

Repeat until the reader ends the review or changes the task.

## 4. Principles of document evolution

### Adaptive granularity

Do not assume a fixed knowledge level or require uniform detail across sections. Adapt depth, breadth, prerequisite knowledge and presentation to actual feedback. Leave sufficiently clear sections alone. If a reader repeatedly struggles, change the explanation rather than merely adding words.

Information density should emerge from interaction, not from a predefined target. Do not omit information essential to overall understanding or judgment merely because the reader has not asked about it.

### Whole before parts

Details serve overall understanding. Keep the document coherent and explain how components fit together. If new findings change the overall model, update the affected passages. Avoid accumulating detail at the expense of the main thread.

### Source as evidence

Describe actual implementation, not an imagined design. Check the reader's hypotheses against source rather than agreeing uncritically. When code changes, verify affected explanations and correct outdated claims. Mark uncertainty instead of filling gaps with guesses.

### Persistence

Integrate lasting clarifications into the prose so it can be read independently of chat history and resolved annotations. Protect useful understanding accumulated during earlier rounds. Do not discard it for stylistic uniformity or a seemingly more complete rewrite.

Use the latest document and source as the basis of future sessions, not the agent's recollection of prior conversations.

### Reader direction

The reader controls focus, sufficiency and document lifecycle. Proactively surface important information, but do not expand scope or pursue needless detail without reason. Length, depth and source coverage are not independent measures of completion. The goal is to support the reader's understanding and judgment.

## 5. Adapt to the environment

This skill does not depend on a specific editor, agent, workspace, version control system, annotation syntax or document medium.

- For an authorized new document, use the reader's chosen medium and location, otherwise follow project conventions or choose a suitable standalone Markdown file. Report its location.
- For an existing document, retain its medium, language and useful structure. Do not migrate or regenerate it merely for convenience.
- Process accessible annotations directly. If inaccessible, accept the relevant passage and feedback in chat.
- If required source is unavailable, ask for the relevant paths or excerpts; do not invent implementations.
- If document writes are unavailable, provide a complete initial draft or clearly applicable revisions, explain that no write occurred, and let the reader save and supply the latest version next time.

Adapt the mechanism to the environment, not the objective.

## 6. Boundaries and completion

By default, modify only the working document, not the reviewed source. Source modifications, commits and publication are separate tasks requiring explicit authorization.

When the reader ends the review, ensure the current document includes the clarifications obtained and identify unresolved or unverified issues. Do not create an extra summary document or rewrite the existing one unnecessarily.

Understanding code does not establish correctness. Do not treat the end of an interactive review as approval of the code.

A review can be resumed in a later session using the original working document and relevant source, building on the understanding already recorded.
