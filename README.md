# enrich-release-clickup-action
A github action to enrich a particular Release with sub-bullets corresponding to the tickets referenced by that PR. As an example:

```
 * This is my release #123
```

Would become:

```
 * This is my release #123
   * [CU-128401](clickup_url)
   * [CU-128405](clickup_url)
```

## Usage

To use this action, add a github action to your repository that is similar to the below:

```
- uses: Monkeyjump-Labs/enrich-release-clickup-action@v1
  id: enrich_release_with_clickup_info
  name: Enrich Release with clickup information
  with:
    release_number: ${{ steps.my_release_creation.outputs.release_number }}
    clickup_api_token: ${{ secrets.CLICKUP_API_TOKEN }}
```

This will trigger addition of release notes based on `Task Linked: ` comments.
