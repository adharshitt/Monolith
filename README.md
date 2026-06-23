# Monolith

### Improvise your AI-generated code structure


**Monolith** is an open-source, token-efficient custom skill for AI editors and agents (including Google Antigravity and other LLM coding interfaces). 

AI editors are incredibly fast at writing code, but left to their own devices, they frequently output monolithic, single-file scripts or disorganized code structures. **Monolith** enforces strict, clean multi-file conventions, language-specific layouts, and formatting constraints directly inside the AI's agentic context loop.

---

##  Key Features

*    **Multi-File & Folder Structure Enforcement:** Forces AI agents to decompose logic into clean directories and modular files (e.g. Go `/cmd` and `/internal`, Python package structures, Java package directories) rather than monolithic modules.
*    **Token-Efficient References:** Includes **22 language and formatting guides** stored in a modular `references/` directory. The AI agent only fetches the specific files it needs for the task at hand, keeping context sizes small and saving API tokens.
*    **Core Scoping & Safety Constraints:** Embeds language-specific namespace rules (like C++ trailing class underscores `member_`, Python protected single underscores `_member`, and Shell `local` scoping).
*   ️ **Modern Exception Policies:** Explicitly directs AI agents away from code smells, such as banning raw C++ exceptions (opting for `absl::Status` objects) and banning swallowed Java catches.
*    **Plug-and-Play Integration:** Fully compatible with Google Antigravity custom skill roots and standard `skills.json` workspace files.

---

##  Directory Structure

```text
├── skills.json                        # Shared custom skill configuration
├── LICENSE                            # MIT License
├── README.md                          # Repository Documentation
└── skills/
    └── monolith/
        ├── SKILL.md                   # Main semantic entrypoint for AI editors
        └── references/                # Token-efficient style guide databases
            ├── cpp_guide.md           # Google C++ Style
            ├── python_guide.md        # Google Python Style
            ├── java_guide.md          # Google Java Style
            ├── js_guide.md            # Google JavaScript Style
            ├── typescript_guide.md    # Google TypeScript Style
            ├── go_guide.md            # Google Go Style
            ├── kotlin_guide.md        # Google Kotlin Style
            ├── swift_guide.md         # Google Swift Style
            ├── shell_guide.md         # Google Bash/Shell Style
            ├── htmlcss_guide.md       # Google HTML/CSS Style
            ├── enterprise_policies.md # Augment 10 Enterprise Policies
            ├── awesome_guidelines.md  # General SQL, MongoDB, and API Guide index
            └── ... (10+ other languages)
```

---

## ️ How It Works (Token Efficiency)

Standard AI prompts are expensive. Including full style manuals directly inside the AI's system prompt bloats token usage. 

Monolith uses a **semantic YAML trigger** inside the [SKILL.md](skills/monolith/SKILL.md) entrypoint:

```yaml
---
name: Monolith
description: Enforces language-specific style guides (C++, Python, Java, JS, Shell, HTML/CSS), API specifications, database design, commit hygiene, and automated merge gates for AI editors and agents.
---
```

When you prompt your editor with instructions like *"implement this API using our ai-coding-standards"* or *"review my python formatting"*, the AI editor's customizations engine matches these keywords and loads **only the top-level SKILL.md**. The agent then semantically resolves and reads the specific reference page (e.g. `python_guide.md`) *only when executing that specific language edit*.

---

##  Installation & Setup

### Option 1: Standard Workspace Setup (Recommended)
To activate Monolith for a specific project:
1. Clone this repository into your project root as a submodule or nested folder:
   ```bash
   git clone https://github.com/Kristories/monolith.git
   ```
2. Update your workspace's `.agents/skills.json` file to inherit this skill path:
   ```json
   {
     "inherits": [
       { "path": "path/to/cloned/monolith/skills.json" }
     ]
   }
   ```

### Option 2: Global Setup (All Workspaces)
To make Monolith active globally across every project on your machine, copy the skill folder directly into your global configurations root:
```bash
# For Google Antigravity / agy CLI
mkdir -p ~/.gemini/config/skills/
cp -r skills/monolith ~/.gemini/config/skills/
```

---

##  Sources & Bibliography
The guidelines and rules curated in Monolith are synthesized from these official industry-standard open-source style sheets:
*   [Google Open Source Style Guides](https://google.github.io/styleguide/) (C++, Java, Python, Go, Swift, Kotlin, JS, HTML/CSS)
*   [Augment Code 10 Enterprise Coding Standards](https://www.augmentcode.com/guides/10-enterprise-coding-standards-every-dev-org-needs) (Refactoring budgets, error observability, CI/CD gates)
*   [Simon Holywell SQL Style Guide](https://www.sqlstyle.guide/) (Uppercase queries, explicit joins)
*   [Microsoft REST API Guidelines](https://github.com/Microsoft/api-guidelines) (Collection pagination, async operations)
*   [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) (Var ban, literal creation)

---

##  License
Monolith is open-source software licensed under the [MIT License](LICENSE).
