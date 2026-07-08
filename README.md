# JsonParser

A high-performance, standards-compliant JSON parser for .NET.

## Features

- **RFC 8259 Compliant**: Full support for JSON specification
- **High Performance**: Optimized lexer and parser with minimal allocations
- **Streaming Support**: SAX-style event-driven parsing for large files
- **Error Recovery**: Detailed error messages with line/column information
- **Zero Dependencies**: Pure .NET implementation
- **Comprehensive Testing**: 100% coverage on critical paths

## Installation

```bash
dotnet add package JsonParser
```

## Quick Start

```csharp
using JsonParser;

// Parse JSON string
var json = @"{""name"": ""John"", ""age"": 30}";
var value = JsonParser.Parse(json);

// Access values
var name = value["name"].AsString();  // "John"
var age = value["age"].AsInt();       // 30

// Serialize back to JSON
var output = value.ToString();
```

## Documentation

See [FEATURES.md](FEATURES.md) for complete feature list and implementation roadmap.

## License

MIT
