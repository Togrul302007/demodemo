This document analyzes the architectural, functional, and accessibility differences between a Profile Settings Form built using two different AI interaction models.

Round 1 Branch (feat-settings-vague): Built using a lazy, single-sentence prompt.

Round 2 Branch (feat-settings-precise): Built using a structured prompt with constraints, file references, and a verification loop.

1. Correctness & Architecture
The architectural gap between the two rounds is immense:

feat-settings-vague (Round 1): The AI relied on basic React useState hooks for each input field. While it "worked," the entire component re-rendered on every single keystroke. It utilized raw inline validation checks upon form submission.

feat-settings-precise (Round 2): The AI utilized React Hook Form combined with a Zod validation schema. Form state changes are isolated, eliminating unnecessary re-renders. The validation is declarative, separation of concerns is maintained, and schema types are strictly inferred to ensure TypeScript safety.

2. Accessibility (a11y)
Accessibility was completely ignored in Round 1 and systematically implemented in Round 2.

Round 1: HTML inputs used basic placeholder text without semantic <label> elements or visual associations. Screen readers would have struggled to identify the fields.

Round 2: The precise prompt forced the AI to implement proper a11y standards:

Input fields are explicitly linked to labels using htmlFor.

Error elements are assigned unique id attributes and connected to inputs via aria-describedby.

Inputs programmatically toggle aria-invalid="true" when validation fails.

3. Edge Cases
The behavior of both implementations under unexpected conditions reveals the fragility of lazy code:

Vague Implementation: Trimming whitespace was ignored. An input consisting only of spaces ("   ") was accepted as a valid username.

Precise Implementation: The Zod schema incorporated .trim() and .min(2) modifiers. Edge cases—such as trying to submit special characters or checking password confirmation mismatches—were handled instantly on the client side before hitting any mock submit handler.

4. Review Effort & Time
Round 1 (Fast start, slow finish): It took under 30 seconds to prompt and get the code. However, making it production-ready (manually fixing performance lags, writing validation logic, and writing tests from scratch) would have taken 1.5 to 2 hours of intense manual coding.

Round 2 (Slow start, fast finish): Crafting the prompt took 5 minutes, and watching the AI iterate in Plan Mode took another 3 minutes. However, because it generated high-quality code along with passing unit tests (Vitest) right out of the box, the total time to ship was under 15 minutes.

5. Caught AI Mistake
The Mistake: In the first iteration of the precise round, the AI attempted to validate a custom phone number field inside the Zod schema using a naive regex pattern: /^\d{10}$/.

The Issue & Fix: This rejected standard international phone formats (e.g., +1-234-567-8900). Since my precise prompt demanded automated unit tests, the test suite immediately caught this bug by failing on the test case should accept valid international phone numbers. I instructed the AI to update the regex to support optional country codes and hyphens, which resolved the issue immediately.