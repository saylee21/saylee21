# Saylee Bharsakle

**I work on the layer between a request and the model.** Routing, scheduling, and observability for LLM serving on Kubernetes. I find the bug, measure it, fix it, and land the fix in the project that owns it.

**29 merged pull requests upstream:** llm-d-router 11, vLLM AIBrix 8, Flux 4, kueue 3, kgateway 2, Volcano 1.

Writing about inference and serving internals at [@say_lee_x](https://x.com/say_lee_x).

---

## The problem I keep coming back to

You run two replicas behind a router. You scale to three. The new pod sits at zero requests.

It is not a bug in the scaler. The router sends each request to the replica that already holds that prompt's KV cache, and the new pod holds nothing. Measured on a two replica llm-d deployment: 18 of 18 requests went to the warm replica when the prompt shared a cached prefix, and 8 of 8 when no cache existed anywhere.

That is right for latency and wrong for capacity. The fleet looks balanced on paper and is lopsided in practice.

So I work on both halves. Making it visible: per endpoint publish outcomes, staleness, and cycle duration on the cross-replica data path ([#4255](https://github.com/llm-d/llm-d-router/pull/4255)). Making it fixable: a design issue arguing that joining, draining, and leaving endpoints are states, not a boolean ([#4308](https://github.com/llm-d/llm-d-router/issues/4308)).

---

## Best work

### 1. One routing decision was sending 5,006 Redis commands. Now it sends 17.

[aibrix#3467](https://github.com/vllm-project/aibrix/pull/3467). The in-flight saturation filter was reading every ready pod before the strategy had picked candidates, so the bounded read never happened. At 5,000 ready pods that is 5,006 Redis commands per decision, at the exact load where Redis is hottest.

Reordering the pipeline makes all three stages read the same snapshot, so the read stays O(K). Measured on real Redis 8.10.3 over 5,000 pods, 2,000 decisions at concurrency 1 and 4,000 at concurrency 32: 5,006 commands down to 17.

[aibrix#3466](https://github.com/vllm-project/aibrix/pull/3466) pins that count as a test, so a future change cannot quietly reintroduce the fan-out.

### 2. Correctness bugs that only appear under state, not on the happy path

- [llm-d-router#3378](https://github.com/llm-d/llm-d-router/pull/3378): two models whose name and cache salt concatenate to the same bytes shared a block hash seed, so their prefix caches collided. Fixed by length-prefixing each field.
- [llm-d-router#3950](https://github.com/llm-d/llm-d-router/pull/3950): requests shed by the probabilistic admitter were recorded as failures instead of rejections, which quietly poisoned the outcome metric.
- [llm-d-router#3944](https://github.com/llm-d/llm-d-router/pull/3944): a flow aged from its first connection instead of its last, so long-lived clients were timed out early.
- [volcano#6205](https://github.com/volcano-sh/volcano/pull/6205): rolling back an eviction re-ran the allocate handlers and left a victim pod restored but inconsistent in the predicates node info.
- [kgateway#14987](https://github.com/kgateway-dev/kgateway/pull/14987): the chart baked `GOMAXPROCS` from the CPU limit at container start, so an in-place limit change left the process sized for the old machine.

### 3. Repo wide maintenance that keeps a project honest

[fluxcd/pkg#1382](https://github.com/fluxcd/pkg/pull/1382), with [flux2#6124](https://github.com/fluxcd/flux2/pull/6124), [go-git-providers#331](https://github.com/fluxcd/go-git-providers/pull/331), and [image-automation-controller#1125](https://github.com/fluxcd/image-automation-controller/pull/1125): run `go fix` across every module as part of `make test`, looping until the diff stops changing, so a stale rewrite fails the dirty check instead of accumulating. 125 Go files across 30 modules.

### 4. A test suite anyone can run in a minute

[openai-gateway-conformance](https://github.com/saylee21/openai-gateway-conformance): CPU-first contract tests for OpenAI-compatible gateways. Not "does the JSON parse", but the promises a client depends on. A cut answer says `length`, usage adds up, request ids survive the hop, a stream has exactly one finish event, buffered proxies cannot fake a fast first token.

20 of 20 tests pass against a contract-correct mock, 11 of 11 injected faults are caught, and LiteLLM 1.108.0 passes 16 of 20 with the same four gaps first recorded on 1.106.0. Every run is published as a report.

### 5. Textbooks I wrote to learn the field

[inference-infra-from-zero](https://github.com/saylee21/inference-infra-from-zero): day-long public textbooks on inference infrastructure, built from measurements rather than slides, CC BY 4.0.

---

## Everything else merged upstream

**[llm-d-router](https://github.com/llm-d/llm-d-router) (11):** the three fixes listed above, plus [chart ConfigMap name scoped to the release](https://github.com/llm-d/llm-d-router/pull/4014), [token producer backend selected explicitly](https://github.com/llm-d/llm-d-router/pull/3688), [abandoned enqueue cancelled](https://github.com/llm-d/llm-d-router/pull/3674), [default EPP config asserted](https://github.com/llm-d/llm-d-router/pull/3673), [utilization filter covered under prefix affinity](https://github.com/llm-d/llm-d-router/pull/3672), [per request producer state released on early return](https://github.com/llm-d/llm-d-router/pull/3484), [SGLang model name read and rewritten](https://github.com/llm-d/llm-d-router/pull/3375), [explicit zero cache read kept in Anthropic usage](https://github.com/llm-d/llm-d-router/pull/3803)

**[vLLM AIBrix](https://github.com/vllm-project/aibrix) (8):** the Redis pair above, plus [TPOT histogram divided by its count](https://github.com/vllm-project/aibrix/pull/3332), [audio speech requests routed through the gateway](https://github.com/vllm-project/aibrix/pull/3024), [timezone dependent job expiry test de-flaked](https://github.com/vllm-project/aibrix/pull/3000), [non ASCII filenames in Files API downloads](https://github.com/vllm-project/aibrix/pull/2979), [read size limit enforced on bytes actually read](https://github.com/vllm-project/aibrix/pull/2977), [job restore after a metadata crash de-flaked](https://github.com/vllm-project/aibrix/pull/2959)

**[kueue](https://github.com/kubernetes-sigs/kueue) (3):** [every unhealthy node dropped by the new TAS assignment](https://github.com/kubernetes-sigs/kueue/pull/18118), [Workload update log keys spelled out](https://github.com/kubernetes-sigs/kueue/pull/17782), [ProvisioningRequest retry loop documented](https://github.com/kubernetes-sigs/kueue/pull/17641)

**[kgateway](https://github.com/kgateway-dev/kgateway) (2):** [leader election durations configurable](https://github.com/kgateway-dev/kgateway/pull/14828), plus the GOMAXPROCS fix above

**[Volcano](https://github.com/volcano-sh/volcano) (1):** [unevicted victim restored in the predicates node info](https://github.com/volcano-sh/volcano/pull/6205)

**[Flux](https://github.com/fluxcd) (4):** the `go fix` change above, landed in pkg, flux2, go-git-providers, and image-automation-controller

---

## How I work

- **Reproduce before fixing.** A failing test or a minimal script first, so the fix has something to prove.
- **Measure, do not estimate.** The Redis number above is from two thousand decisions on real Redis, not from reading the code.
- **Send it upstream.** Every fix here lives in the project that owns the bug, under review by that project's maintainers.
- **Write down what the result does not prove.** Numbers without limits get quoted as guarantees.

## Stack

Go, Python, Kubernetes, vLLM and SGLang, OpenAI-compatible gateways, SSE, Redis, Postgres.

---

**Open to inference infrastructure and platform roles, full-time or contract.** Remote or India. Fastest way to reach me is [X](https://x.com/say_lee_x), or open an issue on anything above.
