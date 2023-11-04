# PECL Windows Dependency Installer (vcpkg)

This action installs Windows dependencies using vcpkg for PECL extensions.

used in conjunction with https://github.com/derickr/setup-php-sdk

## Inputs

### `cache-hit`

**Required** Whether the cache was hit.

### `libraries`

**Required** Libraries to install with vcpkg, comma-separated.

### `target-prefix`

**Optional** Target directory prefix where the dependencies will be installed. Default `".\..\deps"`

### `vcpkg-version`

**Optional** Specific version of vcpkg to use. Default `"latest"`.

## Example usage

```yaml
jobs:
  build:
    runs-on: windows-latest
    steps:
      - name: Checkout the code
        uses: actions/checkout@v2

      - name: Cache vcpkg and libraries
        id: cache-vcpkg
        uses: actions/cache@v2
        with:
          path: |
            vcpkg
            vcpkg/installed
          key: vcpkg-${{ runner.os }}-${{ hashFiles('**/vcpkg.json') }}
          restore-keys: |
            vcpkg-${{ runner.os }}-

      - name: Install dependencies with vcpkg
        uses: omars44/pecl-windows-deps-installer-action@v1
        with:
          cache-hit: ${{ steps.cache-vcpkg.outputs.cache-hit }}
          libraries: 'zlib,libxml2'
          arch: 'x64' # or 'x86'
          target-prefix: '../deps' # optional
          
```


