---

layout: default
---

# Trailing Commas and JSONC

## Why Trailing Commas Aren't Part of the JSONC Specification?

Trailing commas are not part of the JSONC Specification because the reference implementation, [jsonc-parser](https://www.npmjs.com/package/jsonc-parser), does not allow them unless explicitly configured. The `allowTrailingComma` option is set to `false` by default, so any trailing comma will result in a parsing error. 

The reason why this specification chose the default behavior of the parser as the reference for the standard is to ensure that JSONC remains compatible with the broader JSON ecosystem, which does not allow trailing commas. This decision helps maintain consistency and predictability across different parsers and implementations. Namely, the [TSConfig](https://www.typescriptlang.org/tsconfig/) and [ESLint config](https://eslint.org/docs/latest/use/configure/configuration-files) files, which are widely used in the JavaScript ecosystem, do not allow trailing commas in their JSONC files.

| File Name                   | Main Parser       | Main parser supports trailing commas | Known issues with other tools |
|-----------------------------|-------------------|--------------------------------------|-------------------------------|
| tsconfig.json               | [TypeScript][1]   | ✅                                   |                               |
| .eslintrc.json              | [eslint][2]       | ❌                                   |                               |
| .babelrc                    |                   |                                      |                               |
| .devcontainer.json          |                   |                                      |                               |
| .jscsrc                     |                   |                                      |                               |
| .jshintrc                   |                   |                                      |                               |
| .jslintrc                   |                   |                                      |                               |
| .swcrc                      |                   |                                      |                               |
| api-extractor.json          |                   |                                      |                               |
| devcontainer.json           |                   |                                      |                               |
| jsconfig.json               |                   |                                      |                               |
| language-configuration.json |                   |                                      |                               |
| tslint.json                 |                   |                                      |                               |

The exclusion of trailing commas also facilitates the creation of tools and libraries that can parse JSONC without needing to handle additional syntax variations. This helps ensure that JSONC remains a lightweight and straightforward extension of JSON, primarily focused on adding comments without introducing significant complexity.

## Can a Parser That Chooses to Support Trailing Commas Still Be Considered a JSONC Parser?

Yes, however this is not part of the JSONC.org specification and such support would be considered an extension or variation of the standard JSONC format. This means that while a parser may allow trailing commas, it may not be compatible with all JSONC parsers or tools that strictly adhere to the JSONC specification without trailing commas.

## Trailing Commas in VS Code

The "JSON with Comments" mode in VS Code used to allow trailing commas without any warnings by default, but this was eventually changed to discourage their use and promote better compatibility with other JSONC parsers ([source](https://github.com/microsoft/vscode/issues/102061)).

At the time of writing this document, the "JSON with Comments" mode still accepts trailing commas, but it discourages their usage by displaying a warning ([source](https://code.visualstudio.com/docs/languages/json#_json-with-comments)) unless the file is one of the VS Code official configuration files. The exclusion of those configuration files comes from the JSON schema used. The schema for these files explicitly allow trailing commas, which is why they are accepted without warnings in that specific context.



[1]: https://github.com/microsoft/TypeScript/blob/5f183ad73dc1500209619cf52e174c45d73f8617/src/compiler/parser.ts#L1646
[2]: https://github.com/eslint/eslint