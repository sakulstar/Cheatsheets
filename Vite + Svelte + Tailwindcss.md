# Vite + Svelte + Tailwindcss
### Info
In this guide tailwindcss version 3.3.3 is being installed.

---
### Prerequisites
1. [Node.js](https://nodejs.org/en)
2. [npm](https://www.npmjs.com/) which comes installed with node.js

---
### Install

1. Create a vite project with svelte while replacing "my-project" with the name of your project.
```bash
npm create vite@latest my-project -- --template svelte
cd my-project
```
2. Install Tailwind CSS and generate the *tailwind.config.js* and *postcss.config.js* files.
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```
3. Replace contents inside of *tailwind.config.js* with the code below.
```js
/** @type {import('tailwindcss').Config} */

export default {
  content: ["./index.html", "./src/**/*.{svelte,js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  variants: {
    backgroundColor: ['responsive', 'hover', 'focus', 'active']
  },
  plugins: [],
};
```
4. Add the tailwind directives to the *app.css* files located at *./src/app.css*.
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
5. Add automatic build script inside of *package.json*.
```json
{
  "name": "my-project",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "watch": "vite build --watch", // This needs to be added.
    "preview": "vite preview"
  },
  "devDependencies": {
    "@sveltejs/vite-plugin-svelte": "^2.4.2",
    "autoprefixer": "^10.4.15",
    "postcss": "^8.4.28",
    "prettier": "^3.0.2",
    "prettier-plugin-svelte": "^3.0.3",
    "prettier-plugin-tailwindcss": "^0.5.3",
    "svelte": "^4.0.5",
    "tailwindcss": "^3.3.3",
    "vite": "^4.4.5"
  }
}
```
5. Start the build process.
```bash
npm install
npm run dev
```
6. In different terminal run watch script.
```bash
npm run watch
```
7. Remember to have the two commands above running each time you develop.

---
### Extra
1. Install [IntelliSense for VS Code](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss)
2. Install [Prettier tailwindcss plugin](https://github.com/tailwindlabs/prettier-plugin-tailwindcss)
3. Turn off [unknownAtRules](https://bobbyhadz.com/blog/unknown-at-rule-tailwindcss-warning-solved)