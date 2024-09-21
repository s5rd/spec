# s5rd Specification Appendix

version 0.1.0 (2024-09-22)

## s5rd-text syntax without indents and literal details

```
s5rd
  documents

documents
  document
  "***" ws document documents
  "***"

document
  values

ws
  " "
  "\n"
  "\t"
  ","

values
  \varempty
  value ws values

value
  binary
  identifier
  number
  text
  array
  keyvalue

key
  binary
  identifier
  number
  double-quoted-identifier

array
  values (exclude \varempty)
  "[" ws values ws "]"
  "{" ws keyvalues ws "}"

keyvalues
  \varempty
  keyvalue ws keyvalues

keyvalue
  key ":" ws value
```

`text` in `key` is not treated as text. It is treated as identifier.
Key does not contain text. Only double-quoted identifiers are allowed.
