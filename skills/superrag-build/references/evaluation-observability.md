# Evaluation And Observability

RAG quality is mostly retrieval quality plus evidence discipline. Evaluate before optimizing.

## Golden Set

Include:

- direct lookup
- synthesis across multiple chunks
- multi-hop
- exact IDs/codes/part numbers
- table/value questions
- image/diagram questions if multimodal
- multilingual paraphrases
- ambiguous questions
- out-of-corpus/no-answer questions
- stale/conflicting source questions
- permission-denied questions

Each case should define:

- question
- expected behavior
- expected source IDs
- acceptable answer facts
- disallowed claims
- required citation behavior

## Retrieval Metrics

Track:

- hit@k: expected source appears in top k
- MRR: expected source ranking
- nDCG: graded relevance ranking
- precision@k: noise in context
- recall by metadata slice: language, source type, tenant, document age
- reranker lift: before/after rankings

## Answer Metrics

Track:

- faithfulness / groundedness
- citation accuracy
- answer completeness
- unsupported claim count
- refusal correctness
- instruction-following
- formatting correctness
- latency
- token and API cost

## Debug Trace

Every query should be inspectable:

```json
{
  "query_id": "...",
  "user_query": "...",
  "normalized_query": "...",
  "filters": {},
  "retrieval": [{"chunk_id": "...", "score": 0.72}],
  "rerank": [{"chunk_id": "...", "score": 0.91}],
  "context_ids": ["..."],
  "model": "model-name",
  "latency_ms": 1200,
  "answer_policy": "answered|refused|clarified",
  "citations": []
}
```

## Failure Review

Classify failures:

- missing source document
- parser/OCR failure
- bad chunking
- wrong metadata/filter
- embedding mismatch
- vector-only miss
- reranker mistake
- context too small
- prompt hallucination
- citation mismatch
- stale index
- permission leak

Fix in that order. Do not fix retrieval failures with prompt wording alone.

## Eval Gates

Suggested release gates:

- no permission leaks
- no critical unsupported claims
- expected source hit@10 above target
- reranked expected source hit@5 above target
- no-answer questions refuse or clarify
- p95 latency under target
- cost under budget

## Observability

Log:

- ingestion runs and errors
- source manifests and hashes
- chunk counts by source/type/language
- index version
- query traces
- retrieval and rerank scores
- answer policy decisions
- feedback
- model versions
- latency/cost

Dashboards:

- query volume
- retrieval miss rate
- refusal rate
- no-answer false positive/negative rate
- latency p50/p95/p99
- token/cost trend
- feedback trend
- ingestion freshness
