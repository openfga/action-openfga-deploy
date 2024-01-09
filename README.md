# OpenFGA Github Action - Deploy

This action can be used to deploy your authorization model to an OpenFGA store.

## Parameter

| Parameter  | Required/Optional  | Description   |
|----------|--------------|--------------|
| `api-url` | Required | The URL to your OpenFGA server     |
| `api-token` | Required | The token used to when preshared keys are used to authenticate the OpenFGA server     |
| `store-id` | Required | The store to which the model should be deployed     |
| `model-file-path` | Optional | The path to your model file relative to the root of your project     |
| `model` | Optional | The model value if file is not used, Ensure it is character escaped.     |
| `format` | Optional | The format of the authorization model     |

## Example

```yaml
name: Deploy Action

on:
  workflow_dispatch:

jobs:
  test:
    name: Deploy Authorization Model
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: jaxxstorm/action-install-gh-release@v1.10.0
        with:
          repo: openfga/openfga
          cache: enable
      - name: Run a local OpenFGA Server
        shell: bash
        ## The key used here is for demo purpose
        run: |
          openfga --authn-method preshared --authn-preshared-keys key1 run&
      - name: Create a store
        shell: bash
        run: | 
          curl -X POST http://localhost:8080/stores  -H "content-type: application/json" -H "Authorization: Bearer key1" -d '{"name": "FGA Demo Store"}' > id.json
      - name: Get store ID
        shell: bash
        run: |
          echo "STORE_ID=$(jq -r '.id' id.json)" >> $GITHUB_ENV
      - name: Deploy using a file
        uses: openfga/action-openfga-deploy@v0.1
        with:
          api-url: http://localhost:8080
          api-token: key1
          store-id: ${{ env.STORE_ID }}
          model-file-path: ./example/model.fga
      - name: Deploy using a string
        uses: openfga/action-openfga-deploy@v0.1
        with:
          api-url: http://localhost:8080
          api-token: key1
          store-id: ${{ env.STORE_ID }}
          format: json
          model: '{\"schema_version\":\"1.1\",\"type_definitions\":\[\{\"type\":\"user\"\},\{\"type\":\"document\",\"relations\":\{\"reader\":\{\"this\":\{\}\},\"writer\":\{\"this\":\{\}\},\"owner\":\{\"this\":\{\}\}\},\"metadata\":\{\"relations\":\{\"reader\":\{\"directly_related_user_types\":\[\{\"type\":\"user\"\}\]\},\"writer\":\{\"directly_related_user_types\":\[\{\"type\":\"user\"\}\]\},\"owner\":\{\"directly_related_user_types\":\[\{\"type\":\"user\"\}\]\}\}\}\}\]\}'
```
