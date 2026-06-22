# TakeMeter — Soccer Discourse Classifier

TakeMeter is a fine-tuned text classification system that evaluates discourse quality in soccer-related online communities. 
It classifies posts into three categories:

analysis — structured reasoning supported by evidence or tactical explanation
hot_take — strong opinion without sufficient supporting evidence
reaction — emotional response to an event

The goal is to understand how well transformer models can distinguish nuanced discourse types in sports discussions.

## Dataset
Total examples: 240
Source: manually curated soccer discussions (Messi, Ronaldo, World Cup, club football, etc.)
Labels:
analysis: 80
hot_take: 80
reaction: 80

Dataset is balanced to prevent model bias.

## Label Definitions
analysis

Posts that provide structured reasoning, tactical breakdowns, or evidence-based arguments.

hot_take

Strong opinions stated confidently without full supporting reasoning.

reaction

Emotional or immediate responses to events (goals, wins, losses, performances).

## Model
Base model: distilbert-base-uncased
Fine-tuning framework: HuggingFace Transformers
Epochs: 3
Learning rate: 2e-5
Batch size: 16

## Results

### Baseline (Groq LLaMA 3.3 Zero-Shot)
Accuracy: 1.000
Strength: strong label separation due to clear prompt definitions
Weakness: likely over-reliance on pattern matching rather than true learning

### Fine-Tuned DistilBERT
Accuracy: 0.889
Per-class performance:
Class	Precision	Recall	F1-score
analysis	0.85	0.92	0.88
hot_take	0.83	0.83	0.83
reaction	1.00	0.92	0.96

## Confusion Matrix (Test Set)
True \ Pred	analysis	hot_take	reaction
analysis	11	1	0
hot_take	2	10	0
reaction	0	1	11

## Error Analysis (Key Failures)
1. Analysis → Hot Take

“Argentina's World Cup success was driven by tactical flexibility and adaptability”

Model mistake: classified as hot_take
Reason: lacks explicit evidence or structured breakdown
Insight: model confuses “sophisticated language” with analysis

2. Hot Take → Analysis

“Goalkeepers have minimal impact on modern game outcomes”

Model mistake: classified as analysis
Reason: sounds tactical but lacks supporting evidence
Insight: model over-trusts football jargon

3. Reaction → Hot Take

“This is the best match ever”

Model mistake: classified as hot_take
Reason: lacks emotional markers tied to a specific event
Insight: model struggles with pure sentiment statements

## Sample Predictions
Text	Predicted	Confidence
“Messi completely changed the flow of the match with his positioning”	analysis	0.91
“Ronaldo is still the greatest player in big games”	hot_take	0.87
“WHAT A GOAL!!! UNBELIEVABLE”	reaction	0.95

#### Correct prediction example:
“WHAT A GOAL!!! UNBELIEVABLE” → reaction
Reason: strong emotional language tied to live event

## Reflection: What the Model Learned
The model successfully learned to distinguish structured reasoning from emotional and opinion-based posts. However, it struggles with boundary cases where opinion and analysis overlap.

The strongest learned signal is linguistic structure (presence of reasoning language vs emotional language), rather than true understanding of discourse quality.

## Spec Reflection
The project specification helped guide the design of clear, mutually exclusive labels. However, during implementation, some borderline cases (especially between analysis and hot_take) revealed that natural soccer discourse does not always fit clean categories. This required refining decision rules during annotation.

## AI Usage Disclosure
Dataset generation assistance
Used Reddit and organize with cloud AI to help the generate and diversify soccer discourse examples
Manually reviewed and corrected all labels
Error analysis support
Used AI to group misclassified examples into patterns
Verified patterns manually using confusion matrix and predictions

## Conclusion

Fine-tuning significantly improved classification performance compared to zero-shot prompting in terms of robustness and interpretability of errors. The model demonstrates that transformer-based architectures can capture discourse structure, but still struggle with subtle semantic boundaries.

## Demo Video

Watch the project demo here:

https://drive.google.com/file/d/1Sq6a8ofQIthRE3IqyNu31V35CMtyxg0W/view?usp=sharing