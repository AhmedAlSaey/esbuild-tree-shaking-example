# Esbuild Tree Shaking Example
This repository is an example of how tree shaking works in esbuild.

## What is tree shaking?

Tree shaking is a term used to describe the removal of dead code. With esbuild, this operation is static and relies in the import/export statements statically defined in the code.

## Usage

1 - Clone the repository

2 - Install node packages
```bash
npm install
```  

3 - Open `input_typescript.ts` and notice how this file only uses one function from the `library.ts` file, and explicitly imports an unused function.

`input_typescript.ts`:

```typescript
import { SayHello, SayHi } from "./library";

SayHello();
```

`library.ts`:

```typescript
const SayHello = () => {
  console.log("Hello, esbuild!");
};
const SayHi = () => {
  console.log("Hi, esbuild!");
};

export { SayHello, SayHi };

```  

4 - Bundle your code with this command:
```bash
npx esbuild input_typescript.ts --outfile=output.js --bundle --loader:.ts=ts
```  
Treee shaking is the default when you use the `--bundle` flag, but you can explicitly allow tree shaking by using this flag: `--tree-shaking=true`

5 - Open `output.js`, notice how to bundler only included the function used by the code, and ignore the unused import:  

`output.js`:
```javascript
(() => {
  // library.ts
  var SayHello = () => {
    console.log("Hello, esbuild!");
  };

  // input_typescript.ts
  SayHello();
})();
```
