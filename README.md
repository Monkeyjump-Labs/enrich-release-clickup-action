# enrich-release-clickup-action

A GitHub Action to enrich a particular Release with sub-bullets corresponding to the tickets referenced by that PR. As an example:

```md
- This is my release #123
```

Would become:

```md
- This is my release #123
  - [CU-128401](clickup_url)
  - [CU-128405](clickup_url)
```

## Usage

To use this action, add a GitHub Action to your repository that is similar to the below:

```yaml
- uses: Monkeyjump-Labs/enrich-release-clickup-action@v1
  id: enrich_release_with_clickup_info
  name: Enrich Release with clickup information
  with:
    release_number: ${{ steps.my_release_creation.outputs.release_number }}
    # Optional: link ticket tags like #SMART-1234 written in the release notes
    clickup_team_id: ${{ vars.CLICKUP_TEAM_ID }}
```

### Inputs

| Input             | Required | Description                                                                                                                                |
| ----------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `release_number`  | yes      | Release ID or tag.                                                                                                                         |
| `github_token`    | no       | Token used to read PR comments and update the release. Defaults to `github.token`.                                                         |
| `clickup_team_id` | no       | ClickUp workspace (team) ID. When set, `#SMART-1234` tags on a release-note line link to `https://app.clickup.com/t/<team_id>/SMART-1234`. |

### Where the tickets come from

The action looks at each bullet line in the release notes that links a PR, either as `.../pull/123` (GitHub's generated
notes) or `.../issues/123` (release-please). Under that line it lists the ClickUp tasks found in:

- ticket tags such as `#SMART-1234` on the line itself, when `clickup_team_id` is set;
- `Task linked: [CU-xxxx Name](url)` comment lines on the PR (enrich-pr-clickup-action v1);
- `- **Ticket**: [SMART-1234](url)` comment lines on the PR (enrich-pr-clickup-action v2), named from the comment's
  heading.

A ticket found more than once is listed once. Running the action again on notes it already enriched adds only the
ClickUp links not yet listed under each line.
