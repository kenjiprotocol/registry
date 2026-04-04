# Kenji Registry

The **Kenji Registry** is a GitHub-native index of AI agent skills and stacks. It stores only JSON metadata files — the actual skill instruction files live in the repositories referenced by each entry.

Skills and stacks from this registry are searchable and installable via the Kenji CLI:

```bash
kenji search react
kenji install kenji/react-debug
kenji stack install ai-saas-stack
```

---

## Repository Structure

```
your-registry-repo/
  skills/
    <namespace>/
      <skill-name>.json
  stacks/
    <namespace>/
      <stack-name>.json
```

- Only the `skills/` and `stacks/` top-level folders are indexed. Any other directory structure is ignored.
- Each skill or stack is a single `.json` file placed inside a `<namespace>/` subfolder.
- Deeper nesting is not supported.
- The registry repository must **not** contain the actual skill Markdown files. Those live in the repository referenced by the `repo` field inside each JSON file.

---

## Namespace

The namespace is the folder name under `skills/` or `stacks/`. By convention, it should match the GitHub username of the skill's author, since it becomes the prefix shown in search results (e.g. `kenji/react-debug`, `yourname/my-skill`).

### Rules

- Must match the pattern: `[a-zA-Z0-9_-]`
- Cannot contain slashes or spaces
- Using your GitHub username is strongly recommended for clarity

---

## Skill JSON Schema

**File location:** `skills/<namespace>/<skill-name>.json`

**Example:**

```json
{
  "name": "react-debug",
  "description": "React debugging skill for AI agents",
  "repo": "someuser/react-debug-skill",
  "tags": ["react", "debugging"],
  "entry": "SKILL.md"
}
```

### Fields

| Field | Required | Description |
|---|---|---|
| `name` | Yes | Skill identifier. Must match the JSON filename (without `.json`). |
| `description` | Yes | Short human-readable description of the skill. |
| `repo` | Yes | GitHub repo containing the actual skill markdown, in `user/repo` format. |
| `tags` | No | Array of search keywords. |
| `entry` | No | Path to a specific file inside `repo` to install. If omitted, Kenji installs all non-README `.md` files found in the repo. |

---

## Stack JSON Schema

**File location:** `stacks/<namespace>/<stack-name>.json`

**Example:**

```json
{
  "type": "kenji-stack",
  "version": "1.0",
  "name": "ai-saas-stack",
  "description": "Starter stack for building AI SaaS apps",
  "skills": [
    { "type": "registry", "value": "kenji/react-debug" },
    { "type": "github", "value": "someuser/backend-prompts" },
    { "type": "url", "value": "https://github.com/user/repo/tree/main/skills/frontend" },
    { "type": "global", "value": "my-local-skill" }
  ]
}
```

### Required Fields

| Field | Required | Description |
|---|---|---|
| `type` | Yes | Must be exactly `"kenji-stack"`. |
| `version` | Yes | Must be a non-empty string (e.g. `"1.0"`). |
| `name` | Yes | Stack identifier. Alphanumeric with hyphens and underscores only. |
| `description` | No | Short description of the stack's purpose. |
| `skills` | Yes | Array of skill entries. Each must be a `{ type, value }` object. |

> **Note:** Plain string entries in the `skills` array are not accepted. Each entry must be a structured `{ type, value }` object. Files containing bare strings will fail validation.

### Skill Entry Types

| Type | Format | Description |
|---|---|---|
| `"registry"` | `namespace/name` | A skill from any Kenji registry |
| `"github"` | `user/repo` | A GitHub repository |
| `"url"` | Full URL | A GitHub tree URL, blob URL, or raw file URL |
| `"global"` | Folder name | A locally installed global skill |

Any entry with an unrecognized type will be skipped during `kenji stack install`.

---

## Validation Rules

When a registry is submitted via `kenji registry add`, Kenji validates that:

- The GitHub repository exists and is public
- A `skills/` or `stacks/` directory is present
- At least one valid JSON file exists with the required fields

During aggregation, each JSON file is validated against the schema. Invalid files are skipped without breaking the rest of the registry.

### Name and Namespace

- Must match: `/^[a-zA-Z0-9_-]+$/`
- Slashes and spaces are rejected

### Skill JSON

- `name` and `repo` are required
- `repo` must be in `user/repo` format

### Stack JSON

- `type`, `version`, `name`, and `skills` are required
- Each entry in `skills` must be a `{ type, value }` object

---

## Community Registries

Anyone can create a community registry and connect it to the Kenji network:

```bash
kenji registry add user/kenji-registry
```

Accepts a GitHub repo path (`user/repo`) or a full GitHub URL. The repo must contain at least one valid `skills/` or `stacks/` directory with conforming JSON files. Duplicate submissions are ignored.

Once accepted, the registry's skills and stacks appear in search results alongside official registry items, almost immediately.

> **Warning:** Community skills are not reviewed by the Kenji team. When installing a community skill, Kenji displays a warning and asks for confirmation before proceeding.

---

## How to Contribute to the Official Registry

1. Fork this repository: [kenjiprotocol/registry](https://github.com/kenjiprotocol/registry)
2. Create a JSON file in `skills/<your-namespace>/` or `stacks/<your-namespace>/`
3. Commit and open a Pull Request
4. Once merged, the item is available globally via the Kenji CLI

---

## Registry Philosophy

The Kenji Registry is:

- **GitHub-native** — stored entirely as files in public repositories
- **Open and transparent** — every entry is readable as plain JSON
- **File-driven** — no database, no proprietary format
- **Decentralized** — official and community registries are aggregated at query time

It is not a centralized database. It is an index that points to public skill repositories.
