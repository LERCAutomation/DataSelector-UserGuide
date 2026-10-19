***********
Quick start
***********

These steps assume an administrator has installed DataSelector and prepared
the database connection and profiles. Use the :ref:`administrator_checklist`
for installation and upgrades. Routine users do not need to edit XML.

.. index::
   single: Quick start
   pair: Queries; routine workflow
   single: Workspace; optional background mapping

.. _quick_start:

Run a routine selection
========================

1. **Open MapInfo.** Open a prepared workspace if you need background maps;
   no preloaded source layers are required for the SQL selection.
#. **Launch DataSelector and choose a profile.** Use the Data Selector button
   in **Home --> Tools**, or **Tools --> Data Selector --> Run Data Selector**
   in the classic interface. A single profile is selected automatically.
#. **Select one SQL table and enter or load a query.** Check **Columns**,
   **Where**, **Group By** and **Order By**. A loaded QSF restores those fields
   only; select the matching table and options yourself.
#. **Check the output options.** Choose a format, TAB symbology and plotting
   if required. Check the plotting columns and threshold. Leave column
   deletion unchecked unless deliberately removing them from a TAB result.
   Check **Clear log file?** only if the previous log can be deleted.
#. **Run.** Click **Ok**, then choose an existing destination folder and a
   filename beginning with a letter. Use a fresh name to retain old results.
#. **Review.** Answer the close-form prompt, then read the entire log in
   Notepad and check the files, counts, attributes and geometry. A completion
   message alone does not confirm success.

:numref:`figSelectorForm` identifies the controls.
:ref:`output_formats`, :ref:`saved_queries` and :ref:`run_logs` provide details.

.. index::
   single: Worked example
   pair: Queries; recent birds example
   pair: Outputs; example filenames

.. _worked_example:

Worked example: recent bird records
===================================

This illustrative example uses the :ref:`appendix profile <example_xml>`.
An administrator must adapt its paths, configure the file DSN and procedures,
and prepare ``dbo.Example_Spp`` with these columns:

* ``TaxonName`` and ``TaxonGroup`` as text.
* ``RecYear`` as a numeric year.
* ``Easting`` and ``Northing`` as numeric British National Grid coordinates.
* ``GRPrecision`` as a positive grid size in metres.

Include some Birds records from 2020 onwards, with valid coordinates and
sizes such as 100 or 1,000. Ensure the database metadata supports selecting
these attributes as a non-spatial subset. The guide does not supply this
database, example records or a workspace.

Choose ``DefaultProfile`` if prompted and select **Example_Spp**. Enter:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Field
     - Value
   * - Columns
     - ``TaxonName, TaxonGroup, RecYear, Easting, Northing, GRPrecision``
   * - Where
     - ``TaxonGroup = 'Birds' AND RecYear >= 2020``
   * - Group By
     - Leave blank.
   * - Order By
     - ``TaxonName, RecYear DESC``

Choose **MapInfo (.tab) file**, **Standard Points/Polygons** symbology and
check **Plot data spatially?**. Set **X Col** to Easting, **Y Col** to
Northing, **Size Col** to GRPrecision, and **Max Points Precision** to 100.
Leave **Delete columns after plotting?** unchecked. For a fresh example,
check **Clear log file?** after preserving any previous log you need.

Click **Ok** and save as ``D:\DataSelector\Extracts\Birds_2020.tab`` in an
existing test folder. With the example paths unchanged, expect:

.. list-table::
   :header-rows: 1
   :widths: 44 56

   * - Output
     - Expected result
   * - Birds_2020.tab and companion files
     - Matching rows, all six selected attributes and plotted geometry.
       Sizes up to 100 become points at the grid origin; larger sizes
       become squares. The configured point/region styles are applied.
   * - D:\DataSelector\Logs\DataSelector_<user>.log
     - The run messages, including the selected-row count and plotting steps.

The table appears in an existing front map or a new map. Review the log
and check the plotted locations and counts against the prepared data;
no fixed count is promised. No SHP, CSV or TXT output is requested by
these choices.

Optionally click **Save** before closing the form to create
``D:\DataSelector\Queries\Recent_Birds.qsf``. Loading it later restores the
four SQL fields; reselect the source and check all other options.
