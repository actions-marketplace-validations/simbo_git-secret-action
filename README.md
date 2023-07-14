# git-secret GitHub Action

A GitHub action to setup [git-secret](https://sobolevn.me/git-secret/) and
reveal secrets in a repository.

## Usage

Use `simbo/git-secret-action@v1` in your GitHub action workflow.

### Example

```yml
jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - name: 🛎 Checkout
        uses: actions/checkout@v3

      - name: 🔓 Reveal Secrets
        uses: simbo/git-secret-action@v1
        with:
          private-key: ${{ secrets.GPG_PRIVATE_KEY }}
          passphrase: ${{ secrets.GPG_PASSPHRASE }}
```

## Inputs

| Input            | Required | Default  | Description                                                                                                                  |
| ---------------- | -------- | -------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `version`        | no       | (latest) | git-secret version to use                                                                                                    |
| `private-key`    | yes      | –        | base64-encoded single-line gpg private key to decrypt secrets                                                                |
| `passphrase`     | yes      | –        | gpg passphrase to decrypt secrets                                                                                            |
| `github-com-pat` | no       | –        | GitHub.com PAT to retrieve latest git-secret version number from GitHub API (recommended for GitHub Enterprise environments) |

### Providing a GPG Private Key

A GPG private key is a large multi-line string. To enable GitHub Actions to work
with this, it should be converted to a **_base64-encoded single-line string_**.

The following command will…

- export the private key for `<EMAIL>`
- encode it with base64
- convert it to a single-line string
- save it as `private_key.txt`

```sh
gpg --armour --export-secret-keys <EMAIL> | base64 | tr -d '\n' > private_key.txt
```

Store the generated string as GitHub Actions secret.

## Outputs

This action has no outputs. 🤷‍♂️

## Development

### Creating a new Version

Use `./release.sh <major|minor|patch>` which will create a git tag for the
respective version.

A release workflow will pick up the tag when pushed to GitHub, create a release
and move major, minor and latest tags accordingly.

To publish the release into the GitHub marketplace open
[releases](https://github.com/simbo/vale-action/releases) and
update the release for marketplace publishing.

## License

[MIT &copy; Simon Lepel](http://simbo.mit-license.org/)
