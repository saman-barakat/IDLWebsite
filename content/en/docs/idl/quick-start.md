---
title: "How to use IDL"
description: "One page summary of how to start a new Doks project."
lead: ""
date: 2020-11-16T13:59:39+01:00
lastmod: 2020-11-16T13:59:39+01:00
draft: false
images: []
menu:
  docs:
    parent: "idl"
weight: 110
toc: true
---

IDL dependencies can be straightforwardly specified in OAS specifications using **IDL4OAS**, an OAS extension. You just need to include the keyword **x-dependencies** at the operation level, and include the list of dependencies as an array. Take a look at this simple example:

```markdown
paths:
  /example/route:
    get:
      operationId: exampleOperation
      parameters:
        - in: query
          type: boolean
          required: false
          name: p1
        - in: query
          type: integer
          required: false
          name: p2
        - in: query
          type: string
          required: false
          name: p3
      x-dependencies:
        - Or(p1, p2, p3);
        - IF p1==true THEN p2;
```

## Requirements

- [Git](https://git-scm.com/) — latest source release
- [Node.js](https://nodejs.org/) — latest LTS version or newer

{{< details "Why Node.js?" >}}
Doks uses npm (included with Node.js) to centralize dependency management, making it [easy to update]({{< relref "how-to-update" >}}) resources, build tooling, plugins, and build scripts.
{{< /details >}}

## Start a new Doks project

Create a new site, change directories, install dependencies, and start development server.

### Create a new site

Doks is available as a child theme and a starter theme.

#### Child theme

- Intended for novice to intermediate users
- Intended for minor customizations
- [Easily update npm packages]({{< relref "how-to-update" >}}) — __including__ [Doks](https://www.npmjs.com/package/@hyas/doks)

```bash
git clone https://github.com/h-enk/doks-child-theme.git my-doks-site
```

#### Starter theme

- Intended for intermediate to advanced users
- Intended for major customizations
- [Easily update npm packages]({{< relref "how-to-update" >}})

```bash
git clone https://github.com/h-enk/doks.git my-doks-site
```

{{< details "Help me choose" >}}
Not sure which one is for you? Pick the child theme.
{{< /details >}}

### Change directories

```bash
cd my-doks-site
```

### Install dependencies

```bash
npm install
```

### Start development server

```bash
npm run start
```

Doks will start the Hugo development webserver accessible by default at `http://localhost:1313`. Saved changes will live reload in the browser.

## Other commands

Doks comes with commands for common tasks. [Commands →]({{< relref "commands" >}})
