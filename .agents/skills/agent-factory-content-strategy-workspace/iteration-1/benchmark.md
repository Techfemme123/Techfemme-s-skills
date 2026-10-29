# Benchmark: agent-factory-content-strategy

| Config | Pass rate | Time (s) | Tokens |
|---|---|---|---|
| with_skill | 90% | 190.9 | 102768 |
| without_skill | 85% | 176.0 | 85206 |
| delta | +0.05 | +15.0 | +17562 |

## Per eval

| Eval | with_skill | without_skill |
|---|---|---|
| pcao-f-certification-campaign | 6/7 | 5/7 |
| roman-urdu-foundations-series | 5/6 | 5/6 |
| thesis-linkedin-thought-leadership | 5/5 | 5/5 |

## Notes

- One run per configuration, so there is no variance estimate; treat the deltas as directional.
- Baselines were strong: every baseline used the Agent Factory System of Record connector on its own, so the skill is being compared against an informed starting point.
- Non-discriminating (pass in both configs): PCAO-F path statement, no invented fees, no outcome guarantees, free-sample CTA; Roman Urdu post count, hook/topic/CTA, Roman Urdu hooks, English terms; and all 5 LinkedIn assertions. These still guard against harmful output but need tightening to measure the skill.
- Where the skill helped: linking each piece to a specific book URL (baseline failed this in 2 of 3 cases: 0/12 Shorts posts had a URL; PCAO-F calendar rows had none), and stating that Anthropic exams are optional (baseline failed).
- Where the skill hurt: in the Roman Urdu series it added a trailer and recap and merged two courses, so 3 posts were not mapped to a single Foundations course; the baseline gave each of the 10 courses its own post. In PCAO-F, its weekly calendar added derivative items (clips, story polls) without language tags.
- Grader saw qualitative wins not covered by any assertion: the with-skill plans flagged unverified claims (e.g. 'free', Zia Tutor price) where the baseline stated them as fact, gave a full fact-check list, and set targets relative to a baseline instead of invented absolute numbers.
- Cost: the skill adds about 21% tokens (+17.6k mean) and about 15s per run.
