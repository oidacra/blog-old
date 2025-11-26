---
title: "Angular 21 MCP: 6 Stable Tools + 1 Experimental You Should Know"
datePublished: Tue Nov 18 2025 02:10:52 GMT+0000 (Coordinated Universal Time)
cuid: cmi3xslxd000302jp2e6z8h36
slug: angular-21-mcp-6-stable-tools-1-experimental-you-should-know
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/wEJK4q_YlNQ/upload/d94866e5c300ca751500995cc4616119.jpeg
tags: ai, angular, mcp

---

Angular 21 brings several MCP updates: a tool that was experimental is now stable, and only one remains in controlled beta.

If you haven't set up MCP with Cursor or Claude yet, check out my [previous guide on MCP setup](https://arcadioquintero.com/stop-writing-outdated-angular-code-mcp-setup-with-cursor-and-claude-code) before diving into this update.

## What You Get by Default

When you run `npx @angular/cli mcp` with no flags, you get all 6 stable tools automatically:

* `list_projects`
    
* `get_best_practices`
    
* `search_documentation`
    
* `find_examples`
    
* `ai_tutor`
    
* `onpush-zoneless-migration`
    

Experimental tools are **opt-in only**—you must explicitly enable them with `-E`.

**Note:** You can't disable individual stable tools. If you need to restrict the surface, use `--read-only` or `--local-only` to filter by capability instead of by name.

## `ng mcp` Flags

* `--read-only`: enables only tools marked as `read-only`, ideal when the host blocks writes.
    
* `--local-only`: filters tools that don't depend on the internet (`local-only`), perfect for isolated environments.
    
* `--experimental-tool`/`-E`: opt-in for beta features. Quick example: `npx @angular/cli mcp -E modernize`.
    

## Stable Tools (enabled by default)

* `list_projects` (read-only, local): gives the IDE a full workspace map. Cursor/Claude uses it automatically before suggesting changes or paths.
    
* `get_best_practices` (read-only, local): syncs AI with the official project guide; the assistant queries it to follow the correct version.
    
* `search_documentation` (read-only, requires network): enables searches on [angular.dev](http://angular.dev) when you ask for explanations or references inside the editor.
    
* `find_examples` (read-only, local): finds authoritative code examples from a curated database of official, best-practice examples, focusing on modern, new, and recently updated Angular features.
    
* `ai_tutor` (read-only, local): activates step-by-step tutor mode inside the IDE, no extra config needed.
    
* `onpush-zoneless-migration` (read-only, local): analyzes components or folders and returns the recommended next step toward OnPush and zoneless.
    

## Experimental Tools (enable with `-E`)

* `modernize` (local, writes to disk): runs modern migrations like `control-flow`, `signal-input`, or `standalone`. Great shortcut to catch up.
    

## How Angular's MCP Is Designed

* **General purpose:** the MCP server acts as a safe CLI facade for AI assistants, prioritizing these tools over arbitrary commands.
    
* **Recommended baseline flow:** always start with `list_projects` to discover the workspace, then `get_best_practices` to align standards, and reserve `search_documentation`/`find_examples` depending on whether the question is conceptual or code-related.
    
* **Key concepts:** a workspace (`angular.json`) can contain multiple projects; always use the `workspaceConfigPath` returned by `list_projects` to target the right project, especially in monorepos.
    

## Recommended IDE Scenarios

* **Learning modern Angular:** enable `ai_tutor` by default; from chat, ask "Start the tutorial" and let it guide each step.
    
* **Need docs or recent snippets:** keep `search_documentation` and `find_examples` enabled; request "show me how to use signal inputs" and the assistant will combine docs with examples.
    
* **Planning to migrate code:** add `-E modernize` to your MCP config and ask "update this folder to new control flow"; the AI will invoke the tool and give you instructions or changes.
    
* **Going for OnPush/zoneless:** `onpush-zoneless-migration` is now enabled by default; just ask "what's next to migrate this module?" and you'll get concrete steps without running manual commands.
    

## Ready-to-Copy MCP Configs

**Default setup (all stable tools):**

```json
{
  "mcpServers": {
    "angular-cli": {
      "command": "npx",
      "args": ["-y", "@angular/cli", "mcp"]
    }
  }
}
```

This gives you: `list_projects`, `get_best_practices`, `search_documentation`, `onpush-zoneless-migration`, `find_examples`, and `ai_tutor`.

---

**Restricted environment (read-only + no network):**

```json
{
  "mcpServers": {
    "angular-cli": {
      "command": "npx",
      "args": ["-y", "@angular/cli", "mcp", "--read-only", "--local-only"]
    }
  }
}
```

Keeps only the 5 tools that are both read-only and local-only, excluding `search_documentation` which requires network access.

---

**Code modernization enabled:**

```json
{
  "mcpServers": {
    "angular-cli": {
      "command": "npx",
      "args": ["-y", "@angular/cli", "mcp", "-E", "modernize"]
    }
  }
}
```

Adds the experimental `modernize` tool for running migrations like `control-flow`, `signal-input`, or `standalone`. Note: This tool writes to disk and is in preview status.

---

## Wrapping Up

Angular 21's MCP tools are simple to set up and immediately useful: six stable tools for everyday development, one experimental for migrations. Pick the config that fits your workflow, drop it into your IDE, and your AI assistant now speaks Angular fluently—pulling from official sources instead of outdated answers.

If you're already using Cursor/VSCode or Claude Code, this is a no-brainer. The tooling continues to improve with each release.

Happy augmented coding!