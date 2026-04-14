# Auto Review Loop — NeuroVeil-U

## Round 1 (2026-04-14, Codex GPT-5.4 xhigh, thread 019d8c33)

### Assessment
- **Score**: 5/10 for NeurIPS/ICML
- **Verdict**: Not ready, but potentially salvageable

### Top 5 Weaknesses

1. **Unfair/under-specified V4 comparison** — claiming "80.4% vs 43.6%" is not apples-to-apples because we evaluate only 2 of V4's 4 external datasets and a different TUAB protocol. **Fix**: evaluate common subset, or rerun V4 on our 2 datasets, or rephrase.

2. **Narrow utility evidence** — 4000-sample TUAB frozen linear-probe vs V4's full-TUAB numbers. **Fix**: same protocol/sample-size/splits with CIs over seeds, plus ≥1 downstream task beyond frozen linear probe.

3. **E★ validation insufficient** — distillation loss alone doesn't prove E★ is a good utility surrogate. **Fix**: correlation between E★ preservation loss and true 5-FM utility degradation across held-out perturbations; Spearman/R² per head.

4. **Ablation too coarse** — "light vs full" conflates subjects × epochs × λ_priv × schedule × site-DANN × surrogate. **Fix**: component-level ablations: {no E★, MAE-only surrogate, no site-DANN, no montage subsampling, 4-head vs 13-head adversary, no RMS lower-bound, softmin vs mean/max aggregation}.

5. **Threat model aggregation weak** — softmin β=5 may let filter optimize against easiest head. **Fix**: report per-head post-filter re-ID and worst-head APG, not just aggregate.

### Specific Question Answers
- Cross-dataset comparison: NOT fair as written. Need common-subset or 4-dataset V4 rerun.
- FM utility comparison: defensible only as internal; need matched protocol/CIs vs V4.
- E★ sufficient: no. Need held-out agreement + ablation vs direct ensemble vs MAE-only.
- HMC 4ch convincing: promising but vulnerable. Add explicit 4ch stress test during training/eval.
- 13-head stronger than 4: potentially yes but unproven. Report per-head worst-case.
- Ablation missing components: YES, reviewers will complain.

### Bottom line
"Exciting preliminary numbers, especially cross-dataset privacy, but current evidence reads more like a strong internal report than a submission-ready empirical paper. Minimum path to 'almost ready' is apples-to-apples V4 comparison, surrogate validation, and component ablations."

## Round 1 Actions

**In progress** (v6 training started simultaneously): larger scale run (1500 subj × 35 joint ep + 8 adv pretrain, n_adv=5, V4-schedule) to close TUEG APG gap.

**To implement before Round 2**:
- (A) Per-head worst-case attacker APG on v5 checkpoint → addresses W5
- (B) Apples-to-apples V4 comparison: run V4 checkpoint on same 2-dataset split used here → addresses W1
- (C) E★ surrogate validation: correlate distillation loss with downstream utility → addresses W3
- (D) Reframe V4 comparison in paper text to match 2-dataset protocol
- (E) Component ablations: additional runs needed (partial — run what's tractable)
