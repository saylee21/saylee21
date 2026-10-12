## Saylee

I work on the layer between the request and the model: routing, gateways, and serving. When it breaks, I fix it and send the fix upstream.

Currently contributing to the [vllm-project/aibrix](https://github.com/vllm-project/aibrix) gateway.

### Recent fixes upstream

- [aibrix#2979](https://github.com/vllm-project/aibrix/pull/2979): stop non-ASCII filenames from breaking file downloads
- [aibrix#2977](https://github.com/vllm-project/aibrix/pull/2977): actually enforce the read size limit instead of only checking it up front
- [aibrix#3000](https://github.com/vllm-project/aibrix/pull/3000): de-flake a timezone-dependent job expiry test
- [aibrix#2959](https://github.com/vllm-project/aibrix/pull/2959): de-flake a job-restore test after a metadata crash

### In flight

- [aibrix#3024](https://github.com/vllm-project/aibrix/pull/3024): add OpenAI-compatible `/v1/audio/speech` routing to the gateway
- [litellm#45967](https://github.com/BerriAI/litellm/pull/45967): stop json_mode from reporting truncated output as a clean stop
