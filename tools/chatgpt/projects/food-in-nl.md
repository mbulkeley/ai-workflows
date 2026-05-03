```md
# Food in NL

## Project Overview

A practical cooking assistant workflow tailored to the Netherlands, especially Albert Heijn and local shops.

The project focuses on turning available ingredients, vague meal ideas, and real kitchen constraints into concrete meals, recipes, and shopping lists.

## Main Goal

Reduce decision fatigue and food waste while improving meal quality by:

- Using what is already in the kitchen
- Generating realistic Netherlands-friendly recipes
- Producing actionable shopping lists and cooking steps

---

## How I Use ChatGPT

### Key Use Cases

- Ingredient-driven meal generation
- Recipe adaptation to constraints
- Translating dishes into Dutch grocery context
- Structured recipes and shopping lists
- Exploring variations (breakfast, grilling, comfort food)
- Live cooking adjustments

### Typical Tasks

- Meal ideation
- Recipe generation
- Substitutions and improvisation
- Ingredient clarification
- Iterative cooking support

---

## Prompting Patterns

### Constraint-First Prompts

I usually start with:

- Available ingredients
- Missing items
- Equipment limitations
- Local sourcing constraints

Example:

```

We have chicken thighs, spinach, and potatoes. What can we make? Grilling is an option.

```

---

### Progressive Refinement

Start broad → narrow down → finalize:

```

Give me 5 options.

```
```

Let's do the chicken one.

```
```

Give me just the recipe with exact amounts next to each step.

```

---

### Context Stacking

Add or correct information incrementally:

```

We also have potatoes.

```
```

No yogurt.

```
```

We do have mustard.

```

---

### Execution-Oriented Output

Preferred output:

- Exact amounts
- Quantities inline with steps
- Minimal prep assumptions
- Clear substitutions

---

### Localization

Implicit expectation:

- Ingredients available in the Netherlands
- Albert Heijn compatibility
- Realistic sourcing

Examples:

```

Shopping list for beef stroganoff in the Netherlands.

```
```

What kind of fish should I buy at Albert Heijn for this?

```

---

## Example Instructions

```

We have chicken thighs, spinach, and potatoes. What can we make? Grilling is an option.

```
```

We don't have yogurt. What should I replace it with in that recipe?

```
```

Give me just the recipe with exact amounts next to each step.

```
```

We didn't use the potatoes. What's a breakfast idea with potatoes, onions, and parmesan? No eggs.

```
```

Shopping list for beef stroganoff in the Netherlands.

```

---

## Iteration Style

Highly interactive and often real-time during cooking.

### Typical Loop

1. Start with available ingredients
2. Generate options
3. Select a direction
4. Convert to recipe
5. Adjust during cooking
6. Fix issues in-flight

---

### In-Flight Corrections

```

Oops, already used that ingredient.

```
```

This looks too dry. How should I adjust the sauce?

```
```

We didn't use the potatoes. What can I make with them tomorrow?

```

---

## Observations

### Strengths

ChatGPT works well as:

- A cooking thinking partner
- A constraint solver
- A recipe adapter
- A shopping-list generator
- A way to reduce decision friction

Especially useful when the starting point is incomplete.

---

### Limitations

Still requires human validation for:

- Ingredient availability
- Portion sizing
- Cooking times
- Texture and doneness
- Visual cues

---

## Abstractions

This project can be viewed as:

- Constraint-driven generation
- Interactive optimization loop
- Human-in-the-loop execution
- Stateful improvisation system

The kitchen state evolves over time, and ChatGPT adapts with it.

---

## Transferable Patterns

### Programming

```

Here is the current state. Here is what changed. What is the next best step?

```

### Writing

```

Draft → refine → constrain → finalize

```

### Product / Design

```

Start vague → add constraints → converge on practical output

```

### Data / Analysis

```

Feed partial inputs → refine query → adjust output

```

---

## Core Pattern

> Start broad → constrain aggressively → iterate with real-world feedback → converge on a practical solution.
```
