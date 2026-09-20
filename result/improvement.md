# Concrete System Improvement

### 1. What was observed
During initial evaluation with open models on OpenRouter, latency was excessive (>35s per research query) and citation recall dropped to 60% due to unicode bracket formatting (`【chunk_id】`) emitted by the LLM which failed strict regex parsing.

### 2. What was changed
1. **Model Switch & Hardware Acceleration:** Migrated to Groq's high-speed LPU engine with `max_retries=5` for HTTP 429 bounded backoff.
2. **Robust Multi-Bracket Citation Extraction:** Upgraded the Synthesizer regex logic to support Western brackets, Chinese brackets, and boundary-separated tags.
3. **Multi-Turn Pronoun Resolution:** Added standalone query reformulation in the Router node before retrieval.
