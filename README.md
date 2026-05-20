# MADNIS GammaBoard API

MADNIS sampler implementation for GammaBoard using the
`gammaboard_process.run_sampler(...)` Python wrapper.

## Build A Runtime

Apptainer is the most portable path for UBELIX and other non-Nix systems:

```bash
apptainer build --force madnis.sif apptainer.def
```

The definition file builds from Git, not from the local checkout. Pin the exact
source when needed:

```bash
API_REF=<branch-or-commit> apptainer build --force madnis.sif apptainer.def
```

On UBELIX, run the build from the GammaBoard workspace:

```bash
python ubelix.py build apptainer resources/processes/madnis_gammaboard_api/madnis.sif resources/processes/madnis_gammaboard_api/apptainer.def
```

Nix is still supported where available:

```bash
nix build .#runtime
```

## Use With GammaBoard

`madnis_test_run.toml` is a ready-to-copy run template. It shows the Apptainer
command as the active option and keeps the Nix command commented next to it.

The sampler command uses `$resources` because GammaBoard expands it in process
commands. Sampler `args` are passed through unchanged, so `save_path` should be
relative to the process cwd, which defaults to `$resources`; paths such as
`processes/madnis_gammaboard_api/checkpoints/...` are portable between local,
ITPHLIES, and UBELIX deployments.

The process entrypoint is:

```bash
python -u -m run_sampler
```
