# Kenji Registry

The **Kenji Registry** is a GitHub-native index of AI agent skills and stacks.

It enables users to:

- Discover skills
- Install skills via CLI
- Install curated stacks
- Share reusable agent capabilities

The registry is powered entirely by the file structure of this repository.

---

## Registry Structure

```
skills/
  <namespace>/
    skill-name.json
stacks/
  <namespace>/
    stack-name.json
```

Each namespace is typically a GitHub username or organization name.

---

## Namespace Rules

- Namespace must match: `[a-zA-Z0-9_-]`
- One namespace per folder
- Skill and stack filenames must be unique inside a namespace
- Recommended: use your GitHub username or org name

**Example:**

```
skills/
  kenji/
    react-debug.json
```

---

## Skill Definition Format

**File location:**

```
skills/<namespace>/<skill-name>.json
```

**Example:**

```json
{
  "name": "react-debug",
  "description": "React debugging skill for AI agents",
  "repo": "someuser/react-debug-skill",
  "entry": "SKILL.md",
  "tags": ["react", "debugging"]
}
```

### Fields

| Field | Required | Description |
|---|---|---|
| `name` | yes | Skill identifier |
| `description` | yes | Short explanation |
| `repo` | yes | GitHub repo containing skill |
| `entry` | optional | Specific instruction file to install |
| `tags` | optional | Search keywords |

If `entry` is omitted, Kenji installs all non-README `.md` files in the repo.

---

## Stack Definition Format

**File location:**

```
stacks/<namespace>/<stack-name>.json
```

**Example:**

```json
{
  "name": "ai-saas-stack",
  "description": "Starter stack for building AI SaaS apps",
  "skills": [
    "kenji/react-debug",
    "anotheruser/backend-prompts",
    "https://raw.githubusercontent.com/user/repo/main/SKILL.md"
  ]
}
```

### Fields

| Field | Required | Description |
|---|---|---|
| `name` | yes | Stack identifier |
| `description` | optional | Stack purpose |
| `skills` | yes | Array of skill references |

Skill references can be:

- `namespace/skill`
- `user/repo`
- raw GitHub URL
- GitHub tree URL

---

## How to Add a Skill or Stack

1. Fork this repository
2. Create a JSON file in the correct folder
3. Commit and open a Pull Request
4. Once merged, the item becomes available globally via CLI

**Example usage after merge:**

```bash
kenji search react
kenji install namespace/react-debug
```

Registry changes become available after cache refresh.

---

## Registry Philosophy

Kenji Registry is:

- GitHub-native
- open and transparent
- file-driven
- infrastructure-focused

It is not a centralized database.

It is an index that points to public skill repositories.
