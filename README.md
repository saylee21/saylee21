## Saylee Bharsakle

**Kubernetes-native LLM inference.** I work on the layer between the request and the model: scheduling, routing, gateways, and serving. When it breaks, I reproduce it, measure it, fix it, and send the fix upstream.

Building inference infrastructure in public, one merged fix at a time -> [@say_lee_x](https://x.com/say_lee_x)

**Open to inference infrastructure roles and contracts.** Remote, India.

**Now:** working across the Kubernetes inference stack: [llm-d-router](https://github.com/llm-d/llm-d-router) for prefix-cache-aware routing and KV events, [AIBrix](https://github.com/vllm-project/aibrix) for the OpenAI-compatible gateway and batch/file APIs, [kueue](https://github.com/kubernetes-sigs/kueue) for GPU job scheduling, [KServe](https://github.com/kserve/kserve) for model serving, and [kgateway](https://github.com/kgateway-dev/kgateway) for Gateway API behavior. Also sending fixes to [litellm](https://github.com/BerriAI/litellm), [Knative](https://github.com/knative/serving), [Volcano](https://github.com/volcano-sh/volcano), and [Crossplane](https://github.com/crossplane/crossplane).

**16 merged upstream** across AIBrix, llm-d-router, kueue and kgateway.

### Merged upstream

| PR | What it fixed |
|---|---|
| [llm-d-router#3803](https://github.com/llm-d/llm-d-router/pull/3803) | keep an explicit zero cache read in Anthropic usage |
| [llm-d-router#3688](https://github.com/llm-d/llm-d-router/pull/3688) | select the token producer backend explicitly |
| [llm-d-router#3672](https://github.com/llm-d/llm-d-router/pull/3672) | cover the default utilization filter with prefix affinity |
| [llm-d-router#3674](https://github.com/llm-d/llm-d-router/pull/3674) | cancel an abandoned enqueue when the client can no longer address it |
| [llm-d-router#3484](https://github.com/llm-d/llm-d-router/pull/3484) | release per-request producer state on early return |
| [llm-d-router#3378](https://github.com/llm-d/llm-d-router/pull/3378) | separate the model name and cache salt in the prefix-cache block hash, so distinct models no longer collide |
| [llm-d-router#3375](https://github.com/llm-d/llm-d-router/pull/3375) | read and rewrite the model name in the SGLang `/generate` parser |
| [aibrix#3332](https://github.com/vllm-project/aibrix/pull/3332) | divide the TPOT histogram sum by its count so `avg_tpot_pod_5m` is an average |
| [aibrix#3024](https://github.com/vllm-project/aibrix/pull/3024) | route OpenAI-compatible `/v1/audio/speech` requests through the gateway |
| [aibrix#3000](https://github.com/vllm-project/aibrix/pull/3000) | de-flake a timezone-dependent job expiry test |
| [aibrix#2979](https://github.com/vllm-project/aibrix/pull/2979) | stop non-ASCII filenames from breaking Files API downloads |
| [aibrix#2977](https://github.com/vllm-project/aibrix/pull/2977) | enforce the read size limit on the bytes actually read |
| [aibrix#2959](https://github.com/vllm-project/aibrix/pull/2959) | de-flake job restore after a metadata crash |
| [kueue#17782](https://github.com/kubernetes-sigs/kueue/pull/17782) | spell out `previous` in Workload update log keys |
| [kueue#17641](https://github.com/kubernetes-sigs/kueue/pull/17641) | show the ProvisioningRequest retry loop in the admission cycle docs |
| [kgateway#14828](https://github.com/kgateway-dev/kgateway/pull/14828) | make leader election durations configurable |

### Building now

A public conformance suite for OpenAI-compatible gateways: [openai-gateway-conformance](https://github.com/saylee21/openai-gateway-conformance). It checks the contract, not just the JSON: `finish_reason` after a `max_tokens` cut, `usage` that adds up, request ids that survive the hop, one finish event per stream, usage after finish, non-cumulative deltas, `[DONE]` at the end, and buffered streams that fake a fast first token.

The first version runs 19 tests and 10 fault modes, with a pass/fail matrix. It is CPU-first, so it runs in CI without a GPU. The mock passes 19/19 and catches 10/10 faults. LiteLLM 1.106.0 passes 15/19, with four recorded gaps in error shape and request tracing. Next targets: AIBrix and llm-d-router.

### Selected open work

- [llm-d-router#3702](https://github.com/llm-d/llm-d-router/pull/3702): report live-only pods and snapshot load duration from KV events
- [kueue#18118](https://github.com/kubernetes-sigs/kueue/pull/18118): drop every unhealthy node the new assignment leaves
- [kserve#6517](https://github.com/kserve/kserve/pull/6517): report Ready from the deployment's Available condition
- [knative/serving#16723](https://github.com/knative/serving/pull/16723): fix Revision readiness when image cache reconciliation fails
- [litellm#45967](https://github.com/BerriAI/litellm/pull/45967): stop json_mode from reporting truncated output as a clean stop
- [litellm#45423](https://github.com/BerriAI/litellm/pull/45423): keep `delta.refusal` chunks instead of dropping them
- [kgateway#14815](https://github.com/kgateway-dev/kgateway/pull/14815): populate GatewayExtension status with an Accepted condition

More on [GitHub](https://github.com/saylee21).

### How I work

- Reproduce first: a failing test or a minimal script before the fix.
- Measure the before and after, not just the patch.
- Fix the root cause and send it upstream, with the regression test.

### Stack

Go · Python · Kubernetes · vLLM and SGLang · OpenAI-compatible gateways · SSE · Redis · Postgres

**Open to inference infrastructure and platform roles, full-time or contract.** The fastest way to reach me is [X](https://x.com/say_lee_x).
