# Create Version Bump PR Action

A composite action that creates a pull request, approves it, and automatically merges it. Designed for automated version bump workflows.

## Inputs

### Required

- `repository`: Target repository (e.g., "pattern-labs/maps")
- `commit-message`: Commit message for the changes
- `title`: Pull request title
- `add-paths`: Files to add to the commit (space or newline separated)
- `repo-access-token`: Token with repo access for creating and merging PRs
- `pr-approver-token`: Token for approving the pull request

### Optional

- `body`: Pull request body/description (default: "Automated PR created by reusable action")
- `branch`: Branch name for the pull request (default: "automation/${{ github.run_id }}")
- `path`: Working directory path (default: ".")
- `delete-branch`: Whether to delete the branch after merge (default: "true")
- `merge-method`: Merge method - squash, merge, or rebase (default: "squash")

## Outputs

- `pull-request-number`: The number of the created pull request
- `pull-request-url`: The URL of the created pull request

## Usage Example

```yaml
- name: Create Version Bump PR
  uses: Pattern-Labs/.github/.github/actions/create-version-bump-pr@main
  with:
    repository: pattern-labs/maps
    commit-message: "Bumping versions for map(s) ${{ steps.detect-changes.outputs.changed_maps }}"
    title: "Bumping versions for map(s) ${{ steps.detect-changes.outputs.changed_maps }}"
    add-paths: map-versions.json
    repo-access-token: ${{ secrets.REPO_ACCESS_PAT }}
    pr-approver-token: ${{ secrets.PATTERN_PR_APPROVER }}
    body: |
      Automated version bump performed by workflow.

      Changed maps: ${{ steps.detect-changes.outputs.changed_maps }}
```

## How It Works

1. **Creates PR**: Uses `peter-evans/create-pull-request` to create a pull request with the specified changes
2. **Approves PR**: Uses GitHub CLI to approve the pull request (only if PR was created)
3. **Merges PR**: Uses GitHub CLI to merge the pull request with auto-merge (only if PR was created)

## Notes

- If there are no changes to commit, the PR creation will be skipped and the action will complete without error
- The approval and merge steps are conditional - they only run if a PR was actually created
- The `--auto` flag enables auto-merge, which merges the PR once all required checks pass
