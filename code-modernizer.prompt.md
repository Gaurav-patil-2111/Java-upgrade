---
description: 'Modernize Java code to use latest language features — Records, var, text blocks, pattern matching, switch expressions, virtual threads'
mode: agent
---

# Code Modernizer

Modernize this Java codebase to use modern Java language features.

## Step 1: Detect Java Version

Read the build file to determine which Java version is configured. This determines which features are available:

| Feature | Minimum Java |
|---|---|
| `var` (local variable type inference) | 10 |
| Text blocks `"""` | 15 |
| Records | 16 |
| Sealed classes | 17 |
| Pattern matching for instanceof | 16 |
| Switch expressions | 14 |
| Pattern matching for switch | 21 |
| Virtual threads | 21 |
| Sequenced collections | 21 |
| Record patterns | 21 |
| Unnamed variables `_` | 22 |

## Step 2: Scan for Modernization Opportunities

Search the codebase for these patterns:

### Records (Java 16+)
- Look for classes that are pure data carriers (DTOs, value objects)
- Have: only fields, constructor, getters, equals, hashCode, toString
- Especially classes using Lombok `@Data` or `@Value` on immutable types
- **Convert to**: `record ClassName(Type field1, Type field2) {}`

### var (Java 10+)
- Look for verbose local variable declarations where the type is obvious
- `Map<String, List<Integer>> map = new HashMap<String, List<Integer>>()`
- **Convert to**: `var map = new HashMap<String, List<Integer>>()`
- ⚠️ Only use when type is obvious from right-hand side. Don't obfuscate.

### Text Blocks (Java 15+)
- Look for multi-line string concatenation, especially JSON, SQL, HTML
- `"SELECT *\n" + "FROM users\n" + "WHERE id = ?"`
- **Convert to**: Text block with `"""`

### Pattern Matching for instanceof (Java 16+)
- Look for: `if (obj instanceof Foo) { Foo foo = (Foo) obj; ... }`
- **Convert to**: `if (obj instanceof Foo foo) { ... }`

### Switch Expressions (Java 14+)
- Look for switch statements that assign to a variable
- **Convert to**: `var result = switch(x) { case A -> ...; };`

### Stream API Improvements
- Look for traditional for-loops doing filter/map/collect patterns
- **Convert to**: Stream operations where it improves readability
- ⚠️ Don't force streams where a simple loop is clearer

### java.time (Java 8+, but still often missed)
- Look for `java.util.Date`, `Calendar`, `SimpleDateFormat`
- **Convert to**: `LocalDateTime`, `ZonedDateTime`, `DateTimeFormatter`

### Optional (Java 8+)
- Look for null-check chains: `if (x != null && x.getY() != null)`
- **Convert to**: `Optional.ofNullable(x).map(X::getY).orElse(default)`
- ⚠️ Only where it improves clarity. Don't wrap everything in Optional.

## Step 3: Present Findings

```
CODE MODERNIZATION OPPORTUNITIES
═════════════════════════════════

Available (Java [version] detected):

📦 Records (X candidates found)
  - [ClassName] — [file path] — pure data class, [N] fields
  - [ClassName] — [file path] — Lombok @Data, immutable

📝 Text Blocks (X candidates found)
  - [file:line] — multi-line SQL string
  - [file:line] — multi-line JSON

🔍 Pattern Matching instanceof (X candidates found)
  - [file:line] — instanceof + cast

🔀 Switch Expressions (X candidates found)
  - [file:line] — switch assigning to variable

📅 java.time Migration (X candidates found)
  - [file:line] — uses java.util.Date
  - [file:line] — uses SimpleDateFormat

Would you like me to apply all, or select specific categories?
```

## Step 4: Execute

- Apply one category at a time
- Build and test after each category
- Show before/after for each transformation
- Skip any transformation the user declines

## Rules

- **Don't modernize test code** unless the user asks — focus on main source first
- **Don't change public API signatures** unless explicitly asked
- **Don't force patterns** where the old code is clearer
- **Preserve all comments and documentation**
- Records: only convert if the class is truly a data carrier (no business logic in methods)
