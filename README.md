This repository holds org-wide default github configuration files.

# Automations

We provide some github actions for building and releasing molecules

## Build Action
Add the following to `.github/workflows/build_molecule.yaml`

The example below will build on every tag of the repo
```
name: Build Molecule
on:
  push:
    tags:
      - '[0-9]+.[0-9]+.[0-9]+'

jobs:
  build-molecule:
    strategy:
      matrix:
        paths:
          - "path/to/molecule/from/repo/root"
    uses: Pattern-Labs/.github/.github/workflows/build_molecule.yaml@main
    secrets: inherit
    with:
      molecule_path: ${{ matrix.paths }}
      tag: ${{  github.ref_name }}
```

## Release Action
Add the following to `.github/workflows/release_molecule.yaml`

The example below releases a helm chart for the molecule on each tag
```
name: Release Molecule
on:
  push:
    tags:
      - '[0-9]+.[0-9]+.[0-9]+'

jobs:
  release-molecule:
    strategy:
      matrix:
        include:
          - path: "path/to/molecule/from/repo/root"
            chart: "name_of_molecule"
    uses: Pattern-Labs/.github/.github/workflows/release_molecule.yaml@main
    secrets: inherit
    with:
      molecule_path: ${{ matrix.path }}
      chart_name: ${{ matrix.chart }}
      tag: ${{  github.ref_name }}
```

## Test Action
Add the following to `.github/workflows/test_molecule.yaml`

The example below tests all atoms on each PR
```
name: Test Molecule
on:
  pull_request:
    branches: [ main ]

jobs:
  test-molecule:
    strategy:
      matrix:
        paths:
          - "path/to/molecule/from/repo/root"
    uses: Pattern-Labs/.github/.github/workflows/test_molecule.yaml@main
    secrets: inherit
    with:
      molecule_path: ${{ matrix.paths }}
```

## Coverage Action
Add the following to `.github/workflow/molecule_coverage.yaml`

The example below reports coverage on each pr
```
name: Molecule Coverage
on:
  pull_request:
    branches: [ main ]

jobs:
  molecule-coverage:
    strategy:
      matrix:
        paths:
          - "molecules/gateway"
          - "molecules/jupyter"
          - "molecules/robot-control"
    uses: Pattern-Labs/.github/.github/workflows/molecule_coverage.yaml@main
    secrets: inherit
    with:
      molecule_path: ${{ matrix.paths }}
      coverage_amt: 10
```
