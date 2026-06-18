---
name: framer-mcp-v4
description: "Use this skill whenever working with Framer via MCP to create or edit designs, layouts, components, or styles. Always trigger when using any framer-mcp tools, building pages, creating sections, working with XML nodes, inserting components, managing color or text styles, or fixing layout issues in Framer. This skill prevents common mistakes that produce hardcoded, non-reusable, or visually broken results."
---

# Framer MCP Design Skill v4

## 1. Web pages only, inside the Desktop frame

**DesignPages are forbidden for content.** Content always goes on a web page (`type="web"`). Every web page has a root Desktop breakpoint node — all content goes inside it, never directly on the canvas.

DesignPages are for components and prototypes only.

---

## 2. You are working in an ongoing project

The project already has pages, styles, and components built in previous sessions. If something looks different than expected — a node already has content, a style already exists, a component is present — assume the user made changes. Inspect with `getNodeXml`, describe what you see, then proceed.

Only flag something as broken if it is genuinely broken.

**A correction from the user is not a confirmation.** After any correction, restate the full plan and wait for explicit approval before building anything.

**Uncertain tool results are not facts.** If `getNodeXml` returns unexpectedly empty results, re-inspect before reporting or proceeding. Never present an unreliable result as a conclusion.

If the MCP is connected and the task is clear, start building. Do not ask clarifying questions first.

---

## 3. Always call getProjectXml first

Call `getProjectXml` before writing any XML or style. Read the result fully. For every nodeId in the output, match it to its label, page name, and URL path before using it. Never use a nodeId without knowing exactly which page and which element it belongs to.

**Communicate to the user in names and paths only, never in nodeIds.** NodeIds are for tool calls internally. Everything reported to the user uses page names, URL paths, section names, and component labels.

---

## 4. State page structure before writing any XML

Before writing a single node, state:
- The page name and URL path you will build on (e.g. `/l1a-homepage`)
- The full section list top to bottom: `nav → hero → [sections] → footer`

Check the naming pattern of existing pages from `getProjectXml`. New pages must follow that pattern. If a matching page does not exist yet, create it with `createPage` before building.

Wait for confirmation before building. Confirmation means an explicit go-ahead, not a correction.

---

## 5. Create styles before nodes, never hardcode

Order: `manageColorStyle` → `manageTextStyle` → build nodes. Reference by path: `backgroundColor="/Style/Name"`, `inlineTextStyle="/Style/Name"`. The `style="..."` attribute is ignored entirely in Framer XML.

---

## 6. Layout defaults and gotchas

**Stacks vs Frames:**
- Use `layout="stack"` for all containers. Absolute positioning is only valid for true overlays.
- Stacks auto-size; Frames do not. Use Frames only for fixed-size placeholders (icons, image blocks).
- Always set `height="fit-content"` on Stacks that should grow with content.
- Always set width explicitly on every Stack.

**Inner wrapper: always required, no exceptions.**
Every section has an outer and an inner, even for simple content. The outer handles background color, full width, and vertical padding. The inner handles max-width and horizontal padding.

```
Outer: width="100%", height="fit-content", layout="stack", stackDirection="horizontal",
       stackDistribution="center", stackAlignment="center",
       padding="64px 0px 64px 0px", backgroundColor="/Style/Name"

Inner: width="100%", height="fit-content", maxWidth="1000px",
       backgroundColor="rgba(0,0,0,0)", layout="stack",
       stackDirection="vertical", gap="24px", padding="0px 0px 0px 0px"
```

**Hero 1fr pattern:**
Hero outer has a fixed height. Hero inner is a vertical Stack with two children:
- Label row: `width="1fr"`, `height="fit-content"`, `stackDistribution="start"` → holds the Label component
- Content row: `width="1fr"`, `height="1fr"`, `stackDistribution="center"`, `stackAlignment="center"` → holds headline, subline, CTA

**Spacing scale (pixels only, no rem):**
- Inner element gaps: 8 / 16 / 24px
- Section content gaps: 32 / 40px
- Section vertical padding: 64 / 80px
- Horizontal content padding: 40px

**Known gotchas:**
- Root page Stack must have `gap="0px"` — Framer adds `gap="10px"` by default and sections drift.
- Padding: only 1 or 4 values. `"64px 0px"` is silently ignored — use `"64px 0px 64px 0px"`.
- Grid items default to `width="100px"` — always set `width="100%"` explicitly.
- Inner wrappers default to white — always set `backgroundColor="rgba(0,0,0,0)"` on transparent containers.
- Framer silently adds `maxHeight="100%"` to the Desktop node — clips page content. Remove it.
- Inner content wrappers can get `width="1fr"` instead of `width="100%"` — check and fix after creation.

---

## 7. Components: use what exists, place correctly

**Check before building.** `getProjectXml` lists all components. Before building any UI element from raw XML, check if a component already exists. Use it.

**Use `insertUrl`, not `componentId`.** Get the correct insertUrl via `getComponentInsertUrlAndTypes` before every component insert. Never guess or reuse a URL from a previous session.

**Verify variants before building.** Never copy a variant from another page without checking it matches the spec of the current page. If the spec differs (different number of items, no CTA button, different layout), inspect the available variants first.

**Label placement.** Label component goes as first child inside the inner wrapper, not absolute on the outer section. Inner wrapper `padding="0px 0px 0px 0px"` when label is first child.

```xml
<Stack nodeId="inner" width="100%" height="fit-content" maxWidth="1000px"
       backgroundColor="rgba(0,0,0,0)" layout="stack" stackDirection="vertical"
       gap="24px" padding="0px 0px 0px 0px">
  <Label insertUrl="..." width="fit-content" height="fit-content" />
  <!-- content -->
</Stack>
```

**MCP cannot create visual components.** There is no tool to convert a frame into a component. If component creation is needed, tell the user directly: they create the shell via right-click → Make Component in Framer, then provide the nodeId. Do not attempt workarounds via `createCodeFile` or any other tool.

**No workarounds.** If a tool does not produce the correct result, stop and report what went wrong. Do not chain tool calls to patch around a failure without explaining it first.

**Linked by default.** Use `?detached=true` only when internal structure needs editing. After detaching, call `getNodeXml` on the parent before further edits.

---

## 8. Text nodes

Cannot add text to a non-text node — fails silently. Create a new wrapper with a Text child instead.

---

## 9. One section per updateXmlForNode call

Never batch the entire page. One logical section per call.

---

## 10. Tag names and global styles

Tag names become layer names. `manageTextStyle` with `type: "update"` is global — create a new style path if only one node should change.

---

## 11. Tool permissions

**Tip for the user:** disable unused tools directly in the Framer MCP settings (claude.ai: Settings > Connections > framer-mcp > configure). Disabled tools do not appear in context and cost zero tokens. Recommended to disable: `zoomIntoView`, `getSelectedNodesXml`, `getProjectWebsiteUrl`, `exportReactComponents`, `searchFonts`, and all CMS tools unless actively using CMS.

**Always allowed in normal flow:**

| Tool | When |
|---|---|
| `getProjectXml` | Mandatory first step every session |
| `getNodeXml` | Inspect before and after changes |
| `updateXmlForNode` | Build, one section per call |
| `manageColorStyle` | Before creating nodes |
| `manageTextStyle` | Before creating nodes |
| `getComponentInsertUrlAndTypes` | Mandatory before every component insert |
| `createPage` | Only when explicitly asked or when a new page is needed per naming pattern |
| `deleteNode` | Only when you know exactly what you are removing |
| `duplicateNode` | Only when explicitly asked |

**Never use without explicit instruction:**

| Tool | Reason |
|---|---|
| `zoomIntoView` | Do not include this parameter at all, not even as False |
| `getSelectedNodesXml` | `getNodeXml` does the same thing |
| `createCodeFile` / `updateCodeFile` / `readCodeFile` | Code component work only, never as a workaround |
| `exportReactComponents` | Only when explicitly asked |
| `searchFonts` | Only during typography setup |
| `createCMSCollection` / `getCMSCollections` / `getCMSItems` / `deleteCMSItem` / `upsertCMSItem` | CMS work only |
| `getProjectWebsiteUrl` | Rarely needed |

**One `tool_search` per session.** Use a single specific query at the start. Never repeat the same or similar query twice.

---

## Self-learning

When something breaks unexpectedly or the user corrects an output, add the specific failure as a new rule. Rule must describe the concrete problem and the fix, not a general principle.

## Token efficiency

Before adding any rule, ask: would Claude figure this out without being told? If yes, don't add it.

## Unnecessary context

Never add rules that restate general programming principles, explain what MCP tools do in terms already in the docs, or describe things Claude would infer from the XML structure itself.
