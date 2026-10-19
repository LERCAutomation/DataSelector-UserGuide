***********
Writing SQL
***********

.. index::
   pair: SQL; query fields
   single: Columns
   single: Where
   single: Group By
   single: Order By

Four query fields
=================

The form supplies parts of a SQL Server selection to the installed
``AFSelectSppSubset`` procedure. Enter expressions only; omit ``SELECT``,
``FROM``, ``WHERE``, ``GROUP BY`` and ``ORDER BY`` keywords. The source comes
from **SQL Tables**. Leave optional clauses blank when they are not needed.
This form is intended for selections, rather than general SQL scripts.

.. list-table::
   :header-rows: 1
   :widths: 22 78

   * - Field
     - Contents
   * - Columns
     - Comma-separated names or expressions; ``*`` selects all columns.
   * - Where
     - A condition such as ``RecYear >= 2020``.
   * - Group By
     - Non-aggregated expressions used in a summary, such as ``TaxonGroup``.
   * - Order By
     - Sort expressions, such as ``TaxonGroup ASC``.

Column names in these examples are illustrative. Use names and data types
from your selected reporting table. Double-clicking a table can fill
**Columns** with its names, excluding ``MI_STYLE`` and ``MI_PRINX``; the tool
asks before replacing existing text.

.. index::
   pair: SQL; filtering
   single: SQL; aliases
   single: SQL; quoted text

Selecting records
=================

For a recent-record report with an agreed set of attributes:

.. list-table::
   :header-rows: 1
   :widths: 22 78

   * - Field
     - Example
   * - Columns
     - ``TaxonName, CommonName AS Name, RecYear``
   * - Where
     - ``RecYear >= 2020 AND TaxonGroup = 'Birds'``
   * - Group By
     - Leave blank.
   * - Order By
     - ``TaxonName, RecYear DESC``

Use single quotes around SQL text values. A literal apostrophe within a
value is doubled, for example ``CommonName = 'Pallas''s Warbler'``. The tool
separately escapes the expressions when passing them as procedure parameters;
enter normal SQL expressions in the form.

.. index::
   pair: SQL; summaries
   single: COUNT
   pair: Geometry; grouped queries

Summarising records
===================

For a non-spatial count by taxon group:

.. list-table::
   :header-rows: 1
   :widths: 22 78

   * - Field
     - Example
   * - Columns
     - ``TaxonGroup, COUNT(*) AS RecordCount``
   * - Where
     - ``RecYear >= 2020``
   * - Group By
     - ``TaxonGroup``
   * - Order By
     - ``TaxonGroup``

Use CSV or TXT, or a non-spatial TAB, and turn plotting off for this summary.
All selected non-aggregated expressions must be included in **Group By**.
Geometry cannot simply be added to this example as another ordinary grouped
column. Ask the database administrator for a tested spatial aggregation when
a grouped spatial result is required.

.. index::
   single: Geometry columns
   pair: SQL; plotting attributes

Preparing spatial results
=========================

To retain SQL geometry, include the actual geometry column expected by your
database procedures and MapInfo metadata; its name is installation-specific.
To plot from coordinates, include X, Y and grid-size columns under the same
names selected in the plotting controls. Renaming or omitting them can cause
plotting to be cancelled. Include any columns used by symbology clauses too.

The :ref:`worked_example` uses coordinate plotting without requiring a SQL
geometry column. For query errors, read the SQL Server message and check
the selected table, names, types, quotes and grouped expressions. The length
of expressions accepted by the deployed procedures is a database-side limit;
the reviewed checkout does not establish that limit.
