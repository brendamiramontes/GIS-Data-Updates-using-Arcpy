# GIS-Data-Updates-using-Arcpy

## Udate Feature Class Table Script
## 📌Project Overview
This project automates the process of synchronizing attributes and geometry between a local feature class and an enterprise geodatabase (SDE) versioned feature class.

The script was built to:
Ensure that PLANSETNUMBER is populated when missing (defaulting to "needs review").
Align XY geometry coordinates in SDE with those from the local dataset.
Run updates inside a versioned edit session, so that all changes are isolated to a private version before reconcile/post.

## ⚙️ Workflow

Create SDE Connection File
A database connection file (.sde) is generated and stored securely (outside of source control).

This connection should point directly to the target version (e.g., "user.versionName").

Connection files must never be committed to GitHub for security reasons.

Run Script

The script opens an edit session (arcpy.da.Editor) on the enterprise workspace.

It builds a dictionary of local FACILITYID → PLANSETNUMBER, XY.

Using an UpdateCursor, it iterates through the SDE features and applies updates.

Geometry updates only occur when XY values differ to avoid unnecessary writes.

Data Governance

All edits are applied to a private version, not sde.DEFAULT.

Final QA and reconciliation/posting should follow organizational data governance practices.

Audit logs (counts of updated records) are produced for transparency.

🚀 Features

Updates PLANSETNUMBER only if null/empty.

Updates geometry XY only when values differ (prevents duplicate instance errors).

Tracks counts of updated PLANSETNUMBER vs geometry for reporting.

Designed to scale from small batches to hundreds/thousands of records.

🔐 Governance Notes

Connection files (.sde) are environment-specific and must not be stored in this repository.

Credentials are never hardcoded; use secure database authentication methods.

This script assumes traditional versioning; for branch versioning, remove the arcpy.da.Editor block.

Before posting edits to DEFAULT, follow your organization’s QA/QC and approval workflow.
