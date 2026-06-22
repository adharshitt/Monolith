---
name: Monolith
description: Enforces language-specific style guides (C++, Python, Java, JS, Shell, HTML/CSS), API specifications, database design, commit hygiene, and automated merge gates for AI editors and agents.
---

# Monolith

Monolith configures AI editors and coding agents to enforce our organization's highly classified coding structures, style guides, and lifecycle policies.

By activating this skill, the AI editor will automatically adhere to the style rules, exception handling rules, dependency bounds, and architecture guidelines.

---

## ️ Global Core Directives for AI Editors

1. **Automatic Formatting Compliance:**
   - Always output code matching the designated indent and column parameters.
   - Never use raw tabs (`\t`) for indentation.
   - Do not engage in formatting debates; always format code programmatically.

2. **Naming Scoping Safety:**
   - Trailing underscores for C++ class member variables (`data_member_`).
   - Leading underscores for Python protected variables (`_protected_member`).
   - `local` scope separation for Shell variables to avoid global pollution.

3. **Exception Safety & Reliability:**
   - In C++, use `absl::Status` or `absl::StatusOr` instead of exceptions.
   - In Java, never swallow exceptions. Add explanation comments or rename variable to `unused`.

4. **Multi-File & Folder Structure Organization:**
   - Always structure codebases into clean multi-file modules and logical folder hierarchies rather than single monolithic files.
   - Align folder structures to the specific language standards (e.g., modular directory mapping for Go, package namespaces for Java, module packages for Python).
   - Ensure folder naming is clean, consistent, and separates entrypoints (executable mains/runners) from core library logic.

---



## Automatic Coding Architecture Selection (Orchestrator Rules)

When analyzing a workspace or adding new code, the AI editor must dynamically select the appropriate reference guidelines from the references database based on the target files:

* C++ files (.cc, .cpp, .h): Use [cpp_guide.md](references/cpp_guide.md)
* Python files (.py): Use [python_guide.md](references/python_guide.md)
* Java files (.java): Use [java_guide.md](references/java_guide.md)
* JavaScript / TypeScript files (.js, .jsx, .ts, .tsx): Use [js_guide.md](references/js_guide.md) & [typescript_guide.md](references/typescript_guide.md)
* Shell scripts (.sh): Use [shell_guide.md](references/shell_guide.md)
* HTML / CSS files (.html, .css): Use [htmlcss_guide.md](references/htmlcss_guide.md)
* SQL queries (.sql): Read SQL rules in [awesome_guidelines.md](references/awesome_guidelines.md)
* APIs and Enterprise Lifecycle: Read [enterprise_policies.md](references/enterprise_policies.md) and [awesome_guidelines.md](references/awesome_guidelines.md)

If a task spans multiple language environments, the editor must execute separate validation passes corresponding to each active reference file.

##  Reference Guidelines (Token-Efficient Index)

To keep this customization token-efficient, the complete guides (representing the ~1MB+ style knowledge base) are stored as external references. Read these files *only* when working on their respective languages or tasks:

*  **C++ Guidelines:** [cpp_guide.md](references/cpp_guide.md)
*  **Python Guidelines:** [python_guide.md](references/python_guide.md)
*  **Java Guidelines:** [java_guide.md](references/java_guide.md)
*  **JavaScript/TypeScript Guidelines:** [js_guide.md](references/js_guide.md)
*  **Shell Scripting Guidelines:** [shell_guide.md](references/shell_guide.md)
*  **HTML/CSS Guidelines:** [htmlcss_guide.md](references/htmlcss_guide.md)
*  **Enterprise & Lifecycle Policies:** [enterprise_policies.md](references/enterprise_policies.md)
*  **Awesome Guidelines Index:** [awesome_guidelines.md](references/awesome_guidelines.md)
*  **C# Guidelines:** [csharp_guide.md](references/csharp_guide.md)
*  **Go Guidelines:** [go_guide.md](references/go_guide.md)
*  **Kotlin Guidelines:** [kotlin_guide.md](references/kotlin_guide.md)
*  **Swift Guidelines:** [swift_guide.md](references/swift_guide.md)
*  **Typescript Guidelines:** [typescript_guide.md](references/typescript_guide.md)
*  **Objective-C Guidelines:** [objc_guide.md](references/objc_guide.md)
*  **R Guidelines:** [r_guide.md](references/r_guide.md)
*  **Vim Script Guidelines:** [vimscript_guide.md](references/vimscript_guide.md)
*  **XML Guidelines:** [xml_guide.md](references/xml_guide.md)
*  **JSON Guidelines:** [json_guide.md](references/json_guide.md)
*  **Common Lisp Guidelines:** [lisp_guide.md](references/lisp_guide.md)
*  **AngularJS Guidelines:** [angularjs_guide.md](references/angularjs_guide.md)
*  **Markdown Guidelines:** [markdown_guide.md](references/markdown_guide.md)
*  **Dart Guidelines:** [dart_guide.md](references/dart_guide.md)
