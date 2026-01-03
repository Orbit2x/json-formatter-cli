# JSON Formatter CLI & Online Tools - Complete Guide

[![JSON Formatter](https://img.shields.io/badge/Try%20Online-JSON%20Formatter-blue)](https://orbit2x.com/json-formatter)
[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

> **Need to format JSON quickly?** Try the free [JSON Formatter at Orbit2x](https://orbit2x.com/json-formatter) - beautify, minify, validate, and sort JSON instantly in your browser.

## Quick Start - Format JSON Online

**No installation needed!** Use the free web tool:

👉 **[JSON Formatter at Orbit2x](https://orbit2x.com/json-formatter)**

Features:
- ✅ Beautify JSON with 2/4 space indentation
- ✅ Minify JSON for production
- ✅ Validate JSON syntax with detailed errors
- ✅ Sort keys alphabetically
- ✅ Syntax highlighting (Dracula theme)
- ✅ Real-time statistics (size, keys, depth)
- ✅ Keyboard shortcuts (Ctrl+F to format)
- ✅ Export formatted JSON
- ✅ 100% client-side, privacy-focused

---

## Command Line Tools

### Using `jq` (Most Powerful)

```bash
# Install jq
brew install jq              # macOS
sudo apt-get install jq      # Ubuntu/Debian
choco install jq             # Windows

# Beautify JSON
cat input.json | jq '.'

# Minify JSON
cat input.json | jq -c '.'

# Sort keys alphabetically
cat input.json | jq -S '.'

# Format and save
jq '.' input.json > output.json

# Pretty print with 4-space indent
jq --indent 4 '.' input.json

# Extract specific fields
jq '.users[].name' data.json

# Filter arrays
jq '.[] | select(.age > 30)' users.json
```

### Using `python` (Built-in)

```bash
# Beautify JSON (2-space indent)
python -m json.tool input.json

# Beautify with 4-space indent
python -m json.tool --indent 4 input.json

# Minify JSON
python -c "import json,sys;print(json.dumps(json.load(sys.stdin)))" < input.json

# Sort keys
python -c "import json,sys;print(json.dumps(json.load(sys.stdin),indent=2,sort_keys=True))" < input.json
```

### Using `node` (JavaScript)

```bash
# Install globally
npm install -g json

# Beautify JSON
json < input.json

# Minify JSON
json -c < input.json

# Sort keys
json -o inspect < input.json
```

### Custom Bash Function

Add to `~/.bashrc` or `~/.zshrc`:

```bash
# Format JSON from clipboard (macOS)
jsonformat() {
    pbpaste | jq '.' | pbcopy
    echo "✅ JSON formatted and copied to clipboard"
}

# Format JSON file
jsonfile() {
    if [ -z "$1" ]; then
        echo "Usage: jsonfile <input.json> [output.json]"
        return 1
    fi

    local input=$1
    local output=${2:-$1}

    jq '.' "$input" > /tmp/json_temp && mv /tmp/json_temp "$output"
    echo "✅ Formatted: $output"
}

# Validate JSON file
jsonvalidate() {
    if [ -z "$1" ]; then
        echo "Usage: jsonvalidate <file.json>"
        return 1
    fi

    if jq empty "$1" 2>/dev/null; then
        echo "✅ Valid JSON: $1"
    else
        echo "❌ Invalid JSON: $1"
        return 1
    fi
}

# Minify JSON file
jsonminify() {
    if [ -z "$1" ]; then
        echo "Usage: jsonminify <input.json> [output.json]"
        return 1
    fi

    local input=$1
    local output=${2:-$1}

    jq -c '.' "$input" > /tmp/json_temp && mv /tmp/json_temp "$output"
    echo "✅ Minified: $output"
}
```

Usage:
```bash
jsonformat                          # Format clipboard
jsonfile data.json                  # Format in place
jsonfile input.json output.json     # Format to new file
jsonvalidate data.json              # Validate JSON
jsonminify input.json output.min.json  # Minify
```

---

## Programming Examples

### JavaScript/Node.js

```javascript
// Beautify JSON
const data = { name: "John", age: 30, city: "NYC" };
const beautified = JSON.stringify(data, null, 2);
console.log(beautified);
/*
{
  "name": "John",
  "age": 30,
  "city": "NYC"
}
*/

// Minify JSON
const minified = JSON.stringify(data);
console.log(minified);
// {"name":"John","age":30,"city":"NYC"}

// Sort keys alphabetically
const sorted = JSON.stringify(data, Object.keys(data).sort(), 2);

// Parse and format JSON string
const jsonString = '{"name":"John","age":30}';
const parsed = JSON.parse(jsonString);
const formatted = JSON.stringify(parsed, null, 2);
```

### Python

```python
import json

# Beautify JSON
data = {"name": "John", "age": 30, "city": "NYC"}
beautified = json.dumps(data, indent=2)
print(beautified)
"""
{
  "name": "John",
  "age": 30,
  "city": "NYC"
}
"""

# Minify JSON
minified = json.dumps(data, separators=(',', ':'))
print(minified)
# {"name":"John","age":30,"city":"NYC"}

# Sort keys
sorted_json = json.dumps(data, indent=2, sort_keys=True)

# Parse and format JSON file
with open('input.json', 'r') as f:
    data = json.load(f)

with open('output.json', 'w') as f:
    json.dump(data, f, indent=2, sort_keys=True)
```

### Go

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
)

func main() {
    // Sample JSON
    jsonStr := `{"name":"John","age":30,"city":"NYC"}`

    // Beautify
    var prettyJSON bytes.Buffer
    err := json.Indent(&prettyJSON, []byte(jsonStr), "", "  ")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("Beautified:")
    fmt.Println(prettyJSON.String())

    // Minify
    var data interface{}
    json.Unmarshal([]byte(jsonStr), &data)
    minified, _ := json.Marshal(data)
    fmt.Println("\nMinified:")
    fmt.Println(string(minified))

    // Marshal with custom indent
    type Person struct {
        Name string `json:"name"`
        Age  int    `json:"age"`
        City string `json:"city"`
    }

    person := Person{Name: "John", Age: 30, City: "NYC"}
    formatted, _ := json.MarshalIndent(person, "", "    ")
    fmt.Println("\nFormatted struct:")
    fmt.Println(string(formatted))
}
```

### PHP

```php
<?php
// Beautify JSON
$data = [
    "name" => "John",
    "age" => 30,
    "city" => "NYC"
];

$beautified = json_encode($data, JSON_PRETTY_PRINT);
echo $beautified;
/*
{
    "name": "John",
    "age": 30,
    "city": "NYC"
}
*/

// Minify JSON
$minified = json_encode($data);
echo $minified;
// {"name":"John","age":30,"city":"NYC"}

// Sort keys
ksort($data);
$sorted = json_encode($data, JSON_PRETTY_PRINT);

// Parse and format JSON file
$jsonString = file_get_contents('input.json');
$data = json_decode($jsonString, true);
file_put_contents('output.json', json_encode($data, JSON_PRETTY_PRINT));
?>
```

---

## Advanced Use Cases

### 1. Validate JSON Syntax

```bash
# Using jq
if jq empty input.json 2>/dev/null; then
    echo "✅ Valid JSON"
else
    echo "❌ Invalid JSON"
fi

# Using Python
python -c "import json,sys; json.load(open('input.json'))" && echo "✅ Valid" || echo "❌ Invalid"

# Using node
node -e "try { require('./input.json'); console.log('✅ Valid'); } catch(e) { console.log('❌ Invalid'); }"
```

**Online**: [JSON Formatter](https://orbit2x.com/json-formatter) shows detailed error messages

### 2. Extract Specific Fields

```bash
# Extract all names from array of users
jq '.users[].name' data.json

# Extract nested fields
jq '.company.employees[].email' data.json

# Multiple fields
jq '.users[] | {name: .name, age: .age}' data.json
```

### 3. Convert JSON to CSV

```bash
# Using jq
jq -r '.[] | [.name, .age, .city] | @csv' users.json > output.csv

# Using online tool
# 1. Format JSON: https://orbit2x.com/json-formatter
# 2. Convert to CSV: https://orbit2x.com/converter
```

### 4. Compare Two JSON Files

```bash
# Using diff
diff <(jq -S '.' file1.json) <(jq -S '.' file2.json)

# Using node
node -e "
const f1 = require('./file1.json');
const f2 = require('./file2.json');
console.log(JSON.stringify(f1) === JSON.stringify(f2) ? '✅ Same' : '❌ Different');
"
```

### 5. Merge Multiple JSON Files

```bash
# Using jq
jq -s '.[0] * .[1]' file1.json file2.json > merged.json

# Merge array of objects
jq -s 'add' *.json > combined.json
```

### 6. Remove Comments from JSON5

```javascript
// JSON5 allows comments - need to strip for standard JSON
const JSON5 = require('json5');
const fs = require('fs');

const json5Content = fs.readFileSync('config.json5', 'utf8');
const parsed = JSON5.parse(json5Content);
const standardJSON = JSON.stringify(parsed, null, 2);
fs.writeFileSync('config.json', standardJSON);
```

---

## Common JSON Errors & Fixes

### Error 1: Trailing Comma

```json
// ❌ Invalid
{
  "name": "John",
  "age": 30,  // <-- Trailing comma
}

// ✅ Valid
{
  "name": "John",
  "age": 30
}
```

### Error 2: Single Quotes

```json
// ❌ Invalid
{
  'name': 'John'  // <-- Single quotes
}

// ✅ Valid
{
  "name": "John"
}
```

### Error 3: Unquoted Keys

```json
// ❌ Invalid
{
  name: "John"  // <-- Unquoted key
}

// ✅ Valid
{
  "name": "John"
}
```

### Error 4: Comments

```json
// ❌ Invalid
{
  // This is a comment
  "name": "John"
}

// ✅ Valid
{
  "name": "John"
}
```

### Error 5: Undefined/Function Values

```javascript
// ❌ Invalid JSON
const obj = {
  name: "John",
  greet: function() { return "Hello"; },  // Functions not allowed
  data: undefined  // undefined not allowed
};

// ✅ Valid JSON
const obj = {
  name: "John",
  greet: "Hello",
  data: null  // Use null instead of undefined
};
```

> **Detailed error messages**: Use [JSON Formatter](https://orbit2x.com/json-formatter) for line-by-line error detection

---

## JSON Best Practices

### 1. Use Consistent Indentation
```json
// ✅ 2-space indent (recommended for web)
{
  "user": {
    "name": "John"
  }
}

// ✅ 4-space indent (recommended for code)
{
    "user": {
        "name": "John"
    }
}
```

### 2. Sort Keys Alphabetically
```json
// ✅ Easier to find keys
{
  "age": 30,
  "city": "NYC",
  "name": "John"
}
```

### 3. Use Meaningful Key Names
```json
// ❌ Bad
{
  "a": "John",
  "b": 30
}

// ✅ Good
{
  "name": "John",
  "age": 30
}
```

### 4. Keep Arrays Homogeneous
```json
// ❌ Mixed types
{
  "data": [1, "two", { "three": 3 }]
}

// ✅ Consistent types
{
  "numbers": [1, 2, 3],
  "strings": ["one", "two", "three"],
  "objects": [{ "id": 1 }, { "id": 2 }]
}
```

### 5. Minify for Production, Beautify for Development
```bash
# Development
jq '.' api-response.json > dev.json

# Production
jq -c '.' api-response.json > prod.json
```

---

## Performance Tips

### Minification Savings

| Original Size | Minified Size | Savings |
|--------------|---------------|---------|
| 100 KB | 75 KB | 25% |
| 1 MB | 700 KB | 30% |
| 10 MB | 7 MB | 30% |

**Rule of thumb**: Minification saves **25-35% file size**

### When to Minify
- ✅ API responses in production
- ✅ Configuration files in Docker images
- ✅ Static JSON served over CDN
- ❌ Development files (keep readable)
- ❌ JSON stored in databases (DB compresses)

### gzip Compression
```bash
# Minify + gzip for maximum savings
jq -c '.' large.json | gzip > compressed.json.gz

# Result: 60-80% smaller than original
```

---

## Tools & Resources

### Online Tools (No Installation)
- **[JSON Formatter](https://orbit2x.com/json-formatter)** - Professional JSON editor with Dracula theme
- **[JSON/YAML Formatter](https://orbit2x.com/formatter)** - Format both JSON and YAML
- **[Text Diff Tool](https://orbit2x.com/diff)** - Compare JSON files side-by-side
- **[File Format Converter](https://orbit2x.com/converter)** - Convert JSON to CSV, XML, YAML

### CLI Tools
- **[jq](https://stedolan.github.io/jq/)** - Swiss Army knife for JSON
- **[json](https://www.npmjs.com/package/json)** - Node.js JSON tool
- **[fx](https://github.com/antonmedv/fx)** - Interactive JSON viewer

### Libraries
- **JavaScript**: Native `JSON.stringify()` and `JSON.parse()`
- **Python**: Built-in `json` module
- **Go**: `encoding/json` package
- **PHP**: `json_encode()` and `json_decode()`
- **Rust**: [serde_json](https://crates.io/crates/serde_json)
- **Java**: [Jackson](https://github.com/FasterXML/jackson)

### References
- **[JSON Specification (RFC 8259)](https://datatracker.ietf.org/doc/html/rfc8259)**
- **[JSON Schema](https://json-schema.org/)** - Validate JSON structure
- **[JSON Lines](https://jsonlines.org/)** - Newline-delimited JSON format

---

## FAQ

### Q: What's the difference between JSON and JSON5?
**A**: JSON5 allows comments, trailing commas, unquoted keys. Standard JSON does not. Use [JSON Formatter](https://orbit2x.com/json-formatter) to convert JSON5 → JSON.

### Q: How do I format JSON in VSCode?
**A**:
1. Select JSON text
2. Press `Shift+Alt+F` (Windows/Linux) or `Shift+Option+F` (macOS)
3. Or use Command Palette → "Format Document"

### Q: Can I format JSON from API responses?
**A**: Yes!
```bash
# cURL + jq
curl https://api.example.com/users | jq '.'

# Or use online: https://orbit2x.com/json-formatter
```

### Q: What's the maximum JSON file size I can format?
**A**:
- **CLI tools**: Unlimited (limited by RAM)
- **Online tools**: Most support up to 10MB
- **[Orbit2x JSON Formatter](https://orbit2x.com/json-formatter)**: Handles 50MB+ files (client-side processing)

### Q: How do I fix "Unexpected token" errors?
**A**: Use [JSON Formatter](https://orbit2x.com/json-formatter) - it shows exact line/column of errors with suggestions.

---

## Related Tools

### JSON Tools
- **[JSON Formatter (Advanced)](https://orbit2x.com/json-formatter)** - Professional editor with syntax highlighting
- **[JSON/YAML Formatter](https://orbit2x.com/formatter)** - Format and validate JSON/YAML
- **[CSV to SQL Converter](https://orbit2x.com/csv-to-sql)** - Convert CSV to SQL INSERT statements
- **[XML Formatter](https://orbit2x.com/xml-formatter)** - Format and validate XML

### Developer Tools
- **[Regex Tester](https://orbit2x.com/regex-tester)** - Test regular expressions
- **[Base64 Encoder](https://orbit2x.com/encoder)** - Encode/decode Base64
- **[Text Diff Tool](https://orbit2x.com/diff)** - Compare text files
- **[All 130+ Tools](https://orbit2x.com/tools)** - Complete developer toolkit

---

## Contributing

Found a better way to format JSON? Have a useful script? Open a PR!

## License

MIT License - Free to use in your projects

---

**Made with ❤️ by [Orbit2x](https://orbit2x.com) - Free Developer Tools**

**Try it now**: [JSON Formatter](https://orbit2x.com/json-formatter) • [All Tools](https://orbit2x.com/tools)
