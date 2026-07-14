### Code Quality & Architecture Rules

1. **Form Management & Validation:**
   - All forms must be implemented using `react-hook-form` paired with a `zod` validation schema.
   - Raw React `useState` hooks for input fields or uncontrolled HTML inputs are strictly forbidden.
   - All validation logic must reside within the schema definition, not inline within component functions.

2. **Form Accessibility (a11y):**
   - Every input element must have a corresponding `<label>` element explicitly linked via `htmlFor`.
   - Dynamic error messages must be rendered within a container that has a unique `id`, and the corresponding input must reference this ID using `aria-describedby` when invalid.
   - Inputs must programmatically toggle `aria-invalid="true"` when the field fails schema validation.

3. **Data Integrity & Edge Cases:**
   - All string inputs within a Zod schema must explicitly include the `.trim()` modifier to prevent whitespace-only submissions from passing validation.
   - Any password or sensitive configuration input must be verified through a secondary confirmation field using a Zod `.refine()` block to enforce match equality before submission.