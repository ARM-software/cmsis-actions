# ARM-software/cmsis-actions

<!-- markdownlint-disable MD013 -->
Collection of reusable [GitHub actions](https://github.com/features/actions) to automate your workflows
for embedded projects with Arm processors and/or CMSIS components.

## Action: armlm

This action activates an
[Arm user based license](https://developer.arm.com/documentation/102516/1-2/Activate-and-deactivate-your-product-license/Activate-your-product-using-a-license-server)
using the `armlm` license manager.

```yaml
- name: Activate Arm license
  uses: ARM-software/cmsis-actions/armlm@v1
  with:
    code: <personal product code>
    server: <custom license server>
    product: KEMDK-COM0
```

inputs     | default value | description
-----------|---------------|--------------
`code:`    | n/a           | License activation code (takes precedence over server/product).
`server:`  | n/a           | Get a product license from the server specified by a URL.
`product:` | KEMDK-COM0    | Get a license for the specified product from the server (default MDK Community Edition).

The action can be either called with a
[license activation `code:`](https://developer.arm.com/documentation/102516/1-2/Activate-and-deactivate-your-product-license/Activate-your-product-using-an-activation-code)
or a [license `server:`  with `product:` specification](https://developer.arm.com/documentation/102516/1-2/Activate-and-deactivate-your-product-license/Activate-your-product-using-a-license-server).
Without any `inputs`, it activates a license for the [Keil MDK - Community Edition](https://www.keil.arm.com/keil-mdk/) which is a free-to-use,
non-commercial license which can be used for evaluation.

### Usage examples armlm

- [github.com/Arm-Examples/AVH_CI_Template](https://github.com/Arm-Examples/AVH_CI_Template/blob/main/.github/workflows/basic.yml) actives a
  [Keil MDK - Community Edition](https://www.keil.arm.com/keil-mdk/) as it uses no inputs.

- [github.com/ARM-software/CMSIS_6](https://github.com/ARM-software/CMSIS_6/blob/main/.github/workflows/corevalidation.yml) actives a commerical product using a license activation code
  that is provided via a [GitHub secret](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions).

## Action: vcpkg

This action installs a development environment with the tools described in the file `vcpkg-configuration.json`.
The tools are downloaded from the [Arm Tools Artifactory](https://artifacts.tools.arm.com).
Refer to [CMSIS-Toolbox - Installation - vcpkg - Setup using CLI  installation guide](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/blob/main/docs/installation.md#vcpkg---setup-using-cli)
for more information.

```yaml
- name: Setup vcpkg environment
  uses: ARM-software/cmsis-actions/vcpkg@v1
  with:
    config: "./vcpkg-configuration.json"
    vcpkg-downloads: "${{ github.workspace }}/.vcpkg/downloads"
    cache: "-"
```

inputs              | default value                              | description
--------------------|--------------------------------------------|--------------
`config:`           | "./vcpkg-configuration.json"               | Path and filename of the vcpkg configuration file.
`vcpkg-downloads:`  | "${{ github.workspace }}/.vcpkg/downloads" | Download folder for vcpkg tool artifacts
`cache:`            | "-" (no cache used)                        | [Create a cache with a key](https://github.com/actions/cache?tab=readme-ov-file#creating-a-cache-key) to store tool artifacts.

The activated environment is preserved into `$GITHUB_PATH` and `$GITHUB_ENV` so that it can be used by subsequent steps. The cache is based on
[actions/cache](https://github.com/actions/cache) and improves workflow execution time.

### Usage examples vcpkg

- [github.com/Arm-Examples/AVH_CI_Template](https://github.com/Arm-Examples/AVH_CI_Template/blob/main/.github/workflows/basic.yml) uses `vcpkg` to install the required tools.

- [https://github.com/ARM-software/CMSIS-FreeRTOS](https://github.com/ARM-software/CMSIS-FreeRTOS/blob/main/.github/workflows/cmsis_rv2.yaml)
  uses `vcpkg` with a cache. As there are multiple test executions, the workflow execution speed is improved.
