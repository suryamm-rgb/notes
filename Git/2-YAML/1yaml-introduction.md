# YAML - Complete Introduction

## Introduction

- YAML evolved as a replacement for XML and JSON because both XML and JSON are more verbose in nature. YAML is designed to be simpler, cleaner, and more human-readable, making it an excellent choice for configuration files and data serialization.

- extenstion is .yml

## Topics Covered

- Introduction to YAML
- What is YAML?
- Why YAML?
- YAML Use Cases
- XML vs JSON vs YAML
- Basic YAML Syntax
- Scalars
- Strings
- Sequences (Lists)
- Dictionaries (Mappings)
- Comments
- Dates and Timestamps
- Tags
- YAML Basic Concepts
- YAML Advanced Concepts

## What is YAML?

YAML (YAML Ain't Markup Language) is a human-readable data serialization standard that can be used with almost every programming language.

It is commonly used to write configuration files because it is simple, clean, and easy to understand.

### Features

- Human-readable and hierarchical
- Flexible data types
- Cross-platform
- Easy to maintain
- Less verbose than XML and JSON
- Supports comments
- Supports complex data structures

## Common Uses of YAML

- Configuration Management (Ansible, Kubernetes, Docker Compose)
- Data Storage
- Data Serialization

## XML vs JSON vs YAML

| Feature             | XML  | JSON      | YAML        |
| ------------------- | ---- | --------- | ----------- |
| Human Readable      | ❌   | ✅        | ✅✅        |
| Easy to Write       | ❌   | ✅        | ✅✅        |
| Supports Comments   | ✅   | ❌        | ✅          |
| Verbose             | High | Medium    | Low         |
| Configuration Files | Rare | Sometimes | Very Common |

### XML

```xml
<person>
  <name>John</name>
  <age>25</age>
</person>
```

### JSON

```json
{
  "name": "John",
  "age": 25
}
```

### YAML

```yaml
name: John
age: 25
```

## YAML Basic Concepts

```yaml
person:
  name: John
  age: 25
```

### Scalars

```yaml
name: John
age: 25
salary: 65000
active: true
```

### Strings

```yaml
message: |
  Multi-line
  string

description: >
  Folded
  string
```

### Sequences

```yaml
languages:
  - Java
  - Python
  - Go
```

### Dictionaries

```yaml
employee:
  id: 101
  name: Surya
```

### Comments

```yaml
# This is a comment
name: John
```

### Dates

```yaml
joiningDate: 2025-06-15
createdAt: 2025-06-15T10:30:45Z
```

### Tags

```yaml
age: !!int 25
price: !!float 99.5
```

## Advanced Concepts

### Anchors & Aliases

```yaml
defaults: &defaults
  timeout: 30
  retries: 5

production:
  <<: *defaults
  timeout: 60
```

### Multiple Documents

```yaml
---
name: Dev
---
name: Production
```

## Popular Tools Using YAML

- Docker Compose
- Kubernetes
- GitHub Actions
- Prometheus
- Spring Boot
- AWS CloudFormation
- Azure Pipelines
- CircleCI
- Jenkins
- Chef
- Red Hat OpenShift

## Summary

YAML is simple, readable, and the standard configuration language for modern DevOps and cloud-native applications.
