# Contribute to this documentation

This documentation is open for contribution from all community members. The current source is in this repository.

This page explains the structure and language guidelines for contributing to this documentation. See the [README][readme] for how to set up a local preview.

## Language

Write in a clear, concise, and actionable manner. This documentation uses the [Google Developer Documentation Style Guide][google-style] with the following exceptions:

- Do not capitalize the first word after a colon.
- Use semi-bold for UI elements.

## File format

Pages are written in [MDX][mdx-docs] (Markdown with JSX), stored as `.mdx` files. Every page must include a YAML frontmatter block at the top with at least a `title` and `description`:

```mdx
---
title: Page title (max 60 characters)
description: Concise summary of the page for SEO and navigation (max 200 characters).
---
```

Variables and component imports go after the closing `---` tag, before the page content.

## Edit individual pages

Each major doc section has a dedicated directory with `.mdx` files representing its child pages. An `index.mdx` file serves as the section landing page. To edit a page, modify the corresponding `.mdx` file.

- **Regular pages** should include:

  - A short intro paragraph that summarizes the content in the first sentence.
  - A visual illustrating the main concept, if relevant.
  - Paragraphs with descriptive headers using keywords.
  - Step-by-step procedures for actionable content.
  - Links to other relevant resources.

- **API pages** should include:

  - The function name as the title.
  - A brief description of the function.
  - One or two usage samples with code blocks.
  - An arguments table with columns: `Name`, `Type`, `Default`, `Required`, `Description`.
  - A returns section.

- **Troubleshoot pages** should include:

  - Frontmatter fields: `title`, `section`, `products` or `topics`.
  - Clear identification of the problem.
  - Step-by-step resolution procedures.

## Edit the navigation hierarchy

Navigation is controlled by `docs.json` in the root directory. Add or remove pages by editing the `navigation` array. The structure mirrors the folder structure:

```json
{
  "group": "Tiger Cloud services",
  "pages": [
    "deploy-and-operate/tiger-cloud/services/service-overview",
    "deploy-and-operate/tiger-cloud/services/service-explorer",
    "deploy-and-operate/tiger-cloud/services/troubleshooting"
  ]
}
```

Each entry is a path to the `.mdx` file relative to the root, without the file extension. To customise how a page label appears in the sidebar without changing the `title` frontmatter, use `sidebarTitle` in the page's own frontmatter.

See the [docs.json schema][docsjon-schema] for the full reference.

> **Note:** The dropdown navigation means the same change often has to be made in many places throughout `docs.json`. Rather than editing it by hand, use Claude or another AI tool and describe the change you want — for example, "add a page at X path under the Y group". The AI can find every place that needs updating and make all the edits at once.

## Reuse text in multiple pages

Snippets let you reuse content across multiple pages. All snippets live in the `snippets/` top-level directory, organised by topic (for example, `snippets/manage-data/`, `snippets/api-reference/timescaledb/`).

To use a snippet:

1. Create a `.mdx` file in the appropriate `snippets/` subdirectory.
2. Import it in the target page after the frontmatter:

   ```mdx
   import MySnippet from '/snippets/path/to/my-snippet.mdx';
   ```

3. Place the component in the page content where you want the snippet to appear:

   ```mdx
   <MySnippet />
   ```

Snippets render in their own scope. If a snippet uses variables, it must import them itself — it does not inherit imports from the parent page.

## Variables

Product names, features, and UI elements are stored as named exports in `snippets/vars.mdx`. Use `{VARIABLE_NAME}` syntax in page content.

Variables must be explicitly imported in each file that uses them. They do not work in frontmatter.

```mdx
---
title: My Page
---

import { CLOUD_LONG, CONSOLE } from '/snippets/vars.mdx';

{CLOUD_LONG} is managed through {CONSOLE}.
```

If a page imports snippets, import only the variables the parent page content uses directly. Variables used only inside snippets must be imported by those snippets, not the parent.

See `snippets/vars.mdx` for the full list of available variables.

## Formatting

In addition to standard Markdown, the following Mintlify components are available:

- `<Tabs>` and `<Tab>` — tabbed content
- Code blocks with language tags and an optional filename
- Multi-tab code blocks
- `<PricePlanBadge>` — displays which pricing plans a feature is available on
- Mermaid diagrams

Avoid admonition and callout blocks (`<Info>`, `<Warning>`, `<Note>`, `<Tip>`) unless the reader will cause serious harm to their data or system without the warning. If in doubt, work the information into the prose instead.

See existing pages for usage examples.

## Links

- **Internal links**: use relative paths without the domain. For example, `/manage-data/hypertables/about-hypertables`.
- **External links**: use the full URL as-is.

Do not use absolute URLs for internal links.

## Visuals

When adding screenshots, aim for a full-screen view to provide context. Minimise wasted space by reducing your browser window size before capturing.

Store images in the `images/` directory. Use descriptive filenames and always include alt text on image tags.

## SEO optimization

To make a page discoverable:

- Set `title` to a keyword-rich phrase under 60 characters.
- Set `description` to an action-oriented summary under 200 characters.
- Summarize the contents of each paragraph in its first sentence.
- Include the main page keywords in the title, first header, and intro paragraph.

[readme]: README.md
[google-style]: https://developers.google.com/style
[mdx-docs]: https://mdxjs.com/
[docsjon-schema]: https://mintlify.com/docs.json
