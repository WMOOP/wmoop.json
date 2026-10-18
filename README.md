<p align="center">
  <img alt="wmoop.json" src="brand/wmoop.json.svg" width="340">
</p>

# wmoop.json

**The canonical machine contract for WML** — the *WMOOP Modeling Language*. This repo is the
single source of truth for the **grammar** of WML, expressed as two interchangeable faces of one
schema:

| File | What it is |
|------|------------|
| [`wml.schema.json`](wml.schema.json) | **JSON Schema 2020-12** meta-schema for a WML module file (the YAML/JSON form). |
| [`wml.xsd`](wml.xsd) | The equivalent **W3C XML Schema** for the WML-in-XML serialization. |

Both encode the same rules — identifier shapes (QNames), concept/role/datatype/enumeration/
interface structure, cardinalities, value constraints, and required provenance. Use whichever
matches your toolchain.

> This is part of **WMOOP — World Map Object-Oriented Programming**: one shared, language-neutral,
> object-oriented model of the real world, from which idiomatic code is generated in many languages.

## Where everything else lives

This repo holds **only the schema** (the machine contract). The rest of the system lives in
[`wmoop.doc`](https://github.com/WMOOP/wmoop.doc):

- **The model** (`model/**`, the WML source of truth) and **the language specification**
  (`docs/03-architecture.md`, and `schema/README.md` — the full construct-by-construct reference).
- **The validator** (`schema/validate_wml.py`) — grammar gate (against the schema here) **plus** the
  cross-file semantic gates (inheritance depth, LSP-safe overrides, roles-not-classes, acyclic
  dependencies, referential integrity) that a context-free grammar cannot express.
- **The YAML→XML projector** and the **worked XML examples** (`schema/examples/*.wml.xml`) that
  exercise `wml.xsd`.

`wmoop.doc` keeps line-for-line **mirrors** of the two files here and a freshness check
(`schema/check_canonical.py`) that fails CI if they drift from this repo. **Edit the schema here**,
then re-sync `wmoop.doc`.

## Usage

Validate WML sources / documents against the schema with any standard tool:

```bash
# JSON side — validate the YAML model files against the meta-schema
pipx run check-jsonschema --schemafile wml.schema.json path/to/*.wml.yaml

# XML side — validate a WML-in-XML document against the XSD
xmllint --schema wml.xsd path/to/module.wml.xml --noout
```

For the full pipeline (grammar + semantic gates + XML projection), use the validator in `wmoop.doc`.

## Versioning & status

**v0 (experimental)**, in lockstep with the rest of WMOOP — expect breaking changes. The JSON
Schema `$id` is `https://wmoop.org/schema/wml.schema.json`; the XSD target namespace is
`https://wmoop.org/wml/1`. Breaking grammar changes are classified MAJOR/MINOR/PATCH as in
`wmoop.doc/docs/03-architecture.md` §4.3.

## License

Apache-2.0 — see [`LICENSE`](LICENSE). (The schema is a code artifact; the WMOOP *model data and
documentation* in `wmoop.doc` are CC-BY-4.0.)
