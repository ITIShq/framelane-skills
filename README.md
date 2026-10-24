# FrameLane agent skills

Skills that teach an AI agent to make video with [FrameLane](https://framelane.io): compose a project, check it on frames the server renders, hand it to a person in the browser editor and export it. They work through the FrameLane MCP server, which the Claude Code plugin in this repository connects for you.

| Skill | Kind | Use it when |
| --- | --- | --- |
| `framelane` | reference | FrameLane's shared contract for building, previewing, reviewing, handing off and exporting a versioned video project over MCP. Read it before the first composition in any FrameLane workflow. |
| `framelane-brief-to-video` | workflow | Assemble a narrated, editable FrameLane video from a written brief and the user's own media. Use when someone hands over a brief or script plus footage, images or a voiceover. |
| `framelane-caption` | workflow | Add or revise timed captions in FrameLane from a real transcript. Use when a video needs subtitles, burned-in captions or word-by-word caption styling. |
| `framelane-explainer` | workflow | Explain an idea with motion graphics in FrameLane, silent or with supplied narration. Use for concept, process, comparison or how-it-works explainers built from text, shapes and simple diagrams. |
| `framelane-localize` | workflow | Create a translated locale variant of a FrameLane project that preserves meaning and readability, with honest caption timing and script checks. Use when someone wants a video translated or adapted for another language or market. |
| `framelane-onboard` | workflow | Introduce FrameLane's workflows and, when someone wants to try editing, create a real, editable sample project. Use when someone is new to FrameLane or asks what it can do. |
| `framelane-reframe` | workflow | Create faithful versions of a FrameLane project for another aspect ratio. Use when someone needs 9:16, 1:1 or 16:9 variants of an existing video. |
| `framelane-review` | workflow | Review a FrameLane project from server-rendered frames and lint, and repair it with targeted edits when asked. Use when asked to check, critique, QA or fix an existing video project. |
| `framelane-shorts` | workflow | Cut captioned vertical shorts from real footage in FrameLane. Use when someone has a long video recording, such as an interview, talk or podcast, and wants 9:16 clips. |
| `framelane-styles` | reference | Named visual styles for FrameLane videos, such as dark keynote, Swiss motion, kinetic type or silent film. Use when a brief asks for a stylized look or names a style, before the first composition. |
| `framelane-talking-head` | workflow | Clean up a talking-head recording in FrameLane: remove ums, stutters, false starts, retakes, mistakes and dead air by transcript, then add titles, captions and B-roll. Use when someone asks to clean up or polish their recording. |
| `framelane-teaser` | workflow | Build a short branded launch teaser in FrameLane. Use for a product or feature launch, an event or an announcement with one clear promise and one next action. |

Start with `framelane`, the handbook: every workflow builds on it. A workflow carries one job from brief to delivery; a reference is read beside one. Each skill folder carries the shared craft notes it cites (color, type, motion, layout, delivery, speech editing), copied into its `references/craft/` folder. Where a skill names another as `skills.<name>`, that is the `framelane-<name>` folder here (the handbook is `framelane`), and the same name is a `get_capabilities` section on the MCP server.

## Install

### Claude Code

```
/plugin marketplace add ITIShq/framelane-skills
/plugin install framelane@framelane
```

The plugin brings the skills and the MCP server configuration together.

### The skills CLI

```
npx skills add ITIShq/framelane-skills
```

It asks which agents to install for and puts the skills in their skill folders. It installs into the current project by default; add `-g` to install for your user instead, into folders such as `~/.claude/skills` and `~/.agents/skills`.

### Codex, Cursor and other agents

Copy the folders under `skills/` into the folder your agent reads:

| Agent | Folder |
| --- | --- |
| Codex | `~/.codex/skills` |
| Cursor | `~/.cursor/skills` |
| Agents that share one folder | `~/.agents/skills` |

```
git clone https://github.com/ITIShq/framelane-skills.git framelane-skills
cp -R framelane-skills/skills/* ~/.codex/skills/
```

### Voyager

Voyager indexes `~/.claude/skills`, `~/.codex/skills`, `~/.agents/skills` and `~/.cursor/skills`, and the same folders inside a project, but not Claude Code's plugin cache. Install with the skills CLI or copy the folders as above: the plugin alone does not reach it. When two folders hold a skill with the same name, the first one found wins, so keep every copy current.

## Connect the MCP server and sign in

The skills call tools on the FrameLane MCP server at `https://mcp.framelane.io/mcp` (Streamable HTTP). With the Claude Code plugin the server is already configured: run `/mcp`, choose `framelane` and sign in with your FrameLane account. Anywhere else, add the URL as a remote MCP server and sign in when your agent asks, or, for headless use, send a FrameLane API key as `Authorization: Bearer <key>`. Setup for each host is in the [MCP guide](https://docs.framelane.io/mcp/overview).

## About this repository

It is generated from the skills the FrameLane MCP server serves, and every build replaces it, so a change belongs in that source rather than in these files. `skill.md` (the handbook with an index of the workflows) and `llms.txt` (one line per skill) are generated for the FrameLane homepage. The plugin version is the date of the commit it was built from plus a digest of the shipped files, so it changes whenever a file here does.

Licensed under the MIT License; see [LICENSE](LICENSE).
