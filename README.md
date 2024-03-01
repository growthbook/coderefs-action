# GrowthBook Code References with GitHub Actions

This GitHub Action can be used with GrowthBook to scan your codebase for feature flag references and have them surfaced in your GrowthBook UI.

## Configuration

Create a new Actions workflow in your selected GitHub repository (e.g. `code-references.yml`) in the `.github/workflows` directory of your repository. Under "Edit new file", paste the following code:

```yaml
name: Find feature flag code references
on:
    pull_request:
        branches:
            - main
jobs:
    codeRefs:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout
              uses: actions/checkout@v4
              with:
                  # This value must be set if the lookback configuration option is
                  # defined for find-code-refs. Read more:
                  # https://github.com/growthbook/gb-find-code-refs#searching-for-unused-flags-extinctions
                  fetch-depth: 11
            - name: GrowthBook Code References
              uses: growthbook/coderefs-action@2.11.5-14
              with:
                  apiKey: ${{ secrets.GB_API_TOKEN }}
                  apiHost: ${{ secrets.GB_API_HOST }}
```

<!-- action-docs-inputs -->

## Inputs

| parameter    | description                                                                                                                                                                                                                                                                                                                                                                                                                     | required | default         |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | --------------- |
| apiKey       | Your GrowthBook API Key.                                                                                                                                                                                                                                                                                                                                                                                                        | `true`   |                 |
| apiHost      | The URL to your GrowthBook API Host (for cloud users, this is `https://api.growthbook.io`).                                                                                                                                                                                                                                                                                                                                     | `true`   |                 |
| contextLines | The number of context lines above and below a code reference for the job to send to GrowthBook. By default, the flag finder will not send any context lines to GrowthBook. If < 0, it will send no source code to GrowthBook. If 0, it will send only the lines containing flag references. If > 0, it will send that number of context lines above and below the flag reference. You may provide a maximum of 5 context lines. | `false`  | 2               |
| debug        | Enable verbose debug logging.                                                                                                                                                                                                                                                                                                                                                                                                   | `false`  | false           |
| lookback     | Set the number of commits to search in history for whether you removed a feature flag from code. You may set to 0 to disable this feature. Setting this option to a high value will increase search time.                                                                                                                                                                                                                       | `false`  | 10              |

<!-- action-docs-inputs -->
