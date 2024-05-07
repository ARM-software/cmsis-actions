# ARM-software/cmsis-actions

Collection of reusable GitHub actions used for embedded Arm and CMSIS projects.

## Action: armlm

This action can be used to activate an
[Arm user based license](https://developer.arm.com/documentation/102516/1-2/Activate-and-deactivate-your-product-license/Activate-your-product-using-a-license-server?lang=en)
using `armlm` license manager.

```yaml
- name: Activate Arm license
  uses: ARM-software/cmsis-actions/armlm@v1
  with:
    server: <custom license server>
    product: KEMDK-COM0
    code: <personal product code>
```

By default, the action activates the free
[Keil MDK v6 Community license](https://learn.arm.com/learning-paths/microcontrollers/vcpkg-tool-installation/licenseactivation/).

## Action: vcpkg

This action can be used to activate a development environment based on a `vcpkg-configuration.json` file as described
in the [CMSIS-Toolbox installation guide](https://github.com/Open-CMSIS-Pack/cmsis-toolbox/blob/main/docs/installation.md#vcpkg---setup-using-cli).

```yaml
- name: Setup vcpkg environment
  uses: ARM-software/cmsis-actions/vcpkg@v1
  with:
    config: "./vcpkg-configuration.json"
    vcpkg-downloads: "${{ github.workspace }}/.vcpkg/downloads"
    cache: "-"
```

The activated environment is preserved into `$GITHUB_PATH` and `$GITHUB_ENV` so that it can be used by subsequent steps.
