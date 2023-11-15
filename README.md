# Reusable Workflow Context

Reusable Workflow Context is a GitHub Action that enables you to access reusable workflow context within a running reusable workflow. This action is intended to address the limitations of GitHub. Namely the fact that [neither reusable workflow ref nor reusable workflow sha are available in the context of a running reusable workflow](https://github.com/actions/toolkit/issues/1264).

## Usage

To use the action, add the following step to your GitHub Actions reusable workflow:

```
- id: reusable-workflow
  name: Retrieve reusable workflow context
  uses: ipdxco/reusable-workflow-context@v1
  with:
    path: the-name-of-the-reusable-workflow.yml
```

See the [example reusable workflow](./.github/workflows/reusable.yml) that we use to [test](./.github/workflows) this action.

### Inputs

* `path`: The path where the reusable workflow is located. Required. We suggest using the name of the reusable workflow as the path.

### Outputs

* `ref`: For a reusable workflow executing an action, this is the ref of the reusable workflow being executed.
* `sha`: For a reusable workflow executing an action, this is the sha of the reusable workflow being executed.

## How it works

The action uses GitHub API to retrieve the reusable workflows referenced by the current worfklow run. It then filters the reusable workflows based on the path input and selects the first matching workflow. Finally, it exposes the `ref` and `sha` of the reusable workflow it found.

## Known limitations

1. If you use multiple reusable workflows in your workflow, you should watch out for possible path clashes.
2. If you use multiple instances of a reusable workflow with different ref/sha, you should be concious of the fact that the action will only return the ref/sha of the first instance of the reusable workflow that it finds in each instance.
3. If you use request a reusable workflow by sha, the ref output of the action will be empty.

---
