# Arch 06 — Asymmetric Causal Encoder-Decoder

A deep technical dive into the Causal Encoder-Decoder (CED) of DeepSeek-V4.1-Flash (arXiv:2609.19969): a 40-layer model split into a 20-layer causal encoder and a 20-layer decoder whose global KV is projected from the last encoder hidden state, so prefill runs only the encoder (8B activated parameters per token) while decode runs the whole model (16B). It covers decoder-only versus classic encoder-decoder (T5) versus CED, where the decoder's KV comes from, the replay window that keeps the decoder's sliding-window state, activated parameters and FLOPs per token, the KV-cache saving with the arithmetic shown, serving it with asymmetric prefill and decode pools (the SGLang RFC), measured pool sizing from a discrete-event simulator, trade-offs and open questions, and how CED relates to cross-layer KV sharing (YOCO, CLA, CSA2).

**Live site:** https://brendanjameslynskey.github.io/Arch_06_Asymmetric_Causal_Encoder_Decoder/

Every model number is taken from the paper and cited by section or page. Every simulator number comes from [examples/results.md](https://github.com/BrendanJamesLynskey/Disaggregated_Inference_Sim/blob/main/examples/results.md) in [Disaggregated_Inference_Sim](https://github.com/BrendanJamesLynskey/Disaggregated_Inference_Sim) (sections 16&ndash;18, the `--ced-*` options), measured on a dense Llama-3-70B-shape proxy split 40 + 40. The proxy and the agent-loop prompt:output ratios are illustrative, not DeepSeek-V4.1-Flash itself.

Part of the [Modern Architectures sub-hub](https://github.com/BrendanJamesLynskey/LLM_Hub_Modern_Architectures)
