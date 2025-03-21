# Changelog

## Unreleased

## 3.5.0 (2021-06-03)

- Transform generated folder into QML Module when building Javascript templates for the Qt platform. ([bug 1707896](https://bugzilla.mozilla.org/show_bug.cgi?id=1707896)
    - Import the Glean QML module from inside each generated file, removing the requirement to import Glean before importing any of the generated files;
    - Prodive a `qmldir` file exposing all generated files;
    - Drop the `namespace` option for Javascript templates;
    - Add a new `version` option for Javascript templates, required when building for Qt, which expected the Glean QML module version.

## 3.4.0 (2021-05-28)

- Add missing import for Kotlin code ([#339](https://github.com/mozilla/glean_parser/pull/341))
- Use a plain Kotlin type in the generated interface implementation ([#339](https://github.com/mozilla/glean_parser/pull/341))
- Generate additional generics for event metrics ([#339](https://github.com/mozilla/glean_parser/pull/341))
- For Kotlin skip generating `GleanBuildInfo.kt` when requested (with `with_buildinfo=false`) ([#341](https://github.com/mozilla/glean_parser/pull/341))

## 3.3.2 (2021-05-18)

- Fix another bug in the Swift code generation when generating extra keys ([#334](https://github.com/mozilla/glean_parser/pull/334))

## 3.3.1 (2021-05-18)

- Fix Swift code generation bug for pings ([#333](https://github.com/mozilla/glean_parser/pull/333))

## 3.3.0 (2021-05-18)

- Generate new event API construct ([#321](https://github.com/mozilla/glean_parser/pull/321))

## 3.2.0 (2021-04-28)

- Add option to add extra introductory text to generated markdown ([#298](https://github.com/mozilla/glean_parser/pull/298))
- Add support for Qt in Javascript templates ([bug 1706252](https://bugzilla.mozilla.org/show_bug.cgi?id=1706252))
  - Javascript templates will now accept the `platform` option. If this option is set to `qt`
  the generated templates will be Qt compatible. Default value is `webext`.

## 3.1.2 (2021-04-21)

- BUGFIX: Remove the "DO NOT COMMIT" notice from the documentation.

## 3.1.1 (2021-04-19)

- Recommend to not commit as well as to not edit the generated files. ([bug 1706042](https://bugzilla.mozilla.org/show_bug.cgi?id=1706042))
- BUGFIX: Include import statement for labeled metric subtypes in Javascript and Typescript templates.

## 3.1.0 (2021-04-16)

- Add support for labeled metric types in Javascript and Typescript templates.

## 3.0.0 (2021-04-13)

- Raise limit on number of statically-defined lables to 100. ([bug 1702263](https://bugzilla.mozilla.org/show_bug.cgi?id=1702263))
- BUGFIX: Version 2.0.0 of the schema now allows the "special" `glean_.*` ping names for Glean-internal use again.
- Remove support for JWE metric types.

## 2.5.0 (2021-02-23)

- Add parser and object model support for `rate` metric type. ([bug 1645166](https://bugzilla.mozilla.org/show_bug.cgi?id=1645166))
- Add parser and object model support for telemetry_mirror property. ([bug 1685406](https://bugzilla.mozilla.org/show_bug.cgi?id=1685406))
- Update the Javascript template to match Glean.js expectations. ([bug 1693516](https://bugzilla.mozilla.org/show_bug.cgi?id=1693516))
  - Glean.js has updated it's export strategy. It will now export each metric type as an independent module;
  - Glean.js has dropped support for non ES6 modules.
- Add support for generating Typescript code. ([bug 1692157](https://bugzilla.mozilla.org/show_bug.cgi?id=1692157))
  - The templates added generate metrics and pings code for Glean.js.

## 2.4.0 (2021-02-18)

- **Experimental:** `glean_parser` has a new subcommand `coverage` to convert raw coverage reports
  into something consumable by coverage tools, such as codecov.io
- The path to the file that each metric is defined in is now stored on the
  `Metric` object in `defined_in["filepath"]`.

## 2.3.0 (2021-02-17)

- Leverage the `glean_namespace` to provide correct import when building for Javascript.

## 2.2.0 (2021-02-11)

- The Kotlin generator now generates static build information that can be passed
  into `Glean.initialize` to avoid calling the package manager at runtime.

## 2.1.0 (2021-02-10)

- Add support for generating Javascript code.
  - The templates added generate metrics and pings code for Glean.js.

## 2.0.0 (2021-02-05)

- New versions 2.0.0 of the `metrics.yaml` and `pings.yaml` schemas now ship
  with `glean_parser`. These schemas are different from version 1.0.0 in the
  following ways:

  - Bugs must be specified as URLs. Bug numbers are disallowed.
  - The legacy ping names containing underscores are no longer allowed. These
    included `deletion_request`, `bookmarks_sync`, `history_sync`,
    `session_end`, `all_pings`, `glean_*`). In these cases, the `_` should be
    replaced with `-`.

  To upgrade your app or library to use the new schema, replace the version in
  the `$schema` value with `2-0-0`.

- **Breaking change:** It is now an error to use bug numbers (rather than URLs)
  in ping definitions.

- Add the line number that metrics and pings were originally defined in the yaml
  files.

## 1.29.1 (2020-12-17)

- BUGFIX: Linter output can now be redirected correctly (1675771).

## 1.29.0 (2020-10-07)

- **Breaking change:** `glean_parser` will now return an error code when any of
  the input files do not exist (unless the `--allow-missing-files` flag is
  passed).
- Generated code now includes a comment next to each metric containing the name
  of the metric in its original `snake_case` form.
- When metrics don't provide a `unit` parameter, it is not included in the
  output (as provided by probe-scraper).

## 1.28.6 (2020-09-24)

- BUGFIX: Ensure Kotlin arguments are deterministically ordered

## 1.28.5 (2020-09-14)

- Fix deploy step to update pip before deploying to pypi.

## 1.28.4 (2020-09-14)

- The `SUPERFLUOUS_NO_LINT` warning has been removed from the glinter.
  It likely did more harm than good, and makes it hard to make
  `metrics.yaml` files that pass across different versions of
  `glean_parser`.
- Expired metrics will now produce a linter warning, `EXPIRED_METRIC`.
- Expiry dates that are more than 730 days (\~2 years) in the future
  will produce a linter warning, `EXPIRATION_DATE_TOO_FAR`.
- Allow using the Quantity metric type outside of Gecko.
- New parser configs `custom_is_expired` and `custom_validate_expires`
  added. These are both functions that take the `expires` value of the
  metric and return a bool. (See `Metric.is_expired` and
  `Metric.validate_expires`). These will allow FOG to provide custom
  validation for its version-based `expires` values.

## 1.28.3 (2020-07-28)

- BUGFIX: Support HashSet and Dictionary in the C\## generated code.

## 1.28.2 (2020-07-28)

- BUGFIX: Generate valid C\## code when using Labeled metric types.

## 1.28.1 (2020-07-24)

- BUGFIX: Add missing column to correctly render markdown tables in generated
  documentation.

## 1.28.0 (2020-07-23)

- **Breaking change:** The internal ping `deletion-request` was misnamed in
  pings.py causing the linter to not allow use of the correctly named ping for
  adding legacy ids to. Consuming apps will need to update their metrics.yaml if
  they are using `deletion_request` in any `send_in_pings` to `deletion-request`
  after updating.

## 1.27.0 (2020-07-21)

- Rename the `data_category` field to `data_sensitivity` to be clearer.

## 1.26.0 (2020-07-21)

- Add support for JWE metric types.
- Add a `data_sensitivity` field to all metrics for specifying the type of data
  collected in the field.

## 1.25.0 (2020-07-17)

- Add support for generating C\## code.
- BUGFIX: The memory unit is now correctly passed to the MemoryDistribution
  metric type in Swift.

## 1.24.0 (2020-06-30)

- BUGFIX: look for metrics in send\_if\_empty pings. Metrics for these kinds of
  pings were being ignored.

## 1.23.0 (2020-06-27)

- Support for Python 3.5 has been dropped.
- BUGFIX: The ordering of event extra keys will now match with their enum,
  fixing a serious bug where keys of extras may not match the correct values in
  the data payload. See <https://bugzilla.mozilla.org/show_bug.cgi?id=1648768>.

## 1.22.0 (2020-05-28)

- **Breaking change:** (Swift only) Combine all metrics and pings into a single
  generated file `Metrics.swift`.

## 1.21.0 (2020-05-25)

- `glinter` messages have been improved with more details and to be more
  actionable.
- A maximum of 10 `extra_keys` is now enforced for `event` metric types.
- BUGFIX: the `Lifetime` enum values now match the values of the implementation
  in mozilla/glean.

## 1.20.4 (2020-05-07)

- BUGFIX: yamllint errors are now reported using the correct file name.

## 1.20.3 (2020-05-06)

- Support for using `timing_distribution`'s `time_unit` parameter to control
  the range of acceptable values is documented. The default unit for this use
  case is `nanosecond` to avoid creating a breaking change. See [bug
  1630997](https://bugzilla.mozilla.org/show_bug.cgi?id=1630997) for more
  information.

## 1.20.2 (2020-04-24)

- Dependencies that depend on the version of Python being used are now specified
  using the [Declaring platform specific dependencies syntax in
  setuptools](https://setuptools.readthedocs.io/en/latest/setuptools.html##declaring-platform-specific-dependencies).
  This means that more recent versions of dependencies are likely to be
  installed on Python 3.6 and later, and unnecessary backport libraries won't
  be installed on more recent Python versions.

## 1.20.1 (2020-04-21)

- The minimum version of the runtime dependencies has been lowered to increase
  compatibility with other tools. These minimum versions are now tested in CI,
  in addition to testing the latest versions of the dependencies that was
  already happening in CI.

## 1.20.0 (2020-04-15)

- **Breaking change:** glinter errors found during the `translate` command will
  now return an error code. glinter warnings will be displayed, but not return
  an error code.
- `glean_parser` now produces a linter warning when `user` lifetime metrics are
  set to expire. See [bug
  1604854](https://bugzilla.mozilla.org/show_bug.cgi?id=1604854) for additional
  context.

## 1.19.0 (2020-03-18)

- **Breaking change:** The regular expression used to validate labels is
  stricter and more correct.
- Add more information about pings to markdown documentation:
  - State whether the ping includes client id;
  - Add list of data review links;
  - Add list of related bugs links.
- `glean_parser` now makes it easier to write external translation
  functions for different language targets.
- BUGFIX: `glean_parser` now works on 32-bit Windows.

## 1.18.3 (2020-02-24)

- Dropped the `inflection` dependency.
- Constrained the `zipp` and `MarkupSafe` transitive dependencies to versions
  that support Python 3.5.

## 1.18.2 (2020-02-14)

- BUGFIX: Fix rendering of first element of reason list.

## 1.18.1 (2020-02-14)

- BUGFIX: Reason codes are displayed in markdown output for built-in
  pings as well.
- BUGFIX: Reason descriptions are indented correctly in markdown
  output.
- BUGFIX: To avoid a compiler error, the `@JvmName` annotation isn't
  added to private members.

## 1.18.0 (2020-02-13)

- **Breaking Change (Java API)** Have the metrics names in Java match the names
  in Kotlin. See [Bug
  1588060](https://bugzilla.mozilla.org/show_bug.cgi?id=1588060).
- The reasons a ping are sent are now included in the generated markdown
  documentation.

## 1.17.3 (2020-02-05)

- BUGFIX: The version of Jinja2 now specifies < 3.0, since that version no
  longer supports Python 3.5.

## 1.17.2 (2020-02-05)

- BUGFIX: Fixes an import error in generated Kotlin code.

## 1.17.1 (2020-02-05)

- BUGFIX: Generated Swift code now includes `import Glean`, unless generating
  for a Glean-internal build.

## 1.17.0 (2020-02-03)

- Remove default schema URL from `validate_ping`
- Make `schema` argument required for CLI
- BUGFIX: Avoid default import in Swift code for Glean itself
- BUGFIX: Restore order of fields in generated Swift code

## 1.16.0 (2020-01-15)

- Support for `reason` codes on pings was added.

## 1.15.6 (2020-02-06)

- BUGFIX: The version of Jinja2 now specifies < 3.0, since that version no
  longer supports Python 3.5 (backported from 1.17.3).

## 1.15.5 (2019-12-19)

- BUGFIX: Also allow the legacy name `all_pings` for `send_in_pings` parameter
  on metrics

## 1.15.4 (2019-12-19)

- BUGFIX: Also allow the legacy name `all_pings`

## 1.15.3 (2019-12-13)

- Add project title to markdown template.
- Remove "Sorry about that" from markdown template.
- BUGFIX: Replace dashes in variable names to force proper naming

## 1.15.2 (2019-12-12)

- BUGFIX: Use a pure Python library for iso8601 so there is no compilation
  required.

## 1.15.1 (2019-12-12)

- BUGFIX: Add some additional ping names to the non-kebab-case allow list.

## 1.15.0 (2019-12-12)

- Restrict new pings names to be kebab-case and change `all_pings` to
  `all-pings`

## 1.14.0 (2019-12-06)

- `glean_parser` now supports Python versions 3.5, 3.6, 3.7 and 3.8.

## 1.13.0 (2019-12-04)

- The `translate` command will no longer clear extra files in the output
  directory.
- BUGFIX: Ensure all newlines in comments are prefixed with comment markers
- BUGFIX: Escape Swift keywords in variable names in generated code
- Generate documentation for pings that are sent if empty

## 1.12.0 (2019-11-27)

- Reserve the `deletion_request` ping name
- Added a new flag `send_if_empty` for pings

## 1.11.0 (2019-11-13)

- The `glinter` command now performs `yamllint` validation on registry files.

## 1.10.0 (2019-11-11)

- The Kotlin linter `detekt` is now run during CI, and for local
  testing if installed.
- Python 3.8 is now tested in CI (in addition to Python 3.7). Using
  `tox` for this doesn't work in modern versions of CircleCI, so the
  `tox` configuration has been removed.
- `yamllint` has been added to test the YAML files on CI.
- ⚠ Metric types that don't yet have implementations in glean-core
  have been removed. This includes `enumeration`, `rate`, `usage`, and
  `use_counter`, as well as many labeled metrics that don't exist.

## 1.9.5 (2019-10-22)

- Allow a Swift lint for generated code
- New lint: Restrict what metric can go into the `baseline` ping
- New lint: Warn for slight misspellings in ping names
- BUGFIX: change Labeled types labels from lists to sets.

## 1.9.4 (2019-10-16)

- Use lists instead of sets in Labeled types labels to ensure that the order of
  the labels passed to the `metrics.yaml` is kept.
- `glinter` will now check for duplicate labels and error if there are any.

## 1.9.3 (2019-10-09)

- Add labels from Labeled types to the Extra column in the Markdown template.

## 1.9.2 (2019-10-08)

- BUGFIX: Don't call `is_internal_metric` on `Ping` objects.

## 1.9.1 (2019-10-07)

- Don't include Glean internal metrics in the generated markdown.

## 1.9.0 (2019-10-04)

- Glinter now warns when bug numbers (rather than URLs) are used.
- BUGFIX: add `HistogramType` and `MemoryUnit` imports in Kotlin generated code.

## 1.8.4 (2019-10-02)

- Removed unsupported labeled metric types.

## 1.8.3 (2019-10-02)

- Fix indentation for generated Swift code

## 1.8.2 (2019-10-01)

- Created labeled metrics and events in Swift code and wrap it in a
  configured namespace

## 1.8.1 (2019-09-27)

- BUGFIX: `memory_unit` is now passed to the Kotlin generator.

## 1.8.0 (2019-09-26)

- A new parser config, `do_not_disable_expired`, was added to turn off the
  feature that expired metrics are automatically disabled. This is useful if you
  want to retain the disabled value that is explicitly in the `metrics.yaml`
  file.
- `glinter` will now report about superfluous `no_lint` entries.

## 1.7.0 (2019-09-24)

- A `glinter` tool is now included to find common mistakes in metric naming
  and setup. This check is run during `translate` and warnings will be
  displayed. ⚠ These warnings will be treated as errors in a future revision.

## 1.6.1 (2019-09-17)

- BUGFIX: `GleanGeckoMetricsMapping` must include `LabeledMetricType`
  and `CounterMetricType`.

## 1.6.0 (2019-09-17)

- NEW: Support for outputting metrics in Swift.
- BUGFIX: Provides a helpful error message when `geckoview_datapoint` is used on
  an metric type that doesn't support GeckoView exfiltration.
- Generate a lookup table for Gecko categorical histograms in
  `GleanGeckoMetricsMapping`.
- Introduce a 'Swift' output generator.

## 1.4.1 (2019-08-28)

- Documentation only.

## 1.4.0 (2019-08-27)

- Added support for generating markdown documentation from `metrics.yaml` files.

## 1.3.0 (2019-08-22)

- `quantity` metric type has been added.

## 1.2.1 (2019-08-13)

- BUGFIX: `includeClientId` was not being output for PingType.

## 1.2.0 (2019-08-13)

- `memory_distribution` metric type has been added.
- `custom_distribution` metric type has been added.
- `labeled_timespan` is no longer an allowed metric type.

## 1.1.0 (2019-08-05)

-   Add a special `all_pings` value to `send_in_pings`.

## 1.0.0 (2019-07-29)

- First release to start following strict semver.

## 0.1.0 (2018-10-15)

- First release on PyPI.
