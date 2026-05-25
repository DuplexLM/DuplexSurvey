<!-- <div align='center'>
<img src="./asset/title.png" alt="paper-title" style="zoom: 30%;" />
</div> -->

# 🚀 Quick Start

This repository accompanies **A Survey of Full-Duplex Spoken Dialogue Systems: Interaction Ontology, Decision State Machine, and Architectural Hierarchy**.

- [Introduction](#introduction)
- [A Brief History of Full-Duplex Spoken Dialogue Systems](#a-brief-history-of-full-duplex-spoken-dialogue-systems)
- [Architectural Hierarchy and Cross-System Audit](#architectural-hierarchy-and-cross-system-audit)
- [The T × I × R Interaction Ontology](#the-t--i--r-interaction-ontology)
- [The Full-Duplex Decision State Machine](#the-full-duplex-decision-state-machine)
- [Frontiers and Conclusion](#frontiers-and-conclusion)
- [Appendix: Public Resources](#appendix-public-resources)
    - [Publicly Available Full-Duplex Models](#publicly-available-full-duplex-models)
    - [Full-Duplex Datasets](#full-duplex-datasets)
    - [Full-Duplex Benchmarks](#full-duplex-benchmarks)
- [Cite](#cite)

# 🔥What's new

## Introduction

This repository is the official repository of **A Survey of Full-Duplex Spoken Dialogue Systems: Interaction Ontology, Decision State Machine, and Architectural Hierarchy** [[Paper page](./asset/Beyond_Turn_Taking.pdf)] [[Demo](https://duplexlm.github.io/DuplexLM/demo.html)].

> Abstract
>
> Since the demonstration of GPT-4o in May 2024, over a dozen "full-duplex" spoken dialogue systems have emerged, yet each author has a distinct understanding of these systems. Existing surveys collapse them onto a single axis (cascaded/end-to-end, or engineered/learned) and miss the distinctions that matter most for builders. We argue the field's confusion is not architectural but *taxonomical*, and offer three frameworks that together make full-duplex analytically tractable: **(i)** an **L0-L3 Architectural Hierarchy** locating where in the model stack the duplex decision is made (external module, hidden state, token sequence, or shared latent); **(ii)** a **T x I x R Interaction Ontology** that names every interaction a full-duplex system might face along temporal, intent, and response axes; and **(iii)** a **Decision State Machine** (Idle/Listen/Speak/Wait/Dual) that turns "what is the system doing right now" into a shared vocabulary. Across published systems and benchmarks we audit the field through these three lenses and surface two open frontiers —— the open-source-versus-industrial data gap and the still-unrealized L3 representation-level layer —— as the highest-leverage targets for next-generation data construction and full-duplex model design.
> The related material is available at [this https url](https://duplexlm.github.io/DuplexLM/demo.html).

## A Brief History of Full-Duplex Spoken Dialogue Systems

Full-duplex spoken dialogue systems moved from modular pre-LLM pipelines, to speech-token LLMs, to GPT-4o-style live interaction. After LSLM and Moshi, systems quickly split across L0 modular control, L1 hidden-state prediction, L2 token-level synchronization, and still-unrealized L3 representation-level design.

<div align='center'>
<img src="./asset/timeline_pic.png" alt="timeline of full-duplex spoken dialogue systems" style="zoom: 25%;" />

Figure 1: Timeline of published full-duplex spoken dialogue systems, 2021-2026, grouped by the L0-L3 architectural layer defined below.
</div>

## Architectural Hierarchy and Cross-System Audit

This section reframes the architecture debate around one diagnostic question: **where does the system decide whether to listen, speak, wait, or handle overlap?** Labels such as cascaded, end-to-end, engineered, and learned are too coarse for full-duplex systems, because two systems with similar pipelines may make the duplex decision at very different layers. The L0-L3 hierarchy separates this decision locus from the rest of the implementation: L0 keeps it in an external controller; L1 reads LLM hidden states through a sidecar predictor; L2 lets the token sequence itself carry duplex timing; L3 would move the decision into a shared latent representation.

<div align='center'>
<img src="./asset/figure_l0_l3_architecture.png" alt="L0-L3 architectural hierarchy" style="zoom: 25%;" />

Figure 2: The L0-L3 architectural hierarchy. The decision migrates from an external scheduler (L0), to a sidecar predictor reading LLM hidden states (L1), to the token stream itself (L2), and finally to a hypothetical shared latent representation (L3, not yet realized).
</div>

The hierarchy is not a progress ladder. The cross-system audit shows that **L0 remains competitive rather than obsolete**; **L1 has become a broad architectural attractor for production-oriented speech systems**; **L2 is the most populous and fully state-reachable but internally heterogeneous**; and **L3 is still an unrealized architectural frontier**.

<div align='center'>
<img src="./asset/tables/table1_audit.svg" alt="Cross-system audit by L-layer with FSM-state reachability" style="zoom: 100%;" />
</div>

The main lesson is reachability versus realization. Architecture decides which FSM states a system can reach by construction; data and training decide whether it behaves correctly inside those states. Two L2 systems may both reach **Dual**, yet still diverge on T3-I2-R1 backchanneling, because recognizing "uh-huh" as support rather than interruption is learned behavior, not just an architectural affordance.

## The T × I × R Interaction Ontology

If L0-L3 asks where the duplex decision is made, T × I × R asks **what interaction that decision is about**. The core idea is that "overlap" is not a single capability: user speech during assistant speech may be a backchannel, a floor claim, a repair, or irrelevant third-party audio, and each case requires a different system response. The ontology therefore decomposes every moment into three orthogonal axes: temporal relation (T), user intent (I), and required system response (R).

<div align='center'>
<img src="./asset/tables/table2_tir_axes.svg" alt="TIR interaction ontology axes" style="zoom: 100%;" />
</div>

The full grid gives a coordinate system for full-duplex behavior. From it, the paper highlights six acid-test cells: a compact stress suite that exposes the main ways current systems fail.

<div align='center'>
<img src="./asset/tables/six_acid_test_cells.svg" alt="Six acid-test cells for full-duplex interaction" style="zoom: 100%;" />
</div>

<div align='center'>
<img src="./asset/figure_interaction_scenarios.png" alt="six canonical full-duplex interaction scenarios" style="zoom: 25%;" />

Figure 3: Six canonical full-duplex interaction scenarios. Each panel concretizes one (T, I, R) ontology cell on User (U) / Assistant (A) audio timelines.
</div>

These cells are not just examples. They become **data slices**, **benchmark criteria**, and **ablation targets**: a dataset can state which cells it covers, a benchmark can isolate one cell, and a model audit can report exactly where behavior breaks. "Fails on T3-I2-R1" is more useful than "not always full-duplex," because it says the system mistakes a backchannel for an interruption.

## The Full-Duplex Decision State Machine

The decision state machine completes the three-part framework. L0-L3 says where a duplex decision is made, and TIR says which interaction the system is facing; the FSM says **what the system is doing now** and when it should switch modes. This matters because full-duplex behavior is not only a classification problem. A system must keep a running policy over time: listen, speak, wait through hesitation, or integrate overlapping speech without losing the thread.

<div align='center'>
<img src="./asset/tables/table3_state_machine.svg" alt="Five states in the full-duplex decision state machine" style="zoom: 100%;" />
</div>

The five states extend earlier Speak/Listen formulations with two crucial additions: **Wait**, for semantic hesitation where silence does not mean the user is finished, and **Dual**, for the genuinely duplex regime where both speakers produce audio and the system must still interpret the user. The eleven transitions group into onset, turn-handoff, and overlap transitions. Overlap is the hard case: the destination depends on whether the user audio is a backchannel, a floor claim, third-party speech, or another intent.

<div align='center'>
<img src="./asset/figure_interaction_state_machine.png" alt="full-duplex decision state machine" style="zoom: 25%;" />

Figure 4: The full-duplex decision state machine: five states and eleven transitions. Each transition is labelled by its (T, I) trigger and the resulting R action.
</div>

TIR cells map directly to transition traces. T3-I4-R2 becomes Speak → Dual → Listen; T3-I2-R1 and T3-I7-R5 become Speak → Dual → Speak; T5-I6-R3 becomes Listen → Wait → Listen. This gives builders and evaluators a shared language for failure analysis: "the model never enters Wait" or "the Dual → Speak transition confuses backchannel with interruption" is more actionable than saying the model has weak full-duplex ability.

## Frontiers and Conclusion

The audit points to two frontiers. **Data coverage** is the first: architecture may make a state reachable, but public data still under-covers T4 concurrent speech and I7 third-party speech, so many systems cannot learn the hardest cells well. **L3 architecture** is the second: no published system has yet made Dual native to a shared latent representation.

Together, T × I × R, the five-state FSM, and L0-L3 replace "is this full-duplex?" with a structured profile: cells covered, states reached, and implementation layer. The point is not just to survey existing systems, but to give future data construction, benchmarking, and model design a common coordinate system.

## Appendix: Public Resources

### Publicly Available Full-Duplex Models

| Model | URL |
|---|---|
| dGSLM | [https://github.com/facebookresearch/fairseq/tree/main/examples/textless_nlp/dgslm](https://github.com/facebookresearch/fairseq/tree/main/examples/textless_nlp/dgslm) |
| Duplex-Model | [https://github.com/thunlp/duplex-model](https://github.com/thunlp/duplex-model) |
| FireRedChat | [https://github.com/FireRedTeam/FireRedChat](https://github.com/FireRedTeam/FireRedChat) |
| X-Talk | [https://github.com/xcc-zach/xtalk](https://github.com/xcc-zach/xtalk) |
| SoulX-Duplug | [https://github.com/Soul-AILab/SoulX-Duplug](https://github.com/Soul-AILab/SoulX-Duplug) |
| Freeze-Omni | [https://github.com/VITA-MLLM/Freeze-Omni](https://github.com/VITA-MLLM/Freeze-Omni) |
| Fun-Audio-Chat | [https://github.com/FunAudioLLM/Fun-Audio-Chat](https://github.com/FunAudioLLM/Fun-Audio-Chat) |
| Covo-Audio | [https://github.com/Tencent/Covo-Audio](https://github.com/Tencent/Covo-Audio) |
| Step-Audio R1.1 | [https://github.com/stepfun-ai/Step-Audio-R1](https://github.com/stepfun-ai/Step-Audio-R1) |
| Moshi | [https://github.com/kyutai-labs/moshi](https://github.com/kyutai-labs/moshi) |
| Mini-Omni2 | [https://github.com/gpt-omni/mini-omni2](https://github.com/gpt-omni/mini-omni2) |
| DuplexMamba | [https://github.com/khfs/DuplexMamba](https://github.com/khfs/DuplexMamba) |
| SALMONN-omni | [https://github.com/bytedance/SALMONN](https://github.com/bytedance/SALMONN) |

### Full-Duplex Datasets

| Name | TIR coverage | Scale | URL |
|---|---|---:|---|
| Fisher English Training Speech | T1 Sequential / T3 Overlap; I2 Backchannel / I4 Floor-claim / I6 Disfluency; R1 Continue / R2 Stop / R4 Backchannel | ~1,960 h | [LDC Part 1](https://catalog.ldc.upenn.edu/LDC2004S13); [LDC Part 2](https://catalog.ldc.upenn.edu/LDC2005S13) |
| Switchboard-1 Release 2 | T1 Sequential / T3 Overlap / T5 Silence; I2 Backchannel / I3 Repair / I4 Floor-claim / I6 Disfluency; R3 Wait / R4 Backchannel | ~260 h | [LDC97S62](https://catalog.ldc.upenn.edu/LDC97S62) |
| CALLHOME American English Speech | T1 Sequential / T3 Overlap; I4 Floor-claim / I6 Disfluency / I7 Third-party | 120 calls / up to 30 min each | [LDC97S42](https://catalog.ldc.upenn.edu/LDC97S42) |
| HCRC Map Task Corpus | T1 Sequential / T2 Latched / T3 Overlap / T5 Silence; I3 Repair / I5 Floor-yield / I6 Disfluency; R4 Backchannel | 128 dialogues / ~18 h | [Official downloads](https://groups.inf.ed.ac.uk/maptask/maptasknxt.html) |
| Chiba University Japanese Map Task Dialogue Corpus | T1 Sequential / T2 Latched / T3 Overlap / T5 Silence; I2 Backchannel / I3 Repair / I6 Disfluency | 128 dialogues / ~23 h | [NII-SRC](https://research.nii.ac.jp/src/en/MapTask.html) |
| IFA Dialog Video Corpus (IFADV) | T3 Overlap; I2 Backchannel | 20 dialogues / ~5 h | [Official page](https://www.fon.hum.uva.nl/IFA-SpokenLanguageCorpora/IFADVcorpus/) |
| CANDOR Corpus | T1 Sequential / T3 Overlap; I2 Backchannel / I4 Floor-claim | 1,656 conversations / 850+ h | [TalkBank](https://talkbank.org/ca/access/CANDOR.html) |
| Multi-stream Spontaneous Conversation Training Datasets Chinese | T1 Sequential / T3 Overlap; I2 Backchannel / I4 Floor-claim | 27 conversations / ~10 h | [MagicHub](https://magichub.com/datasets/multi-stream-spontaneous-conversation-training-datasets_chinese/) |
| Multi-stream Spontaneous Conversation Training Datasets English | T1 Sequential / T3 Overlap; I2 Backchannel / I4 Floor-claim | 8 conversations / 5 h | [MagicHub](https://magichub.com/datasets/multi-stream-spontaneous-conversation-training-datasets_english/) |
| otoSpeech-full-duplex-280h | T1 Sequential / T3 Overlap; I4 Floor-claim | ~280 h | [Hugging Face](https://huggingface.co/datasets/otoearth/otoSpeech-full-duplex-280h) |
| otoSpeech-full-duplex-processed-141h | T1 Sequential / T3 Overlap; I4 Floor-claim | 141 h | [Hugging Face](https://huggingface.co/datasets/otoearth/otoSpeech-full-duplex-processed-141h) |
| Japanese CALLHOME | T1 Sequential / T3 Overlap / T5 Silence; I2 Backchannel / I6 Disfluency; R4 Backchannel | ~49 h full corpus; J-Moshi uses 16 h | [LDC96S37](https://catalog.ldc.upenn.edu/LDC96S37) |
| AliMeeting | T1 Sequential / T3 Overlap / T5 Silence | 118.75 h | [OpenSLR 119](https://www.openslr.org/119/) |
| MMedFD | T1 Sequential / T3 Overlap; I4 Floor-claim | 39.04 h | [Hugging Face](https://huggingface.co/datasets/HanselZz/MMedFD) |
| HumDial-FDBench / HumDial Track 2 Dataset | T3 Overlap / T5 Silence; I2 Backchannel / I4 Floor-claim / I6 Disfluency / I7 Third-party; R2 Stop / R3 Wait / R5 Ignore | train 8,898 / dev 1,800 / test 5,000 instances; train 107+ h | [Hugging Face](https://huggingface.co/datasets/ASLP-lab/HumDial-FDBench) |

### Full-Duplex Benchmarks

| Name | TIR coverage | Scale | URL |
|---|---|---:|---|
| Full-Duplex-Bench | T3 Overlap; I2 Backchannel / I4 Floor-claim; R2 Stop / R4 Backchannel | 727 samples | [GitHub](https://github.com/DanielLin94144/Full-Duplex-Bench) |
| Full-Duplex-Bench v1.5 | T3 Overlap; I2 Backchannel / I4 Floor-claim / I7 Third-party; R1 Continue / R2 Stop | 499 samples | [GitHub](https://github.com/DanielLin94144/Full-Duplex-Bench/tree/main/v1_v1.5) |
| Full-Duplex-Bench v2 | T3 Overlap; I4 Floor-claim; R2 Stop / R4 Backchannel | 200 tasks | [GitHub](https://github.com/DanielLin94144/Full-Duplex-Bench/tree/main/v2) |
| Full-Duplex-Bench v3 | T3 Overlap; I4 Floor-claim; R2 Stop | 100 examples, 79 unique scenarios, 12 speakers | [GitHub](https://github.com/DanielLin94144/Full-Duplex-Bench/tree/main/v3) |
| FD-Bench | T3 Overlap; I2 Backchannel / I4 Floor-claim; R2 Stop / R4 Backchannel | 293 conversations / 1,196 interruptions / ~40 h | [HF audio input](https://huggingface.co/datasets/pengyizhou/FD-Bench-Audio-Input) |
| MTR-DuplexBench | T3 Overlap; I2 Backchannel / I4 Floor-claim / I7 Third-party; R2 Stop / R4 Backchannel | 200 + 200 + 300 + 520 items | [Hugging Face](https://huggingface.co/datasets/Jeff0918/MTR-DuplexBench) |
| MMedFD | T1 Sequential / T3 Overlap; I4 Floor-claim | 39.04 h | [Hugging Face](https://huggingface.co/datasets/HanselZz/MMedFD) |
| HumDial Challenge Track 2 | T3 Overlap / T5 Silence; I2 Backchannel / I4 Floor-claim / I6 Disfluency / I7 Third-party; R2 Stop / R3 Wait / R5 Ignore | train 8898 / dev 1,800 / test 5,000 instances | [Hugging Face](https://huggingface.co/datasets/ASLP-lab/HumDial-FDBench) |
| τ-Voice | T3 Overlap; I2 Backchannel / I4 Floor-claim / I7 Third-party; R2 Stop / R4 Backchannel / R5 Ignore | 278 tasks | [GitHub](https://github.com/sierra-research/tau2-bench) |
| SID-Bench | T3 Overlap; I2 Backchannel / I4 Floor-claim; R2 Stop | 3,700 instances / ~10 h | [Hugging Face](https://huggingface.co/datasets/kxxia/SID-bench) |
| DualTurn Turn-Taking Datasets | T3 Overlap; I2 Backchannel; R4 Backchannel | OtoSpeech 287 h; Switchboard features 256.7 h | [OtoSpeech HF](https://huggingface.co/datasets/anyreach-ai/dualturn-otospeech-turn-taking); [Switchboard HF](https://huggingface.co/datasets/anyreach-ai/dualturn-switchboard-turn-taking) |

## Cite

```bibtex
TBD
```
