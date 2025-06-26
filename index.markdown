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

JSONC doesn't allow trailing commas; however, we encourage parsers to be lenient and handle trailing commas gracefully where possible to reduce the risk of human edits introducing parsing errors.[^1]

## Semantics

Comments in JSONC are ignored during parsing, allowing developers to annotate their JSON data without affecting its structure or content.

## File extensions

The recommended file extension for JSONC documents is `.jsonc`.

The extension `.json` should be avoided, but is supported if a mode line is present at the start of the file:

For instance:
```jsonc
// -*- mode: jsonc -*-
```
or
```jsonc
// -*- jsonc -*-
```

## Main Use Cases

- Configuration Files: JSONC is useful for configuration files where comments can provide explanations or instructions.
- Data Annotation: JSONC allows developers to annotate JSON data with comments for better understanding and maintenance.

## JSONC Validator

Try out JSONC validation directly in your browser:

<div class="jsonc-validator">
  <textarea id="jsonc-input" placeholder="Enter your JSONC data here...
{
  // This is a comment
  &quot;name&quot;: &quot;Example&quot;,
  &quot;version&quot;: &quot;1.0.0&quot;
}" rows="10"></textarea>
  
  <div class="validator-controls">
    <button id="validate-btn" onclick="validateJSONC()">Validate JSONC</button>
    <button id="clear-btn" onclick="clearValidator()">Clear</button>
  </div>
  
  <div id="validation-result" class="validation-result"></div>
</div>

<script type="module">
import * as jsoncParser from 'https://cdn.skypack.dev/jsonc-parser@3.2.0';

// Make the parser available globally for the onclick handlers
window.jsoncParser = jsoncParser;
</script>
<script>
async function validateJSONC() {
  const input = document.getElementById('jsonc-input');
  const result = document.getElementById('validation-result');
  const validateBtn = document.getElementById('validate-btn');
  
  const jsoncText = input.value.trim();
  
  if (!jsoncText) {
    result.innerHTML = '<div class="error">Please enter some JSONC data to validate.</div>';
    return;
  }
  
  validateBtn.textContent = 'Validating...';
  validateBtn.disabled = true;
  
  try {
    // Wait for the ES module to be loaded
    if (!window.jsoncParser) {
      result.innerHTML = '<div class="error">JSONC parser library is still loading. Please try again in a moment.</div>';
      validateBtn.textContent = 'Validate JSONC';
      validateBtn.disabled = false;
      return;
    }
    
    const parseErrors = [];
    const parsed = window.jsoncParser.parse(jsoncText, parseErrors);
    
    if (parseErrors.length > 0) {
      let errorMessages = parseErrors.map(error => {
        const line = jsoncText.substring(0, error.offset).split('\n').length;
        const column = error.offset - jsoncText.lastIndexOf('\n', error.offset - 1);
        return `Line ${line}, Column ${column}: ${getErrorMessage(error.error)}`;
      }).join('<br>');
      
      result.innerHTML = `<div class="error">
        <strong>❌ Invalid JSONC</strong><br>
        ${errorMessages}
      </div>`;
    } else {
      const jsonString = JSON.stringify(parsed, null, 2);
      result.innerHTML = `<div class="success">
        <strong>✅ Valid JSONC!</strong><br>
        Successfully parsed ${Object.keys(parsed || {}).length} top-level properties.
        <details>
          <summary>Parsed JSON (click to expand)</summary>
          <pre><code>${escapeHtml(jsonString)}</code></pre>
        </details>
      </div>`;
    }
  } catch (error) {
    result.innerHTML = `<div class="error">
      <strong>❌ Parsing Error</strong><br>
      ${escapeHtml(error.message)}
    </div>`;
  }
  
  validateBtn.textContent = 'Validate JSONC';
  validateBtn.disabled = false;
}

function clearValidator() {
  document.getElementById('jsonc-input').value = '';
  document.getElementById('validation-result').innerHTML = '';
}

function getErrorMessage(errorCode) {
  const errorMessages = {
    1: 'Invalid symbol',
    2: 'Invalid number format',
    3: 'Property name expected',
    4: 'Value expected',
    5: 'Colon expected',
    6: 'Comma expected',
    7: 'Closing brace expected',
    8: 'Closing bracket expected',
    9: 'End of file expected',
    10: 'Invalid comment token',
    11: 'Unexpected end of comment',
    12: 'Unexpected end of string',
    13: 'Unexpected end of number',
    14: 'Invalid Unicode',
    15: 'Invalid escape character',
    16: 'Invalid character'
  };
  return errorMessages[errorCode] || `Error code ${errorCode}`;
}

function escapeHtml(text) {
  const div = document.createElement('div');
  div.textContent = text;
  return div.innerHTML;
}
</script>

## Tools and Libraries
Several tools and libraries support JSONC, enabling developers to parse and generate JSONC data easily.

Here is a non-exhaustive list:

**JavaScript/TypeScript**:
- [microsoft/node-jsonc-parser](https://github.com/microsoft/node-jsonc-parser)

**C++**
- [stephenberry/glaze](https://github.com/stephenberry/glaze)

**Elexir**
- [massivefermion/jsonc](https://github.com/massivefermion/jsonc)

**Go**
- [tidwall/jsonc](https://github.com/tidwall/jsonc)

**Python**
- [NickolaiBeloguzov/jsonc-parser](https://github.com/NickolaiBeloguzov/jsonc-parser)

**Rust**
- [dprint/jsonc-parser](https://github.com/dprint/jsonc-parser)

---

[^1]: In VS Code, "the [JSONC] mode also accepts trailing commas, but they are discouraged and the editor will display a warning." ([source](https://code.visualstudio.com/docs/languages/json#_json-with-comments))


