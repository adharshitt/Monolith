# Google Style Guides Analysis Report

This report presents a detailed analysis and comparison of the official Google Style Guides across six core development technologies: **C++**, **Python**, **Java**, **JavaScript**, **Shell (Bash)**, and **HTML/CSS**.

---

## 1. Core Styling Philosophy

Across all languages, Google's style guides center on three fundamental pillars:
1. **Optimize for the Reader, Not the Writer:** Code is read far more often than it is written. Clean, predictable styles reduce cognitive overhead for code reviewers and future maintainers.
2. **Be Mindful of Scale:** In a monorepo of hundreds of millions of lines of code with thousands of engineers, small choices (like polluting the global namespace) can lead to catastrophic refactoring costs or name collisions.
3. **Consistency is King:** The guide emphasizes that "consistency is more important than individual preferences." If a local project style differs slightly, developers must conform to the local conventions.

---

## 2. Structural & Formatting Standards

Below is a comparative matrix of structural formatting rules:

| Language/Tech | Indentation Style | Line Length Limit | Core Formatting & Line Wrap Exceptions |
| :--- | :--- | :--- | :--- |
| **C++** | 2 spaces | 80 characters | Headers/Includes, URLs in comments, template arguments if wrapping hurts readability. |
| **Python** | 4 spaces | 80 characters | Long import paths, URLs in comments/docstrings, type comments. |
| **Java** | 2 spaces | 100 characters | Package statements, import statements, long Javadoc URLs, command-line arguments. |
| **JavaScript** | 2 spaces | 80 characters | ES modules import/export statements, `goog.module` declarations, long URLs/RegExes. |
| **Shell (Bash)** | 2 spaces | 80 characters | Long command strings can use here-documents (`cat <<EOF`) or variables to split lines. |
| **HTML/CSS** | 2 spaces | No hard limit | HTML line wrapping is optional but recommended to align attributes. CSS properties can be alphabetized. |

> [!IMPORTANT]
> **No Tabs Allowed:** Google strictly prohibits the use of raw tab characters (`\t`) for indentation across all language style guides. Editors must be configured to emit spaces when the tab key is pressed.

---

## 3. Naming Conventions

The naming style immediately informs the reader about the entity's nature (class, local variable, constant) without looking up its declaration.

| Entity | C++ | Python | Java / JavaScript | Shell (Bash) | HTML/CSS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **File Names** | `lower_with_under.cc`<br>`my_header.h` | `lower_with_under.py` *(No dashes)* | Matches class name (`MyClass.java`) | `lower_with_under.sh` *(No dashes)* | `lower-with-hyphen.html` |
| **Classes / Types** | `CamelCase`<br>(PascalCase) | `CapWords`<br>(PascalCase) | `PascalCase` | *N/A* | *N/A* |
| **Functions / Methods** | `CamelCase`<br>(PascalCase) | `lower_with_under` | `camelCase` | `lower_with_under`<br>`pkg::func` *(libraries)* | *N/A* |
| **Local Variables** | `snake_case` | `lower_with_under` | `camelCase` | `lower_with_under` | *N/A* |
| **Class Data Members** | `snake_case_` *(trailing `_`)* | `_protected_var`<br>`__private_var` *(discouraged)* | `camelCase` | *N/A* | *N/A* |
| **Constants** | `kPascalCase` *(e.g., `kMaxConnections`)* | `ALL_CAPS` | `UPPER_SNAKE_CASE` | `UPPER_SNAKE_CASE` *(readonly)* | *N/A* |
| **Macros** | `ALL_CAPS` | *N/A* | *N/A* | *N/A* | *N/A* |
| **CSS Selectors** | *N/A* | *N/A* | *N/A* | *N/A* | `.ads-sample`<br>*(hyphen-separated)* |

---

## 4. Exception Handling Policies

Handling errors in Google code is highly structured and varies significantly due to legacy codebase scale.

> [!WARNING]
> **C++ Exception Ban:** Google forbids the use of standard C++ exceptions (`throw`/`catch`). 
> * **Rationale:** Legacy code is not exception-safe, and enabling exceptions makes control flow tracing extremely difficult. 
> * **Alternative:** Functions must return `absl::Status` or `absl::StatusOr<T>` to indicate errors explicitly.

* **Python:** Exceptions are allowed but must be handled carefully. Developers should use built-in exceptions (`ValueError`, `TypeError`) for precondition checks and validation. Do not use `assert` blocks for application logic, as they may be optimized away.
* **Java:** It is **never** correct to catch an exception and do nothing. If an exception must be ignored, the developer must:
  1. Write a comment explaining why it is safe to do so.
  2. Rename the caught variable to `unused` or `_` if supported by the compiler to signal intent.
  * Rethrowing impossible exceptions as `AssertionError` is standard practice.
* **JavaScript:** Standard exceptions are allowed and follow typical JS error handling blocks.

---

## 5. Imports and Sourcing Rules

To manage dependency graphs and build systems at scale:

* **Java (No Wildcards):** Static and standard wildcard imports (e.g., `import java.util.*;`) are **strictly forbidden**. Every dependency must be imported individually to ensure clear origin tracking and compilation efficiency.
* **Python (Absolute Imports Only):** Relative imports are forbidden. All imports must be absolute (e.g., `from package.subpackage import module`). Group imports in three sections: standard library, third-party libraries, and local project modules.
* **C++ (Include What You Use):** Source files must include everything they depend on directly. Do not rely on transitive inclusions. Includes are ordered strictly (local headers first, then system library headers).
* **Shell (Bash Local Scope):** Inside functions, all variables must be declared with the `local` keyword to prevent namespace pollution. 
  > [!TIP]
  > **Command Substitution Scope:** When assigning a command output to a local variable, separate the declaration and assignment to avoid masking exit codes:
  > ```bash
  > # RECOMMENDED
  > local output
  > output=$(command)
  > 
  > # AVOID (hides the return code of 'command')
  > local output=$(command)
  > ```
* **HTML/CSS (Separation of Concerns):** Keep structure (HTML), presentation (CSS), and behavior (JS) separate. Inline styles and inline JS `onclick` attributes are forbidden.

---

## 6. HTML and CSS Specifics

* **Lowercase Only:** All HTML element names, attributes, CSS properties, and CSS values (except strings) must be written in lowercase. Hex color codes must be lowercase (e.g., `#ffffff`, not `#FFFFFF`).
* **HTML Doctype:** Always begin HTML pages with `<!doctype html>` to avoid triggering legacy quirks mode in browsers.
* **Unnecessary IDs:** Avoid using ID attributes (`id="..."`) for styling or scripting wherever possible. Use class attributes for styling and `data-*` attributes for JavaScript references.
  * *Reasoning:* IDs are exposed as named global properties on the `window` object in browsers, which can pollute the JS global namespace.
  * *Hyphen Rule:* If an ID is required, it must include a hyphen (e.g., `user-profile`) which prevents it from being referenced as a standard JavaScript global variable.
* **CSS Hacks & `!important`:** Google discourages writing browser hacks or overriding the cascade with `!important` unless resolving third-party integration constraints.
