# Product Backlog: Batch Traceability App for Small Manufacturers

## Overview

This backlog is the output of an initial pass of requirements elicitation for a simple web app that helps small businesses that make physical products, such as food or hardware, track where their parts and batches go. Each batch gets a QR code, workers complete inspection checklists on their phones, and a recall search shows every product affected by one bad batch or supplier lot.

The backlog was generated with a large language model (Claude) and then reviewed by the author. The prompt used is included at the bottom of this file.

## Metadata Key

- **Type:** Functional (F) or Non-Functional (NF)
- **Priority:** MoSCoW (Must, Should, Could, Won't for this term)
- **Size:** S (1 to 2 days), M (3 to 5 days), L (1 to 2 weeks)
- **Depends On:** requirement IDs that must be done first
- **Tags:** free-form labels for filtering

## Epics

| Epic | Name |
|------|------|
| E1 | Accounts and Access |
| E2 | Suppliers, Products, and Batches |
| E3 | Labeling and Scanning |
| E4 | Inspections |
| E5 | Traceability and Recall |
| E6 | Reporting and Notifications |
| E7 | Quality Attributes |

## Requirements

| ID | Epic | Requirement | Type | Priority | Size | Depends On | Tags |
|----|------|-------------|------|----------|------|------------|------|
| REQ-001 | E1 | As a business owner, I can register and log in so that my data is private. | F | Must | S | None | auth, security |
| REQ-002 | E1 | As an admin, I can assign roles (admin, inspector, viewer) so that users only do what they are allowed to. | F | Must | M | REQ-001 | auth, roles |
| REQ-003 | E2 | As an admin, I can create and edit supplier records so that incoming materials can be linked to a source. | F | Must | S | REQ-001 | suppliers, data-entry |
| REQ-004 | E2 | As an admin, I can create and edit product types so that batches belong to a known product. | F | Must | S | REQ-001 | products, data-entry |
| REQ-005 | E2 | As a worker, I can record an incoming material lot from a supplier, with lot number and date received. | F | Must | M | REQ-003 | lots, data-model |
| REQ-006 | E2 | As a worker, I can create a production batch for a product type with a date and quantity. | F | Must | M | REQ-004 | batches, data-model |
| REQ-007 | E2 | As a worker, I can link one or more material lots to a batch so that we know what went into it. | F | Must | M | REQ-005, REQ-006 | lots, batches, traceability, data-model |
| REQ-008 | E3 | As a worker, I can generate a unique QR code for each batch. | F | Must | M | REQ-006 | qr, labeling |
| REQ-009 | E3 | As a worker, I can print a label sheet of QR codes with batch name and date. | F | Should | S | REQ-008 | qr, printing |
| REQ-010 | E3 | As a worker, I can scan a QR code with my phone camera to open that batch's page. | F | Must | M | REQ-008 | qr, mobile |
| REQ-011 | E4 | As an admin, I can create inspection checklist templates for each product type. | F | Must | M | REQ-002, REQ-004 | inspection, templates |
| REQ-012 | E4 | As an inspector, I can complete a checklist for a batch from my phone. | F | Must | M | REQ-010, REQ-011 | inspection, mobile |
| REQ-013 | E4 | As an inspector, I can mark a batch pass or fail and add notes so that quality issues are recorded. | F | Must | S | REQ-012 | inspection, quality |
| REQ-014 | E4 | As an inspector, I can attach a photo to an inspection. | F | Could | M | REQ-012 | inspection, media |
| REQ-015 | E5 | As an admin, I can create customer records so that shipments can be linked to a recipient. | F | Should | S | REQ-001 | customers, data-entry |
| REQ-016 | E5 | As a worker, I can record a shipment that links batches to a customer and ship date. | F | Must | M | REQ-006, REQ-015 | shipments, traceability, data-model |
| REQ-017 | E5 | As a manager, I can search by batch and see every affected shipment and customer. | F | Must | L | REQ-007, REQ-016 | recall, search, core-feature |
| REQ-018 | E5 | As a manager, I can search by supplier lot and see every downstream batch, shipment, and customer. | F | Must | L | REQ-017 | recall, search, core-feature |
| REQ-019 | E5 | As a manager, I can export a recall report as CSV. | F | Should | S | REQ-017 | recall, export |
| REQ-020 | E6 | As a manager, I can view a dashboard of defect rates over time by product and supplier. | F | Could | M | REQ-013 | dashboard, analytics |
| REQ-021 | E6 | As a manager, I receive an email when a batch fails inspection. | F | Could | M | REQ-013 | notifications, email |
| REQ-022 | E7 | The system keeps an audit log of who created or changed records and when. | NF | Should | M | REQ-001 | audit, security |
| REQ-023 | E7 | The interface works on phone screens and current mobile browsers. | NF | Must | S | None | mobile, usability |
| REQ-024 | E7 | A recall search returns results in under 2 seconds with up to 10,000 batches. | NF | Should | M | REQ-017 | performance |
| REQ-025 | E7 | Data is stored in a persistent database with automated daily backups. | NF | Should | S | None | reliability, data |
| REQ-026 | E7 | Business data is isolated so that one company can never see another company's records. | NF | Must | M | REQ-001 | security, multi-tenant |

## Dependency Summary

The longest dependency chain, which forms the critical path for the core feature, is:

REQ-001, then REQ-003 and REQ-004, then REQ-005 and REQ-006, then REQ-007, then REQ-016 (also needs REQ-015), then REQ-017, then REQ-018.

Other notable chains:

- Scanning and inspection: REQ-006, REQ-008, REQ-010, REQ-012, REQ-013, then REQ-020 and REQ-021
- Export and performance both build on the recall search: REQ-017, then REQ-019 and REQ-024
- REQ-026 should be settled early because it affects the data model for every other requirement

## Suggested First Iteration

REQ-001, REQ-003, REQ-004, REQ-005, REQ-006, REQ-007, REQ-023, and REQ-026. This builds the data model that everything else depends on.

## Assumptions and Open Questions

1. Each business has its own account and data, with no sharing between businesses.
2. A batch can contain multiple material lots, and a lot can be used in multiple batches.
3. Is serial-level tracking of individual units needed, or is batch-level tracking enough? This backlog assumes batch-level.
4. Are workers always online, or is offline scanning required? This backlog assumes online only.
5. Which regulations, such as food safety rules, affect required record retention? This is not covered yet.
6. Out of scope for this term: barcode hardware integrations, accounting integrations, and native mobile apps.

## Prompt Used to Generate This Backlog

> I am building a simple web app that helps small businesses that make physical products, like food or hardware, track where their parts and batches go. Each batch gets a QR code, workers fill out inspection checklists on their phone, and each batch is linked to its supplier and shipment. The main feature is a search that shows every product affected by one bad batch. Act as a requirements analyst and produce an initial product backlog from this description. For each requirement, include an ID, epic, user story or requirement statement, type (functional or non-functional), MoSCoW priority, relative size, tags, and a list of requirement IDs it depends on. Then summarize the critical dependency path, suggest a first iteration, and list assumptions and open questions that I should clarify with stakeholders.
