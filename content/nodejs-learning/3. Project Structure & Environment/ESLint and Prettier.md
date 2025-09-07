**ESLint** and **Prettier** are essential tools for keeping your Node.js code **consistent, readable, and error-free**. Let’s go step by step.

---

## 📦 1. Install ESLint & Prettier

```bash
npm install --save-dev eslint prettier eslint-config-prettier eslint-plugin-prettier
```

- `eslint` → detects errors & enforces coding standards.
    
- `prettier` → automatically formats code.
    
- `eslint-plugin-prettier` → integrates Prettier rules into ESLint.
    
- `eslint-config-prettier` → disables ESLint rules that conflict with Prettier.
    

---

## ⚙️ 2. Initialize ESLint

```bash
npx eslint --init
```

Follow the prompts:

- How would you like to use ESLint? → To check syntax, find problems, and enforce code style
    
- What type of modules? → CommonJS (Node) or ES Modules
    
- Framework? → None (for pure Node.js)
    
- Where will your code run? → Node
    
- Preferred style guide? → Standard / Airbnb / Custom
    
- Format of config file → `.eslintrc.js`
    

This creates an `.eslintrc.js` config file.

---

## 📝 3. Configure ESLint + Prettier

`.eslintrc.js` example:

```js
module.exports = {
  env: {
    node: true,
    es2021: true,
  },
  extends: [
    "eslint:recommended",
    "plugin:prettier/recommended"
  ],
  parserOptions: {
    ecmaVersion: 12,
    sourceType: "module",
  },
  rules: {
    "no-unused-vars": "warn",
    "no-console": "off",
  },
};
```

- `"plugin:prettier/recommended"` → ensures ESLint runs Prettier rules automatically.
    

---

## ⚡ 4. Create Prettier Config

Optional, but useful for consistent formatting.

`.prettierrc` example:

```json
{
  "semi": true,
  "singleQuote": true,
  "printWidth": 80,
  "trailingComma": "all"
}
```

- `semi` → add semicolons
    
- `singleQuote` → use single quotes
    
- `printWidth` → line length
    
- `trailingComma` → add commas to multi-line objects/arrays
    

---

## 🛠️ 5. Add Scripts in `package.json`

```json
"scripts": {
  "lint": "eslint .",
  "lint:fix": "eslint . --fix",
  "format": "prettier --write ."
}
```

- `npm run lint` → check code for errors
    
- `npm run lint:fix` → auto-fix lint issues
    
- `npm run format` → auto-format code with Prettier
    

---

## ✅ 6. Benefits

- **Consistency:** All developers follow the same style.
    
- **Error prevention:** ESLint catches common mistakes (undefined vars, typos).
    
- **Automatic formatting:** Prettier makes code readable without manual effort.
    
- **Integration:** Works with most IDEs (VSCode, WebStorm) for real-time feedback.
    

---
