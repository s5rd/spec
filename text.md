# s5rd-text Specification

version 0.1.0 (2024-09-22)

## 1. Overview

`s5rd` is a lightweight, recursive, and streaming-aware data exchange format designed to represent structured data in a compact and extensible manner. Its syntax is simple, making it ideal for streaming contexts and scenarios where flexible, nested structures are required. 

### Key Features:
- **Lightweight and Recursive Syntax**: Supports recursive structures such as arrays and key-value pairs.
- **Extensible**: Allows identifiers to remain undefined within the format, enabling future extensions.
- **Streaming Awareness**: Concatenating two values with whitespace creates a valid value, supporting continuous data streams.

## 2. Syntax Definition

### 2.1. Whitespace: `ws`

Whitespace in `s5rd` consists of spaces, newlines, tabs, and commas. Whitespace is optional and may appear between elements. It does not affect the meaning of the data.

#### Grammar:
```
ws
  " "
  "\n"
  "\t"
  ","
```

### 2.2. Top-level Structure: `s5rd`

The top-level structure of an `s5rd` document consists of one **documents**.

#### Grammar:
```
s5rd
  documents
```

### 2.3. Document Collection: `documents`

**documents** is a collection of **document**.
Each **document** is separated by the delimiter `***`.

#### Grammar:
```
documents
  document
  "***" ws document documents
  "***"
```

- **document**: An individual document that contains structured data.
- `***`: The document delimiter. Documents are separated by three consecutive asterisks (`***`).
- **ws**: Optional whitespace that can occur between documents.

### 2.4. Document: `document`

Each document consists of one **values**.

#### Grammar:
```
document
  values
```

- **values**: A sequence of one or more **value** elements.

### 2.5. Values: `values`

A **values** can contain an empty structure, single value, or multiple values separated by whitespace.

#### Grammar:
```
values
  \varempty
  value ws values
```

- **\varempty**: Represents an empty value, indicating no content.
- **value**: A single value, which can be of various types.
- **ws**: Optional whitespace separating values.

### 2.6. Value Types: `value`

A **value** in `s5rd` can be one of the following types: binary, identifier, number, text, array, or key-value.

#### Grammar:
```
value
  binary
  identifier
  number
  text
  array
  keyvalue
```

#### Value Descriptions:

- **binary**: Represents binary data, enclosed in single quotes (`'`).
- **identifier**: An undefined identifier that may optionally be enclosed in grave characters (`` ` ``).
- **number**: A numeric value, including only non-negative integers.
- **text**: A string of text, enclosed in double quotes (`"`).
- **array**: A collection of values, enclosed in square brackets (`[]`) or key-value pairs, enclosed in curly braces (`{}`).
- **keyvalue**: A key-value pair where a key is mapped to a value.

### 2.7. Key-Value Pairs: `keyvalue`

Key-value pairs allow structured data to be represented. A **key** is followed by a colon (`:`), and then by the associated **value**.

#### Grammar:
```
keyvalue
  key ":" ws value
```

- **key**: The identifier or value representing the key in the key-value pair.
- **":"**: The colon symbol separating the key and value.
- **value**: The value associated with the key.
- **ws**: Optional whitespace between the colon and the value.

### 2.8. Key Types: `key`

**key** in key-value pairs can be of various types: binary, identifier or number.

#### Grammar:
```
key
  binary
  identifier
  number
  double-quoted-identifier
```

### 2.9. Arrays: `array`

Arrays in `s5rd` can hold multiple **value**s or **keyvalue** pairs. Arrays may be enclosed in square brackets (`[]`) for values, and curly braces (`{}`) for key-value pairs.

#### Grammar:
```
array
  values (exclude \varempty)
  "[" ws values ws "]"
  "{" ws keyvalues ws "}"
```

- **[]**: A list of values.
- **{}**: A list of key-value pairs.
- **values**: A sequence of **value** elements.
- **keyvalues**: A sequence of **keyvalue** elements.

### 2.10. Key-Value Arrays: `keyvalues`

A key-value array is a collection of **keyvalue** pairs, each separated by optional whitespace.

#### Grammar:
```
keyvalues
  \varempty
  keyvalue ws keyvalues
```

- **keyvalue**: A single key-value pair.
- **ws**: Optional whitespace between key-value pairs.
- **\varempty**: Represents an empty key-value array.

## 3. Literal Definitions

### 3.1. Binary

**binary** data is enclosed in single quotes (`'`). Binary data can represent raw byte sequences.

#### Example:
```
'\NUL'
```

### 3.2. Identifier

**identifier** is a string, which contains only restricted characters and which may be optionally enclosed in grave characters (`` ` ``).
Numeric characters may be included but such **identifier** must be enclosed in grave characters.

#### Example:
```
name
`0y2k`
```

### 3.3. Number

**number** represents non-negative integers.

#### Example:
```
42
```

### 3.4. Text

**text** is a string enclosed in double quotes (`"`). It can contain any characters.

#### Example:
```
"hello world"
"sample text"
```

## 4. Streaming Awareness

In `s5rd`, concatenating two values with whitespace between them is valid. This property makes the format well-suited for streaming data, where values may be continuously appended or streamed in real-time.

#### Example:
```
"hello" "world"
```

This is equivalent to having two separate values `"hello"` and `"world"`.

## 5. Examples

### 5.1. Simple Document

```
***
user: "Alice"
age: 30
***
```
In this example, the two values `user: "Alice"` and `age: 30` are separated by whitespace (newline character) and still form a valid structure.

### 5.2. Nested Arrays and Key-Values

```
***
{
  user: {
    name: "Alice",
    age: 30,
    skills: ["coding", "writing"]
  }
}
***
```

## 6. Conclusion

`s5rd` is a flexible, extensible, and streaming-aware format for representing structured data.
Its lightweight syntax allows for a wide range of applications where recursive data structures and continuous streams are needed.
The extensibility provided by the undefined **identifier** ensures that the format can grow and adapt to new requirements over time.
