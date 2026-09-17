# OSCAL policy catalog

`web3-cryptographic-key-management-catalog.json` is the machine-managed representation of the Web3 Cryptographic Key Management Policy. It uses the OSCAL Catalog model and is the file to update for controls-as-code workflows. The Markdown policy remains the conversion source and human-readable draft.

## Model

- OSCAL release: `1.2.3`
- Model: Catalog
- Policy sections: OSCAL groups
- Policy subsections: nested OSCAL groups
- Numbered policy clauses: OSCAL controls
- List subclauses: nested OSCAL controls
- Requirement text: `statement` parts
- Original clause numbers: `label` properties
- Clause-level NIST provenance: `source-classification` properties

A `source-classification` value of `pending-control-mapping` must be replaced only after the clause is mapped to a specific source and revision. Section 17 controls are marked `web3-specific` and must not be represented as direct NIST requirements without a separate, cited mapping decision.

## Validate

Run schema validation against the pinned OSCAL release:

```sh
curl -fsSL https://github.com/usnistgov/OSCAL/releases/download/v1.2.3/oscal_catalog_schema.json -o /tmp/oscal_catalog_schema-1.2.3.json
npx --yes --package=ajv-cli@5.0.0 --package=ajv-formats@3.0.1 ajv validate \
  --spec=draft7 \
  --strict=false \
  -c ajv-formats \
  -s /tmp/oscal_catalog_schema-1.2.3.json \
  -d oscal/web3-cryptographic-key-management-catalog.json
```

Validation must pass before merging catalog changes. Control IDs and `label` properties are stable identifiers and must not be renumbered without an approved policy revision and migration plan for downstream references.
