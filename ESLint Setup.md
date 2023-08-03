# ESLint Setup

## 1. `ng add @angular-eslint/schematics@14.1.2` (control your version)
## 2. `ng g @angular-eslint/schematics:convert-tslint-to-eslint your_project_name`
## 3. `npm i prettier eslint-plugin-prettier eslint-config-prettier eslint-plugin-import --save-dev`

## 4. Create .prettierrc.json (on same level as package.json) with
`{ "singleQuote": true, "trailingComma": "none", "endOfLine": "auto" }`

## 5. Start rules .eslintrc

`{
"root": true,
"plugins": [
"@typescript-eslint/eslint-plugin",
"import"
],
"ignorePatterns": ["projects/**/*"],
"overrides": [
{
"files": [
"*.ts"
],
"parserOptions": {
"project": [
"tsconfig.json"
],
"createDefaultProgram": true
},
"extends": [
"eslint:recommended",
"plugin:@angular-eslint/recommended",
"plugin:@angular-eslint/template/process-inline-templates",
"plugin:prettier/recommended",
"plugin:import/errors",
"plugin:import/warnings",
"plugin:import/typescript"
],
"rules": {
"@angular-eslint/component-selector": [
"error",
{
"prefix": "app",
"style": "kebab-case",
"type": "element"
}
],
"@angular-eslint/directive-selector": [
"error",
{
"prefix": "app",
"style": "camelCase",
"type": "attribute"
}
],
"@typescript-eslint/no-empty-function": "off",
"@typescript-eslint/no-unused-vars": "error",
"@typescript-eslint/no-use-before-define": "error",
"import/no-unresolved": "off",
"import/named": "warn",
"import/namespace": "warn",
"import/no-name-as-default": "off",
"import/export": "warn",
"import/order": [
"error",
{
"groups": ["builtin", "external", "parent", "sibling", "index"],
"newlines-between": "always",
"alphabetize": {
"order": "asc",
"caseInsensitive": true
}
}
],
"import/newline-after-import": [
"error",
{
"count": 1
}
],
"no-undef": "off",
"spaced-comment": [
"error",
"always",
{
"line": {
"markers": ["/"]
}
}
]
}
},
{
"files": ["*.component.html"],
"extends": [
"plugin:@angular-eslint/template/recommended"
],
"rules": {
"max-len": ["error", { "code": 140 }],
"spaced-comment": [
"off",
"always",
{
"line": {
"markers": ["/"]
}
}
]
}
},
{
"files": ["*spec.ts", "*mock.ts"],
"rules": {
"no-undef": "off",
"@typescript-eslint/no-unused-vars": [
"error",
{
"vars": "local",
"args": "none"
}
]
}
}
],
"rules": {
"prettier/prettier": [
"error",
{
"endOfLine": "auto"
}
],
"@angular-eslint/component-class-suffix": "error",
"@angular-eslint/component-selector": [
"error",
{
"type": "element",
"prefix": "app",
"style": "kebab-case"
}
],
"@angular-eslint/directive-class-suffix": "error",
"@angular-eslint/directive-selector": [
"error",
{
"type": "attribute",
"prefix": "app",
"style": "camelCase"
}
],
"@angular-eslint/no-host-metadata-property": "error",
"@angular-eslint/no-input-rename": "error",
"@angular-eslint/no-inputs-metadata-property": "error",
"@angular-eslint/no-output-rename": "error",
"@angular-eslint/no-outputs-metadata-property": "error",
"@angular-eslint/use-lifecycle-interface": "error",
"@angular-eslint/use-pipe-transform-interface": "error",
"@typescript-eslint/explicit-function-return-type": [
"warn"
],
"@typescript-eslint/naming-convention": [
"off"
],
"@typescript-eslint/consistent-type-definitions": "error",
"@typescript-eslint/dot-notation": "off",
"@typescript-eslint/explicit-member-accessibility": [
"off",
{
"accessibility": "explicit"
}
],
"@typescript-eslint/member-delimiter-style": [
"error",
{
"multiline": {
"delimiter": "semi",
"requireLast": true
},
"singleline": {
"delimiter": "semi",
"requireLast": false
}
}
],
"@typescript-eslint/member-ordering": "error",
"@typescript-eslint/no-explicit-any": "error",
"@typescript-eslint/no-empty-function": "off",
"@typescript-eslint/no-empty-interface": "error",
"@typescript-eslint/no-inferrable-types": "error",
"@typescript-eslint/no-misused-new": "error",
"@typescript-eslint/no-non-null-assertion": "error",
"@typescript-eslint/no-unused-expressions": [
"error", { "allowTernary": true }],
"no-unused-vars": "off",
"@typescript-eslint/no-unused-vars": "error",
"@typescript-eslint/no-use-before-define": "error",
"@typescript-eslint/prefer-function-type": "error",
"@typescript-eslint/type-annotation-spacing": "error",
"@typescript-eslint/unified-signatures": "error",
"@typescript-eslint/no-shadow": [
"error",
{
"hoist": "all"
}
],
"no-multiple-empty-lines": "error",
"arrow-body-style": "off",
"camelcase": "off",
"curly": "error",
"eqeqeq": [
"error", "smart"
],
"guard-for-in": "error",
"id-blacklist": "off",
"id-match": "off",
"max-len": [
"error",
{
"code": 256
}
],
"no-bitwise": "error",
"no-caller": "error",
"no-console": "error",
"no-debugger": "error",
"no-empty": "off",
"no-eval": "error",
"no-fallthrough": "error",
"no-new-wrappers": "error",
"no-redeclare": "error",
"no-restricted-imports": "error",
"no-throw-literal": "error",
"no-undef-init": "error",
"no-underscore-dangle": "off",
"no-unused-labels": "error",
"no-var": "error",
"prefer-const": "error",
"radix": "error",
"spaced-comment": [
"error",
"never",
{
"line": {
"markers": ["/"]
}
}
],
"lines-between-class-members": [
"error", "always", { "exceptAfterSingleLine": true }]
}
}`

## 6. Configure your package.json

`"scripts": {
"ng": "ng",
"start": "ng serve",
"build": "ng build",
"test": "ng test",
"lint": "npx eslint 'src/**/*.{js,jsx,ts,tsx,html,css,scss}' --quiet --fix",
"format": "npx prettier 'src/**/*.{js,jsx,ts,tsx,html,css,scss}' --write",
"e2e": "ng e2e"
}`

## FIX eslint error this.libOptions.parse (if you have it)
### `npm install eslint@8.22.0 --save-exact`

## Usage:
### `npm run lint:fix`
