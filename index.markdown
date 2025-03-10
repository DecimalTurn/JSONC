---

layout: default
---

**Notice:** This is a draft of the JSONC Specification and is subject to change.

## Introduction

JSONC (JSON with Comments) is an extension of JSON (JavaScript Object Notation) that allows comments within JSON data. This specification defines the syntax and semantics of JSONC.

## Syntax

JSONC follows the same syntax rules as JSON with the addition of JavaScript style comments. Comments can be either single-line or multi-line.

### Single-line Comments

Single-line comments start with `//` and extend to the end of the line.

```jsonc
{
    // This is a single-line comment
    "name": "John Doe",
    "age": 30
}
```

### Multi-line Comments

Multi-line comments start with `/*` and end with `*/`. They can span multiple lines.

```jsonc
{
    /*
      This is a multi-line comment
      that spans multiple lines
    */
    "name": "Jane Doe",
    "age": 25
}
```

## Trailing commas

JSONC doesn't allow trailing commas; however, we encourage parsers to be lenient and handle trailing commas gracefully where possible to reduce the risk of human edits introducing parsing errors.

## Semantics

Comments in JSONC are ignored during parsing, allowing developers to annotate their JSON data without affecting its structure or content.

## File extensions

## Main Use Cases

- Configuration Files: JSONC is useful for configuration files where comments can provide explanations or instructions.
- Data Annotation: JSONC allows developers to annotate JSON data with comments for better understanding and maintenance.

## Tools and Libraries
Several tools and libraries support JSONC, enabling developers to parse and generate JSONC data easily.

Here is a non-exhaustive list:

**JavaScript/TypeScript**:
- [microsoft/node-jsonc-parser](https://github.com/microsoft/node-jsonc-parser)

**Python**
- [NickolaiBeloguzov/jsonc-parser](https://github.com/NickolaiBeloguzov/jsonc-parser)

**C++**
- [stephenberry/glaze](https://github.com/stephenberry/glaze)
  
**Go**
- [tidwall/jsonc](https://github.com/tidwall/jsonc)

**Elexir**
- [massivefermion/jsonc](https://github.com/massivefermion/jsonc)
