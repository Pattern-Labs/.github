# Create PR, Approve, and Merge

A composite action that creates a pull request, approves it, and automatically merges it. Designed for workflows where a post-PR-merge action needs to push file(s) to main.

## Usage Example

```yaml
- name: Create Version Bump PR
  uses: Pattern-Labs/.github/.github/actions/create_pr_approve_merge@main
  with:
    repository: maps
    commit-message: "Bumping versions for map(s) ${{ steps.detect-changes.outputs.changed_maps }}"
    title: "Bumping versions for map(s) ${{ steps.detect-changes.outputs.changed_maps }}"
    add-paths: map-versions.json
    repo-access-token: ${{ secrets.REPO_ACCESS_PAT }}
    pr-approver-token: ${{ secrets.PATTERN_PR_APPROVER }}
```

## Notes

- If there are no changes to commit, the PR creation will be skipped and the action will complete without error
- The approval and merge steps are conditional - they only run if a PR was actually created
- The `--auto` flag enables auto-merge, which merges the PR once all required checks pass