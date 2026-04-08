# Food Distribution Reporting System

Backend system for importing, normalizing, and reporting on nonprofit intake and delivery data.

The system supports two related reporting workflows: donor-facing reporting for food recovery partners and organization-level reporting for nonprofit operations and compliance.

## Why this exists

California food recovery requirements increased the reporting burden on both donor businesses and the nonprofits that receive and distribute donated food. In practice, that meant more paperwork, more spreadsheet tracking, and more pressure to produce accurate summaries in two directions: donor-facing reports for businesses that need donation records for audits or compliance, and organization-level reports for the nonprofit’s own reporting obligations.

This project is inspired by a real nonprofit reporting workflow involving intake and distribution ledgers, monthly summaries, and annual valuation reporting. It is a backend-first proof of concept that moves that workflow into a structured system with reproducible imports, canonical name mapping, SQL-backed reports, auditable import batches, and Dockerized local setup.

This repository uses sanitized sample data and is designed as a portfolio project rather than an official production system.

## Goals

- Replace spreadsheet-side summary logic with database-backed reporting
- Preserve the real workflow of intake and delivery ledgers
- Make imports traceable and easier to validate
- Generate both donor-level and organization-level reports from the same normalized source data
- Generate useful monthly and annual reports from SQL
- Demonstrate backend engineering skills in Python, PostgreSQL, Docker, and data modeling

## Scope

Version 1 focuses on:

- importing intake records
- importing delivery records
- mapping raw names to canonical donors and recipient sites
- generating donor-facing summaries for food recovery partners
- generating organization-level summaries for nonprofit operations and compliance reporting
- generating monthly trend reports
- calculating annual valuation summaries

Version 1 does **not** try to solve every operational problem. It is intentionally narrow so the core workflow is finished well.

## Stack

- Python
- FastAPI
- PostgreSQL
- SQLAlchemy
- Alembic
- Docker Compose
- Pytest

## Domain model

The system is centered on the real reporting workflow:

- **intake records**: incoming donations by date, donor/source, and pounds
- **delivery records**: outgoing distributions by date, recipient site, and pounds given
- **donor sources**: canonical donor/store/source records
- **recipient sites**: canonical pantry/program/partner records
- **name aliases**: maps inconsistent spreadsheet names to canonical entities
- **import batches**: tracks uploaded files, row counts, status, and import history
- **valuation rates**: stores annual value-per-pound rates for reports

## Example reports

The system is designed to generate two classes of reports.

### Donor-facing reports
- total pounds donated by a donor in a date range
- monthly totals for one donor/store
- annual donor/store comparison report
- donor valuation summaries

### Organization-level reports
- total pounds donated in a date range
- total pounds distributed in a date range
- pantry/site comparison report
- monthly intake trends
- monthly delivery trends
- annual valuation summaries

## Architecture

The project follows a backend-first structure:

- API routes for health, imports, and reports
- service layer for import and reporting logic
- repository/query layer for database access
- PostgreSQL as the source of truth
- Docker Compose for reproducible local setup
- Alembic migrations for schema management

The goal is to keep business logic out of controllers and make reporting queries deliberate and testable.

## Quickstart

### 1. Clone the repository

```bash
git clone https://github.com/josephzazo/food-distribution-reporting-system.git
cd food-distribution-reporting-system
