<div align='center'>
<img src="./asset/title.png" alt="paper-title" style="zoom: 30%;" />
</div>

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
> Since the demonstration of GPT-4o in May 2024, over a dozen "full-duplex" spoken dialogue systems have emerged, yet each author has a distinct understanding of these systems. Existing surveys collapse them onto a single axis (cascaded/end-to-end, or engineered/learned) and miss the distinctions that matter most for builders. We argue the field's confusion is not architectural but *taxonomical*, and offer three frameworks that together make full-duplex analytically tractable: **(i)** an **L0--L3 Architectural Hierarchy** locating where in the model stack the duplex decision is made (external module, hidden state, token sequence, or shared latent); **(ii)** a **T x I x R Interaction Ontology** that names every interaction a full-duplex system might face along temporal, intent, and response axes; and **(iii)** a **Decision State Machine** (Idle/Listen/Speak/Wait/Dual) that turns "what is the system doing right now" into a shared vocabulary. Across published systems and benchmarks we audit the field through these three lenses and surface two open frontiers —— the open-source-versus-industrial data gap and the still-unrealized L3 representation-level layer —— as the highest-leverage targets for next-generation data construction and full-duplex model design.
> The related material is available at [this https url](https://duplexlm.github.io/DuplexLM/demo.html).

## A Brief History of Full-Duplex Spoken Dialogue Systems

The history of full-duplex spoken dialogue systems can be read as three eras separated by two pivots. Before the LLM era, industrial modular pipelines such as Google Duplex, Ant Group's Duplex Conversation, and Alibaba DAMO's outbound-call systems had already deployed VAD, end-of-turn prediction, and dialogue-management stacks at production scale. The first pivot came when speech became a token, enabling end-to-end speech-text LLMs. The second pivot came when GPT-4o's May 2024 demonstration made full-duplex interaction a visible product expectation.

After LSLM and Moshi opened the LLM-era full-duplex wave in 2024, the field quickly fragmented into several architectural patterns: L0 modular controllers, L1 hidden-state predictors, L2 token-level synchronization, and the still-unrealized L3 representation-level layer. This proliferation motivates the rest of the survey: systems share the same "full-duplex" label, but differ in where the duplex decision is made, which interactions they can handle, and how they transition between dialogue states.

<div align='center'>
<img src="./asset/timeline_pic.png" alt="timeline of full-duplex spoken dialogue systems" style="zoom: 25%;" />

Figure 1: Timeline of published full-duplex spoken dialogue systems, 2021-2026, grouped by the L0-L3 architectural layer defined below.
</div>

## Architectural Hierarchy and Cross-System Audit

The L0--L3 hierarchy locates **where** a full-duplex system makes its duplex decision. This layer is the first organizing framework of the survey because it directly compares systems that otherwise share the same "full-duplex" label but implement the decision at different points in the model stack.

<div align='center'>
<img src="./asset/figure_l0_l3_architecture.png" alt="L0-L3 architectural hierarchy" style="zoom: 25%;" />

Figure 2: The L0--L3 architectural hierarchy. The decision migrates from an external scheduler (L0), to a sidecar predictor reading LLM hidden states (L1), to the token stream itself (L2), and finally to a hypothetical shared latent representation (L3, not yet realized).
</div>

Two observations are central. First, **L1 is a broad architectural attractor**: systems such as MinMo, Freeze-Omni, Fun-Audio-Chat, and Covo-Audio use the hidden-state-level pattern for full-duplex dialogue, while Qwen2.5/3.5-Omni's Thinker--Talker shows the same structural shape for streaming text+speech generation without explicitly claiming full-duplex behavior. Second, **L0 remains contested rather than legacy**: modular systems such as FireRedChat, FlexDuo, X-Talk, and SoulX-Duplug remain competitive for latency, interpretability, and engineering cost.

<div align='center'>
<img src="./asset/tables/table1_audit.svg" alt="Cross-system audit by L-layer with FSM-state reachability" style="zoom: 100%;" />
</div>

The table also gives the section's main caution: architectural reachability is necessary but not sufficient. Two L2 systems may both reach the **Dual** state in principle, yet behave differently on the T3-I2-R1 backchannel cell because reaching a state is a function of design, while traversing it correctly is a function of training data.

## The T × I × R Interaction Ontology

The L0--L3 hierarchy tells us **where** the duplex decision is made; the T × I × R ontology tells us **which** interaction the system is facing. We decompose every conversational moment into a triple: the temporal relation between user and system audio, the communicative intent of the user's contribution, and the required system response. The three axes are deliberately orthogonal: timing, meaning, and policy are separate design questions.

<div align='center'>
<img src="./asset/tables/table2_tir_axes.svg" alt="TIR interaction ontology axes" style="zoom: 100%;" />
</div>

For exposition and evaluation, the survey highlights six canonical cells that exercise the main failure modes of current full-duplex systems:

<div align='center'>
<img src="./asset/tables/six_acid_test_cells.svg" alt="Six acid-test cells for full-duplex interaction" style="zoom: 100%;" />
</div>

<div align='center'>
<img src="./asset/figure_interaction_scenarios.png" alt="six canonical full-duplex interaction scenarios" style="zoom: 25%;" />

Figure 3: Six canonical full-duplex interaction scenarios. Each panel concretizes one (T, I, R) ontology cell on User (U) / Assistant (A) audio timelines.
</div>

The ontology serves as a shared substrate throughout the survey. The same cells can be used as **data slices** for corpus construction, **evaluation criteria** for benchmarks, and **ablation targets** for system analysis. A claim such as "System X fails on T3-I2-R1" is more useful than the binary statement "System X is not always full-duplex."

## The Full-Duplex Decision State Machine

The L0--L3 hierarchy names **where** the duplex decision is made, and the T × I × R ontology names **which** interaction the system faces. The decision state machine names **what the system is doing now** and when it should switch modes. It completes the survey's vocabulary: hierarchy for implementation, ontology for situation, and state machine for policy.

<div align='center'>
<img src="./asset/tables/table3_state_machine.svg" alt="Five states in the full-duplex decision state machine" style="zoom: 100%;" />
</div>

The state machine contains eleven transitions, grouped into three families: **onset transitions** such as Idle → Listen and Idle → Speak, **turn-handoff transitions** such as Listen → Speak and Wait → Speak, and **overlap transitions** such as Speak → Dual → Speak or Speak → Dual → Listen. The overlap transitions are the duplex-specific part of the machine: their destination depends on whether the overlapping user audio is a backchannel, a floor claim, third-party speech, or another intent.

<div align='center'>
<img src="./asset/figure_interaction_state_machine.png" alt="full-duplex decision state machine" style="zoom: 25%;" />

Figure 4: The full-duplex decision state machine: five states and eleven transitions. Each transition is labelled by its (T, I) trigger and the resulting R action.
</div>

The state machine and the TIR ontology compose directly. For example, the cooperative-barge-in cell T3-I4-R2 maps to a Speak → Dual → Listen transition, while the backchannel cell T3-I2-R1 maps to a brief Speak → Dual → Speak excursion. Third-party speech T3-I7-R5 has the same transition shape as backchanneling but requires a different intent classification, and hesitation T5-I6-R3 maps to a Listen → Wait → Listen loop. This mapping turns "is this system full-duplex?" into a structured profile of states reached, transitions supported, and ontology cells covered.

## Frontiers and Conclusion

The survey identifies two independent frontiers for full-duplex spoken dialogue systems.

**Data is the first frontier.** Within each L-layer, the gap between which states a system can architecturally reach and which ontology cells it actually handles well remains wide. Two L2 systems with similar decoders can behave differently on T3-I2-R1 backchanneling or T3-I7-R5 third-party speech, and the most common cause is training data rather than architecture. Public corpora are still dominated by T1/T2 turn-taking cells, while T4 concurrent speech and I7 third-party speech are systematically under-represented in academic data.

**L3 is the second frontier.** The empty L3 row is the most concrete architectural frontier in the survey. A successful L3 system would dissolve the token-level boundary between perception and generation and make the Dual state native to a shared latent representation. Whether this is best reached through joint speech-text latents, dual-stream diffusion, or representation-level world models remains open.

Together, the T × I × R interaction ontology, the five-state decision machine, and the L0--L3 hierarchy replace the binary question "is this full-duplex?" with a structured profile: what interaction cells a system faces, what policy states it reaches, and where that policy is implemented. This vocabulary makes models, datasets, benchmarks, and open problems comparable under the same framework.

## Appendix: Public Resources

The following tables organize public full-duplex resources under the T × I × R ontology. The TIR coverage column is conservative: only explicitly evidenced cells are listed.

### Publicly Available Full-Duplex Models

TBD.

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
| HumDial Challenge Track 2 | T3 Overlap / T5 Silence; I2 Backchannel / I4 Floor-claim / I6 Disfluency / I7 Third-party; R2 Stop / R3 Wait / R5 Ignore | train 9,418 / dev 1,800 / test 5,000 utterances | [Hugging Face](https://huggingface.co/datasets/ASLP-lab/HumDial-FDBench) |
| τ-Voice | T3 Overlap; I2 Backchannel / I4 Floor-claim / I7 Third-party; R2 Stop / R4 Backchannel / R5 Ignore | 278 tasks | [GitHub](https://github.com/sierra-research/tau2-bench) |
| SID-Bench | T3 Overlap; I2 Backchannel / I4 Floor-claim; R2 Stop | 3,700 instances / ~10 h | [Hugging Face](https://huggingface.co/datasets/kxxia/SID-bench) |
| DualTurn Turn-Taking Datasets | T3 Overlap; I2 Backchannel; R4 Backchannel | OtoSpeech 287 h; Switchboard features 256.7 h | [OtoSpeech HF](https://huggingface.co/datasets/anyreach-ai/dualturn-otospeech-turn-taking); [Switchboard HF](https://huggingface.co/datasets/anyreach-ai/dualturn-switchboard-turn-taking) |

## Cite

```bibtex
TBD
```
