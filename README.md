# s5rd Specification

## What is s5rd?

s5rd (/ˈstændərd/, numeronym of the word "standard")
is lightweight recursive binary/text data format.
seeing as YAML, treating as JSON.

- **lightweight**:

  YAML is very big format, therefore buggy.

  s5rd is thin-designed and tiny format.

- **recursive**:

  Parser implementation is easy.

- **binary/text**:

  4-byte aligned recursive binary format.
  varies from JSON-like to YAML-like text format.

- extensible: We do not define what is identifier,
  another type of string differ than binary-string or text-string.
  You can use `true`, `false` and `null` as identifier.

- streaming-aware: Also includes NDJSON.

## Version

The version of this specification follows
[Semantic Versioning](https://semver.org/).

## CommonMark

This specification is marked by [CommonMark](https://commonmark.org/).
