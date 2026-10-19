********************
Extending Recorder 6
********************

.. index::
   pair: Recorder 6; reporting tables
   single: Master tables; refresh
   single: Confidential records
   single: Sensitive records

Prepared reporting data
=======================

An administrator prepares master tables or views containing the data that
DataSelector users may query. The preparation scripts determine which
surveys, samples and occurrences are included, the attributes and names
available, verification and zero-abundance rules, and how dates,
designations, counts and comments are represented.

Confidentiality and sensitivity rules must also be agreed at this stage.
DataSelector has no separate confidentiality checkbox or automatic
sensitivity filter. Users' SQL conditions operate on the reporting data
made available to them.

Master-table refreshes are separate from the tool. Record the last refresh
and any relevant exclusions so users can interpret results. Several master
tables or views can expose different reporting datasets.

.. index::
   pair: Geometry; SQL Server
   pair: Geometry; coordinate plotting
   single: Spatial_Tables
   single: MAPINFO_MAPCATALOG

Spatial and non-spatial data
============================

Prepared SQL tables may include geometry and the metadata needed for
MapInfo to recognise it. Where existing geometry is required, include the
appropriate source geometry column in the selected columns and use the
organisation's tested SQL procedures and spatial metadata.

Alternatively, the tool can plot or re-plot TAB and SHP results from numeric
X, Y and grid-size attributes. Coordinates must match the output table's
coordinate system, and grid sizes must be positive and in matching units.
The :ref:`plotting` instructions explain the grid alignment and point/region
threshold. A preloaded map or prepared workspace is optional: the tool
queries SQL Server, rather than selecting from open MapInfo layers.

The database-side dependencies and spatial metadata are described under
:ref:`sql_database_setup`. Their installed definitions must be checked by
the database administrator; current SQL scripts are not included in the
reviewed MapInfo checkout.

.. index::
   single: Workflow; selection and export
   pair: Outputs; file sets

Tool workflow and outputs
==========================

1. Choose a profile and one SQL source table.
#. Enter or load the four query fields and check the output options.
#. Choose a destination after clicking **Ok**.
#. SQL Server creates a user-specific subset. MapInfo checks its size,
   downloads it and unlinks the downloaded table from the server.
#. The tool exports attributes, plots geometry or applies TAB symbology as
   requested. Non-TAB formats use an intermediate TAB table that is normally
   removed after export.
#. Review the log and actual files before using the results.

A mappable TAB output is added to the front map window, or a new map is
created if the front window is not a map. A non-spatial TAB output opens in
a browser. SHP, CSV and TXT outputs are saved without being added to the map.
See :ref:`output_formats` for the full reference.

Keep TAB files with their companion data, index and map files, and keep
shapefile components together. Renaming or moving only the main TAB or SHP
file can make the result unusable.

Choose a destination with sufficient space for both the intermediate TAB
table and the requested export.
