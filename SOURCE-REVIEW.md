# DataSelector for MapInfo source review

Reviewed on 19 October 2026 against the clean local checkout at `5d4bf20`
(11 February 2022), whose source reports v1.0.24. This is a static review:
MapBasic compilation, MapInfo execution and the deployed SQL procedures
were not tested. No tool-source changes have been made. Source links below
refer to the reviewed commit, so the evidence is stable.

## Findings

### 1. Failed or cancelled selections still report completion; failed SHP exports can lose their intermediate TAB

**High priority.** `SelectSpecies` has no success result. Its early exits and
error handler return to `ProcessSelection`, which unconditionally records
`Process completed!` and displays the ordinary completion prompt. The SHP
translator is also a Sub: after it logs a translation failure and returns,
the caller continues to the non-TAB cleanup and drops the intermediate table.
A failed export can therefore leave no shapefile and no retained TAB while
still showing completion.

Evidence: [selection call and unconditional completion](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L1773),
[translation call](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L2255),
[intermediate-table removal](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L2394),
[translator failure exits](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/TranslateToSHP.mb#L112).

Suggested correction: return an explicit outcome from selection/translation,
verify the expected exported components, and retain the TAB on failure. Report
completion according to that outcome. This does not require changing how log
messages are written.

### 2. External-process launch failures can be treated as success

**High priority.** `ExecuteAndWait` overwrites return codes from
`CreateProcessA`, `WaitForSingleObject` and `GetExitCodeProcess` without checking
them. Its error handler returns `FALSE` (zero), while translation treats only
a result greater than zero as failure. A missing/unlaunchable FME or IMUT can
therefore avoid the intended failure branch. `SetUTDirectory` can also report
failure and return without stopping its caller.

Evidence: [process launch and result handling](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/GenFunctions.mb#L135),
[translator directory and process use](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/TranslateToSHP.mb#L50).

Suggested correction: check each API result, return a distinct failure result
for launch/wait errors, propagate directory failure and close both process
and thread handles. Test a deliberately missing translator executable.

### 3. Cancelled subsets and error paths bypass SQL cleanup

**Medium priority.** The procedure creates a subset before the tool checks
zero rows, RecMax, row width and data size. Those branches exit before
`AFClearSppSubset`, and the general error handler also exits directly. Subset
tables and associated spatial metadata can remain after unsuccessful runs.

Evidence: [zero-row and RecMax exits](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L1892),
[width/size exits](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L1969),
[later cleanup call](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L2078),
[error exit](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L2410).

Suggested correction: use common cleanup that knows whether a subset was
created and clears only the current run's resources. Database-side effects
must be checked against the installed procedures before implementing cleanup.

### 4. User-based subset names do not guarantee concurrent-run isolation

**High priority when runs overlap.** The suffix replaces spaces and full stops
with underscores and truncates the username to fit a 32-character name. A
30-character source table leaves one username character; two users whose
names start with A then share a subset. A 31-character source leaves no user
characters, and a source longer than 31 characters is not shortened at all.
Multiple MapInfo sessions using the same account also share a suffix.

Evidence: [username normalisation](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L510),
[truncation and subset naming](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L1834).

Suggested correction: agree a unique run identifier and valid length handling
with the SQL procedures. Until then, avoid overlapping sessions whose suffixes
collide. This qualifies the old guide's unrestricted concurrency claim.

### 5. Column deletion is not gated on plotting, and the SHP control has no effect

**Medium priority.** `bDeletePlotColumns` is tested in the TAB branch without
also requiring successful plotting. If a user checks deletion, then turns
plotting off, the hidden checkbox can remain checked. The non-plotting branch
sets `sSizeCol` to the default size column, so a mappable TAB can lose that
attribute even though it was not re-plotted. Conversely, SHP translation runs
before this TAB-only deletion block, so the visible deletion option for SHP
does not remove its plotting attributes.

Evidence: [non-plotting size-column assignment and SHP translation](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L2236),
[TAB-only deletion condition](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L2314).

Suggested correction: gate deletion on successful plotting; reset it when
plotting is disabled. Either implement deliberate SHP attribute removal before
translation or hide the option for SHP. The guide documents the present scope.

### 6. Plotting does not validate positive grid size before division

**Medium priority.** Every selected row assigns its size to `Z`, then divides
both coordinates by `Z`. A zero value raises an error, and negative or invalid
sizes can produce invalid geometry or abort processing after partial updates.
Only the form's threshold receives a numeric check; row data is not validated.

Evidence: [threshold check](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L1330),
[unvalidated row-size division](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L2467).

Suggested correction: validate coordinates and a positive size before updating,
with an explicit policy for rejected rows. Restore FastEdit/progress settings
and close plotting queries on error. Check the actual data types returned by
the installed SQL procedures.

### 7. Profile-loading failure does not reliably stop form creation

**Medium priority.** `GetDefaultValues` reads directly into global settings
and can return after a parse/load error without clearing previous values or
returning success. Even the branch that sets `sXMLFile` blank after failing to
create the log folder is followed by unconditional form creation in its
caller. Following a previous successful run, an invalid new profile can leave
partly old settings in use.

Evidence: [profile reading](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L285),
[failure paths](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L448),
[caller proceeds after loading](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/DataSelector.mb#L526).

Suggested correction: load into temporary settings, validate required fields
and return success before adopting them or opening the form. Test switching
from a valid profile to a malformed or incomplete one.

### 8. SHP translation leaves basename arguments unquoted

**Medium priority.** Output names containing spaces pass the tool's filename
checks, but `_BASENAME` and `_FULLBASENAME` are concatenated into the translator
command without quotes. The input and destination paths are quoted, so this
remaining gap can split a basename into multiple command arguments.

Evidence: [translator basename arguments](https://github.com/LERCAutomation/DataSelector-MapInfo/blob/5d4bf201f57210b4c72636c213203ffd7c396f65/MapBasic/TranslateToSHP.mb#L127).

Suggested correction: quote these argument values consistently and test a
basename such as `Recent Birds`. The guide recommends simple underscore names
in the meantime. The translator's exact response still needs a runtime test.

## Additional verification boundaries

- **Build configuration:** the checked-in MBP links `MIMenu32.mbo` and the DEF
  enables the 32-bit includes. A reproducible 64-bit rebuild needs deliberate
  changes to those selections. This may be the intended manual build workflow;
  it is not evidence that the supplied v16 MBX is faulty. `GetFileList` also
  contains `Exit Sub` in a Function; verify compiler acceptance when rebuilding.
- **Database compatibility:** current SQL procedure scripts are absent. The
  historic selection procedure has seven arguments; today's caller passes
  eight. Do not deploy that historic script as the current implementation.
- **Size estimate:** integer multiplication before conversion to kilobytes
  deserves a boundary test, but overflow before the 2 GB guard is unconfirmed
  without verifying MapBasic's expression promotion/runtime behaviour. It is
  not recorded above as a confirmed bug or presented as one in the guide.
- **Geometry and styles:** point-at-grid-origin placement and Point/Region-only
  style updates follow the current source. They are documented behaviour,
  rather than an assertion that the implementation should change.

## Documentation validation

The accompanying guide updates correct the two-file profile model, hidden
plotting defaults, output display/retention, query-file scope, spatial metadata
descriptions and the success-message caveat. They also add routine and admin
workflows, a worked example, SQL text examples, source-based release history,
downloadable XML, indexed settings and a three-image replacement checklist.
HTML/LaTeX source build and link checks are recorded in the task's final report.
The screenshots remain placeholders and the worked example still needs an
end-to-end run with prepared data in MapInfo.
