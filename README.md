# semantic-release-changelog-with-github-protected-branches

Using [@semantic-release/changelog](https://github.com/semantic-release/changelog) with GitHub Protected Branches.

## The Problem

If you are using protected branches and require a pull request before merging then the GitHub action for `@semantic-release/git` cannot push directly to the protected branch.

## The Solution

What you _could_ do, and what this repo demonstrates, is create a new pull request.

This satisfies the protected branch rule.

_However_ it then gives you another PR to merge, which negates some of the automation.

This can be fixed by using [peter-evans/create-pull-request](https://github.com/peter-evans/create-pull-request) to create the new pull request, [peter-evans/enable-pull-request-automerge](https://github.com/peter-evans/enable-pull-request-automerge) to set it to automerge (once all requirements are satisfied), and then [juliangruber/approve-pull-request-action](https://github.com/juliangruber/approve-pull-request-action) to approve the new pull request which triggers the automerging.

### Requirements

- [x] settings -> branch protection of the main branch must be on
- [x] settings -> branch protection -> "require a pull request before merging" must be on
- [x] settings -> branch protection -> "require approvals" must be on to give auto-merging a trigger
- [x] settings -> pull request -> "Allow auto-merge" must be on to enable auto-merging
- [ ] settings -> pull request -> "Automatically delete head branches" _should_ be turned on to clean up some of the mess created by automatically generating pull requests
- [x] settings -> secrets -> you must have a Personal Access Token added as a secret to allow the PR approval to come from you<sup>1</sup>

1 - the `github-actions(bot)` will create the Pull Request and you will (automatically) approve it. This will satisfy the "require approvals" check and trigger the auto-merge.

## Example workflow

See [`.github/workflows/release.yaml`](https://github.com/stickeepaul/semantic-release-changelog-with-github-protected-branches/blob/main/.github/workflows/release.yaml).
