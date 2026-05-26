# MADNIS GammaBoard API

MADNIS sampler implementation for GammaBoard using the
`gammaboard_process.run_sampler(...)` Python wrapper.

## Runtime Options

### Direct venv

For local demos or machines where Apptainer is not available, install the
sampler directly into a virtual environment under the GammaBoard resources
folder:

```bash
cd ~/gammaboard/resources
git clone https://github.com/alphal00p/madnis_gammaboard_api.git
cd madnis_gammaboard_api

uv venv --python 3.13 --seed .venv
. .venv/bin/activate
python -m pip install .
```

Use this GammaBoard process command:

```toml
command = ["$resources/madnis_gammaboard_api/.venv/bin/madnis-gammaboard-sampler"]
cwd = "$resources"
```

With `cwd = "$resources"`, sampler `save_path` values should be relative to
the GammaBoard resources directory, for example:

```toml
save_path = "madnis_gammaboard_api/checkpoints/ghost_bump_madnis"
```

### Apptainer

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

`madnis_test_run.toml` is a ready-to-copy run template. It uses the direct venv
command by default and keeps Apptainer and Nix alternatives commented next to it.

The sampler command uses `$resources` because GammaBoard expands it in process
commands. Sampler `args` are passed through unchanged, so `save_path` should be
relative to the configured process `cwd`.

The process entrypoint is:

```bash
python -u -m run_sampler
```
