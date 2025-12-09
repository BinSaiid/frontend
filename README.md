Shopping List (Home Assistant) — Extended UX + metadata

Overview
--------

This document describes the Shopping List changes used in this work: the list is exposed as a To‑do entity (`todo.shopping_list`) and supports optional per‑item metadata (quantity and store). Metadata is kept backward compatible by encoding it in the To‑do ``description`` field.

What’s included
---------------

Core (Home Assistant Core)

- Expose shopping list as a To‑do entity (`todo.shopping_list`).
- Support optional metadata for items: **Quantity** and **Store**. Metadata is encoded/decoded by core using the To‑do item description.

Frontend (Home Assistant Frontend)

- Search/filter UI for instant filtering by item summary.
- Duplicate guard: avoids creating duplicate active items (same name + same store); if an item is completed, adding it again can revive it.
- Inline quantity display (badge like ``×7``) and store filtering.

User-visible behavior
---------------------

- Add items with name only, or with quantity and optional store (e.g. ``Apples ×3 (Store: Market)``).
- Duplicate guard prevents duplicate active items and revives completed items when re‑added.
- Search and store filter let users quickly find items.

Technical overview
------------------

Metadata encoding (compatibility)

To remain backward compatible, metadata is carried in the To‑do description using a simple line format::

  Store: <value>
  Quantity: <value>

Example

  Description stored on a To‑do item::

    milk
    Store: ica
    Quantity: 4

  Parsed result in core:

  - name: "milk"
  - store: "ica"
  - quantity: 4

Separation of concerns

- Core: parsing/serialization, persistence, websocket/service endpoints.
- Frontend: UX (search, duplicate guard, presentation).

Repository structure
--------------------

- Core changes: ``homeassistant/components/shopping_list/``
- Tests: ``tests/components/shopping_list/``

Testing
-------

Run the shopping list tests locally (examples):

```bash
pytest tests/components/shopping_list -q
pytest -k shopping_list
```

Relevant test files:

- ``tests/components/shopping_list/test_todo.py``
- ``tests/components/shopping_list/test_todo_meta.py``

Related notes
-------------

- This file focuses on the core behaviour and compatibility approach. Frontend UX work may live in the frontend repository; link to it if you maintain a frontend branch.
- See the top-level ``CONTRIBUTING.md`` for contribution and testing guidance.

License and contribution
------------------------

This repository follows the project licensing and contribution guidelines. See the repository root for ``LICENSE.md`` and ``CONTRIBUTING.md``.
