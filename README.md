# KMS Policy

Web3 Cryptographic Key Management Policy: a NIST-aligned governance document, its OSCAL machine-readable representation, and the build tooling used to produce a formatted PDF.

## Contents

```
drafts/     Human-readable Markdown policy (source of truth for prose)
oscal/      OSCAL Catalog representation of the policy (controls-as-code)
source/     Reference NIST SP source documents (Markdown + PDF)
output/     Rendered PDF build output
build_governance_pdf.py   Script that renders the policy to a styled PDF
```

### `drafts/`

- `01-key-management-policy.md` — initial draft of the policy.
- `02-key-management-policy-reindexed.md` — current version, with every requirement numbered per clause (e.g. `1.1`, `1.2`) for unambiguous cross-referencing and OSCAL mapping.

The policy has 18 sections:

1. Purpose
2. Scope
3. Definitions
4. Governance and Control Architecture
5. Roles and Responsibilities
6. Key Inventory
7. Key Classification and Handling
8. Key Lifecycle Management
9. Key Protection and Cryptographic Selection
10. Key Access, Usage, and Transaction Authorization
11. Backup and Recovery
12. Key Compromise Management
13. Key Rotation and Migration
14. Key Revocation, Archival, and Destruction
15. Logging, Monitoring, Audit, and Compliance
16. Exception Management and Emergency Authority
17. Web3-Specific Key Management Principles
18. References and Document Maintenance

### `oscal/`

`web3-cryptographic-key-management-catalog.json` is the machine-managed representation of the policy, using the [OSCAL](https://pages.nist.gov/OSCAL/) Catalog model (release `1.2.3`):

- Policy sections → OSCAL groups (subsections nest as child groups)
- Numbered clauses → OSCAL controls (list subclauses nest as child controls)
- Requirement text → `statement` parts
- Original clause numbers → `label` properties
- Clause-level NIST provenance → `source-classification` properties

A `source-classification` of `pending-control-mapping` must only be replaced once a clause is mapped to a specific NIST source and revision. Section 17 controls are `web3-specific` and must not be represented as direct NIST requirements without a separate, cited mapping decision.

Validate against the pinned OSCAL schema:

```sh
curl -fsSL https://github.com/usnistgov/OSCAL/releases/download/v1.2.3/oscal_catalog_schema.json -o /tmp/oscal_catalog_schema-1.2.3.json
npx --yes --package=ajv-cli@5.0.0 --package=ajv-formats@3.0.1 ajv validate \
  --spec=draft7 \
  --strict=false \
  -c ajv-formats \
  -s /tmp/oscal_catalog_schema-1.2.3.json \
  -d oscal/web3-cryptographic-key-management-catalog.json
```

See `oscal/README.md` for details.

### `source/`

Reference NIST Special Publications the policy draws on, provided as both the original PDF and a converted Markdown copy:

- `NIST.SP.800-57pt1r6.ipd.{md,pdf}` — SP 800-57 Part 1 Rev. 6 (Recommendation for Key Management)
- `NIST.SP.800-131Ar3.ipd.{md,pdf}` — SP 800-131A Rev. 3 (Transitioning the Use of Cryptographic Algorithms and Key Lengths)

### `output/`

`output/pdf/nist-key-management-governance-prysm.pdf` is the current rendered PDF build of the policy.

## Building the PDF

The build script uses [ReportLab](https://www.reportlab.com/) to render the policy as a styled PDF.

```sh
python3 -m venv .venv && source .venv/bin/activate
pip install reportlab
python3 build_governance_pdf.py
```

Output is written to `output/pdf/nist-key-management-governance-prysm.pdf`.

## Document hierarchy

This Policy sits at the top of a governance hierarchy:

- **Policy** (this document) — mandatory outcomes and governance
- **Processes** — lifecycle flows
- **Standards** — technical criteria (e.g. the Cryptographic Baseline)
- **Procedures / SOPs** — approved actions and evidence

Subordinate documents must remain consistent with this Policy.
