**************************
Frequently asked questions
**************************

.. index::
   single: FAQ
   see: Troubleshooting; FAQ
   single: Downloads

General questions
=================

**Where can I get the tool?**

The `MapInfo repository <https://github.com/LERCAutomation/DataSelector-MapInfo>`_
contains source, example XML and compiled applications. Ask the administrator
for the tested build and profiles appropriate to your MapInfo installation.
See :ref:`administrator_checklist` before upgrading.

.. index::
   single: QGIS

**Does the tool work with QGIS?**

The MapBasic application runs in MapInfo. QGIS would require a separate
implementation; a shapefile or CSV exported by the tool can be used there.

.. index::
   single: ArcGIS Pro

**Is there an ArcGIS Pro version?**

Yes. The separate
`ArcGIS Pro implementation <https://github.com/LERCAutomation/DataSelector-ArcPro>`_
has its own requirements. Use configuration appropriate to that application.

.. index::
   pair: Users; concurrent runs
   pair: Outputs; shared destinations

**Can several people run selections at the same time?**

Each person can load the tool in their MapInfo session, but outputs must
use distinct destinations. Database subsets use a suffix derived from the
Windows username, which may be shortened for long source table names.
Separate sessions using the same account, or usernames that become identical
after normalisation or shortening, can share a subset name. Administrators
should check naming before allowing overlapping runs against the same source.
The same account and log folder also use the same log filename.

.. index::
   pair: SQL Tables; missing source
   pair: Profiles; wrong database

Configuration and queries
=========================

**Why is a SQL table missing from the list?**

Check the profile and database connection, then ask the administrator to
check :ref:`setting_TableListSQL`, :ref:`setting_DatabaseSchema` and access
permissions. Names should be returned without a schema prefix, and the query
should exclude temporary subsets. Opening a MapInfo layer does not add its
name to this SQL Server list.

.. index::
   pair: Plotting; hidden options

**Why are the plotting options hidden?**

They are controlled by :ref:`setting_DefaultSpatialPlotting` and the output
format. Yes enables them initially; No exposes an unchecked plotting option;
blank hides it. CSV/TXT runs do not plot. The symbology selector applies to
TAB output only.

.. index::
   pair: SQL; errors
   single: Invalid column name

**What does a SQL error mean?**

Read the SQL Server error and check the selected source, column names and
data types, quotes and grouped expressions. Omit SQL keywords from the four
fields. Use :doc:`../sql/sql` for examples. Errors about procedure parameters,
missing subsets or map-catalogue entries need an administrator to check
:ref:`sql_database_setup`; the guide cannot establish the deployed procedure
definitions.

.. index::
   pair: QSF files; options not restored

**Why did loading a query not restore my table or plotting options?**

A QSF stores only the four SQL text fields. Select the source table and
check output format, plotting, symbology, deletion and log choices yourself.
See :ref:`saved_queries`.

.. index::
   pair: Limits; cancelled selections

Outputs and logs
================

**Why was the selection cancelled for exceeding a limit?**

The tool cancels results exceeding RecMax, row-width or intended data-size
limits. It does not return only the first RecMax rows. Reduce columns or
split the selection; see :ref:`selection_limits`.

.. index::
   pair: Geometry; missing output
   pair: TAB; browser output

**Why is the result in a browser, or missing from the map?**

Non-spatial TAB results open in a browser. Check that existing SQL geometry
was included and recognised, or enable coordinate plotting and include its
required attributes. SHP, CSV and TXT outputs are saved without being added
to a map. See :ref:`output_formats` and :ref:`plotting`.

.. index::
   pair: SHP; failed export
   pair: Logs; completion messages

**The tool says processing completed, but my shapefiles are missing.**

The completion message can follow an unsuccessful translation. Read earlier
errors in the main log and inspect the translator's mapping file and MUT
log. Check the Universal Translator installation and try a short basename
without spaces. Use TAB output to retain results for diagnosis. See
:ref:`run_logs`; do not treat the completion prompt as export verification.

.. index::
   pair: Logs; location and refresh

**Where is the log, and why does Notepad show older messages?**

The user-specific log is written directly in :ref:`setting_LogFilePath`.
Notepad opens after the close-form prompt; an already open copy may need
reloading. Check the selected profile's log folder. **Clear log file?**
deletes the old log at the start, even if the output Save dialog is later
cancelled. See :ref:`run_logs`.

.. index::
   single: Bug reports
   pair: Diagnostics; reporting issues

Reporting a problem
===================

Check existing `MapInfo issues
<https://github.com/LERCAutomation/DataSelector-MapInfo/issues>`_ before
reporting a new issue. Include:

* Tool version and the MBX used, plus MapInfo version and 32/64-bit architecture.
* Chosen profile, SQL source and the four query fields, with sensitive data removed.
* Output format/path and plotting, symbology and deletion choices.
* Expected and actual results, the full error text and reproducible steps.
* Relevant main-log messages and, for SHP, translator diagnostics and the
  files actually created.

An administrator may also need to identify the installed stored-procedure
versions. Remove credentials, confidential records and personal paths from
anything shared publicly.
