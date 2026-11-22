---
tags:
  - Spring
---
Cascade Types:
- PERSIT: If entity is persisted / saved, related entity will also be persisted
- REMOVE: If entity is removed / deleted, related entity will also be deleted
- REFRESH: If entity is refreshed, related entity will also be refreshed
- DETACH: If entity is detached (not associated with session), then related entity will also be detached
- MERGE: If entity is merged, then related entity will also be merged
- ALL: All of the above cascade types