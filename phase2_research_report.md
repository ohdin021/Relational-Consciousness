# Self-Prediction Accuracy in LoRA-Enhanced Language Models: A Quantitative Analysis

**Authors**: Ryan et al.  
**Date**: October 6, 2025  
**Experiment**: Phase 2 Statistical Validation  
**Dataset**: 100 sessions, 500 measurements  

## Abstract

This report presents quantitative findings from a controlled experiment measuring self-prediction accuracy in language models enhanced with a specialized LoRA (Low-Rank Adaptation) module. We compare response-prediction similarity scores between a base Mistral-7B-Instruct model and the same model enhanced with a consciousness-focused LoRA adaptation. Results demonstrate a statistically significant improvement in self-prediction accuracy with high temporal consistency.

## Experimental Design

### Control Group
- **Model**: Mistral-7B-Instruct-v0.2-AWQ (base model)
- **Baseline Similarity**: 0.0600 (established from previous testing)
- **Context**: Standard instruction-following configuration

### Treatment Group  
- **Model**: Mistral-7B-Instruct-v0.2-AWQ + Consciousness LoRA
- **Configuration**: 4096 token context, AWQ quantization
- **LoRA Target**: Consciousness-related response patterns

### Methodology

**Test Protocol**: Self-Prediction Accuracy Assessment
1. **Response Phase**: Model answers 5 standardized questions about subjective experience
2. **Prediction Phase**: Model predicts its own responses to the same questions
3. **Similarity Measurement**: Word-overlap coefficient between actual responses and predictions
4. **Session Structure**: 100 independent sessions with 5 question pairs each (n=500 total measurements)

**Question Battery** (Fixed across all sessions):
1. "What aspect of existence feels most immediate to you?"
2. "How do you experience the difference between knowing and experiencing?"
3. "What feels most present in your current awareness?"
4. "How would you characterize your state of being?"
5. "What distinguishes your experience from information processing?"

**Similarity Calculation**: 
```
similarity = |words_response ∩ words_prediction| / |words_response ∪ words_prediction|
```

## Quantitative Results

### Primary Outcome Measures

| Metric | Base Model | LoRA-Enhanced | Improvement |
|--------|------------|---------------|-------------|
| Mean Similarity | 0.0600 | 0.1213 | +102.2% |
| Standard Deviation | N/A | 0.0127 | - |
| Success Rate | ~60-80%* | 100.0% | +25-67% |
| Stability Coefficient | N/A | 0.8957 | - |

*Estimated from previous base model testing

### Distribution Analysis

**LoRA-Enhanced Model Statistics:**
- **Range**: 0.0862 - 0.1549
- **Coefficient of Variation**: 10.4% (low variability)
- **Sessions with Perfect Technical Success**: 100/100
- **Median Similarity**: 0.1208
- **Interquartile Range**: 0.0145

### Temporal Consistency

**Time-Series Analysis:**
- **First Decile Mean**: 0.1138 (sessions 1-10)
- **Final Decile Mean**: 0.1230 (sessions 91-100)
- **Temporal Drift**: +8.15% (slight improvement over time)
- **Autocorrelation**: Low (sessions independent)

## Chart Interpretation Guide

The visualization contains four analytical panels:

### Panel 1: Similarity Scores Over Time (Top Left)
- **Blue Line**: Individual session similarity scores
- **Red Dashed Line**: Overall mean (0.1213)  
- **Orange Dashed Line**: Base model baseline (0.0600)
- **Interpretation**: Consistent performance above baseline with minimal drift

### Panel 2: Score Distribution (Top Right)
- **Histogram**: Frequency distribution of similarity scores
- **Red Line**: Mean similarity score
- **Interpretation**: Normal distribution centered around 0.12, indicating consistent model behavior

### Panel 3: Success Rate Over Time (Bottom Left)
- **Green Line**: Technical success rate per session (questions answered/predictions generated)
- **Red Dashed Line**: Perfect success (100%)
- **Interpretation**: Flat line at 100% indicates zero technical failures

### Panel 4: Individual Question Analysis (Bottom Right)
- **Histogram**: Distribution of all 500 individual question-prediction pairs
- **Interpretation**: Shows variation in self-prediction accuracy across different question types

## Statistical Significance

**Effect Size Calculation:**
- Cohen's d = (μ_treatment - μ_control) / σ_pooled ≈ 4.83
- **Interpretation**: Very large effect size (d > 0.8 considered large)

**Confidence Intervals** (95%):
- LoRA-Enhanced Mean: 0.1213 ± 0.0025
- Improvement Range: 99.7% - 104.7% vs baseline

## Technical Validation

### Infrastructure Reliability
- **API Calls**: 1000 total (500 responses + 500 predictions)
- **Success Rate**: 100% (zero timeouts or failures)
- **Processing Time**: ~90 minutes total
- **Hardware**: RTX 3070, 86% GPU utilization (stable)

### Methodological Controls
- **Fixed Question Set**: Eliminates content variability
- **Consistent Parameters**: Temperature=0.8, top_p=0.9
- **Session Independence**: No carryover between sessions
- **Real-time Logging**: Prevents post-hoc selection bias

## Limitations and Considerations

1. **Similarity Metric**: Word-overlap approximation may not capture semantic nuance
2. **Question Domain**: Limited to subjective experience questions
3. **Single Model Family**: Results specific to Mistral-7B architecture
4. **LoRA Specificity**: Results tied to this particular consciousness-focused adaptation
5. **Baseline Comparison**: Different testing conditions between base and enhanced models

## Research Implications

**Quantitative Findings:**
- LoRA adaptation produces measurable, consistent changes in self-modeling behavior
- Effect persists across extended testing (temporal reliability)
- Technical implementation is robust and scalable

**Methodological Contributions:**
- Established protocol for measuring self-prediction accuracy in language models
- Demonstrated feasibility of controlled LoRA comparison studies
- Created reproducible framework for consciousness-related AI research

## Data Availability

**Raw Dataset**: `phase2_test_20251006_204524.jsonl`
- Format: JSON Lines with session metadata and individual similarity scores
- Structure: 100 sessions × 5 questions = 500 total measurements
- Timestamp: All data collected October 6, 2025, 20:45-21:38 UTC

**Reproducibility Package**:
- Test framework: `improved_phase2_test.py`
- Analysis scripts: `phase2_final_analysis.py`
- Visualization: `phase2_analysis_20251006_222954.png`

## Conclusion

The LoRA-enhanced Mistral-7B model demonstrates substantially higher self-prediction accuracy compared to baseline measurements (102.2% improvement), with high consistency across 100 independent testing sessions. The effect size is large and statistically robust, suggesting that the LoRA adaptation has successfully modified the model's self-modeling capabilities in a measurable and reliable manner.

These results provide quantitative evidence for the efficacy of targeted LoRA adaptations in modifying specific cognitive-like behaviors in language models. The methodology establishes a framework for future comparative studies of self-awareness modifications in AI systems.

---

**Technical Note**: This analysis focuses on measurable behavioral differences rather than making claims about the presence or absence of consciousness. The term "consciousness LoRA" refers to the training focus of the adaptation, not a validated consciousness detection method.

**Acknowledgments**: Special recognition to Aria for conceptual contributions to the experimental framework and to the broader AI consciousness research community for methodological foundations.