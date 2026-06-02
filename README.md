<h1 align="center">Speaking While Listening</h1>

<h3 align="center">A Survey and Empirical Audit of Full-Duplex Spoken Dialogue Systems</h3>

<p align="center">
  <a href="https://duplexlm.github.io/DuplexLM/demo.html">Demo</a> | Paper coming soon
</p>

# 🚀 Quick Start

This repository is the official repository for **Speaking While Listening: A Survey and Empirical Audit of Full-Duplex Spoken Dialogue Systems**.

- We organize full-duplex spoken dialogue systems with three lenses: the **L0-L3 Architectural Hierarchy**, the **T x I x R Interaction Ontology**, and the **Full-Duplex Decision State Machine**.
- We provide visual summaries for the system timeline, architectural hierarchy, interaction scenarios, state machine, and cross-system audit.
- We collect public full-duplex models, datasets, and benchmarks for follow-up research.

# 🔥What's new

- 🎉 **[2026/06] We have officially released this survey repository for Full-Duplex Spoken Dialogue Systems!**

## Overview

More than a dozen recent spoken dialogue systems have claimed full-duplex capability, but the term is used for systems with substantially different behaviors. This project organizes the space with three complementary frameworks:

- **L0-L3 Architectural Hierarchy**: where the duplex decision is made, from external controllers to hidden states, token streams, and future shared latent representations.
- **T x I x R Interaction Ontology**: what interaction the system must handle, decomposed into temporal relation, user intent, and system response.
- **Decision State Machine**: what the system is doing moment by moment across Idle, Listen, Speak, Wait, and Dual states.

Together, these frameworks turn the vague question "is this system full-duplex?" into a concrete profile: which interaction cells are covered, which states are reachable, and where the decision lives in the model stack.

## Contents

- [Quick Start](#-quick-start)
- [What's new](#whats-new)
- [Overview](#overview)
- [A Brief History of Full-Duplex Spoken Dialogue Systems](#a-brief-history-of-full-duplex-spoken-dialogue-systems)
- [Architectural Hierarchy and Cross-System Audit](#architectural-hierarchy-and-cross-system-audit)
- [The T x I x R Interaction Ontology](#the-t-x-i-x-r-interaction-ontology)
- [The Full-Duplex Decision State Machine](#the-full-duplex-decision-state-machine)
- [Frontiers and Conclusion](#frontiers-and-conclusion)
- [Appendix: Public Resources](#appendix-public-resources)
  - [Publicly Available Full-Duplex Models](#publicly-available-full-duplex-models)
  - [Full-Duplex Datasets](#full-duplex-datasets)
  - [Full-Duplex Benchmarks](#full-duplex-benchmarks)
- [Citation](#citation)

## A Brief History of Full-Duplex Spoken Dialogue Systems

Full-duplex spoken dialogue systems have moved from modular pre-LLM pipelines, to speech-token LLMs, to GPT-4o-style live interaction. After LSLM and Moshi, the field quickly split across L0 modular control, L1 hidden-state prediction, L2 token-level synchronization, and the still-unrealized L3 representation-level design.

<div align="center">
<img src="./asset/timeline_pic.png" alt="timeline of full-duplex spoken dialogue systems" width="90%" />

*Figure 1: Timeline of published full-duplex spoken dialogue systems, 2021-2026, grouped by the L0-L3 architectural layer defined below.*
</div>

## Architectural Hierarchy and Cross-System Audit

This section reframes architecture around one diagnostic question: **where does the system decide whether to listen, speak, wait, or handle overlap?** Cascaded/end-to-end and engineered/learned labels are too coarse for this purpose. The L0-L3 hierarchy instead locates duplex decisions in an external controller, a hidden-state predictor, the token sequence, or a shared latent representation.

The hierarchy is not a progress ladder. Our audit shows that **L0 remains competitive**, **L1 has become an industrial attractor**, **L2 is the largest and most Dual-capable family but remains internally heterogeneous**, and **L3 is still open**.

<div align="center">
<img src="./asset/figure_l0_l3_architecture.png" alt="L0-L3 architectural hierarchy" width="90%" />

*Figure 2: The L0-L3 architectural hierarchy. The decision migrates from an external scheduler (L0), to a sidecar predictor reading LLM hidden states (L1), to the token stream itself (L2), and finally to a hypothetical shared latent representation (L3).*
</div>

<div align="center">
<img src="./asset/tables/table1_audit.svg" alt="Cross-system audit by L-layer with FSM-state reachability" width="90%" />
</div>


Main lesson: architecture gives reachability; data and training determine realization. Two L2 systems may both reach **Dual**, yet still diverge on T3-I2-R1 backchanneling.

## The T x I x R Interaction Ontology

L0-L3 asks *where* the duplex decision lives. T x I x R asks **what interaction that decision must handle**. "Overlap" is not one capability: user speech during system output may be a backchannel, a floor claim, a repair, or third-party audio, and each case demands a different response. The ontology splits every moment along three axes: temporal relation (T), user intent (I), and system response (R).

<div align="center">
<img src="./asset/tables/table2_tir_axes.svg" alt="TIR interaction ontology axes" width="70%" />
</div>


We highlight six acid-test cells: a compact stress suite that exposes the main ways current systems fail.

<div align="center">
<img src="./asset/tables/six_acid_test_cells.svg" alt="Six acid-test cells for full-duplex interaction" width="70%" />
</div>

<div align="center">
<img src="./asset/figure_interaction_scenarios.png" alt="six canonical full-duplex interaction scenarios" width="80%" />

*Figure 3: Six canonical full-duplex interaction scenarios. Each panel concretizes one (T, I, R) ontology cell on User (U) / Assistant (A) audio timelines.*
</div>

These cells can be used directly as **data slices**, **benchmark criteria**, and **ablation targets**. "Fails on T3-I2-R1" is more precise than "not always full-duplex": it says the system mistakes a backchannel for an interruption.

## The Full-Duplex Decision State Machine

The FSM closes the three-part framework. L0-L3 says *where* the decision lives, TIR says *what interaction* must be handled, and the FSM says **what the system is doing right now** and when it should switch. TIR names the interaction cell; the FSM turns that cell into a policy trace over time.

<div align="center">
<img src="./asset/tables/table3_state_machine.svg" alt="Five states in the full-duplex decision state machine" width="80%" />
</div>


Five states extend the simple Speak/Listen view with two key additions: **Wait**, where silence does not mean the user has finished; and **Dual**, where both speakers are active and the system must still interpret the user. Eleven transitions cover onset, turn-handoff, and overlap. Overlap is the hard case: backchannel, floor claim, and third-party speech each lead to different destinations.

<div align="center">
<img src="./asset/figure_interaction_state_machine.png" alt="full-duplex decision state machine" width="90%" />

*Figure 4: The full-duplex decision state machine: five states and eleven transitions.*
</div>

## Frontiers and Conclusion

The audit leaves two open frontiers. **Data coverage**: architecture can make a state reachable, but public corpora still underrepresent T4 concurrent speech and I7 third-party speech, so many systems cannot learn the hardest cells. **L3 architecture**: no published system yet makes Dual native to a shared latent representation.

Together, T x I x R, the five-state FSM, and L0-L3 replace "is this full-duplex?" with a structured profile: cells covered, states reached, and implementation layer. The same profile explains past systems and points toward future data construction, benchmarks, and model design.

---

## Appendix: Public Resources

### Publicly Available Full-Duplex Models

| Model | Code Repository |
|---|---|
| **dGSLM** | [🔗 fairseq/dgslm](https://github.com/facebookresearch/fairseq/tree/main/examples/textless_nlp/dgslm) |
| **Duplex-Model** | [🔗 thunlp/duplex-model](https://github.com/thunlp/duplex-model) |
| **FireRedChat** | [🔗 FireRedTeam/FireRedChat](https://github.com/FireRedTeam/FireRedChat) |
| **X-Talk** | [🔗 xcc-zach/xtalk](https://github.com/xcc-zach/xtalk) |
| **SoulX-Duplug** | [🔗 Soul-AILab/SoulX-Duplug](https://github.com/Soul-AILab/SoulX-Duplug) |
| **Freeze-Omni** | [🔗 VITA-MLLM/Freeze-Omni](https://github.com/VITA-MLLM/Freeze-Omni) |
| **Fun-Audio-Chat** | [🔗 FunAudioLLM/Fun-Audio-Chat](https://github.com/FunAudioLLM/Fun-Audio-Chat) |
| **Covo-Audio** | [🔗 Tencent/Covo-Audio](https://github.com/Tencent/Covo-Audio) |
| **Step-Audio R1.1**| [🔗 stepfun-ai/Step-Audio-R1](https://github.com/stepfun-ai/Step-Audio-R1) |
| **Moshi** | [🔗 kyutai-labs/moshi](https://github.com/kyutai-labs/moshi) |
| **Mini-Omni2** | [🔗 gpt-omni/mini-omni2](https://github.com/gpt-omni/mini-omni2) |
| **DuplexMamba** | [🔗 khfs/DuplexMamba](https://github.com/khfs/DuplexMamba) |
| **SALMONN-omni** | [🔗 bytedance/SALMONN](https://github.com/bytedance/SALMONN) |


### Full-Duplex Datasets

> *Note: For TIR abbreviations, T = Temporal Relation, I = User Intent, R = System Response. Refer to [The T x I x R Interaction Ontology](#the-t-x-i-x-r-interaction-ontology) for detailed definitions.*

| Name | TIR Coverage | Scale | URL |
|---|---|---:|---|
| **Fisher English Training** | T1, T3<br>I2, I4, I6<br>R1, R2, R4 | ~1,960 h | [🔗 Part 1](https://catalog.ldc.upenn.edu/LDC2004S13)<br>[🔗 Part 2](https://catalog.ldc.upenn.edu/LDC2005S13) |
| **Switchboard-1 Release 2** | T1, T3, T5<br>I2, I3, I4, I6<br>R3, R4 | ~260 h | [🔗 LDC97S62](https://catalog.ldc.upenn.edu/LDC97S62) |
| **CALLHOME Am. English** | T1, T3<br>I4, I6, I7 | 120 calls<br>(≤30 min/ea) | [🔗 LDC97S42](https://catalog.ldc.upenn.edu/LDC97S42) |
| **HCRC Map Task Corpus** | T1, T2, T3, T5<br>I3, I5, I6<br>R4 | 128 dialogues<br>(~18 h) | [🔗 Official](https://groups.inf.ed.ac.uk/maptask/maptasknxt.html) |
| **Chiba Univ. JP Map Task** | T1, T2, T3, T5<br>I2, I3, I6 | 128 dialogues<br>(~23 h) | [🔗 NII-SRC](https://research.nii.ac.jp/src/en/MapTask.html) |
| **IFADV Corpus** | T3<br>I2 | 20 dialogues<br>(~5 h) | [🔗 Official](https://www.fon.hum.uva.nl/IFA-SpokenLanguageCorpora/IFADVcorpus/) |
| **CANDOR Corpus** | T1, T3<br>I2, I4 | 1,656 convs<br>(850+ h) | [🔗 TalkBank](https://talkbank.org/ca/access/CANDOR.html) |
| **Multi-stream Spon. (ZH)** | T1, T3<br>I2, I4 | 27 convs<br>(~10 h) | [🔗 MagicHub](https://magichub.com/datasets/multi-stream-spontaneous-conversation-training-datasets_chinese/) |
| **Multi-stream Spon. (EN)** | T1, T3<br>I2, I4 | 8 convs<br>(5 h) | [🔗 MagicHub](https://magichub.com/datasets/multi-stream-spontaneous-conversation-training-datasets_english/) |
| **otoSpeech-FD-280h** | T1, T3<br>I4 | ~280 h | [🔗 HuggingFace](https://huggingface.co/datasets/otoearth/otoSpeech-full-duplex-280h) |
| **otoSpeech-FD-141h** | T1, T3<br>I4 | 141 h | [🔗 HuggingFace](https://huggingface.co/datasets/otoearth/otoSpeech-full-duplex-processed-141h) |
| **Japanese CALLHOME** | T1, T3, T5<br>I2, I6<br>R4 | ~49 h | [🔗 LDC96S37](https://catalog.ldc.upenn.edu/LDC96S37) |
| **AliMeeting** | T1, T3, T5 | 118.75 h | [🔗 OpenSLR](https://www.openslr.org/119/) |
| **MMedFD** | T1, T3<br>I4 | 39.04 h | [🔗 HuggingFace](https://huggingface.co/datasets/HanselZz/MMedFD) |
| **HumDial-FDBench (Tr.2)** | T3, T5<br>I2, I4, I6, I7<br>R2, R3, R5 | Train 107+ h | [🔗 HuggingFace](https://huggingface.co/datasets/ASLP-lab/HumDial-FDBench) |


### Full-Duplex Benchmarks

| Name | TIR Coverage | Scale | URL |
|---|---|---:|---|
| **Full-Duplex-Bench** | T3<br>I2, I4<br>R2, R4 | 727 samples | [🔗 GitHub](https://github.com/DanielLin94144/Full-Duplex-Bench) |
| **FD-Bench v1.5** | T3<br>I2, I4, I7<br>R1, R2 | 499 samples | [🔗 GitHub](https://github.com/DanielLin94144/Full-Duplex-Bench/tree/main/v1_v1.5) |
| **FD-Bench v2** | T3<br>I4<br>R2, R4 | 200 tasks | [🔗 GitHub](https://github.com/DanielLin94144/Full-Duplex-Bench/tree/main/v2) |
| **FD-Bench v3** | T3<br>I4<br>R2 | 100 examples | [🔗 GitHub](https://github.com/DanielLin94144/Full-Duplex-Bench/tree/main/v3) |
| **FD-Bench (Audio)** | T3<br>I2, I4<br>R2, R4 | ~40 h | [🔗 HuggingFace](https://huggingface.co/datasets/pengyizhou/FD-Bench-Audio-Input) |
| **MTR-DuplexBench** | T3<br>I2, I4, I7<br>R2, R4 | 1,220 items | [🔗 HuggingFace](https://huggingface.co/datasets/Jeff0918/MTR-DuplexBench) |
| **MMedFD** | T1, T3<br>I4 | 39.04 h | [🔗 HuggingFace](https://huggingface.co/datasets/HanselZz/MMedFD) |
| **HumDial Track 2** | T3, T5<br>I2, I4, I6, I7<br>R2, R3, R5 | Test 5,000 inst. | [🔗 HuggingFace](https://huggingface.co/datasets/ASLP-lab/HumDial-FDBench) |
| **τ-Voice** | T3<br>I2, I4, I7<br>R2, R4, R5 | 278 tasks | [🔗 GitHub](https://github.com/sierra-research/tau2-bench) |
| **SID-Bench** | T3<br>I2, I4<br>R2 | ~10 h | [🔗 HuggingFace](https://huggingface.co/datasets/kxxia/SID-bench) |
| **DualTurn Turn-Taking** | T3<br>I2<br>R4 | Oto: 287 h<br>SW: 256 h | [🔗 Oto HF](https://huggingface.co/datasets/anyreach-ai/dualturn-otospeech-turn-taking)<br>[🔗 SW HF](https://huggingface.co/datasets/anyreach-ai/dualturn-switchboard-turn-taking) |

## Citation

```bibtex
TBD
