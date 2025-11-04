# Algorütm - AI Development Prompts & Cursor Rules

A collection of production-tested AI prompts and Cursor rules, shared as part of the **Prompt of the Week** program.

## 📖 About

This repository accompanies the [Algorütm podcast episode on AI in Development](https://algorytm.captivate.fm/episode/25-09-algorutm-praktilisi-vtteid-ai-rakendamisest-arenduses), where [Soldera](https://soldera.io) shared practical approaches to using AI in software development after nearly 2 years of hands-on experience.

Following listener requests, we're releasing our internal prompts and Cursor rules **one per week** to help the Estonian tech community leverage AI more effectively in their development workflows.

---

## 🎯 What's Inside

### Current Prompts

#### **Python Backend Development Rules** (`python-backend.mdc`)

_Contribution by Soldera_

Comprehensive Flask/SQLAlchemy 2.0 guidelines with component-based architecture, Pydantic validation, RESTful API patterns, and service layer design. Includes migration strategies from legacy structures and testing best practices.

#### **Vue 3 Frontend Guidelines** (`frontend-vue.mdc`)

_Contribution by Soldera_

Vue 3.5 Composition API standards with TypeScript, Bootstrap 5, Pinia state management, and UI-based architecture. Covers component organization, Vuelidate validation, error handling, and declarative programming patterns for maintainable SPAs.

#### **Backend Testing Guidelines** (`backend-testing.mdc`)

_Contribution by Soldera_

Comprehensive backend testing strategies using unittest and BaseDBTestCase. Covers test organization, test data management, mocking external dependencies, and maintaining test isolation. Includes patterns for creating reusable test factories and fixtures.

#### **Frontend Testing Guidelines** (`frontend-testing.mdc`)

_Contribution by Soldera_

Testing standards for Vue 3 applications using spec files. Focuses on testing business logic over implementation details, organizing tests with describe blocks, and maintaining proper test isolation between test suites.

#### **Documentation Guidelines** (`documentation.mdc`)

_Contribution by Soldera_

Language-agnostic documentation standards emphasizing meaningful comments that explain "why" rather than "what". Includes guidelines for security, performance, and workaround annotations, plus principles for avoiding redundant documentation.

#### **Frontend Documentation Guidelines** (`frontend-documentation.mdc`)

_Contribution by Soldera_

TypeScript and Vue-specific documentation patterns using JSDoc. Covers function documentation with throws tags, complex type and interface documentation, Vue component annotations, and API integration documentation strategies.

#### **Spec Management System** (`spec-rules.mdc`)

_Contribution by Soldera_

Comprehensive specification-driven development workflow for managing features from planning to implementation. Includes spec templates, file organization patterns, AI-assisted implementation workflows, and domain-specific context for renewable energy GO trading platforms. Covers architecture patterns, implementation standards, and progress tracking.

---

## 🚀 How to Use

### For Cursor Users

1. **Copy the rule file** to your project's `.cursor/rules/` directory
2. **Configure the glob pattern** in the frontmatter (e.g., `globs: **/*.py`)
3. **Set `alwaysApply`** to `true` (for global rules) or `false` (for file-specific rules)
4. Cursor will automatically apply these rules when working on matching files

### General Use

These prompts can be adapted for:

- Other AI coding assistants (GitHub Copilot, Claude, etc.)
- Team coding guidelines documentation
- Code review checklists
- Onboarding materials

---

## 📅 Prompt of the Week Program

Every week, we'll release a new prompt covering different aspects of AI-assisted development:

- **Backend Development** ✅
- **Frontend Development** ✅
- **Backend Testing** ✅
- **Frontend Testing** ✅
- **Documentation Standards** ✅
- **Spec-Driven Development** ✅
- DevOps & infrastructure (coming soon)
- Code review prompts (coming soon)
- And more...

Follow along on the [Algorütm Facebook Group](https://www.facebook.com/groups/736912960166810) for weekly releases!

---

## 🤝 Contributing

These prompts reflect real-world usage at Soldera. If you have suggestions or want to share your own battle-tested prompts, feel free to:

1. Open an issue with your suggestion
2. Submit a pull request with your prompt (include attribution)
3. Share feedback on the [Algorütm Discord](https://discord.gg/8X5JTkDxcc)

---

## 📚 Resources

- **Podcast Episode**: [Praktilisi võtteid AI rakendamisest arenduses](https://algorytm.captivate.fm/episode/25-09-algorutm-praktilisi-vtteid-ai-rakendamisest-arenduses)
- **Soldera**: [https://soldera.io](https://soldera.io)
- **Algorütm Podcast**: [https://algorytm.captivate.fm](https://algorytm.captivate.fm)

---

## 📜 License

These prompts are shared freely for the benefit of the developer community. Attribution to Soldera is appreciated when sharing or adapting these materials.

---

_Brought to you by the Algorütm podcast and Soldera team. Making AI practical for Estonian developers, one prompt at a time._ 🇪🇪
