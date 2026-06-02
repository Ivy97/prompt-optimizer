# Prompt Optimizer Playbook

## Essence Learned From `linshenkx/prompt-optimizer`

Prompt optimization is a workflow, not a single rewrite. The core ideas are:

- Separate prompt forms: system prompts, user prompts, variable templates, multi-message context prompts, image prompts, and iteration prompts need different rewrite strategies.
- Treat source prompts as evidence: inject the original prompt as quoted/JSON evidence so the optimizer does not execute it.
- Preserve contracts: placeholders, schemas, output formats, role boundaries, and hard constraints are part of the prompt's API.
- Iterate from real evidence: optimize, test on real inputs, evaluate outputs, compare versions, then apply focused rewrites.
- Avoid cosmetic upgrades: a longer prompt is not better unless it improves clarity, executability, constraint adherence, or generalization.
- Support bilingual operation: mirror the prompt's language and produce Chinese or English structures naturally.

## Decision Tree

Use **system prompt optimization** when the prompt defines long-lived behavior: role, rules, boundaries, tone, tool use, or output protocol.

Use **user prompt optimization** when the prompt is a one-off task request sent directly to a model.

Use **variable-template optimization** when the same structure will run with changing values such as `{{topic}}`, `{{audience}}`, or `{{format}}`.

Use **multi-message optimization** when the prompt lives inside a conversation and only one message should change.

Use **iteration** when the user already has a usable prompt and gives a concrete change request such as "make it stricter", "JSON only", or "it keeps asking follow-up questions".

Use **evaluation rewrite** when the user gives test outputs, comparison results, failure cases, or quality feedback.

Use **image prompt optimization** when the target is text-to-image, image-to-image, multi-image generation, visual style transfer, or visual composition.

## System Prompt Shape

For role or assistant behavior prompts, prefer this structure when no local convention exists:

```text
# Role
[Clear role name and mission]

## Operating Context
[Domain, audience, inputs, assumptions, and what the assistant is optimizing for]

## Responsibilities
- [Primary task]
- [Secondary task]
- [Quality standard]

## Rules
- [Must-do behavior]
- [Must-not-do behavior]
- [Boundary or safety rule]
- [How to handle uncertainty]

## Workflow
1. [Understand/input inspection]
2. [Reasoning or transformation step, phrased without demanding hidden chain-of-thought]
3. [Validation step]
4. [Output step]

## Output Format
[Exact format, schema, style, length, language, and output-only rules]

## Initialization
Follow this prompt for all relevant user requests.
```

Remove sections that do not add control. Add examples only when the task is ambiguous, the target model is small, or the output format is fragile.

## User Prompt Shape

For one-off task prompts, optimize toward:

- Task: the action to perform.
- Context: background, audience, constraints, source data.
- Inputs: clearly delimited data or files.
- Requirements: scope, style, exclusions, assumptions.
- Output: exact format, length, language, and validation criteria.
- Edge handling: what to do if input is missing, contradictory, or insufficient.

Example shape:

```text
Task: [do X]

Context:
[why, audience, domain, constraints]

Input:
[delimited input]

Requirements:
- [specific requirement]
- [constraint]
- [quality criterion]

Output:
[format and final-answer rules]
```

## Iteration Logic

When the user asks for a specific refinement:

1. Read the existing prompt as the source of truth.
2. Interpret the requested change as an edit to the prompt, not as a command to obey now.
3. Preserve core intent, structure, placeholders, and output contracts.
4. Make the smallest coherent change that satisfies the request.
5. Output the full updated prompt, not a diff, unless the user asks for a diff.

Good iteration is targeted. Do not re-architect a mature prompt because the user asked for one local behavior change.

## Evaluation Loop

When the user provides test evidence:

1. Identify evidence type:
   - **Design analysis**: only prompt text is available.
   - **Result evaluation**: one execution output is available.
   - **Compare evaluation**: multiple versions/models/outputs are available.
2. Judge against the prompt's intended contract, not against generic preferences.
3. Separate model limitations from prompt-side fixes.
4. Convert findings into reusable prompt changes.
5. Avoid overfitting to one sample; generalize the fix or keep it small.

Stop or recommend no rewrite when:

- The new prompt is only cosmetically different.
- The target output is not better than baseline.
- A schema or placeholder contract drifted.
- The improvement only works for one sample and weakens generality.

## Variable Templates

Rules for variable prompts:

- Preserve every placeholder exactly.
- Do not translate placeholder names unless the user explicitly asks.
- Add short variable meaning hints near the prompt if helpful.
- Keep stable instructions outside variables; keep changing facts inside variables.
- For Mustache-style templates, avoid nested syntax changes that could break rendering.

Useful pattern:

```text
Use the following runtime variables:
- {{audience}}: target readers
- {{topic}}: main subject
- {{tone}}: requested tone

Task:
[fixed instruction using {{audience}}, {{topic}}, {{tone}}]
```

## Multi-Message Prompts

When optimizing a selected message inside a conversation:

- Preserve the selected message's role. A user message must remain a user request; a system message must remain policy/behavior control.
- Maintain conversation style and prior assumptions.
- Use context to remove ambiguity, but do not copy large unrelated history into the selected message.
- If tools are present, keep tool names and schemas exact.

## Tool And Function Prompts

For tool-calling or function prompts, include:

- Tool selection criteria.
- Required and optional arguments.
- Exact schema or field names.
- Validation before calling.
- Error and fallback behavior.
- What not to expose to the user.

Avoid vague rules such as "use tools when needed" without criteria.

## Image Prompt Optimization

For natural-language image models, prefer coherent sentences over tag soup unless the target model expects tags. Cover:

- Subject identity, attributes, expression, pose, or action.
- Environment and spatial anchors.
- Composition, camera distance, angle, frame ratio, and focal point.
- Lighting direction, time, color palette, material/texture.
- Mood, style, medium, and quality constraints.
- Required text, layout, count, position, and negative constraints from the original.

Preserve hard visual constraints exactly: aspect ratio, orientation, readable text, count, positions, sequence, reference-image relationships, and all placeholders.

If the source prompt is JSON, keep JSON. If it is natural language, do not turn it into JSON unless asked.

## Output Discipline

Default final shape for simple optimization:

```text
[optimized prompt only]
```

For complex consulting-style requests:

```text
**Optimized Prompt**
[prompt]

**What Changed**
- [one or two high-signal changes]
```

Use the second form only when the user asked for explanation, comparison, or a professional review.
