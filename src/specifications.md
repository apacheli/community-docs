# Specifications

- [Table Definitions](#table-definitions)
  - [Interfaces](#interfaces)
  - [Constants](#constants)
- [Endpoints](#endpoints)

---

## Table Definitions

Most definitions will be represented with a table. There are two types of tables
that you can expect to find within the documentation:

- **Interfaces** will be represented using three headers: _Name_, _Type_, and
  _Description_.
- **Constants** will be represented using three headers: _Name_, _Value_ and
  _Description_.

### Interfaces

Fields listed under _Name_ and _Type_ will be wrapped in `code`. A field that is
partial will be denoted with _?_ after their name. A field that is nullable will
be denoted with a _?_ after their type.

| Name                | Type      | Description                           |
| ------------------- | --------- | ------------------------------------- |
| `field`             | `string`  | A field.                              |
| `partial`?          | `string`  | This field may or may not be present. |
| `nullable`          | `string`? | This field may be _null_.             |
| `partial_nullable`? | `string`? | Both logics apply to this field.      |

### Constants

Fields listed under _Name_ will always be written in uppercase letters.

## Endpoints
