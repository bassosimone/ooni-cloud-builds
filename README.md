# github.com/bassosimone/ooni-cloud-builds

This repository contains experimental code. I am curious to figure out if I can
convert the scripts I use for preparing OONI releases to GitHub actions.

So, I created this repository to explore this possibility.

## Objectives

This section describes what we'd like to achieve. It's currently unclear if
all of this is possible. We'll figure out together :^).

These are the functionality we want to implement

- [ ] build probe-cli and probe-destkop binary releases;
- [ ] smoke check the generated binaries;
- [ ] pull psiphon's configuration file from ooni/probe-private;
- [ ] sign those releases using a machine GPG key;
- [ ] notarize macOS builds so they can run on macOS systems;
- [ ] sign Windows executables so they run on Windows;
- [ ] publish binaries to releases at github.com/ooni/probe-cli repo;
- [ ] publish Android releases at Maven Central.

The ooni/probe-cli repository _may_ be the right place where to
perform these actions. However, a separate repository _may_ be
better in terms of privilege separation.

Some of the functionality we describe above require secrets. Assuming
that GitHub actions are not going to leak these secrets in the logs,
a separate repo should be safer as long as we don't allow actions from
pull requests from others to run. (Disabling entirely pull requests
would be nice, but it's not implemented yet by GitHub.)

## Implementation

This section describes the repository's concept.

We fork from main the following branches and we let them grow independently:

- `android-staging`

- `ios-staging`

- `miniooni-staging`

- `ooniprobe-cli-{linux,macos,windows-}staging`

- `ooniprobe-desktop-staging`

Each branch contains a single GitHub action in `.github/workflows` that
is responsible of building the given product.

The GitHub action indicates the tag you want to check-out and build.

After the build, the action will sign binaries, etc.

This repository needs the following secrets:

- `GPG_KEY_BASE64`: base64 encoded GPG key used to sign;

- `PERSONAL_ACCESS_TOKEN`: GitHub personal access token
with `repo` scope to clone private repos.
