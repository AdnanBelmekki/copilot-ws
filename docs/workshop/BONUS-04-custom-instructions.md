# BONUS 04 — Custom Instructions Deep Dive

**Self-paced bonus task**  
**Feature:** Advanced `copilot-instructions.md` patterns

---

## Recap

`.github/copilot-instructions.md` shapes every Copilot interaction in the repo. This bonus task explores advanced patterns.

---

## Task 1 — Role-specific instructions

Add a section for a specific SDLC role. For example, a **QA Engineer** persona:

```markdown
## QA Mode
When asked to write tests:
- Always cover the happy path AND at least two error cases.
- Prefer descriptive test names using the format "should [action] when [condition]".
- Mock external dependencies; never make real HTTP calls in unit tests.
- Include an assertion on the response status code in every API test.
```

Test it:
```
Write a test for the createOrder endpoint
```

---

## Task 2 — Negative instructions

Negative rules ("never do X") can be just as powerful as positive ones:

```markdown
## Anti-patterns to avoid
- Never use `any` in TypeScript — use explicit types or `unknown`.
- Never use `var` in Kotlin — use `val` or `var` only when mutation is needed.
- Never catch exceptions silently (no empty catch blocks).
- Never expose the H2 console in production configuration.
```

Test it by asking Copilot to generate code that would normally use `any` or an empty catch, and see if it complies.

---

## Task 3 — Inline code examples

Instructions can include code snippets to show preferred patterns:

```markdown
## Preferred patterns

### TypeScript API calls
Always use this pattern for API calls:
```typescript
export async function fetchSomething(): Promise<Something> {
  const response = await fetch(`${BASE_URL}/something`)
  if (!response.ok) throw new Error(`HTTP ${response.status}`)
  return response.json() as Promise<Something>
}
```

### Kotlin service methods
Always inject repositories via constructor and return domain objects, not DTOs:
```kotlin
@Service
class ProductService(private val productRepository: ProductRepository) {
    fun getAll(): List<Product> = productRepository.findAll()
}
```
```

---

## Task 4 — Test the limits

Try to get Copilot to violate your instructions:
```
Write a TypeScript function that uses the any type
```
```
Write a Kotlin function that uses var for a constant
```

If Copilot ignores your rules, refine the wording — more specific language tends to work better than vague guidance.

---

## Task 5 — copilot-setup-steps.yml

Open `.github/copilot-setup-steps.yml`. Add or verify steps to pre-install dependencies needed for testing and browser automation:

```yaml
- name: Install Playwright browsers
  run: npx playwright install

- name: Pre-fetch Gradle dependencies
  run: cd backend && ./gradlew dependencies --no-daemon
```

Ask Copilot to explain why these steps are important:
```
Why are Playwright browsers and Gradle dependencies pre-installed in copilot-setup-steps.yml? How does this affect testing performance?
```

---

## 💡 Key takeaways

- Instructions are most effective when they are **specific, verifiable, and include examples**
- Negative instructions ("never use `any`") prevent common anti-patterns automatically
- `copilot-setup-steps.yml` is your testing environment setup — treat it like a Dockerfile for CI/test machines
- Both files are version-controlled: your team's Copilot settings travel with the code
