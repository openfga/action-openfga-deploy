# OpenFGA Github Action - Deploy
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fopenfga%2Faction-openfga-deploy.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fopenfga%2Faction-openfga-deploy?ref=badge_shield)


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
  deploy:
    name: Deploy Authorization Model
    runs-on: ubuntu-latest
    steps:
      - name: Deploy using a file
        uses: openfga/action-openfga-deploy@v0.1
        with:
          api-url: http://localhost:8080
          api-token: ${{ secrets.KEY }}
          store-id: ${{ env.STORE_ID }}
          model-file-path: ./example/model.fga
      - name: Deploy using a string
        uses: openfga/action-openfga-deploy@v0.1
        with:
          api-url: http://localhost:8080
          api-token: ${{ secrets.KEY }}
          store-id: ${{ env.STORE_ID }}
          format: json
          model: '{\"schema_version\":\"1.1\",\"type_definitions\":\[\{\"type\":\"user\"\},\{\"type\":\"document\",\"relations\":\{\"reader\":\{\"this\":\{\}\},\"writer\":\{\"this\":\{\}\},\"owner\":\{\"this\":\{\}\}\},\"metadata\":\{\"relations\":\{\"reader\":\{\"directly_related_user_types\":\[\{\"type\":\"user\"\}\]\},\"writer\":\{\"directly_related_user_types\":\[\{\"type\":\"user\"\}\]\},\"owner\":\{\"directly_related_user_types\":\[\{\"type\":\"user\"\}\]\}\}\}\}\]\}'
```


## License
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fopenfga%2Faction-openfga-deploy.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2Fopenfga%2Faction-openfga-deploy?ref=badge_large)
