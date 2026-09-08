# Bundled runtime data guidance

Inherit the root and package guides. This scope exists for shipped runtime assets and their provenance; it is not an
application-data or settings directory.

## Boundary and provenance

- This directory ships application assets, not user state: Xray GeoIP/geosite data, Hysteria MMDB/ACL data, the local
  MapLibre endpoint map, and the bundled font. Settings, subscriptions, caches, and temporary downloads belong elsewhere.
- Preserve upstream licenses, provenance, binary/text formats, filenames, and paths consumed by constants, backends,
  tests, setuptools package data, and Nuitka. Do not incidentally reformat generated ACLs or replace binary assets.
- Markdown files in this directory are repository metadata, not runtime data. Keep top-level and nested Markdown files
  excluded consistently from setuptools package data and Nuitka inclusion while preserving them in the source tree.
- `Deploy.py --download` performs a networked refresh and may rewrite large, time-varying assets. Run it only when that
  mutation is explicitly in scope; review source, checksums, exact changed files, and existing user modifications.

## Local endpoint map

- MapLibre JavaScript/CSS and the host bridge are bundled; the style requests vector tiles and glyphs from
  `tiles.openfreemap.org`. This is not an offline map. Keep executable code local, preserve attribution, and review
  the HTML content-security policy and the widget's attribution-link validation when changing network resources or
  links. Missing tiles/network detail must degrade without crashing the renderer or the application.
- Linux Essentials-only builds deliberately operate without WebEngine; map consumers must retain their non-WebEngine
  fallback. macOS/Windows packaged paths may include WebEngine and must resolve all local resources from the bundle.

## Verification

- Verify the real consuming backend/widget, source and packaged path resolution, package-data/Nuitka inclusion,
  integrity and failure behavior, and license presence. Tests use fixtures or mocked downloads, never live asset
  refreshes. `tests/test_endpoint_info.py` and `tests/test_public_api.py` cover map/resource consumers; release
  artifacts require their own inclusion checks. Revalidate provenance/network claims when an asset provider or
  loader changes.
