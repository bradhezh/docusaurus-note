# Docusaurus

Docusaurus is a static-site generator, building single-page applications with React. It can be used to create personal websites, blogs, or documentation sites, and then users can focus on content written in Markdown.

## Quick Start

```bash
pnpm create docusaurus markdown-example classic --typescript
cd markdown-example
pnpm start

gh repo create markdown-example --public
git init
git add .
git commit -m "first commit"
git branch -M master
git remote add origin https://github.com/bradhezh/markdown-example.git
git push -u origin master
```

## Configuration

```bash
rm -rf docs/* src/pages/index.tsx

# for math formulas
pnpm add remark-math rehype-mathjax
```

**`docusaurus.config.ts`**
```typescript
...
import remarkMath from "remark-math";
import rehypeMathjax from "rehype-mathjax";
...
const config: Config = {
  title: "Markdown Example",
  tagline: "A comprehensive Markdown example using Docusaurus",
  ...
  url: "https://bradhezh.github.io",
  baseUrl: "/markdown-example/",
  organizationName: "bradhezh",
  projectName: "markdown-example",
  ...
  presets: [
    [
      ...
      {
        docs: {
          routeBasePath: "/",
          sidebarPath: "./sidebars.ts",
          remarkPlugins: [remarkMath],
          rehypePlugins: [rehypeMathjax],
        },
        blog: false,
        ...
      } ...,
    ],
  ],

  themeConfig: {
    image: ...,
    colorMode: ...,
    navbar: {
      title: "Markdown Example",
      logo: {
        alt: "Markdown Example Logo",
        ...
      }
      items: [
        {
          type: "docSidebar",
          ...
          label: "Table of Contents",
        },
        {
          href: "https://github.com/bradhezh/markdown-example",
          ...
        },
      ],
    },
    prism: ...,
    docs: { sidebar: { hideable: true } },
    tableOfContents: { minHeadingLevel: 2, maxHeadingLevel: 4 },
  } ...,
};
...
```

**`src/css/custom.css`**
```css
...
:root {
  ...
  --ifm-font-family-base: "Lucida Grande";
  --ifm-font-family-monospace: "Monaco";
}
...
/* for pdf */
.markdown {
  font-size: 0.8rem;
}

.markdown p {
  margin-bottom: 0.5rem;
}

.markdown h1 {
  font-size: 1.6rem;
}

.markdown h2 {
  font-size: 1.4rem;
}

.markdown h3 {
  font-size: 1.2rem;
}
```

**`docs/01-intro.md`**
```md
---
title: 1 - Introduction
slug: /
---

This is the introduction. `slug: /` make this the root.
```

**`docs/02-chapter2/_category_.json`**
```json
{
  "label": "2 - Chapter2"
}
```

**`docs/02-chapter2/01-sect2.1.md`**
```md
---
title: 2.1 - Section2.1
---

This is section 2.1.
```

**`.github/workflows/ghpages.yml`**
```yml
on:
  push:
    branches: [master]
  pull_request:
    branches: [master]

jobs:
  deploy:
    permissions:
      contents: read
      pages: write
      id-token: write
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22
      - name: Setup pnpm
        uses: pnpm/action-setup@v2
        with:
          version: 10
      - name: Install Dependencies
        run: pnpm i --frozen-lockfile
      - name: Build
        run: pnpm build
      - name: Upload
        uses: actions/upload-pages-artifact@v3
        with:
          path: "./build"
      - name: Deploy
        uses: actions/deploy-pages@v4
```

- Configure the repository on GitHub  
  _Settings_ > _Pages_ > _Build and deployment_ > _Source_: _GitHub Actions_


```bash
pnpm dlx docusaurus-docs-to-pdf \
  --docs-url http://localhost:3000/markdown-example \
  --pdf-path pdf/markdown-example.pdf \
  --css " \
    .docusaurus-toc-body * { font-family: 'Lucida Grande' } \
    .toc-title { visibility: hidden } \
    .toc-title::after { \
      content: 'Markdown Example'; visibility: visible; \
      position: absolute; left: 0; top: 0; width: 100%; \
      padding-bottom: 4px; border-bottom: 4px solid #000; \
      font-size: 32px; font-weight: bold; color: #000 } \
    .docusaurus-toc-body a::after { display: none } \
    .docusaurus-toc-body .toc-level-0.toc-directory a, \
    .docusaurus-toc-body .toc-level-1.toc-directory a { font-weight: normal }"
```
