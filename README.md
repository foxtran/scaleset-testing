# Scale Set Testing

A minimal repository for testing `ci-scheduler` against real GitHub Actions infrastructure.

The repository contains one manually triggered workflow. It prints runner and workflow metadata, sleeps for a configurable number of seconds, and can optionally fail to test unsuccessful job completion.

## GitHub setup

On startup, `ci-scheduler` finds or creates a scale set named `testing` in the default runner group, with a single `testing` label. No manual GitHub setup is required.

Only one active `ci-scheduler` may use this scale set. Use a separate scale set for every additional scheduler.

Example `ci-scheduler.toml`:

```toml
github_config_url = "https://github.com/foxtran/scaleset-testing"
cluster_name = "testing"
max_runners = 1
database_path = "testing.db"
```

The numeric scale set ID is resolved automatically. The local config and database files are ignored by Git.

Run the scheduler from the `ci-scheduler` crate:

```bash
GITHUB_TOKEN="github_pat_..." /path/to/ci-scheduler /path/to/ci-scheduler.toml
```

## Run an experiment

Open **Actions → Scale Set Smoke → Run workflow** in GitHub.

- Set **Seconds to keep the job running** to observe a running job.
- Enable **Fail the job after the sleep** to test a failed completion.
- Leave failure disabled for a successful completion.

The workflow has no `push` or scheduled triggers, so jobs start only when requested manually.
