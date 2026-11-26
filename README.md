# Documentation best practices for GitHub and Gitea

A practical guide to writing, reviewing, and maintaining repository documentation that works on both GitHub and Gitea. Use it when creating a `README.md`, contributor guide, tutorial, reference page, or an entire `docs/` directory.

This guide follows its own recommendations: it has a clear purpose, predictable structure, task-focused examples, portable Markdown, and explicit maintenance guidance.

## Contents

- [Quick start](#quick-start)
- [Core principles](#core-principles)
- [Choose the right documentation home](#choose-the-right-documentation-home)
- [Plan before writing](#plan-before-writing)
- [Write for readers](#write-for-readers)
- [Structure Markdown consistently](#structure-markdown-consistently)
- [Make examples trustworthy](#make-examples-trustworthy)
- [Use links, images, and diagrams carefully](#use-links-images-and-diagrams-carefully)
- [Design for accessibility](#design-for-accessibility)
- [Keep documentation portable](#keep-documentation-portable)
- [Review and test changes](#review-and-test-changes)
- [Automate routine checks](#automate-routine-checks)
- [Maintain documentation over time](#maintain-documentation-over-time)
- [Documentation definition of done](#documentation-definition-of-done)
- [README template](#readme-template)
- [Contributing to documentation](#contributing-to-documentation)
- [References](#references)

## Quick start

1. Identify the audience and the task each document helps them complete.
2. Put essential project information in the root `README.md` and longer topics in `docs/`.
3. Write with one H1, ordered headings, short paragraphs, and descriptive link text.
4. Use portable Markdown, relative repository links, language-tagged code fences, and meaningful alt text.
5. Run every command and test every example before publishing.
6. Review documentation in the same pull request as the behavior it describes.
7. Check links, formatting, and rendering on the target platform.
8. Assign ownership and revise documentation whenever the project changes.

## Core principles

### Start with the reader’s goal

Optimize for what readers need to accomplish, not for how the project is organized internally. A new user usually needs installation and a first successful result; a returning user usually needs exact reference information.

### Make the first success easy

Lead with the shortest reliable path to a useful outcome. Move uncommon configuration, troubleshooting, and design rationale to later sections or separate pages.

### Keep documentation close to the code

Store documentation in the repository so changes can be reviewed, versioned, and released alongside the implementation. Prefer pull requests over unreviewed edits.

### Prefer clarity over cleverness

Use familiar words, direct instructions, and conventional Markdown. Documentation is successful when readers can find, understand, and apply it without additional help.

### Treat accuracy as a feature

Incorrect documentation is a defect. Examples, commands, screenshots, defaults, and version requirements must match the current project.

## Choose the right documentation home

| Location | Best for | Avoid using it for |
| --- | --- | --- |
| Root `README.md` | Project purpose, requirements, installation, quick start, support, contribution and license pointers | Complete API references or long tutorials |
| `docs/` | Tutorials, how-to guides, architecture, reference, release-specific guidance | Duplicating the entire README |
| `CONTRIBUTING.md` | Development setup, branch workflow, tests, style, pull request expectations | End-user installation instructions |
| Issue and pull request templates | Reproducible bug reports and review-ready changes | Long-form project documentation |
| Repository wiki | Community notes or supplementary material that does not require the same release cycle | Canonical instructions that must stay synchronized with code |
| Releases or changelogs | Version-specific changes, migrations, deprecations, and upgrade notes | Stable tutorials and general reference material |

Use uppercase names for conventional entry points such as `README.md` and `CONTRIBUTING.md`. Use short, lowercase, hyphenated names for topic files, for example `docs/local-development.md`.

Keep one canonical page for each topic. Elsewhere, summarize only what is necessary and link to the canonical source instead of copying it.

## Plan before writing

Before drafting, record:

- **Audience:** Who needs this document, and what do they already know?
- **Outcome:** What will they be able to do after reading it?
- **Scope:** What is included and explicitly excluded?
- **Prerequisites:** Which tools, permissions, versions, and concepts are required?
- **Owner:** Who reviews the page when the relevant code changes?
- **Validation:** How will commands, examples, and links be checked?

Match the document type to the need:

- **Tutorial:** Guide a beginner through a learning-oriented example.
- **How-to guide:** Solve a specific real-world problem.
- **Reference:** Describe commands, options, APIs, defaults, and errors precisely.
- **Explanation:** Clarify concepts, constraints, trade-offs, and design decisions.

Do not force all four types into one long page.

## Write for readers

### Put the important information first

Begin each page with its purpose and relevance. In procedures, state the expected result before listing steps.

### Use direct, consistent language

- Prefer active voice: “Run the tests,” not “The tests should be run.”
- Use imperative verbs for steps: “Create,” “Copy,” “Verify.”
- Use the same term for the same concept throughout the repository.
- Define acronyms on first use unless the intended audience certainly knows them.
- Avoid unnecessary jargon, idioms, humor, and culturally specific expressions.
- Distinguish requirements from recommendations with “must,” “should,” and “may.”
- State version-specific behavior explicitly instead of writing “currently” or “recently.”

### Make content scannable

- Keep paragraphs focused on one idea.
- Use lists for discrete items and numbered lists for ordered steps.
- Put conditions before instructions: “If you use a proxy, set …”.
- Use tables only for genuinely tabular information, not page layout.
- Move secondary detail below the main path or to a linked page.

### Avoid ambiguous instructions

**Unclear**

> Configure the application appropriately and make sure it works.

**Clear**

> Copy `.env.example` to `.env`, set `DATABASE_URL`, and then run the migration command. A successful migration exits with status `0` and prints `Migrations complete`.

Tell readers what to expect, how to verify success, and where to look when something fails.

## Structure Markdown consistently

- Use exactly one H1 (`#`) per page for its title.
- Use heading levels in order; do not skip from H2 to H4 for visual sizing.
- Use sentence-style, descriptive headings that reflect the content.
- Add a table of contents to long pages, and keep its anchors synchronized with headings.
- Surround headings, lists, tables, and code fences with blank lines.
- Use fenced code blocks with a language identifier such as `bash`, `json`, or `text`.
- Use backticks for filenames, commands, options, identifiers, and literal values.
- Use bold for emphasis sparingly; do not use it as a substitute for headings.
- Use blockquotes for notes only when necessary, and avoid nesting important steps inside them.
- Do not use raw HTML when standard Markdown expresses the same content.

Recommended page order for task-oriented documentation:

1. Purpose and intended audience
2. Prerequisites
3. Procedure or main content
4. Verification and expected output
5. Troubleshooting, if needed
6. Related documentation

## Make examples trustworthy

### Make examples complete enough to run

Show required input, working directory, environment variables, and expected output. Avoid unexplained ellipses in executable commands.

```bash
git clone https://example.com/owner/project.git
cd project
```

The URL above is illustrative, not a working repository. In project documentation, use the real repository URL and test the complete sequence.

### Separate placeholders from literal values

Use an obvious convention and explain it. For example, replace `<project-directory>` with an actual path, but do not include the angle brackets in the command.

Never place real credentials, tokens, private hostnames, personal data, or confidential customer information in examples.

### Show success and failure

Include expected output when it helps verification:

```text
Tests: 24 passed, 0 failed
```

Document common errors, their causes, and recovery steps. Do not hide cleanup steps or destructive side effects; warn before commands that delete or overwrite data.

### Keep examples maintainable

- Prefer small examples that demonstrate one concept.
- Generate reference output from the implementation when practical.
- Test code samples in CI when they are part of supported behavior.
- Update or remove examples when their underlying feature changes.

## Use links, images, and diagrams carefully

### Links

- Use descriptive text such as `[Local development setup](docs/local-development.md)`, not “click here.”
- Use relative links for repository content so branches, forks, and mirrors resolve correctly.
- Match path capitalization exactly; repository hosting paths may be case-sensitive.
- Link to the most specific stable heading or document available.
- Pin external technical references to a version when unversioned pages could become inaccurate.
- Check that link targets exist and that generated heading anchors work on both platforms.

### Images and screenshots

- Prefer text when text communicates the same information.
- Add concise alt text that conveys the image’s purpose, not just “screenshot.”
- Crop images to the relevant area and keep text legible at normal zoom.
- Store images in a stable directory such as `docs/images/` and reference them relatively.
- Update screenshots when the interface changes; include the relevant version when useful.
- Remove secrets, personal information, access tokens, and irrelevant notifications before capture.

### Diagrams

Keep diagrams editable by storing their source alongside any rendered image. Provide a text explanation or equivalent structured content so the meaning does not depend on visual inspection alone.

## Design for accessibility

Accessible documentation benefits every reader.

- Preserve a logical heading hierarchy for navigation by screen readers.
- Write meaningful link text that remains understandable out of context.
- Add alt text to informative images and use empty alt text only for decorative images.
- Do not rely on color, position, or an image alone to communicate status or meaning.
- Keep table headers clear and avoid deeply merged or layout-only tables.
- Use plain language and expand unfamiliar abbreviations.
- Ensure command-line instructions do not depend exclusively on screenshots.
- Provide text alternatives for diagrams and videos.

## Keep documentation portable

GitHub and Gitea both render repository Markdown, but extensions and configuration can differ.

- Prefer widely supported Markdown syntax over platform-specific extensions.
- Verify rendering on every platform the project officially supports.
- Avoid relying on raw HTML, custom styles, or a particular theme.
- Treat advanced features such as mathematical notation and diagram rendering as optional enhancements, not the only way to understand the content.
- Use UTF-8 and normalize line endings through repository configuration.
- Avoid badges unless each badge communicates a current, useful status.
- Do not duplicate repository metadata manually when the platform already displays it reliably.

A repository should remain understandable when its documentation is read as plain text or through a mirror.

## Review and test changes

Documentation deserves the same review discipline as code.

### Author checks

Before opening a pull request:

- Preview the rendered page, including headings, anchors, lists, tables, and images.
- Run commands from a clean environment or a clearly described test setup.
- Confirm that relative links work from the document’s own directory.
- Check spelling, terminology, filenames, version numbers, and expected output.
- Remove obsolete instructions and unrelated changes.

### Reviewer checks

Review for:

- **Accuracy:** Does the document match the implementation?
- **Completeness:** Can the intended reader finish the task without undocumented knowledge?
- **Clarity:** Are prerequisites, steps, and outcomes unambiguous?
- **Scope:** Is the change focused, or does it mix unrelated edits?
- **Maintainability:** Is there a clear owner, and does the change create duplication?
- **Portability and accessibility:** Does the page work for supported platforms and readers?

Request documentation changes in the same pull request as behavior changes. Use code owners or an equivalent review rule for critical guides when available.

## Automate routine checks

Automation should catch repeatable problems while reviewers focus on meaning.

Useful checks include:

- Markdown style and structure linting
- Internal and external link validation
- Spell checking with a project-specific dictionary
- Detection of trailing whitespace and conflicting line endings
- Validation of generated API or configuration reference documentation
- Execution of supported examples and tutorials in a controlled environment

For example, a project using Node.js tooling may run Markdown linting locally with:

```bash
npx markdownlint-cli2 "**/*.md"
```

Choose versions and configuration that fit the repository, commit the configuration, and use the same checks in local development and CI. Do not add a tool merely because it is popular; add it when it prevents a real recurring problem.

If CI platforms differ between GitHub and Gitea, keep validation scripts platform-neutral and call them from thin platform-specific workflow definitions.

## Maintain documentation over time

### Assign ownership

Give each important document or section an accountable owner or owning team. Ownership means reviewing changes and accuracy, not restricting contributions.

### Update with the release process

Include documentation in feature, deprecation, migration, and release checklists. Update the changelog or release notes for user-visible changes rather than silently rewriting history.

### Use explicit version guidance

For version-specific pages, name the versions they cover. When behavior differs, show each supported variant clearly. Avoid leaving old commands without a deprecation or migration path.

### Revisit content regularly

Review high-traffic and high-risk documentation after releases, recurring support issues, security changes, and major refactors. Archive or remove obsolete pages rather than letting them compete with current guidance.

### Learn from readers

Treat repeated questions, failed onboarding, and unclear error reports as documentation feedback. Record documentation bugs as issues with the affected page, reader goal, and expected correction.

## Documentation definition of done

A documentation change is ready when:

- [ ] The audience, goal, and scope are clear.
- [ ] Prerequisites and supported versions are stated where needed.
- [ ] The page has one H1 and a logical heading hierarchy.
- [ ] Instructions include verification and relevant expected outcomes.
- [ ] Commands and examples have been tested.
- [ ] Links, images, and generated anchors have been checked.
- [ ] Images have useful alt text and diagrams have text alternatives.
- [ ] No secrets, personal data, or confidential information are exposed.
- [ ] Related behavior and documentation changes are reviewed together.
- [ ] Obsolete or duplicated content has been updated or removed.
- [ ] Automated checks pass, and any exceptions are justified.
- [ ] An owner or owning team is identifiable for future maintenance.

## README template

Copy this skeleton into a project and replace every illustrative value. Remove sections that do not apply, but preserve the reader-focused order.

```markdown
# Project name

One or two sentences describing what the project does, who it is for, and its main benefit.

## Features

- Key capability and why it matters
- Another key capability

## Requirements

- Supported operating systems or runtimes
- Required versions and external services

## Installation

1. State the prerequisite setup.
2. Provide a tested installation command.
3. Describe any required configuration.

## Quick start

Provide the shortest complete example that produces a useful result.
Include expected output or a verification step.

## Usage

Document common tasks and link to detailed guides in `docs/`.

## Configuration

Explain essential options, defaults, and environment variables.
Link to complete configuration reference documentation.

## Troubleshooting

List common errors, likely causes, and recovery steps.

## Contributing

Explain how to propose changes and link to the contribution guide,
development setup, and test instructions.

## Support

State where users should report bugs, request features, and ask questions.
Include a security-reporting channel when applicable.

## License

Name the project license and link to the license file.
```

## Contributing to documentation

Improvements are welcome when they make documentation more accurate, useful, accessible, or maintainable. For a documentation change:

1. Open an issue for substantial restructuring or when the intended outcome is unclear.
2. Keep corrections small and focused; separate factual fixes from stylistic rewrites when practical.
3. Describe the reader problem and list the validation performed in the pull request.
4. Include rendered previews or screenshots only when they clarify a presentation change.
5. Update related links, navigation, examples, and indexes in the same change.

When adopting this guide in a repository, link it to the project’s contribution workflow, identify its maintainer, and review it whenever the supported documentation or hosting workflow changes.

## References

These sources provide further reading and informed this guide. Platform features can vary by version and configuration, so verify behavior in the environments your project supports.

### GitHub and Gitea

- [GitHub Docs: About the repository README file](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes) — Recommended README content, repository placement, automatic outlines, section links, and relative paths.
- [GitHub Docs: Basic writing and formatting syntax](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) — GitHub’s supported Markdown syntax for headings, code, links, images, lists, and other formatting.
- [Gitea Docs: Markdown](https://docs.gitea.com/usage/repository/markdown/) — Gitea’s Markdown rendering, GitHub Flavored Markdown compatibility, link resolution, and configurable extensions.

### Markdown specifications

- [GitHub Flavored Markdown Spec](https://github.github.com/gfm/) — The formal specification for the Markdown dialect used for GitHub user content.
- [CommonMark Spec](https://spec.commonmark.org/) — The unambiguous base specification on which GitHub Flavored Markdown and many other Markdown implementations are built.

### Documentation design and writing

- [Diátaxis](https://diataxis.fr/) — A systematic framework for separating tutorials, how-to guides, technical reference, and explanation according to reader needs.
- [Write the Docs Documentation Guide](https://www.writethedocs.org/guide/) — Community-authored guidance on documentation planning, writing, tools, maintenance, and team practices.
- [Google developer documentation style guide](https://developers.google.com/style) — Detailed editorial guidance for technical documentation, including voice, terminology, formatting, procedures, and code examples.

### Accessibility

- [W3C Web Accessibility Initiative: Images Tutorial](https://www.w3.org/WAI/tutorials/images/) — Guidance for choosing appropriate alternative text for informative, decorative, functional, and complex images.
- [W3C WCAG: Understanding Link Purpose (In Context)](https://www.w3.org/WAI/WCAG22/Understanding/link-purpose-in-context.html) — The accessibility rationale and requirements for meaningful, identifiable link text.

### Validation tools

- [markdownlint-cli2](https://github.com/DavidAnson/markdownlint-cli2) — Configuration and usage for the Markdown linting command shown in this guide.
- [lychee](https://github.com/lycheeverse/lychee) — A link checker that can validate links in Markdown and other content locally or in CI.
