# Icon source guidance

Inherit repository-wide rules from the root `AGENTS.md`. This top-level scope governs icon source/provenance and the
resource-manifest contract; it does not govern general UI layout.

- Reuse an existing semantic icon before introducing a new asset. SVG sources stay compact vectors without scripts,
  remote resources, embedded rasters, editor metadata, or hard-coded page backgrounds.
- Follow the established monochrome/current-color convention so the shared `AppQ*` presentation layer can tint icons.
  Add a theme-specific variant only when semantic tinting cannot express the design, and do not rely on color alone.
- Preserve license/provenance and the `Resources.qrc` alias contract. Any add, removal, rename, or alias change
  updates all consumers and the manifest, then regenerates `Furious/Frozenlib/AppResources.py` with the selected
  environment's `pyside6-rcc Resources.qrc -o Furious/Frozenlib/AppResources.py`. Never hand-edit generated resource
  code; inspect compiler-version churn separately from the intended alias/asset change.
- Treat the alias as the application-facing identity and the source path as an implementation detail. Search both before
  replacement so an apparently unused file is not removed while still generated or consumed through an alias.
- Verify alias uniqueness and source/package resolution, then inspect the actual control or tray use under both
  themes, high DPI, relevant sizes, disabled/selected states, and platform packaging where applicable. Deployment
  icons also have direct filesystem consumers in `Deploy.py`; a resource alias search alone cannot prove a PNG is
  unused. Revalidate this guide against `Resources.qrc`, `Furious/Qt/QtGui.py`, and deployment consumers when asset
  policy changes.
