# PECL Windows Dependency Installer (vcpkg)

On top of supported pecl library dependencies (https://windows.php.net/downloads/php-sdk/deps/). If there is a missing dependency, then this action would be of help.

can be used in conjunction with https://github.com/derickr/setup-php-sdk

## Inputs


| Inputs          | Required/Optional | Description                                                                 | Default Value     |
|-----------------|-------------------|-----------------------------------------------------------------------------|-------------------|
| cache-hit       | Required          | Whether the cache was hit.                                                  | N/A               |
| libraries       | Required          | Libraries to install with vcpkg, comma-separated.                           | N/A               |
| target-prefix   | Optional          | Target directory prefix where the dependencies will be installed.           | `./../deps`       |
| vcpkg-version   | Optional          | Specific version of vcpkg to use.                                           | `latest`          |

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
          key: vcpkg-${{ hashFiles('**/vcpkg.json') }}
          restore-keys: |
            vcpkg-

      - name: Install dependencies with vcpkg
        uses: omars44/pecl-windows-deps-installer@master
        with:
          cache-hit: ${{ steps.cache-vcpkg.outputs.cache-hit }}
          libraries: 'zlib,libxml2'
          arch: 'x64'
```


