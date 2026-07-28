# XML vs JSON vs YAML

## Evolution of XML, JSON, and YAML

XML was introduced for storing and transporting structured data in enterprise applications. Although powerful, XML is verbose and requires opening and closing tags.

JSON (JavaScript Object Notation) was introduced as a lightweight alternative to XML. It became the standard format for exchanging data between web applications and APIs because of its simplicity and compact structure.

YAML (YAML Ain't Markup Language) was designed to make configuration files easier for humans to read and write. It extends JSON with cleaner syntax and additional features while remaining compatible with JSON.

## YAML is a Superset of JSON

YAML is a **superset of JSON**, meaning every valid JSON document is also valid YAML.

A useful analogy is:

> **YAML is to JSON what TypeScript is to JavaScript.**

Just as TypeScript builds upon JavaScript by adding additional features while maintaining compatibility, YAML extends JSON with:

- Cleaner syntax
- Comments
- Multi-line strings
- Anchors and aliases
- Better readability

while remaining fully compatible with JSON.

---

## Representation Example

### XML

```xml
<person>
    <name>Surya</name>
</person>
```

### JSON

```json
{
  "person": {
    "name": "Surya"
  }
}
```

### YAML

```yaml
person:
  name: Surya
```

---

## XML vs JSON vs YAML

| Feature | XML | JSON | YAML |
|---------|------|------|------|
| Human Readability | Hard | Moderate | Very Easy |
| Syntax | Opening & Closing Tags | Braces {} and [] | Indentation |
| Comments | Yes | No | Yes |
| Hierarchy | Tags | Objects & Arrays | Spaces (Indentation) |
| Storage | Largest | Smaller | Smallest |
| Network Bandwidth | High | Medium | Low |
| Best Use Case | Enterprise Systems | APIs & HTTP | Configuration Files |

### XML

- Harder to read
- More verbose
- Allows comments
- Hierarchy is represented using opening and closing tags
- Requires more storage and network bandwidth
- Best suited for complex enterprise projects requiring strict schemas

### JSON

- Moderate readability
- Explicit and strict syntax
- Comments are not allowed
- Hierarchy is represented using braces and arrays
- Lighter than XML
- Preferred for web development and transmitting data over HTTP

### YAML

- Easier to read and understand
- Minimalist syntax
- Allows comments
- Hierarchy is represented using indentation (spaces)
- Lighter than XML and generally more concise for configuration
- Best suited for configuration files while supporting all JSON features

## Summary

YAML is widely used by modern DevOps tools such as Docker Compose, Kubernetes, GitHub Actions, GitLab CI/CD, Ansible, Azure Pipelines, Jenkins, and Prometheus because it is clean, readable, and easy to maintain.
