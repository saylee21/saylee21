## Saylee Bharsakle

**Kubernetes-native LLM inference.** I work on the layer between the request and the model: scheduling, routing, gateways, and serving. When it breaks, I fix it and send the fix upstream.

365 days of inference infra, in public → [@say_lee_x](https://x.com/say_lee_x)

**Now:** going deep on the Kubernetes inference stack — [kueue](https://github.com/kubernetes-sigs/kueue) (GPU job scheduling), [llm-d](https://github.com/llm-d/llm-d) (CNCF, K8s-native LLM serving), [llm-d-router](https://github.com/llm-d/llm-d-router) (SGLang routing), [litellm](https://github.com/BerriAI/litellm) (LLM gateway), and [vllm-project/aibrix](https://github.com/vllm-project/aibrix) (vLLM on Kubernetes). Also contributing to [argo-cd](https://github.com/argoproj/argo-cd), [crossplane](https://github.com/crossplane/crossplane), [kgateway](https://github.com/kgateway-dev/kgateway) (CNCF), and [Envoy Gateway](https://github.com/envoyproxy/gateway).

34 PRs contributed · 7 merged upstream · 11 in review

### Merged upstream

| PR | What it fixed |
|---|---|
| [llm-d-router#3378](https://github.com/llm-d/llm-d-router/pull/3378) | separate the model name and cache salt in the prefix-cache block hash, so distinct models no longer collide |
| [llm-d-router#3375](https://github.com/llm-d/llm-d-router/pull/3375) | read and rewrite the model name in the SGLang `/generate` parser |
| [aibrix#3024](https://github.com/vllm-project/aibrix/pull/3024) | route OpenAI-compatible `/v1/audio/speech` requests through the gateway |
| [aibrix#3000](https://github.com/vllm-project/aibrix/pull/3000) | de-flake a timezone-dependent job expiry test |
| [aibrix#2979](https://github.com/vllm-project/aibrix/pull/2979) | stop non-ASCII filenames from breaking Files API downloads |
| [aibrix#2977](https://github.com/vllm-project/aibrix/pull/2977) | actually enforce the read size limit instead of only checking it up front |
| [aibrix#2959](https://github.com/vllm-project/aibrix/pull/2959) | de-flake job restore after a metadata crash |

### Under review

- [kueue#17436](https://github.com/kubernetes-sigs/kueue/pull/17436): reject preferred topology for Jobs in the scheduler-library deep integration
- [llm-d#2869](https://github.com/llm-d/llm-d/pull/2869): cache multimodal-serving model weights per node on GKE
- [llm-d-router#3484](https://github.com/llm-d/llm-d-router/pull/3484): release per-request producer state on early return
- [litellm#45967](https://github.com/BerriAI/litellm/pull/45967): stop json_mode from reporting truncated output as a clean stop
- [litellm#45423](https://github.com/BerriAI/litellm/pull/45423): keep `delta.refusal` chunks instead of dropping them
- [litellm#45395](https://github.com/BerriAI/litellm/pull/45395): import soundfile lazily in the transcribe logging handler
- [argo-cd#30312](https://github.com/argoproj/argo-cd/pull/30312): prevent a running sync from clobbering a terminating operation
- [crossplane#7945](https://github.com/crossplane/crossplane/pull/7945): unblock foreground deletion of using resources
- [kgateway#14815](https://github.com/kgateway-dev/kgateway/pull/14815): populate GatewayExtension status with an Accepted condition
- [kgateway#14828](https://github.com/kgateway-dev/kgateway/pull/14828): make leader election durations configurable
- [envoy-gateway#10323](https://github.com/envoyproxy/gateway/pull/10323): set `__Host-` nonce and code verifier cookies to `Path=/`

### Stack

Go · Kubernetes · LLM serving (vLLM, SGLang) · API gateways · Redis · Postgres

**Open to platform / ML-infra roles.** The fastest way to reach me is [X](https://x.com/say_lee_x).
