# Install OpenTofu

A composite GitHub Action that:

* Installs a specified version and architecture of [OpenTofu](https://opentofu.org)
* Caches the `.deb` installer to reduce download time
* Verifies SHA256 checksums by default for enhanced security

Supports installation on **Ubuntu-based runners** using official OpenTofu `.deb` packages.

---

## Usage

```yaml
- name: Install OpenTofu
  uses: cybergavin/install-opentofu@v1
  with:
    version: '1.9.0'
    architecture: amd64
    disable-checksum-validation: false
```

📝 **Note**: All inputs are optional. Defaults will be used if not provided.

---

## Inputs

| Name                          | Required | Default   | Description                                          |
| ----------------------------- | -------- | --------- | ---------------------------------------------------- |
| `version`                     | ❌        | `'1.9.0'` | OpenTofu version to install.                         |
| `architecture`                | ❌        | `'amd64'` | Target architecture.    |
| `disable-checksum-validation` | ❌        | `'false'` | Skip SHA256 checksum verification (not recommended). |

---

## Behavior

### Caching

* Caches the downloaded `.deb` package under `~/.cache/opentofu`
* Cache key is based on `version` and `architecture`

### Installation

* Downloads `.deb` and checksum from official releases
* Validates SHA256 checksum unless explicitly disabled
* Installs the `.deb` package using `dpkg`
* Verifies that OpenTofu was installed by checking its version

---

## Outputs

| Name                | Description                                                 |
| ------------------- | ----------------------------------------------------------- |
| `cache-hit`         | Whether the installer was found in the GitHub Actions cache |
| `installed_version` | The verified installed version of OpenTofu                  |
| `checksum_failure`  | `true` if checksum validation failed, otherwise `false`     |

---

## Security

* Uses **pinned versions** of `actions/cache` to ensure deterministic builds
* SHA256 checksum validation enabled by default for download integrity