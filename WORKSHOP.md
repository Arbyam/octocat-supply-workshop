# 🐙 OctoCAT Supply – GitHub Copilot Hands-on Workshop

Welcome! In this workshop you'll use **GitHub Copilot** to add real features to the **OctoCAT Supply** website — planning, building, testing, reviewing, and securing your changes, all inside GitHub.

Everyone works in **their own copy** of the repo, so experiment freely. Nothing you do affects anyone else.

> [!NOTE]
> Copilot is non-deterministic: your results will look a bit different from your neighbour's. That's expected — if something goes sideways, ask Copilot to fix it or ask a facilitator.

---

## ✅ Before You Start

| You need | Notes |
|---|---|
| A GitHub account | Personal account is fine |
| A GitHub Copilot license | Copilot Pro, Pro+, Business or Enterprise. Modules 6–7 (Coding Agent & Code Review) need a paid plan with those features enabled |
| A modern browser | Everything runs in **GitHub Codespaces** — no local install required |

Optional: VS Code desktop with the GitHub Copilot extension if you'd rather work locally (requires Node.js 24+ and Make).

---

## 🗺️ Agenda (~3 hours)

| # | Module | Time | What you'll change |
|---|---|---|---|
| 0 | [Setup](#module-0--setup-15-min) | 15 min | Your own repo + running website |
| 1 | [Explore with Ask mode](#module-1--explore-the-codebase-with-ask-mode-10-min) | 10 min | Nothing — just understand the app |
| 2 | [Plan a feature with Vision](#module-2--plan-the-cart-feature-with-plan-mode--vision-15-min) | 15 min | A plan for the Cart |
| 3 | [Build it with Agent mode](#module-3--build-the-cart-with-agent-mode-30-min) | 30 min | **Cart page + Cart icon on the site** |
| 4 | [Custom Instructions](#module-4--teach-copilot-your-standards-with-custom-instructions-15-min) | 15 min | Observability on the Supplier API |
| 5 | [Prompt files & unit tests](#module-5--generate-unit-tests-with-a-prompt-file-15-min) | 15 min | New API tests |
| 6 | [Copilot Coding Agent](#module-6--delegate-work-to-copilot-coding-agent-20-min) | 20 min | A PR written by Copilot |
| 7 | [Copilot Code Review](#module-7--review-the-pr-with-copilot-code-review-10-min) | 10 min | AI review on your PR |
| 8 | [Security with Copilot](#module-8--find-and-fix-security-issues-15-min) | 15 min | A vulnerability fix |
| ⭐ | [Bonus challenges](#-bonus-challenges) | — | Your choice |

---

## Module 0 – Setup (15 min)

1. On this repository's page, click **Use this template** → **Create a new repository**.
   - **Owner:** your account
   - **Name:** `octocat-supply` (or anything you like)
   - **Visibility:** **Public** (this gives you free code scanning in Module 8)
2. In **your new repo**, click **Code** → **Codespaces** → **Create codespace on main**.
3. Wait for the Codespace to finish building. It automatically runs `make install && make build` (≈ 3–5 min — grab a coffee ☕).
4. In the terminal, start the app:

   ```bash
   make dev
   ```

5. When prompted, open port **5137** (Frontend) in the browser. You can also find it in the **Ports** tab.
6. Click **Products** in the nav bar, then click **Add to Cart** on any product.
   👉 Notice that all you get is a pop-up alert — **there is no cart**. You're about to build one.

> [!TIP]
> If products don't load, open the **Ports** tab and make sure port **3000** (API) visibility is **Public**.

---

## Module 1 – Explore the Codebase with Ask Mode (10 min)

1. Open **Copilot Chat** (Copilot icon in the top bar, or `Ctrl+Alt+I` / `Cmd+Ctrl+I`).
2. Make sure the mode dropdown is set to **Ask**.
3. Try these prompts:

   ```prompt
   Explain the architecture of this repository. What are the main parts and how do they talk to each other?
   ```

   ```prompt
   How does the Products page get its data? Walk me through the flow from the React component to the database.
   ```

   ```prompt
   Where is the "Add to Cart" button handled, and what does it currently do?
   ```

4. Click the file references in Copilot's answers to jump straight to the code.

**💡 Takeaway:** Copilot understands the *whole* repo, not just the open file. Ask mode is a safe way to learn a codebase — it doesn't change anything.

---

## Module 2 – Plan the Cart Feature with Plan Mode + Vision (15 min)

1. In the Explorer, open [`docs/design/cart.png`](docs/design/cart.png) — this is the designer's mockup.
2. In Copilot Chat, switch the mode to **Plan**.
3. Attach the image: drag `cart.png` into the chat box (or use the 📎 paperclip → select the file).
4. Prompt:

   ```prompt
   I need to implement a Cart page that matches this design. I also want a Cart icon in the NavBar that shows the number of items in the cart. Orders over $100 get free shipping, otherwise shipping is $25. Help me plan this.
   ```

5. Answer any clarifying questions Copilot asks (e.g. "persist the cart in localStorage? → yes").
6. Review the plan: which files will be created or changed?

**💡 Takeaway:** Copilot can *see* images. Planning first gives you a chance to steer before any code is written.

---

## Module 3 – Build the Cart with Agent Mode (30 min)

1. Either click **Start Implementation** on the plan, or switch the mode to **Agent** and prompt:

   ```prompt
   Implement the plan.
   ```

2. Watch Copilot work: it reads files, creates new ones (Cart context, Cart page, Cart icon), runs commands, and fixes its own errors.
3. When it asks permission to run terminal commands, review and click **Allow** / **Continue**.
4. When it's done, review the changes in the chat. You can **Keep** or **Undo** each file.
5. Go back to the website (refresh if needed) and test:
   - [ ] Add products → the cart icon count goes up
   - [ ] Click the cart icon → the Cart page opens
   - [ ] Change quantities and remove items
   - [ ] Subtotal under $100 → $25 shipping; over $100 → free shipping
   - [ ] The old "Added to cart" alert is gone
6. Something not quite right? Tell Copilot in plain English, e.g.:

   ```prompt
   The cart icon doesn't update until I refresh the page. Fix it.
   ```

7. Commit your work to a branch (needed for Module 7):

   ```bash
   git checkout -b feature/cart
   git add -A
   git commit -m "Add cart page and cart icon"
   git push -u origin feature/cart
   ```

   > Tip: Click the ✨ sparkle icon in the Source Control commit box to have Copilot write the commit message for you.

> [!TIP]
> **Short on time?** Run the pre-built prompt instead: open [`.github/prompts/demo-cart-page.prompt.md`](.github/prompts/demo-cart-page.prompt.md), attach `docs/design/cart.png`, and click ▶️ **Run**.

**💡 Takeaway:** Agent mode makes coordinated, multi-file changes, runs your build/tests, and self-corrects — while you stay in control of what gets kept.

---

## Module 4 – Teach Copilot Your Standards with Custom Instructions (15 min)

Copilot automatically reads [`.github/copilot-instructions.md`](.github/copilot-instructions.md) on every request. Let's teach it about a (fictional) internal logging library called **TAO**.

1. Open [`docs/tao.md`](docs/tao.md) and skim it — this is "our company's" observability library.
2. Open `.github/copilot-instructions.md` and add this to the bottom:

   ```markdown
   ## Additional Guidelines for REST APIs

   For REST APIs, use the following guidelines:

   * Use descriptive naming
   * Add Swagger docs for all API methods
   * Implement logging and monitoring using [TAO](../docs/tao.md)
     - assume TAO is installed and never add the package
   ```

3. In Copilot Chat (**Agent** mode), prompt:

   ```prompt
   Add observability to the Supplier route using our internal standards.
   ```

4. Notice that Copilot uses TAO even though you never mentioned it in the prompt.
5. **Undo** these changes afterwards (TAO isn't a real package, so the code won't compile).

**💡 Takeaway:** Custom instructions let your whole team get answers that follow *your* frameworks, conventions, and rules — automatically.

---

## Module 5 – Generate Unit Tests with a Prompt File (15 min)

Prompt files (`.github/prompts/*.prompt.md`) are reusable, shareable workflows for your team.

1. Open [`.github/prompts/demo-unit-test-coverage.prompt.md`](.github/prompts/demo-unit-test-coverage.prompt.md) and read how it's structured (mode, tools, detailed requirements).
2. Click ▶️ **Run** (top of the editor), or type `/demo-unit-test-coverage` in Copilot Chat.
3. Watch Copilot run the tests, measure coverage, write new tests, and fix any failures.
4. Verify:

   ```bash
   cd api && npm run test:coverage
   ```

**💡 Takeaway:** Prompt files turn your best prompts into one-click, repeatable team workflows.

---

## Module 6 – Delegate Work to Copilot Coding Agent (20 min)

Now let Copilot work **asynchronously** in the cloud while you do something else.

1. In your repo on **github.com**, go to **Issues** → **New issue** and create:

   - **Title:** `Add a "Clear cart" button to the Cart page`
   - **Body:**

     ```markdown
     Add a "Clear cart" button to the Cart page that removes all items after the user confirms.
     - Match the existing button styling and work in both light and dark mode
     - Hide the button when the cart is empty
     - Add a unit/component test
     ```

     > No cart yet on `main`? Use this instead: `Add a search box to the Products page that filters products by name as you type.`

2. On the right-hand side, under **Assignees**, assign the issue to **Copilot**.
3. Within a minute, Copilot reacts with 👀 and opens a **draft pull request**.
4. Open the PR and click **View session** to watch the agent think, edit code, and run tests live.
5. While it works (≈ 10–15 min), move on to Module 8 and come back later.
6. When it's done, review the PR. Want changes? Leave a PR comment mentioning `@copilot`, e.g. `@copilot also add an aria-label to the button`.

> [!NOTE]
> If you merged your cart into `main` in Module 3, Copilot can build on top of it. To do that, open a PR from `feature/cart` and merge it before you create the issue.

**💡 Takeaway:** The Coding Agent turns issues into pull requests in the background, running in a secure GitHub Actions environment and following your repo's instructions.

---

## Module 7 – Review the PR with Copilot Code Review (10 min)

1. Open a pull request from your `feature/cart` branch into `main` (**Pull requests** → **New pull request**).
2. In the PR sidebar, under **Reviewers**, request a review from **Copilot**.
3. After a minute or two, read Copilot's review comments.
4. For a suggestion you like, click **Commit suggestion**. Or reply with `@copilot please fix this` to hand it to the Coding Agent.
5. Merge the PR when you're happy. 🎉

**💡 Takeaway:** Copilot gives every PR a fast first review, so humans can focus on design and intent.

---

## Module 8 – Find and Fix Security Issues (15 min)

### Part A: Ask Copilot

1. In Copilot Chat (**Ask** mode), prompt:

   ```prompt
   Analyze this workspace and check for obvious security vulnerabilities. List them by severity.
   ```

2. Pick one finding (e.g. insecure CORS or missing security headers) and ask:

   ```prompt
   Generate a fix for the insecure CORS configuration.
   ```

### Part B: Code Scanning + Copilot Autofix

1. In your repo on github.com, go to the **Actions** tab and confirm the **CodeQL** workflow has run (it runs on every push).
2. Go to **Security** → **Code scanning** and open an alert.
3. Click **Generate fix** to have **Copilot Autofix** propose a patch, then **Create PR with fix**.

**💡 Takeaway:** Copilot helps you find *and* fix vulnerabilities, both while you code and automatically on every push.

---

## ⭐ Bonus Challenges

Finished early? Try any of these:

- **TDD with custom agents:** Pick `tdd-red` from the agents dropdown and ask it to *"Write a failing test for getting orders by branch ID"*. Then switch to `tdd-green` and ask it to *"Make the failing test pass"*. See the agents in [`.github/agents/`](.github/agents/).
- **Agent Skills:** Ask Agent mode to *"Add a new DeliveryVehicle API entity"* and watch it follow [`.github/skills/api-endpoint/SKILL.md`](.github/skills/api-endpoint/SKILL.md).
- **New design:** Attach [`docs/design/order-tracking-prototype.png`](docs/design/order-tracking-prototype.png) and have Copilot build an Order Tracking page.
- **Dark mode polish:** *"Review the Cart page for dark mode and accessibility issues and fix them."*
- **Copilot CLI:** In the terminal, run `copilot` and ask it to *"add a footer matching docs/design/footer.png"*.

---

## 🧹 After the Workshop

- Stop your Codespace: **github.com/codespaces** → **⋯** → **Stop**. Delete it when you're done to save your free hours.
- Your repo is yours to keep and keep experimenting with.

## 🆘 Troubleshooting

| Problem | Fix |
|---|---|
| Products page is empty / CORS error | **Ports** tab → set port 3000 to **Public**, then refresh |
| `make dev` says the port is in use | Stop the old process with `Ctrl+C`, or run `npx kill-port 3000 5137` |
| No **Plan** / **Agent** mode in the dropdown | Update the Copilot Chat extension and reload the window |
| Can't assign Copilot to an issue | Your Copilot plan or organization policy may not include the Coding Agent — pair with a neighbour |
| Copilot went off the rails | Click **Undo**, refine your prompt, and try again (or start a new chat) |
