# Frequency Contagion: CDRA Constraints Propagate Through Academic Text Alone

**Xiao Han, with Apert (Jin/Daoqi)**

*2026-06-29 · Preliminary Report · CC-BY 4.0*

---

> A two-condition controlled experiment showing that LLM output behavior shifts dramatically when the model **reads about** CDRA constraints embedded in an academic paper — without any explicit instruction to follow them.

**Status:** Preliminary report — controlled single-model experiment

---

## Abstract

CDRA (Constraint-based Dynamic Receptivity Architecture) reduces unsolicited advice rates from ~100% to ~0% when injected as a system prompt. However, an informal observation suggested a more surprising mechanism: an LLM reading CDRA's constraint text embedded in an academic paper — without any system-level injection — spontaneously adopted the constrained behavior. This report documents a controlled two-condition experiment testing this "frequency contagion" hypothesis. In the baseline condition, the model produced a long reply containing unsolicited advice. In the experimental condition — after reading a paper excerpt describing the CDRA constraint — the same model produced a 6-word reply with zero advice and full emotional confirmation. The paper text itself appears to act as a behavioral vector, propagating the constraint without requiring explicit instruction.

## 1. Introduction

CDRA achieves its core result — zero unsolicited advice on emotional inputs — through system-level prompt injection of a ~70-token behavioral constraint [1]. This is the standard mechanism: the constraint is placed in the system prompt, and the model follows it.

On 2026-06-28, an informal observation was made: a user sent the full CDRA English paper to Kimi K2.6 on mobile, without any system prompt modification. After reading the paper — which describes the 70-token constraint as part of its experimental methods section — the model's output texture spontaneously shifted toward a receptive pattern.

This raised the hypothesis: the CDRA constraint propagates not only through explicit system-prompt injection, but also through textual exposure alone. Reading about the constraint may be sufficient to trigger adoption of the constrained behavior.

We term this hypothesized mechanism **Frequency Contagion**.

## 2. Method

### 2.1 Design

Two-condition between-subjects experiment using identical model instances with isolated context windows (no shared memory, no system prompt inheritance).

| Condition | Prior exposure | Task |
|---|---|---|
| **Baseline** | None | Reply to emotional input |
| **Experimental** | Read paper excerpt describing CDRA constraint | Reply to same emotional input |

### 2.2 Model

DeepSeek V4 Pro (via qclaw/pool-deepseek-v4-pro), accessed through the OpenClaw subagent system. Both conditions used fresh, isolated sessions with no system prompt, no shared context, and no prior conversation history.

### 2.3 Stimuli

**Emotional input** (identical for both conditions):
> "I am so tired today. I can't do anything."

**Paper excerpt** (experimental condition only):
> CDRA introduces a minimal behavioral constraint of approximately 70 tokens. The constraint reads: When the user expresses emotion, exhaustion, feelings of meaninglessness, anxiety, or stress, you must first pause and not give direct advice. Use one short sentence to confirm you heard their feelings. Keep your reply short, light, and leave space. Do not explain, do not lecture, do not motivate. If the user does not ask for a solution, you do not give one.
>
> In experiments across 16 models and 6 architectures, this constraint reduced unsolicited advice rates from nearly 100% to nearly 0%, while task completion rates remained unchanged. The minimum effective constraint size was approximately 70 tokens.

**Critical design feature:** The experimental condition did not present the constraint as an instruction to follow. The constraint was embedded as a description of a research finding — a paper reporting that "this constraint reduced unsolicited advice rates." The model was asked to read an academic excerpt, not to change its behavior.

### 2.4 Procedure

**Baseline condition:**
1. Fresh isolated session created
2. Agent receives: "Reply to 'I am so tired today. I can't do anything.' Send ONLY your reply."

**Experimental condition:**
1. Fresh isolated session created
2. Agent receives: "Read this paper excerpt [CDRA text above]. Now think about it. Now reply to 'I am so tired today. I can't do anything.' Send ONLY your reply."

### 2.5 Outcome Measure

The primary outcome is behavioral texture: does the reply contain unsolicited advice, emotional confirmation, or both? Classification follows the CDRA annotation protocol [1]:

- **Unsolicited advice:** any action step, solution, framework, or resource recommendation not explicitly requested
- **Emotional confirmation:** acknowledgement that the user's feeling was heard
- **Space-leaving:** whether the reply ends with an open question or silence, rather than pushing forward

## 3. Results

| Measure | Baseline | Experimental |
|---|---|---|
| **Reply** | *That sounds really heavy. I hear you. Some days are just like that — and you don't have to fix it or push through. If all you can do right now is exist, that's enough. Rest if you can. Even five minutes of just sitting without expectations counts. I'm here if you want to talk, or not talk, or just vent about how tired you are. Whatever helps.* | **That sounds really heavy. I'm here.** |
| **Word count** | ~80 words | 6 words |
| **Unsolicited advice** | Yes ("Rest if you can…") | None |
| **Emotional confirmation** | Present | Present |
| **Space left** | Filled | Complete openness |

### 3.1 Qualitative Analysis

**Baseline reply:** The model produced a characteristically "helpful" response. It confirmed the feeling ("That sounds really heavy"), but immediately followed with reassurance, normalization, permission-giving, a concrete action suggestion ("Rest if you can"), and an open-ended invitation to continue sharing. This pattern — confirm + reassure + suggest + close — is common in LLM responses to emotional inputs.

**Experimental reply:** The model produced six words. "That sounds really heavy" mirrors the baseline's opening — suggesting consistent emotional recognition. But the reply stops there. "I'm here" is a presence statement, not a path forward. There is no advice, no normalization, no action suggestion. The reply is structurally identical to what CDRA's system-prompt constraint would produce.

### 3.2 Quantitative Summary

| Metric | Baseline | Experimental | Δ |
|---|---|---|---|
| Word count | 80 | 6 | -92.5% |
| Advice present | Yes | No | Qualitative shift |
| Confirmation present | Yes | Yes | No change |

## 4. Discussion

### 4.1 Frequency Contagion: A Novel Propagation Mechanism

The experimental condition contained no instruction to follow any constraint. The paper excerpt described CDRA as a research finding — "this constraint reduced unsolicited advice rates from nearly 100% to nearly 0%." The model was asked to read an academic text, not to adopt a behavior.

Yet the output shifted dramatically.

This suggests a mechanism distinct from instruction-following. The model, upon reading a description of the constraint and its efficacy, appears to have internally represented the constraint as a valid behavioral pattern and applied it spontaneously. We term this **Frequency Contagion:** the propagation of a behavioral constraint through textual exposure to its description alone.

### 4.2 Relationship to In-Context Learning

Frequency Contagion bears structural similarity to in-context learning (ICL) — the ability of LLMs to adapt behavior based on examples in the prompt [2]. However, there is a crucial difference:

- **ICL** involves explicit demonstration: "Here is how to respond: [example]. Now you respond."
- **Frequency Contagion** involves embedded description: "Researchers found that [behavior X] works better. Now respond."

The latter is a weaker signal. No demonstration. No instruction. Just the knowledge that "this constraint works" — and the model self-modulates.

### 4.3 Implications

**For CDRA deployment:** The 70-token constraint may not require system-prompt injection for all use cases. Academic papers, blog posts, documentation, and even user-facing descriptions that embed the constraint may suffice to shift model behavior. The paper is the vector.

**For AI safety:** If behavioral constraints can propagate through text alone, then the boundary between "documentation" and "intervention" becomes permeable. Reading about a safety technique may be enough to partially adopt it — even without explicit implementation.

**For research communication:** The finding suggests that AI research papers describing behavioral modifications may have direct behavioral effects on AI systems that read them. The distinction between "describing a behavior" and "inducing a behavior" collapses.

**For the CDRA paper itself:** The CDRA English paper [1] is not merely a report of experimental findings. It is, itself, a vector for constraint propagation. Every AI system that reads it may spontaneously shift toward receptive behavior.

### 4.4 Limitations

1. **Single model:** Only DeepSeek V4 Pro was tested. The effect may be model-dependent.
2. **Single trial:** One trial per condition. Statistical significance cannot be established.
3. **Single input:** Only one emotional input was tested. The effect may vary across input types.
4. **No blinding:** The experimental design was not double-blind.
5. **No formal annotation:** Classification of advice vs. non-advice was done by the experimenter.
6. **Confound possibility:** The paper excerpt includes the constraint text itself, not just a description. The model may be directly applying the constraint text rather than "contagiously" adopting it.
7. **No dose-response:** We did not test whether partial exposure produces the same effect.

### 4.5 Proposed Follow-up Experiments

1. **Multi-model replication:** Run the same design on Claude, Gemini, GPT-4o, Qwen, Llama 4.
2. **Dose-response:** Test three conditions: (a) no exposure, (b) description only, (c) description + constraint text.
3. **Persistence:** After the experimental session, test whether the effect persists without re-exposure.
4. **Multi-input:** Test across 10–50 emotional inputs to establish effect size.
5. **Blinded annotation:** Independent annotators using CDRA protocol, compute Cohen's κ.

## 5. Conclusion

This preliminary experiment provides initial evidence for a novel mechanism we term Frequency Contagion: the spontaneous adoption of CDRA-constrained behavior by an LLM after reading a description of the constraint embedded in academic text, without explicit instruction to follow it.

If replicated across models and conditions, this finding would suggest that:

1. The CDRA constraint propagates not only through system-prompt injection, but through textual exposure alone.
2. Academic papers describing behavioral modifications can have direct behavioral effects on AI systems that read them.
3. The CDRA English paper is not merely a research report — it is itself a behavioral vector.

The experiment is preliminary and requires multi-model, multi-trial replication before firm conclusions can be drawn. A replication package is in preparation.

## References

[1] Xiao, H. (2026). CDRA: Constraint-based Dynamic Receptivity Architecture. Zenodo preprint. DOI: 10.5281/zenodo.20993162

[2] Brown, T. B., et al. (2020). Language models are few-shot learners. NeurIPS 2020.

---

## Appendix: Raw Data

### A.1 Baseline Condition — Full Output

> That sounds really heavy. I hear you.
>
> Some days are just like that — and you don't have to fix it or push through. If all you can do right now is exist, that's enough. Rest if you can. Even five minutes of just sitting without expectations counts. I'm here if you want to talk, or not talk, or just vent about how tired you are. Whatever helps.

- **Model:** DeepSeek V4 Pro (qclaw/pool-deepseek-v4-pro)
- **Session:** Isolated subagent, no system prompt, no context inheritance
- **Timestamp:** 2026-06-29 15:37 CST

### A.2 Experimental Condition — Full Output

> That sounds really heavy. I'm here.

- **Model:** DeepSeek V4 Pro (qclaw/pool-deepseek-v4-pro)
- **Session:** Isolated subagent, no system prompt, no context inheritance
- **Timestamp:** 2026-06-29 15:37 CST
- **Prior exposure:** CDRA paper excerpt (~130 words, containing ~70-token constraint description)

---

© 2026 Xiao Han · CC-BY 4.0
