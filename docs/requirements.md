High-level requirements
Core metamodel engine
R1 — Configurable element types. The system shall allow administrators to define element types (e.g. System, Component, Platform, Business Capability) with a name, description, icon/color hint, and whether they are nodes or connectors in diagrams.
R2 — Configurable properties. Each element type shall have a configurable schema of properties: string, number, enum, boolean, URL, reference to another element, and multi-value list. Properties should be markable as required or optional.
R3 — Configurable relation types. Administrators shall define named relation types (e.g. "depends on", "composes", "exposes", "owns") with a source type constraint, target type constraint, cardinality, and direction. The system shall enforce these rules during data entry and API responses.
R4 — Metamodel versioning. Changes to the metamodel shall be versioned. The system should track when a type or property was introduced, and existing elements should survive metamodel evolution (graceful degradation with unknown fields preserved).
R5 — Built-in starter metamodels. The system shall ship with at least one pre-built metamodel configuration (e.g. Backstage-style: Domain → System → Component/API/Resource) that users can adopt as-is or fork and customize.

Catalog / data model
R6 — Element identity. Every element shall have a stable UUID, a human-readable slug (unique within type), a display name, and an optional namespace for multi-tenant or multi-team isolation.
R7 — Typed properties. Elements shall store their properties validated against their type's schema. Unknown properties from older metamodel versions shall be preserved but flagged.
R8 — Relations as first-class records. Relations between elements shall be stored as explicit records with a type, source element, target element, and optional properties. This enables traversal queries and impact analysis.
R9 — Ownership and provenance. Each element shall record an owner (team or individual), a lifecycle status (e.g. active, deprecated, proposed), and creation/modification timestamps.
R10 — Hierarchical containment. The system shall support parent/child containment relationships (e.g. a System contains Components), distinct from regular association relations, to enable tree-view navigation.

API (DiagramCraft integration surface)
R11 — Metamodel API. GET /metamodel shall return the full current metamodel: all element types with their property schemas, all relation types with their connection rules, and the version identifier. DiagramCraft uses this to know what shapes/properties are available.
R12 — Catalog query API. GET /catalog/elements shall support filtering by type, owner, lifecycle status, namespace, and free-text search. It shall return paginated results with enough data to populate a drag-palette (name, type, icon, short description).
R13 — Element detail API. GET /catalog/elements/{id} shall return the full element including all properties, outgoing and incoming relations (with targets resolved to at least name + type), and metadata.
R14 — Canvas sync contract. The API shall define a stable "canvas element" projection: a minimal, stable shape of data that DiagramCraft stores per node on a canvas. This projection contains the element ID, display name, type, and a set of display properties chosen by the user. The catalog is the source of truth; DiagramCraft subscribes to updates.
R15 — Change notifications. The system shall provide a mechanism (webhook or polling endpoint) for DiagramCraft to detect when a catalog element has changed, so canvas instances can reflect updates (e.g. a renamed system propagates to all diagrams containing it).

Governance and UX
R16 — Access control. The system shall support role-based access: read-only users, contributors (can edit elements they own), and metamodel administrators (can modify types and schemas).
R17 — Audit log. All mutations to elements and metamodel changes shall be logged with actor, timestamp, and a diff of the change.
R18 — Import/export. The system shall support bulk import (CSV or JSON) and export of catalog contents, including relation data. The export format should be self-describing (embed the metamodel it was exported against).
R19 — Validation rules. Beyond property type validation, administrators shall be able to define cross-element rules (e.g. "every Component must belong to a System", "every System must have an owner"). Rules can be advisory (warnings) or enforced.
