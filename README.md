# NexTron Cloud records

When a NexTron Cloud training job ends, its record is fixed: the sha256 of every file in the job's export (training
log, per-step metrics and run record, GPU and host samples, hardware snapshots, training config) and of every
checkpoint file, in a `manifest.json`. One line per job is appended here, so anyone can later check that a job's
record and checkpoint are the ones that existed when it finished.

`records/<YYYY-MM>.jsonl`, one JSON object per line:

    {"job_id": "job_...", "manifest_sha256": "...", "fixed_at": "2026-10-28T09:30:12Z"}

To check a job:

1. Download its manifest (`GET /v1/training/jobs/<id>/manifest`, or `manifest.json` in Download logs) and its files.
2. `sha256sum manifest.json` must equal the `manifest_sha256` on the job's line here.
3. Each file's sha256 must equal its entry in `manifest.json`.

Lines are only ever appended. The repository's history (and every clone of it) shows when each line was added.
