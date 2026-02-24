# Schema: context-dump

The raw input to the pipeline. No structure required.

## Accepted formats

Any combination of:
- Pasted text (SOPs, process docs, job descriptions, wiki exports)
- File contents (markdown, plain text, HTML, PDF text)
- Meeting notes or transcripts
- Slack/email thread summaries
- Org charts or team descriptions
- Existing architecture diagrams described in text

There is no wrong input. If it contains signal about what something is, who does it, how it works, or what the rules are, it is valid.

## No context-dump available

If no documents are provided, the pipeline skips `tml-extract` and `tml-interview` runs in cold-start mode (pure conversation, no candidates to confirm).
