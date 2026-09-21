[Česky](../cz/20_PRAVIDLA_DOKUMENTACE.md) | [English](19_DOCUMENTATION_RULES.md)

# Documentation rules

Describe the behavior of the matching flow, then explain purpose, inputs, output and limitations. Distinguish current implementation from planned work. Keep history in the changelog and review findings in the review report.

The reference flow is the primary source for code behavior. Dashboard help and prior manuals explain intent but must not override contradictory implementation. Independent modules have their own repositories; LINEA documents integration boundaries and links to their installation manuals.

Use API version 1.0.0 and schema 1 for this reference, keep examples and OpenAPI consistent, and distinguish flow version from API/configuration schema. A static review does not establish live hardware behavior. See [review scope](../../REVIEW_CZ.md).

[← Documentation](README.md)
