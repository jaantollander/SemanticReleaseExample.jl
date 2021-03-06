# SemanticReleaseExample.jl
[![Release](https://github.com/jaantollander/SemanticReleaseExample.jl/actions/workflows/Release.yml/badge.svg)](https://github.com/jaantollander/SemanticReleaseExample.jl/actions/workflows/Release.yml)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)

## What is semantic-release?
[**Semantic-release**](https://github.com/semantic-release/semantic-release) is a tool for automating package release workflow to make new features and fixes immediately available to users. It uses formalized [**commit message convention**](https://www.conventionalcommits.org) to automatically determine the next semantic version number according to [**semantic versioning**](https://semver.org/) specification and generate release notes. Furthermore, semantic-release works with continuous integration workflows such as GitHub Actions and helps to avoid mistakes with manual releases.

The instructions below explain how to use semantic-release for a Julia package hosted on GitHub. We give the instructions using [Julia](https://julialang.org/), [Git](https://git-scm.com/), and [GitHub](https://cli.github.com/) command-line interfaces. 

These instructions are also available as a video on YouTube at [**How to Configure semantic-release for a Julia package using GitHub Actions**](https://www.youtube.com/watch?v=_npnsESXRno).


## How to setup semantic-release for a Julia package with GitHub actions
### Creating a Julia package with GitHub repository
Let's create a Julia package using `PkgTemplates.jl` without `Git` and `TagBot` plugins.

```julia
using PkgTemplates

# Define the package template
template = Template(;
    user="jaantollander",
    authors="Jaan Tollander de Balsch",
    julia=v"1.6",
    dir=".",
    plugins=[
        License(; name="MIT"),
        !Git,
        !TagBot,
    ],
)

# Define the package name
name = "SemanticReleaseExample"

# Create the package template
template(name)

# Add `.jl` extension to the directory name
mv(name, "$name.jl")
```

We can save the above script to a `pkg.jl` file and execute it with the `julia` command.

```bash
julia pkg.jl
```

Let's create a remote repository on GitHub with the name `SemanticReleaseExample.jl`.

```bash
gh repo create "SemanticReleaseExample.jl" --public --confirm
```

The command initializes an empty remote repository to GitHub and initializes a local repository to our Julia package with the remote repository as `origin`.

### Adding semantic-release workflow
Let's change our working directory inside the Julia package.

```bash
cd SemanticReleaseExample.jl
```

Then, we need to add the [`.github/workflows/Release.yml`](./.github/workflows/Release.yml) GitHub action. You can copy it from this repository.

Next, we need to add the [`.releaserc`](./.releaserc) configuration file. You can also copy it, but you need to change the `RepositoryUrl` to point to your remote repository.

Finally, let's add an empty `CHANGELOG.md` file for automatically generated release notes.

### Creating an initial commit, tag and release
First, we should change the version field on `Project.toml` to `version = "0.0.0"` to correspond with the initial tag. Then, let's add all files to version control, create an initial commit and tag it with an initial version tag `v0.0.0`.

```bash
git add .
git commit -m "chore: initial commit"
git tag v0.0.0
```

Now, we can push the initial commit and tag to our remote repository.

```bash
git push origin master v0.0.0
```

Finally, we need to create an initial release on GitHub with the initial version tag. 

```bash
gh release create v0.0.0 --title "Initial Release" --notes "This release corresponds to the initial commit on the repository and is meant to kickstart the semantic release process."
```

The initial release is required for semantic-release to work.


## Workflow with semantic-release
### Commit message format
[Conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) specifies the commit message format consisting of a mandatory header, optional body, and optional footer with blank lines in between. The header consists of a mandatory type, optional scope, colon, and a short summary.

```
<type>[<optional scope>]: <short summary>

[optional body]

[optional footer(s)]
```

Semantic-release uses the commit type to generate releases and release notes. Commit type `fix` is for bug patches, and `feat` introduces new features. Breaking change footer `BREAKING CHANGE: <description>` presents breaking API changes. We can also use other types such as `build`, `chore`, `ci`, `docs`, `style`, `refactor`, `perf`, and `test`. You can see some examples below.

### Create a patch release

```bash
git commit -m "fix: <short summary>"
```

### Create a feature (minor) release

```bash
git commit -m "feat: <short summary>"
```

### Create a breaking (major) release

```bash
git commit -m "feat: <short summary>" -m "" -m "BREAKING CHANGE: <description>"
```

### Pull changes after new releases
Remember to pull the changes generated by semantic-release after new releases.

```
git pull origin master
```


## Adding semantic-release badge to Readme
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)

You can add the semantic release badge as shown above with the markdown snippet below.

```markdown
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)
```
