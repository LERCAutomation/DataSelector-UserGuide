************
Introduction
************

.. index::
   single: DataSelector; overview
   pair: Recorder 6; data selections
   pair: SQL Server; master tables

Background
==========

DataSelector for MapInfo provides a form for selecting records from prepared
SQL Server tables. It was developed for biodiversity reporting from Recorder 6,
but the available tables, attributes and selection rules depend on each
organisation's database setup.

Recorder 6 has a complex relational structure. An administrator can prepare
master tables or views that bring commonly used attributes together, then
make those tables available through DataSelector. These reporting tables
must be refreshed separately; running DataSelector does not refresh them or
query Recorder 6's underlying tables automatically.

This guide covers the MapInfo tool, version |release|. A separate
`DataSelector for ArcGIS Pro <https://github.com/LERCAutomation/DataSelector-ArcPro>`_
is available and has its own configuration and operating requirements.

Tool overview
=============

Choose a SQL table, enter the columns and optional selection, grouping and
ordering expressions, then select an output format and destination. The
tool sends the expressions to SQL Server through a configured file DSN and
stored procedures. It downloads the resulting subset into MapInfo before
creating TAB, SHP, CSV or tab-separated TXT output.

TAB outputs can retain existing geometry or be plotted from coordinates and
grid sizes, and can have a configured symbology set applied. Non-spatial TAB
results open in a browser. SHP results are translated through MapInfo's
Universal Translator; CSV and TXT results contain attributes. See
:ref:`output_formats` for the differences and :numref:`figSelectorForm` for
the main controls.

Queries can be saved as QSF files for reuse. These save the four SQL text
fields; users still choose the source table, output and processing options
when loading a query. Each run writes messages to a user-specific log.

The :doc:`../quickstart/quickstart` chapter provides the routine workflow and
a worked example. Administrators should start with :doc:`../setup/setup`.

Benefits
========

* Query prepared reporting data without opening Recorder 6.
* Reuse agreed selection expressions and share them where table structures match.
* Produce GIS layers or tabular reports from the same source.
* Control data inclusion, confidentiality, sensitivity and formatting in the
  master-table preparation process.
* Review selected-record counts and processing messages in the run log.
