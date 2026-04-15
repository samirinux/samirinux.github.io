I'll search for the latest official information on Claude Code skills to make sure I provide accurate, up-to-date guidance.Here's a comprehensive guide to implementing Claude Code skills, with tips for efficiency and all the official resources.

---

## What Is a Skill?

A skill is a directory containing a `SKILL.md` file — a markdown document with YAML frontmatter — that teaches Claude how to perform a specific task in a repeatable way. At its simplest, a skill is a directory that contains a SKILL.md file, which must start with YAML frontmatter containing required metadata: name and description. Skills can also bundle scripts, reference files, templates, and other assets alongside the core markdown.

---

## Step-by-Step Implementation

**Step 1: Identify the gap.** Start with evaluation — identify specific gaps in your agents' capabilities by running them on representative tasks and observing where they struggle or require additional context. Before writing anything, identify 2–3 concrete use cases your skill should enable.

**Step 2: Create the directory structure.** Skills are simple to create — just a folder with a SKILL.md file containing YAML frontmatter and instructions. The typical layout looks like:

```text
your-project/
└── .claude/
    └── skills/
        ├── my-skill/
        │   ├── SKILL.md          # Required
        │   ├── scripts/          # Optional executables
        │   ├── references/       # Optional docs loaded on demand
        │   └── assets/           # Optional templates, icons, fonts
```

For global skills available across all projects, place them in `~/.claude/skills/`.

**Step 3: Write the YAML frontmatter.** The frontmatter requires two fields:

```yaml
---
name: my-skill-name
description: Brief description of what this Skill does and when to use it
---
```

The name field must use lowercase letters, numbers, and hyphens only (max 64 characters). The description enables skill discovery and should include both what the skill does and when to use it (max 1024 characters). Anthropic recommends using gerund form (verb + -ing) for names, like `generating-reports` or `reviewing-code`.

**Step 4: Write the markdown body.** Below the frontmatter, write clear instructions in markdown. The markdown content contains the instructions, examples, and guidelines that Claude will follow. A good template:

```markdown
---
name: my-skill-name
description: Does X when user asks for Y or Z
---

# My Skill Name

## Instructions
[Step-by-step guidance for Claude]

## Examples
[Concrete examples of input/output]

## Guidelines
[Constraints, edge cases, formatting rules]
```

**Step 5: Add bundled resources (optional).** As skills grow in complexity, they may contain too much context to fit into a single SKILL.md. Skills can bundle additional files within the skill directory and reference them by name from SKILL.md. For example, a PDF skill might reference a separate `FORMS.md` for form-filling instructions, keeping the core file lean.

**Step 6: Test and iterate.** Anthropic recommends a "Claude A / Claude B" workflow: use one Claude instance to help refine the skill, and a separate fresh instance (with the skill loaded) to test it on real tasks. Observe whether Claude B finds the right information, applies rules correctly, and handles the task successfully.

**Step 7: Install or upload.** In Claude.ai, go to Customize > Skills and upload a ZIP of your skill folder. In Claude Code, place the folder in `.claude/skills/` in your project or `~/.claude/skills/` for global access. For the API, use the skills endpoint.

---

## Tips and Tricks for Efficient Skills

**Write precise descriptions.** The description field in SKILL.md frontmatter is the primary mechanism that determines whether Claude invokes a skill. Use third person, include trigger keywords, and be specific about when the skill applies. Always write in third person — the description is injected into the system prompt, and inconsistent point-of-view can cause discovery problems.

**Make descriptions slightly "pushy."** Instead of "How to build a simple fast dashboard," you might write something that also says "Make sure to use this skill whenever the user mentions dashboards, data visualization, internal metrics, or wants to display any kind of company data."

**Keep SKILL.md concise.** Keep SKILL.md body under 500 lines for optimal performance. If your content exceeds this, split it into separate files using progressive disclosure patterns.

**Use progressive disclosure.** Claude reads only the files needed for each specific task. A skill can include dozens of reference files, but if the task only needs one, Claude loads just that file — the rest remain on the filesystem consuming zero tokens.

**Prefer scripts over inline code.** When Claude runs a script, the script's code never loads into the context window — only the output consumes tokens. This makes scripts far more efficient than having Claude generate equivalent code on the fly.

**Don't explain what Claude already knows.** When reviewing skill drafts, remove unnecessary background explanations. Claude understands common concepts; the skill should focus on your specific workflow, conventions, and edge cases.

**Avoid overlapping descriptions.** If two skills have similar descriptions, Claude may pick the wrong one. Make each description distinct enough that there's no ambiguity.

**Understand skill lifecycle in context.** When you invoke a skill, the rendered SKILL.md content enters the conversation as a single message and stays there for the rest of the session. Claude Code does not re-read the skill file on later turns. Write guidance as standing instructions, not one-time steps. During auto-compaction, Claude re-attaches the most recent invocation of each skill (first 5,000 tokens each, up to 25,000 total).

**Use context: fork for task-based skills.** Skills with `context: fork` run in a forked subagent. The skill content becomes the task, and the agent provides tools optimized for the work. This is great for research or exploratory tasks that benefit from isolation.

**Evaluate with test queries.** Create 20 eval queries (a mix of should-trigger and should-not-trigger) to test whether your description reliably matches the right requests.

---

## Official Resources

Here are all the relevant official Anthropic resources:

- **Skills Overview (API Docs):** https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview
- **Skill Authoring Best Practices:** https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices (also mirrored at https://docs.claude.com/en/docs/agents-and-tools/agent-skills/best-practices)
- **Engineering Blog — "Equipping agents for the real world with Agent Skills":** https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills
- **The Complete Guide to Building Skills for Claude (PDF):** https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf
- **How to Create Custom Skills (Help Center):** https://support.claude.com/en/articles/12512198-how-to-create-custom-skills
- **Using Skills in Claude (Help Center):** https://support.claude.com/en/articles/12512180-use-skills-in-claude
- **Anthropic's Official Skills Repository (GitHub):** https://github.com/anthropics/skills
- **Agent Skills Open Standard:** https://agentskills.io
- **Anthropic Academy — Introduction to Agent Skills (course):** https://anthropic.skilljar.com/introduction-to-agent-skills
- **Claude Code Skills Docs:** https://code.claude.com/docs/en/skills