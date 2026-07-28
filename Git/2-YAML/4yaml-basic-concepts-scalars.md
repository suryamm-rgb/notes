# YAML Basic Concepts

## YAML Data Types

### Basic (Scalars)
- String
- Number
- Boolean
- Date and Time
- Null
- Binary

### Advanced (Collections)
- Sequence (List)
- Set
- Mapping (Key-Value pairs)

## Scalars in YAML

Scalars represent a single, indivisible value. They are the most basic data type in YAML and include strings, numbers, booleans, null values, dates, timestamps, and binary data.

Scalars do not contain nested structures such as lists or dictionaries.

Syntax:
```yaml
<key>: <value>
```

Example:
```yaml
name: Surya
age: 35
is_student: false
```

A space after the colon is mandatory.

Correct:
```yaml
name: Surya
```

Incorrect:
```yaml
name:Surya
```
