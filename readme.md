# Next.js + Inferno

Use [Inferno](https://infernojs.org) :fire: with [Next.js](https://github.com/zeit/next.js) to get even faster :zap: rendering.

## Installation

```
npm install --save next-inferno inferno inferno-compat inferno-clone-vnode inferno-create-class inferno-create-element
```

or

```
yarn add next-inferno inferno inferno-compat inferno-clone-vnode inferno-create-class inferno-create-element
```

## Usage

Create a `next.config.js` in your project

```js
// next.config.js
const withInferno = require('next-inferno')
module.exports = withInferno()
```

Then create a `server.js`

```js
// server.js
require('next-inferno/alias')()
const { createServer } = require('http')
const next = require('next')

const app = next({ dev: process.env.NODE_ENV !== 'production' })
const handle = app.getRequestHandler()
const port = process.env.PORT || 3000

app.prepare()
.then(() => {
  createServer(handle)
  .listen(port, () => {
    console.log(`> Ready on http://localhost:${port}`)
  })
})
```

Then add or change "scripts" section of your `package.json`:
```json
...
"scripts": {
  "dev": "node server.js",
  "build": "next build",
  "start": "NODE_ENV=production node server.js"
},
...
```

Optionally you can add your custom Next.js configuration as parameter

```js
// next.config.js
const withInferno = require('next-inferno')
module.exports = withInferno({
  webpack(config, options) {
    return config
  }
})
```

## TypeScript support
Looking for [TypeScript](http://www.typescriptlang.org/) support? Just follow that easy steps to use it in your Next projects:

1. Install `typescript` and `@zeit/next-typescript` (aslo you can use [`next-awesome-typescript`](https://github.com/saitonakamura/next-awesome-typescript) for perfomance reasons):

```
npm install --save typescript @zeit/next-typescript
```

or

```
yarn add typescript @zeit/next-typescript
```

2. Edit `next.config.js`:

```js
// next.config.js
const withInferno = require('next-inferno')
const withTypescript = require('@zeit/next-typescript')

module.exports = withInferno(withTypescript())
```

3. Create 'tsconfig.json' file:

```json
{
  "compileOnSave": false,
  "compilerOptions": {
    "target": "esnext",
    "module": "esnext",
    "jsx": "preserve",
    "allowSyntheticDefaultImports": true,
    "allowJs": true,
    "moduleResolution": "node",
    "sourceMap": true,
    "removeComments": false,
    "baseUrl": ".",
    "lib": [
      "dom",
      "es2015",
      "es2016"
    ]
  }
}
```

4. Write code :fire:

### Check on compile
By default, next's TypeScript plugin only transpiles your code without type checks. It means that you can see type errors only in your IDE or code editor (for example VS Code can do it out of the box).

If you want to enable typechecks on compile to see type errors in terminal or browser, make the following changes in 'next.config.js':

```js
// next.config.js
const withInferno = require('next-inferno')
const withTypescript = require('@zeit/next-typescript')

module.exports = withInferno(withTypescript({
  webpack(config, options) {
    return config
  },
  typescriptLoaderOptions: {
    transpileOnly: false
  }
}))
```

