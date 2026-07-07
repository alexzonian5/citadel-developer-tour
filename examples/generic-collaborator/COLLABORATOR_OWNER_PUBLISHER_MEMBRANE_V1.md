# Collaborator Owner Publisher Membrane V1

## Purpose
Replace crude browser-driven document creation with a governed owner-controlled publishing membrane that lets Citadel workers generate artifacts quickly while preserving ownership, quality, and receipts.

## Why this exists
The correct law is:
- Citadel prepares and governs the materials
- an owner-controlled publishing lane creates or updates the outward documents
- receipts flow back into Citadel

## Hard rules
- do not rely on doc-by-doc browser paste as the default operating path
- use browser or manual intervention only as an exception or recovery lane
- prefer API-backed publication under the owner-controlled identity

## The 3-lane model

### 1. Sovereign prep lane
This lane stays inside Citadel.
Its output is a governed publish packet.

### 2. Publisher lane
This lane is the only outward membrane.
It is responsible for:
- create document
- update document
- place in correct folder
- preserve title, order, and location law
- return document ids and links

### 3. Receipt and writeback lane
This lane closes the loop.
It records:
- publish success or failure
- document id
- document url
- folder id
- title
- content hash
- published_at
- publishing lane used
- fallback or exception taken

## Core operating packet
Recommended fields:
- `collaborator`
- `artifact_type`
- `title`
- `source_path`
- `target_folder_id`
- `ownership_mode`
- `publish_mode`
- `format_mode`
- `content_hash`
- `content_payload_path`
- `visibility_class`
- `receipt_required`
- `fallback_policy`

## Default execution sequence
1. worker generates governed artifact locally
2. worker emits publish packet
3. publisher lane resolves target folder and title
4. publisher lane creates or updates outward document under owner-controlled identity
5. receipt lane stores publish receipt locally

## Success condition
The system is working when a worker can finish collaborator-facing work without manual document labor, and the result is high-quality, owner-controlled, fast, repeatable, and receipt-backed.
