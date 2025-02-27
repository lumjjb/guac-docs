---
layout: page
title: Set up the GUAC Visualizer
permalink: /guac-visualizer/
parent: GUAC use cases
nav_order: 5
---

# Set up the GUAC Visualizer

The GUAC Visualizer is an experimental utility that can be used to interact
with GUAC services. It acts as a way to visualize the software supply chain
graph, as well as a means to explore the supply chain and prototype policies.

Since the GUAC Visualizer is still in an early experimental stage, it is likely
that there may be some unexpected behavior or usage problems.

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## Requirements

- [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
- [Yarn](https://classic.yarnpkg.com/lang/en/docs/install/#mac-stable)
- [Docker](https://docs.docker.com/get-docker/)
- A fresh copy of the [GUAC service infrastructure through Docker Compose]({{ site.baseurl }}{%link setup.md %})

## Step 1: Clone the GUAC visualizer repo

1. We highly suggest cloning the GUAC visualizer repo in the same path as you cloned the main GUAC repo:
  ```bash
  git clone git@github.com:guacsec/guac-visualizer.git
  ```

2. Change directories into the repo:
  ```bash
  cd guac-visualizer
  ```

All commands run throughout this guide should be in this working directory.

## Step 2: Run the guac-visualizer from source

1. Install the guac-visualizer dependencies:
  ```bash
  yarn install
  ```

   The output should look like:

    ```
    $ yarn install
    yarn install v1.22.19
    warning package-lock.json found. Your project contains lock files generated by tools other than Yarn. It is advised not to mix package managers in order to avoid   resolution inconsistencies caused by unsynchronized lock files. To clear this warning, remove package-lock.json.
    [1/4] 🔍  Resolving packages...
    [2/4] 🚚  Fetching packages...
    warning Pattern ["@apollo/client@latest"] is trying to unpack in the same destination "/Users/lumb/Library/Caches/Yarn/v6/npm-@apollo-client-3.7.12-  9ddd355d0788374cdb900e5f40298b196176952b-integrity/node_modules/@apollo/client" as pattern ["@apollo/client@^3.7.10"]. This could result in non-deterministic      
    behavior, skipping.
    [3/4] 🔗  Linking dependencies...
    warning "@graphql-codegen/cli > @graphql-tools/code-file-loader > @graphql-tools/graphql-tag-pluck > @babel/plugin-syntax-import-assertions@7.20.0" has unmet peer   dependency "@babel/core@^7.0.0-0".
    warning " > @graphql-codegen/typescript-react-apollo@3.3.7" has unmet peer dependency "graphql-tag@^2.0.0".
    warning "cosmos-over-cytoscape > html-webpack-plugin@5.5.0" has unmet peer dependency "webpack@^5.20.0".
    warning " > react-paginated-list@1.1.6" has incorrect peer dependency "react@>=16.8.25 <=16.13.1".
    warning " > react-paginated-list@1.1.6" has incorrect peer dependency "react-dom@>=16.8.25 <= 16.13.1".
    warning " > react-paginated-list@1.1.6" has incorrect peer dependency "styled-components@>=4.4.1 <= 5.1.1".
    warning " > styled-components@5.3.9" has unmet peer dependency "react-is@>= 16.8.0".
    [4/4] 🔨  Building fresh packages...
    ✨  Done in 12.78s.
    ```

2. Run the server (which will run by default on `http://localhost:3000`):
  ```bash
  npm run dev
  ```

   The ouput should look like:

    ```bash
    $ npm run dev

    > guac-visualizer@0.1.0 dev
    > next dev

    ready - started server on 0.0.0.0:3000, url: http://localhost:3000
    warn  - You have enabled experimental feature (appDir) in next.config.js.
    warn  - Experimental features are not covered by semver, and may cause unexpected or broken application behavior. Use at your own risk.

    info  - Thank you for testing `appDir` please leave your feedback at https://nextjs.link/app-feedback
    (node:99525) ExperimentalWarning: stream/web is an experimental feature. This feature could change at any time
    (Use `node --trace-warnings ...` to show where the warning was created)
    event - compiled client and server successfully in 1867 ms (560 modules)
    ```

## Step 3. Access the server

Access the server through a web browser and navigate to
http://localhost:3000. The page will look like the following:

![ui](assets/images/guacvisserver.png)

For more information on how to use the GUAC visualizer, take a look at some of
our [GUAC demos]({{ site.baseurl }}{%link guac-use-cases.md %}).

### Change the GraphQL endpoint

This should run the service by default on http://localhost:3000. However, this
does assume that the GraphQL server is run on http://localhost:8080/query. If
this is not the case, you may edit the `next.config.js`.

The default configuration is set to http://localhost:8080/query as seen here:

```
$ cat next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  transpilePackages: ['react-cytoscapejs'],
  typescript: {
    ignoreBuildErrors: true
  },
  experimental: {
    appDir: true,
  },
  rewrites: async () => {
    return [
      {
        source: '/api/graphql',
        destination: 'http://localhost:8080/query'
      },
    ]
  }
}

module.exports = nextConfig
```

To change the GraphQL endpoint you can edit the rewrites destination:

```
$ cat next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  transpilePackages: ['react-cytoscapejs'],
  typescript: {
    ignoreBuildErrors: true
  },
  experimental: {
    appDir: true,
  },
  rewrites: async () => {
    return [
      {
        source: '/api/graphql',
        destination: 'http://example.org:8080/query'
      },
    ]
  }
}

module.exports = nextConfig
```
