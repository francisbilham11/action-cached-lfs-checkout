# GitHub action: Cached LFS checkout

Storing (large) binary files in Git trees is not recommended because any change to the
file will result in a re-upload of the entire file when pushing, increasing the size of
the Git repository. When updating a 100 MB file 100 times, your repository will be 10 GB
in size!

As a remedy, there is [Git LFS](https://git-lfs.github.com/) which stores certain files
only as pointers in the Git tree, downloading the actual data from somewhere else when
pulling. The repository size remains small.

[GitHub applies some fees when the total amount of LFS downloads surpasses
1GB.](https://github.com/github/roadmap/issues/237) Unfortunately, even gh-actions runs,
when used with

```yaml
- name: Checkout code
  uses: actions/checkout@v2
  with:
    lfs: true
```

count towards that limit. To _cache_ the LFS downloads, you can instead use this action.
Simply replace the above by

```yaml
- name: Checkout code
  uses: francisbilham11/action-cached-lfs-checkout@v1
```

Some of the parameters from the [actions/checkout@v3](https://github.com/marketplace/actions/checkout#usage) action
are available to use too:

```yaml
- name: Checkout code
  uses: francisbilham11/action-cached-lfs-checkout@v1
  with:
    # Repository name with owner. For example, actions/checkout
    # Default: ${{ github.repository }}
    repository: ''

    # The branch, tag or SHA to checkout. When checking out the repository that
    # triggered a workflow, this defaults to the reference or SHA for that event.
    # Otherwise, uses the default branch.
    ref: ''

    # Personal access token (PAT) used to fetch the repository. The PAT is configured
    # with the local git config, which enables your scripts to run authenticated git
    # commands. The post-job step removes the PAT.
    #
    # We recommend using a service account with the least permissions necessary. Also
    # when generating a new PAT, select the least scopes necessary.
    #
    # [Learn more about creating and using encrypted secrets](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets)
    #
    # Default: ${{ github.token }}
    token: ''

    # Relative path under $GITHUB_WORKSPACE to place the repository
    path: ''

    # Number of commits to fetch. 0 indicates all history for all branches and tags.
    # Default: 1
    fetch-depth: ''

    # Whether to checkout submodules: `true` to checkout submodules or `recursive` to
    # recursively checkout submodules.
    #
    # When the `ssh-key` input is not provided, SSH URLs beginning with
    # `git@github.com:` are converted to HTTPS.
    #
    # Default: false
    submodules: ''
```

Check it out [on the GitHub
Marketplace](https://github.com/marketplace/actions/cached-lfs-checkout)!

### Further reading

- [Luca Benci, Avoiding git-lfs bandwidth waste with GitHub and
  CircleCI](https://www.develer.com/en/avoiding-git-lfs-bandwidth-waste-with-github-and-circleci/)
- [actions/checkout issue: Cache for
  LFS](https://github.com/actions/checkout/issues/165)

### License

The scripts and documentation in this project are released under the MIT License.
