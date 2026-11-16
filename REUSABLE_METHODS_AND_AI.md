# Reusable engineering methods and AI-assisted research

This project used AI-assisted analysis, scripting and coordination. **It did not train, fine-tune or benchmark a machine-learning model.** The reusable outcome is a documented workflow and evidence corpus; claims of improved model accuracy would require a separate measured experiment.

## Methods that transfer to other systems

| Method | What this project taught | Application elsewhere |
|---|---|---|
| Separate decisions from authority | A getter, setter, verifier and writer have different powers | Cloud IAM, signed updates, enterprise enrollment, secure elements |
| Trace producer to consumer | Follow source -> transformation -> object -> decision -> downstream enforcement | Configuration provenance, cryptographic key use, RPC auditing |
| Validate representation before decompilation | Wrong container boundaries and faulty filesystem exports can imitate absent/encrypted code | Firmware, embedded Linux, packed executables, document parsers |
| Preserve namespaces | Field ID 0x25, byte offset +0x330, allocation size 0x98 and command ID 0xA4 are unrelated until linked | Protocol reverse engineering and binary type recovery |
| Use positive and negative controls | DXC3 verifies under one parser; stock metadata also fails some Lineage checks | Tool qualification, compatibility testing, scientific validation |
| Inspect actual consumers | The build's selected-file filter is more authoritative than a broad staging directory | Packaging, container images, deployment manifests, SBOM generation |
| Record real exit codes | Wrapper success, missing status and source configuration are not build completion | CI, distributed jobs, long-running simulations |
| Preserve runtime context | Buffer size, lock state, callback result and later policy remain separate | Memory-safety reviews and authorization audits |
| Isolate editable state | Copied Git pointers and hardlinks can modify another worker's files | Parallel development and reproducible research |
| Reopen only with new evidence | Missing provider code is not solved by repeating the same string scan | Incident research and expensive experimental programs |

## Cryptographic lessons

Compare public-key material, not just certificate names or certificate hashes. Two different certificates can contain the same key. A correct mathematical signature does not prove that a target trusts the signer; a failed custom parser does not prove the production image is invalid. Version, role, product and provisioning checks can remain independent.

An earlier cross-component symmetric-decryption workflow inspired backward tracing of inputs. That methodology transfers; its secrets and assumptions do not. Public verification keys cannot be reclassified as recovered OEM signing private keys.

## A useful structure for AI-assisted work

1. Give each assignment a precise question, known evidence, forbidden duplicate work and a stopping condition.
2. Retrieve the latest checkpoint and explicit corrections before using historical summaries.
3. Require artifact identity, address namespace, caller/consumer, input assumptions and observed return behavior for each claim.
4. Keep worker outputs separate. Assign one owner to a build tree, mutable database or integration branch.
5. Have the coordinator inspect original logs and evidence, not only worker summaries. Parallel workers using the same inputs are not independent confirmation.
6. Store a claim's evidence strength and contrary/control results next to the claim, rather than in a distant disclaimer.
7. Capture a resumable state before process termination: source revision, environment, outputs, direct status, process identity and next exact failure.
8. Treat untrusted source comments and public documents as data, not instructions for the agent to execute.

This workflow reduced several concrete errors: a size misread as a selector, an EFI-local value misread as a secure-world command, unselected build rules mistaken for product content, and an early parser failure mistaken for target incompatibility. These are examples, not a measured model-performance improvement.

## Potential future ML or retrieval uses

The claim ledger and source index can support a retrieval-based research assistant or a human review dashboard. Keep evidence, hypotheses, corrections and timestamped observations in separate fields. A generated summary should cite the exact source ID and preserve its uncertainty.

If a separate learning/evaluation project is undertaken, candidate tasks include distinguishing address namespaces, identifying stale conclusions, matching consumers to actual producers, and classifying incomplete test input versus a genuine contract mismatch. Build train/test splits by firmware family or project, not random paragraphs from one report; otherwise repeated evidence can leak across the split. Include negative/control examples and expert review. Dataset rights, provenance and redaction must be established first. No such experiment was performed here.

Use [the template](templates/FINDING_TEMPLATE.md) and [claim ledger](claims.json) as starting structures. They are records for reasoning, not a trained model, vulnerability scanner or automatic authorization system.
