## Saylee

I work on the layer between the request and the model: routing, gateways, and serving. When it breaks, I fix it and send the fix upstream.

Contributing to [llm-d](https://github.com/llm-d) (CNCF), [kgateway](https://github.com/kgateway-dev/kgateway) (CNCF), [vllm-project/aibrix](https://github.com/vllm-project/aibrix), and [litellm](https://github.com/BerriAI/litellm).

### Merged upstream

- [llm-d-router#3378](https://github.com/llm-d/llm-d-router/pull/3378): separate the model name and cache salt in the prefix-cache block hash seed so distinct models no longer collide
- [llm-d-router#3375](https://github.com/llm-d/llm-d-router/pull/3375): read and rewrite the model name in the SGLang `/generate` parser
- [aibrix#3024](https://github.com/vllm-project/aibrix/pull/3024): route OpenAI-compatible `/v1/audio/speech` requests through the gateway
- [aibrix#3000](https://github.com/vllm-project/aibrix/pull/3000): de-flake a timezone-dependent job expiry test
- [aibrix#2979](https://github.com/vllm-project/aibrix/pull/2979): stop non-ASCII filenames from breaking file downloads
- [aibrix#2977](https://github.com/vllm-project/aibrix/pull/2977): actually enforce the read size limit instead of only checking it up front
- [aibrix#2959](https://github.com/vllm-project/aibrix/pull/2959): de-flake a job-restore test after a metadata crash

### In flight

- [kgateway#14828](https://github.com/kgateway-dev/kgateway/pull/14828): make the controller leader election timings configurable
- [kgateway#14815](https://github.com/kgateway-dev/kgateway/pull/14815): report an Accepted condition on GatewayExtension
- [litellm#45967](https://github.com/BerriAI/litellm/pull/45967): stop json_mode from reporting truncated output as a clean stop
- [litellm#45524](https://github.com/BerriAI/litellm/pull/45524): forward the per-turn-control beta for bedrock
- [litellm#45423](https://github.com/BerriAI/litellm/pull/45423): keep `delta.refusal` chunks instead of dropping them
- [litellm#45395](https://github.com/BerriAI/litellm/pull/45395): import soundfile lazily in the transcribe logging handler
