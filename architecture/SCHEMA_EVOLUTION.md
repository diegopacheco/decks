# Schema Evolution

Schema evolution is the practice of changing data structures, API contracts, or message formats over time while maintaining compatibility with existing clients and services. This is critical for zero-downtime deployments in distributed systems.

## Forward Compatibility

Forward compatibility means that old code can read data written by new code. The old system can safely ignore new fields it doesn't understand.

When adding new fields to a schema:
* New fields should be optional with sensible defaults
* Old services can process new messages by ignoring unknown fields
* Allows deploying new producers before updating consumers

## Backward Compatibility

Backward compatibility means that new code can read data written by old code. The new system must handle the absence of fields that didn't exist in older versions.

When reading old data:
* New code must provide defaults for missing fields
* New services can process old messages correctly
* Allows deploying new consumers before updating producers

## Breaking Changes

Breaking changes destroy compatibility and require coordinated deployments. Avoid these whenever possible:

* Removing required fields
* Changing field types
* Renaming fields without aliasing
* Changing field semantics
* Making optional fields required

## Safe Schema Changes

Safe changes maintain compatibility:

* Adding optional fields with defaults
* Removing optional fields
* Adding new enum values at the end
* Adding new message types
* Deprecating fields instead of removing them

## Versioning Strategies

**URL Versioning**: Different versions in the URL path like `/api/v1/users` and `/api/v2/users`

**Header Versioning**: Version specified in request headers like `Accept: application/vnd.api.v2+json`

**Content Negotiation**: Different media types for different versions

**No Versioning**: Evolve schema compatibly without explicit versions. Requires discipline but provides best flexibility.

## Migration Strategies

**Expand-Contract Pattern**: Three-phase deployment for breaking changes:
1. Expand: Add new field alongside old field
2. Migrate: Update all services to use new field
3. Contract: Remove old field after migration complete

**Shadow Reading**: New code reads both old and new formats, writes only new format

**Feature Flags**: Toggle between old and new behavior at runtime

## Database Schema Evolution

Database schemas require special care because data persists:

* Use migrations that can run without downtime
* Add columns as nullable first, backfill data, then make required
* Drop columns in separate deployment after code stops using them
* Use views or aliases to maintain old column names during transitions

Schema evolution is not optional in production systems. Every change must consider compatibility to avoid outages during deployments.
