# Examples

These are starter inputs for hosted LoomLoom templates.

If you still call the workflow `batchjob`, `batchflow`, or “批处理”, those names map to the same LoomLoom template flow.

- `text-image-v1.input.jsonl`
- `text-image-video-v1.input.jsonl`

Use them with:

```bash
./cli/loomloom run submit text-image-v1 -f examples/text-image-v1.input.jsonl
./cli/loomloom run submit text-image-video-v1 -f examples/text-image-video-v1.input.jsonl
```

## Code Review PoC

`text-v1` can also be used as a batch code-review proof of concept.

The helper below scans one local repository, turns each selected code file into one
task row, and writes a JSONL file that can be submitted with `run submit`.

Example:

```bash
python3 scripts/generate-code-review-jsonl.py \
  --repo /Users/zhouyang/project/github/symphony \
  --output /tmp/symphony-code-review.jsonl \
  --max-files 20
```

Then submit it with:

```bash
./cli/loomloom run submit text-v1 -f /tmp/symphony-code-review.jsonl
```

Recommended follow-up:

```bash
./cli/loomloom run watch <run-id>
./cli/loomloom artifact download <run-id> --output-dir ./downloads
```

Current PoC assumptions:

- one code file = one task
- only single-file review, not cross-file reasoning
- best for first-pass screening such as security smells, leak risks, and poor patterns
- large files are truncated on purpose to keep each task bounded
