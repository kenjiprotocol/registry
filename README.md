# Kenji Protocol

**Kenji** is a CLI for installing and managing AI agent instruction sets.

Instead of copying prompts, rules, or SKILL files across projects, Kenji allows you to install them directly into your project with a simple command.

> Think of it as package management for AI agent capabilities.

---

## Installation

```bash
npm install -g @kenji-protocol/kenji
```

Verify installation:

```bash
kenji doctor
```

---

## What Kenji Installs

Kenji installs instruction sets such as:

- `SKILL.md`
- prompt files
- agent rules
- development workflows

These are installed into your project so AI agents can access them during development.

---

## Core Commands

### Search the registry

```bash
kenji search <keyword>
```

**Example:**

```bash
kenji search react
```

---

### Inspect a skill

```bash
kenji info <name>
```

**Examples:**

```bash
kenji info kenji/react-debug
kenji info user/repo
```

---

### Install a skill

Kenji supports multiple install sources.

#### Registry

```bash
kenji install react-debug
```

or

```bash
kenji install kenji/react-debug
```

---

#### GitHub repo

```bash
kenji install user/repo
```

**Example:**

```bash
kenji install anthropics/prompt-engineering
```

---

#### Raw instruction file

```bash
kenji install https://raw.githubusercontent.com/.../SKILL.md
```

---

### Listing installed skills

**Local project skills:**

```bash
kenji list
```

**Global skills:**

```bash
kenji list --global
```

---

### Removing a skill

**Local:**

```bash
kenji remove <skill>
```

**Global:**

```bash
kenji remove <skill> --global
```

---

## Stacks

Stacks allow installing multiple skills together.

**Create a stack:**

```bash
kenji stack create saas-stack
```

**Add skills:**

```bash
kenji stack add saas-stack user/repo
```

**Install stack:**

```bash
kenji stack install saas-stack
```

**Export stack:**

```bash
kenji stack export saas-stack
```

**Import stack:**

```bash
kenji stack import user/repo
```

---

## Registry Cache

Kenji caches the registry locally for performance.

Refresh the cache manually:

```bash
kenji registry update
```

---

## Project vs Global Skills

Kenji supports two installation modes.

### Project install *(default)*

Skills install into the current project folder so agents can access them.

```
./.kenji/skills/
```

---

### Global install

```bash
kenji install <skill> --global
```

Installed to:

```
~/.kenji/skills/
```

---

## Registry

Kenji's registry is stored on GitHub:

[github.com/kenjiprotocol/registry](https://github.com/kenjiprotocol/registry)

Skills and stacks are indexed there and discovered through the CLI.

---

## Vision

Kenji aims to become the standard infrastructure for distributing:

- AI agent skills
- development instruction sets
- reusable agent workflows

---

## License

MIT
