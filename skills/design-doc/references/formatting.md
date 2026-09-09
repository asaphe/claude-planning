# Document formatting

Follow the requested format and the repository's conventions. These are portable defaults
for readable architecture documents, not a dependency on a particular renderer or plugin.

## Structure

- Use descriptive section headings and stable anchors for decisions and review comments.
- Put questions or decisions on their own line, with a labeled answer or rationale beneath.
- Use tables when rows compare the same dimensions: options, costs, requirements coverage.
- Expand acronyms at first use. Add a glossary for domain terms.
- Break dense sections with meaningful subheadings, diagrams, or concise lists.

For example, in Markdown:

```markdown
### D1 — Keep ingestion asynchronous

**Decision:** Accept requests into a durable queue before processing.

**Rationale:** Processing can continue after a request disconnects.

**Tradeoff:** Clients need a status lookup and must handle delayed completion.

**Status:** Proposed; verify queue durability against requirement R2.
```

This example is illustrative, not a verified system design.

## HTML delivery

For a file intended to travel independently, inline its styles and required images or
diagrams. Avoid remote fonts, scripts, and styles that fail offline. If a hosted page uses
external assets, state that dependency rather than promising a self-contained file.

- Provide a table of contents for long documents; make navigation collapse on narrow screens.
- Keep prose around 70–85 characters wide while letting tables and diagrams use more space.
- Give paragraphs and table cells room; wrap wide tables in a horizontally scrollable region
  rather than forcing the whole page to overflow.
- Use semantic headings and sufficient contrast. Risk labels must not rely on color alone.
- If supporting light and dark themes, verify both. Set a page background explicitly and
  keep diagram labels and lines readable in each theme.
- Include print styles: remove navigation, use readable contrast, and check page breaks and
  table clipping. A file that renders on screen may still print incorrectly.

## Diagrams

Choose a diagram format supported by the delivery surface. For standalone HTML, inline SVG
works without a remote renderer. Markdown may use Mermaid if the target renderer supports
it; otherwise include a rendered image with an accessible description.

Show the architecture's important components, interfaces, data paths, and isolation
boundaries. Add sequence diagrams when ordering or recovery is central. Give diagrams
captions and check their labels against the prose and evidence. Do not assume an omitted
component will be inferred by the reader.

## Verification

1. Check headings, link destinations, decision anchors, code fences, and table structure.
2. Inspect rendered output at a normal and narrow viewport using available browser or
   document tools. If rendering is unavailable, name that limitation precisely.
3. For HTML, inspect print output and all supported themes. Confirm embedded assets load
   without a network connection if the file claims to be self-contained.
4. Recheck diagram labels and boundaries against the text after substantive edits.
5. Check the delivered source, rendering, metadata, and appendices for content inappropriate
   for the audience. Do not include credentials or hidden private context in comments.

Report checks actually performed. Balanced markup or working anchors alone cannot establish
that the document is readable or that a visual regression is absent.
