# Configuration and HPC jobs

This folder contains the Conda environment requirements, Slurm job scripts, and
commands used to run the project's notebook pipeline on the **BGU HPC cluster**.
Other clusters or local computers may require different commands, module names,
paths, and resource settings (such as partitions, GPUs, memory, and time limits).
Replace `your_email@example.com` in the job scripts with your email address before
using them, or comment out the `#SBATCH --mail-user` and `#SBATCH --mail-type`
lines to disable email notifications.

- `conda_requirements.yml`: Dependencies for the `i_can_see_u_env` environment.
- `run_*.sbatch`: Slurm batch jobs for individual pipeline steps.
- `run_full_pipeline.sh`: Submits pipeline jobs with dependencies between steps.
- [slurm_commends.md](slurm_commends.md): Example submission and job management commands.

## Conda

Conda manages packages and isolated software environments. An environment keeps
the project's Python version and dependencies separate from other projects.
`conda_requirements.yml` defines the `i_can_see_u_env` environment and its packages.

From the project root, create and activate it with:

```bash
module load anaconda  # BGU HPC setup; other systems may use different commands.
conda env create -f config/conda_requirements.yml
conda activate i_can_see_u_env
```

Create the environment once. For later sessions, activate it again.
`conda run -n i_can_see_u_env <command>` runs a command in that environment.
If you already have an environment under an older name, create the environment
above so the updated job scripts can find it.

## Papermill

Papermill runs Jupyter notebooks from the command line and supplies parameter
values for each run. The batch scripts use it in `i_can_see_u_env` to execute
notebooks from `src/`, pass inputs with `-p` (for example, `input_data_path`), and
save an executed notebook, including cell outputs, to `logs/`.

For example, from `src/` with the environment available and `../logs/` created:

```bash
conda run -n i_can_see_u_env papermill \
    1_translate_topics_entities.ipynb \
    ../logs/translation_run.ipynb \
    -p input_data_path ../data/example.gzip.parquet \
    -p force_restart false
```

Replace the example input path with your dataset. Notebook defaults belong in a
cell tagged `parameters`; Papermill injects the supplied values before executing
the notebook, allowing repeated runs with different inputs.
