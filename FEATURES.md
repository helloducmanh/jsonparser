# JSON Parser - Feature List

## Core Features

### 1. Lexer (Tokenizer)
- [ ] Tokenize JSON input into token stream
- [ ] Support all JSON token types:
  - Structural: `{`, `}`, `[`, `]`, `:`, `,`
  - Literals: `true`, `false`, `null`
  - String: `"..."` with escape sequences
  - Number: integers, floats, scientific notation, negative
- [ ] Handle string escape sequences:
  - `\"`, `\\`, `\/`, `\b`, `\f`, `\n`, `\r`, `\t`
  - Unicode escapes: `\uXXXX` (4-digit hex)
- [ ] Track line and column numbers for error reporting
- [ ] Skip whitespace (space, tab, newline, carriage return)
- [ ] Detect invalid characters and report position

### 2. Parser
- [ ] Recursive descent parser
- [ ] Parse JSON grammar:
  - `value` → object | array | string | number | true | false | null
  - `object` → `{` (string `:` value)* `}`
  - `array` → `[` value* `]`
- [ ] Build Abstract Syntax Tree (AST)
- [ ] Handle nested structures (objects in arrays, arrays in objects)
- [ ] Validate JSON structure (matching braces, correct syntax)

### 3. AST (Abstract Syntax Tree)
- [ ] Node types:
  - `JsonObjectNode` - key-value pairs
  - `JsonArrayNode` - ordered list of values
  - `JsonStringNode` - string value
  - `JsonNumberNode` - numeric value (int/double)
  - `JsonBooleanNode` - true/false
  - `JsonNullNode` - null value
- [ ] Each node stores position (line, column) for error reporting
- [ ] AST visitor pattern support (for future extensions)

### 4. Value Representation
- [ ] Convert AST to usable C# objects:
  - `JsonObject` → `Dictionary<string, JsonValue>`
  - `JsonArray` → `List<JsonValue>`
  - `JsonString` → `string`
  - `JsonNumber` → `double` or `long` (auto-detect)
  - `JsonBoolean` → `bool`
  - `JsonNull` → `null`
- [ ] Type-safe access methods:
  - `GetValue<T>(string key)` - get typed value from object
  - `GetArray<T>(string key)` - get array with typed elements
  - `GetObject(string key)` - get nested object
- [ ] Indexer support: `json["key"]`, `json[0]`

### 5. Error Handling
- [ ] Detailed error messages with:
  - Error type (syntax error, unexpected token, etc.)
  - Position (line, column)
  - Expected vs actual token
- [ ] Error recovery strategies:
  - Stop on first error (strict mode)
  - Continue and collect multiple errors (lenient mode)
- [ ] Custom exception types:
  - `JsonParseException` - base exception
  - `JsonLexerException` - tokenization errors
  - `JsonValidationException` - structure errors

### 6. Serialization (JSON Writer)
- [ ] Convert C# objects back to JSON string
- [ ] Pretty print with indentation (configurable spaces/tabs)
- [ ] Compact output (no whitespace)
- [ ] Escape special characters in strings
- [ ] Handle circular references (detect and throw)
- [ ] Custom serialization options:
  - Sort object keys alphabetically
  - Skip null values
  - Date/time format
  - Number precision

### 7. API Surface
- [ ] Static methods for convenience:
  - `JsonParser.Parse(string json)` → `JsonValue`
  - `JsonParser.ParseFile(string path)` → `JsonValue`
  - `JsonParser.TryParse(string json, out JsonValue result)` → `bool`
- [ ] Instance methods for advanced usage:
  - `new JsonParser(options)` with configuration
  - `Parse(TextReader reader)` for streaming
  - `Parse(Stream stream)` for binary input
- [ ] Extension methods:
  - `string.ToJson()` → parse string
  - `object.SerializeToJson()` → convert to JSON

### 8. Performance Features
- [ ] Streaming parser (SAX-style) for large files:
  - Event-based: `OnObjectStart`, `OnObjectEnd`, `OnArrayStart`, etc.
  - Low memory footprint
  - Process data as it's parsed
- [ ] Memory pooling:
  - Reuse buffers for string parsing
  - Object pooling for AST nodes
- [ ] Span<T> and Memory<T> for zero-allocation parsing where possible
- [ ] Benchmark suite comparing with System.Text.Json

### 9. Validation
- [ ] JSON Schema validation (optional feature):
  - Type checking
  - Required properties
  - String length/pattern
  - Number min/max
  - Array min/max items
- [ ] Custom validators (extensible)
- [ ] Validation result with all errors

### 10. JSON Path Support
- [ ] Query JSON with path expressions:
  - `$.store.book[0].title`
  - `$..book[?(@.price < 10)]` (filter)
  - `$.*` (wildcard)
- [ ] Return matching values or paths

### 11. JSON Pointer (RFC 6901)
- [ ] Navigate JSON structure with pointer:
  - `/store/book/0/title`
  - Escape sequences: `~0` for `~`, `~1` for `/`
- [ ] Get, set, add, remove operations

### 12. JSON Patch (RFC 6902)
- [ ] Apply patches to JSON documents:
  - `add`, `remove`, `replace`, `move`, `copy`, `test`
- [ ] Generate diff between two JSON documents

### 13. Testing
- [ ] Unit tests for each component:
  - Lexer: all token types, edge cases
  - Parser: all grammar rules, nested structures
  - Values: type conversions, accessors
- [ ] Integration tests:
  - Round-trip (parse → serialize → parse)
  - Large files (1MB+)
  - Invalid JSON (error cases)
- [ ] Edge cases:
  - Empty objects/arrays
  - Unicode strings
  - Very deep nesting (stack overflow prevention)
  - Very long strings/numbers
- [ ] Benchmark tests:
  - Parse speed vs System.Text.Json vs Newtonsoft.Json
  - Memory allocation comparison
- [ ] JSON compliance tests (JSONTestSuite)

### 14. Documentation
- [ ] XML documentation comments on all public APIs
- [ ] README.md with:
  - Overview and features
  - Installation instructions
  - Quick start examples
  - API reference
  - Performance benchmarks
  - Comparison with other libraries
- [ ] Architecture diagram
- [ ] Contributing guide

### 15. Build & DevOps
- [ ] Multi-target framework:
  - .NET 6.0, 7.0, 8.0, 9.0, 10.0
  - .NET Standard 2.0, 2.1
- [ ] NuGet package configuration
- [ ] CI/CD pipeline (GitHub Actions):
  - Build on multiple OS (Windows, Linux, macOS)
  - Run tests
  - Code coverage report
  - Publish NuGet package on release
- [ ] Code analysis:
  - Enable nullable reference types
  - Enable all analyzer rules
  - Treat warnings as errors
- [ ] BenchmarkDotNet integration
- [ ] Docker support for testing

## Stretch Goals (Advanced)

### 16. Advanced Features
- [ ] LINQ support for JSON queries
- [ ] Dynamic object support (dynamic keyword)
- [ ] Attributes for custom serialization:
  - `[JsonProperty("name")]`
  - `[JsonIgnore]`
  - `[JsonConverter(typeof(CustomConverter))]`
- [ ] Custom converters (extensible)
- [ ] Immutable JSON objects (thread-safe)
- [ ] Async parsing for large files

### 17. Interoperability
- [ ] Convert to/from:
  - `System.Text.Json.JsonDocument`
  - `Newtonsoft.Json.Linq.JToken`
  - `XmlDocument`
  - `DataTable`
- [ ] Support for BSON, MessagePack (optional)

### 18. Tooling
- [ ] JSON formatter CLI tool
- [ ] JSON validator CLI tool
- [ ] JSON diff tool
- [ ] Visual Studio extension (syntax highlighting, validation)

## Implementation Priority

### Phase 1: Core (Week 1-2)
1. Lexer with all token types
2. Parser with recursive descent
3. AST nodes
4. Basic value representation
5. Error handling with position
6. Unit tests for core

### Phase 2: Serialization (Week 3)
1. JSON writer (pretty + compact)
2. String escaping
3. Round-trip tests

### Phase 3: API Polish (Week 4)
1. Public API surface
2. Extension methods
3. Documentation
4. Integration tests

### Phase 4: Performance (Week 5)
1. Benchmark suite
2. Memory optimization
3. Streaming parser (SAX-style)

### Phase 5: Advanced (Week 6+)
1. JSON Path
2. JSON Pointer
3. JSON Patch
4. Schema validation
5. NuGet packaging

## Success Criteria

- [ ] Parse all valid JSON (100% compliance with RFC 8259)
- [ ] Reject all invalid JSON with clear error messages
- [ ] Performance within 2x of System.Text.Json
- [ ] Zero memory leaks (verified with profiler)
- [ ] 100% code coverage on critical paths
- [ ] Works on all target frameworks
- [ ] Passes JSONTestSuite

## Notes

- This is a learning project focused on understanding compiler fundamentals
- Clean code and architecture are more important than performance initially
- Each feature should be implemented incrementally with tests
- Document design decisions in code comments
- Follow C# coding conventions and best practices
