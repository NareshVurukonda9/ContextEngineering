# Release Notes — Acme Technologies Platform

## v3.2.0 — October 8, 2025

This release introduces the **Platform AI Layer (PAL)**, a set of AI-assisted workflow capabilities
available to all Enterprise Platform Pro subscribers. It also includes several performance improvements,
breaking changes to the Webhooks API, and bug fixes across the Automation Builder and Dashboard modules.

---

### Breaking Changes

> **Action required before upgrading.** Review each item below and update your integrations accordingly.

#### Webhooks API v1 Deprecated

The legacy Webhooks API (`/api/v1/webhooks`) has been removed as of this release. All webhook
configurations must be migrated to the v2 Webhooks API (`/api/v2/webhooks`) introduced in v3.0.0.

Key differences in v2:

- Event payloads now use `snake_case` field names instead of `camelCase`
- The `event_type` field replaces the legacy `type` field
- Webhook secrets are now required; unsigned webhooks will be rejected with HTTP 401
- Retry behavior changed from fixed 3-attempt to exponential backoff (max 8 attempts over 24 hours)

**Migration guide:** See [Webhook Migration Guide](https://docs.acme.io/webhooks/migration) for
field mapping tables and example payloads.

#### Automation Builder: Condition Node Schema Change

Condition nodes in saved automation workflows using the `legacy_compare` operator have been
automatically migrated to the new `compare_v2` operator. If you maintain automation definitions
as JSON/YAML in version control, update the operator field:

```json
// Before (v3.1.x)
{ "operator": "legacy_compare", "field": "status", "value": "active" }

// After (v3.2.0)
{ "operator": "compare_v2", "field": "status", "comparator": "eq", "value": "active" }
```

---

### New Features

#### Platform AI Layer (PAL) — Enterprise Pro

PAL brings three AI-assisted capabilities into the platform core:

**1. Smart Workflow Suggestions**
The system now analyzes your team's workflow history and surfaces suggested automations based on
recurring manual task patterns. Suggestions appear in the Automation Builder sidebar under
*AI Suggestions*. Suggestions can be accepted as-is, edited, or dismissed. All suggestions are
generated on-premise using your tenant's isolated model instance — no data leaves your environment.

**2. Natural Language Condition Builder**
Write automation conditions in plain English and have them compiled to the platform's condition
schema automatically. For example:

```
"trigger when a ticket has been open for more than 3 days and is assigned to no one"
```

Compiles to:

```json
{
  "operator": "and",
  "conditions": [
    { "operator": "compare_v2", "field": "age_hours", "comparator": "gt", "value": 72 },
    { "operator": "compare_v2", "field": "assignee_id", "comparator": "is_null", "value": null }
  ]
}
```

**3. Document Summarization in Context Panel**
Attached documents in task and record context panels now include an AI-generated summary
(max 3 sentences) displayed above the attachment preview. Supported formats: PDF, DOCX, TXT, MD.
Summarization can be disabled per workspace in **Settings → AI Features → Document Summarization**.

#### Bulk Record Operations API

A new bulk operations endpoint allows up to 500 records to be created, updated, or deleted in a
single API call, reducing round-trips for high-volume integrations.

```http
POST /api/v2/records/bulk
Content-Type: application/json

{
  "operation": "upsert",
  "id_field": "external_id",
  "records": [ ... ]
}
```

Response includes a per-record status array indicating success, validation errors, or conflicts.
Rate limit: 10 bulk requests per minute per API key.

#### Dashboard: Metric Comparison Mode

The Analytics Dashboard now supports side-by-side period comparison. Select any two date ranges
using the new **Compare** toggle in the date picker. Metrics, charts, and tables will render
delta values and percentage changes inline. Comparison mode is available for all standard and
custom report types.

#### SSO: SCIM 2.0 Provisioning (Enterprise)

Enterprise customers can now automate user provisioning and deprovisioning via SCIM 2.0. Supported
identity providers at launch: Okta, Azure AD, and JumpCloud. Configuration is available under
**Settings → Security → Directory Sync**. SCIM provisioning replaces manual CSV user imports.

---

### Improvements

- **Performance:** Dashboard load time reduced by 38% for workspaces with more than 10,000 records,
  achieved by query plan optimizations and incremental rendering for large data tables.

- **Automation Builder:** The canvas now supports undo/redo (Ctrl+Z / Ctrl+Shift+Z) for node
  placement and connection operations. History depth: 50 actions.

- **API:** The `/api/v2/records` list endpoint now supports cursor-based pagination in addition to
  offset pagination. Cursor pagination is recommended for large datasets as it prevents result
  drift during concurrent writes. Pass `pagination_type=cursor` and use the `next_cursor` token
  from the response.

- **Notifications:** In-app notification preferences can now be configured at the workspace level
  by admins, providing default settings that individual users can optionally override.

- **Mobile (iOS / Android):** Offline mode now supports creating and editing records. Changes sync
  automatically when connectivity is restored. Conflict resolution uses last-write-wins with a
  conflict log available under **Profile → Sync History**.

---

### Bug Fixes

| ID        | Module              | Description                                                                 |
|-----------|---------------------|-----------------------------------------------------------------------------|
| BUG-2841  | Automation Builder  | Fixed issue where branching nodes with more than 8 outputs rendered overlapping edges |
| BUG-2798  | Dashboard           | Resolved incorrect percentage calculation for cohort retention charts when date range spans a DST boundary |
| BUG-2754  | Webhooks            | Fixed race condition causing duplicate delivery for webhooks under high-concurrency load |
| BUG-2719  | API                 | `updated_at` field now correctly reflects the last field-level change rather than the last API access time |
| BUG-2701  | SSO                 | Resolved SAML attribute mapping failure for IdPs that return group membership as a comma-separated string |
| BUG-2688  | Mobile              | Fixed crash on Android 14 when opening attachments larger than 50 MB       |
| BUG-2671  | Notifications       | Email digests now correctly de-duplicate events that fired multiple times within the same second |

---

### Deprecations

The following features are deprecated in v3.2.0 and will be removed in **v4.0.0** (planned Q2 2026):

- **Legacy Report Builder (v1):** The original report builder UI is superseded by the Analytics
  Dashboard. All saved v1 reports have been automatically migrated. The v1 UI remains accessible
  at `/reports/legacy` until removal.

- **CSV User Import:** Replaced by SCIM 2.0 provisioning for Enterprise customers and by the
  improved manual invite flow for all other tiers. The CSV import endpoint
  (`POST /api/v1/users/import`) will be removed in v4.0.0.

- **`/api/v1/automations` endpoint:** Use `/api/v2/automations`. The v1 endpoint returns a
  deprecation warning header (`X-Deprecation-Notice`) with each response.

---

### Upgrade Notes

**Minimum supported versions after v3.2.0:**

| Component          | Minimum Version |
|--------------------|-----------------|
| Chrome             | 118             |
| Firefox            | 119             |
| Safari             | 17.0            |
| iOS App            | 3.1.0           |
| Android App        | 3.1.0           |
| Node.js SDK        | 2.4.0           |
| Python SDK         | 1.9.0           |

Customers on the Enterprise plan with a dedicated CSM should schedule an upgrade review session
before deploying to production. Contact your CSM or open a ticket at support.acme.io.

---

## v3.1.2 — September 12, 2025

Patch release addressing three production issues identified after the v3.1.0 rollout.

### Bug Fixes

- **BUG-2830:** Fixed memory leak in the Automation Builder canvas that caused browser tab
  slowdown after 90+ minutes of continuous editing in large workflows (50+ nodes).
- **BUG-2822:** Resolved issue where the record list view returned stale data for up to 60 seconds
  after a bulk update operation. Cache invalidation now triggers immediately on bulk write completion.
- **BUG-2815:** API keys created via the admin panel now correctly inherit the permission scopes
  selected during creation. Previously, keys defaulted to read-only regardless of selected scopes.

---

## v3.1.0 — August 28, 2025

See the [v3.1.0 full release notes](https://docs.acme.io/releases/v3.1.0) for the complete
changelog, including the Analytics Dashboard GA launch, the new Permissions model, and
API v2 record schema changes.

---

*Acme Technologies Platform · Documentation: docs.acme.io · Support: support.acme.io*
