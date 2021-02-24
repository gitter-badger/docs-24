---
title: Style guidelines
---

## Markdown guidelines

For now, install Markdown Lint for VSCode and use their default configurations. Refer to their docs for rationale. Any exceptions to these rules will be noted in [exceptions](#exceptions)

### Exceptions

Keep a list of any exceptions here. You can read how to configure VSCode exceptions from MarkdownLint repo

- <https://github.com/DavidAnson/vscode-markdownlint#configure>

#### No inline HTML

Our docs use HTML sometimes and we can ignore this rule - but try to use raw Markdown where reasonably possible.

## Understand links

There are three types of links used for various purposes.

- Internal Links
- External Links
- Doc Task Management Links

### Internal LInks

These links to other pages within this project. Prefer using  using wiki-style brackets:

`[[my link]]`

When there is ambiguity, you can try using relative link path

``[[my links\my links]]``

Exceptions are made if you need to use a hammer, try using absolute link

`[my link](./my links/my_links.md)`

### External Links

To prevent confusion, when an external page is referenced, try to plainly list the link

```markdown
- My External Link
  - <http://www.externalsite.com/link>
```

Tip: Try to trim any additional refers or unnecessary details added to link. If the link is too long, you can wrap it up as a hyperlink, but try to give the reader some clue that they're leaving these docs

```markdown
- [A really long link from www.extrenal.com](www.externasite.com/link/23SDfgetffFFF34t46gf333fjjJdj/E93Fjajgjgaaa353436)
```

### Doc Task Management Links

- Any line of text can be considered a work item
  - Work items are still under construction are listed with one of the following tags
    - #to-do - planned by not started
    - #open - being worked on
    - #review - work needs reviewed
    - #hold - can't be finished
  - No tag for completed items

#### Formatting task tags

- Tags should be formatted using one bracket (NOT wiki-style links)
  - `[#to-do]`
- When item is being worked on, update
  - `[[#open]]`
- After work is considered done, but not complete
  - `[[#review]]`
- After work is complete, the tag is removed.
  - The git revision history will allow us to find old tags
