# MADNIS GammaBoard API

MADNIS sampler implementation plus a small `gammaboard-jsonrpc-v1` process
worker for GammaBoard.

## Build A Runtime

Apptainer is the most portable path for UBELIX and other non-Nix systems:

```bash
apptainer build --force runtime.sif apptainer.def
```

The definition file builds from Git, not from the local checkout. Pin the exact
source when needed:

```bash
API_REF=<branch-or-commit> apptainer build --force runtime.sif apptainer.def
```

On UBELIX, run the build from the GammaBoard workspace:

```bash
python ubelix.py build apptainer resources/runtimes/madnis_gammaboard_api/runtime.sif resources/runtimes/madnis_gammaboard_api/apptainer.def
```

Nix is still supported where available:

```bash
nix build .#runtime
```

## Use With GammaBoard

`madnis_test_run.toml` is a ready-to-copy run template. It shows the Apptainer
command as the active option and keeps the Nix command commented next to it.

The sampler checkpoints are written below the GammaBoard resources directory, so
paths such as `runtimes/madnis_gammaboard_api/checkpoints/...` are portable
between local, ITPHLIES, and UBELIX deployments.
