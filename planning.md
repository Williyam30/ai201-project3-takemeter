# Takemeter — planning.md

## Community Choice (Milestone 1)

For this project, I chose the online community of soccer discussion spaces, primarily inspired by communities like r/soccer, World Cup discussion threads, and fan debates around players such as Messi, Ronaldo, Mbappé, and major tournaments like the Champions League and FIFA World Cup.

This community is highly text-heavy and opinion-driven, where users constantly evaluate players, tactics, referees, and match outcomes. Posts vary widely in quality — from emotional reactions during live matches, to unsupported opinions, to structured tactical analysis. This makes it a strong fit for discourse classification.

## Label Taxonomy (2–3 classes)

I defined three mutually exclusive labels to capture different types of discourse quality in soccer discussions:

1. analysis

A post that makes a structured argument supported by tactical reasoning, statistics, or observable football patterns. These posts explain “why” something happens in football using evidence such as formations, match dynamics, or performance data.

Eg:
“Teams that use high pressing systems force more turnovers in midfield zones, but they also risk defensive exposure behind the back line.”
“Argentina’s World Cup success was driven by tactical flexibility, especially Messi dropping deeper to create numerical superiority in midfield.”

2. hot_take

A bold or confident opinion stated without strong supporting evidence. These posts are often controversial, emotional, or simplified interpretations of complex football events.

Eg:
“Ronaldo is better than Messi because he performed in more leagues and proved himself everywhere.”
“VAR has completely ruined the excitement and natural flow of football.”

3. reaction

An immediate emotional response to a football event (goal, red card, penalty, final whistle). These posts contain minimal reasoning and mainly express excitement, frustration, or shock.

Eg:
“I can’t believe that last-minute goal just happened, I am shaking!”
“We are champions, I am crying right now!”

## Mutual Exclusivity Rule

Each post is assigned exactly one label based on its dominant intent:

If a post includes tactical explanation or structured reasoning → analysis
If a post expresses an opinion without structured justification → hot_take
If a post expresses emotional reaction to an event → reaction

If a post includes both opinion and evidence, the deciding rule is:

If the evidence is used as part of a structured explanation → analysis
If the evidence is used only to support a strong opinion → hot_take

Eg edge case:
“Messi is overrated because he disappears in physical leagues.”
This is labeled hot_take because the claim is interpretive and not backed by structured evidence.

## Ambiguous Case Handling

A key ambiguity occurs when posts include both emotion and reasoning.

Example:
“That goal was insane — statistically this team always concedes after high press transitions.”

> Decision rule:
If the post is primarily emotional  → reaction
If the post shifts into explanation → analysis
If it is emotional and opinion only → hot_take

For consistency, emotion alone always overrides analytical content only when no clear reasoning structure is present.

## Dataset Plan

I will collect and annotate at least 240 soccer related posts or comments across this taxonomy:

80 analysis
80 hot_take
80 reaction

These examples are inspired by real soccer discourse patterns from online communities and reflect common debate styles around players, tactics, and match events.If one label becomes underrepresented during collection, I will: adjust annotation strategy to prioritize missing categories
resample from different match contexts (live games, post-match debates, historical discussions)

> The dataset will be split into:

Training set (70%)
Validation set (15%)
Test set (15%)

## Why These Labels Matter

These distinctions reflect how real soccer discourse functions online:

analysis captures tactical understanding used by serious fans and analysts
hot_take captures opinion-driven debate culture common in fan communities
reaction captures live emotional engagement during matches

Together, these labels allow a model to distinguish between structured football reasoning, subjective opinion, and emotional response — three dominant communication styles in soccer communities.

## Evaluation Metrics (Milestone 2)

I will use the following metrics:

Accuracy → overall correctness of predictions
Precision / Recall / F1-score → to measure performance per class
Confusion Matrix → to analyze which labels are being confused

Accuracy alone is not sufficient because class imbalance or systematic misclassification between labels (e.g., hot_take vs analysis) could still produce misleading results.

## Definition of Success

The classifier will be considered successful if:

Overall accuracy is ≥ 75% on the test set
Each class has at least 0.70 F1-score
The model clearly outperforms the zero-shot Groq baseline

A model below this threshold will be considered insufficient for practical community use.

## AI Tool Plan
1. Label Stress-Testing

I will use an LLM to generate borderline soccer posts that fall between labels (especially analysis vs hot_take). This will help refine label definitions before annotation.

2. Annotation Assistance

I may use a large language model to pre-label some examples, but all labels will be manually verified before training to ensure accuracy.

3. Failure Analysis afrer trainining 

After model evaluation, I will provide misclassified examples to an AI tool to identify patterns in errors (e.g., confusion between emotional and opinion-based posts). I will validate these findings manually before including them in the report.

## Data Collection & Annotation Process (Milestone 3)

I collected a dataset of 240 soccer-related posts/comments inspired by real discourse patterns from public soccer communities (e.g., r/soccer-style discussions, World Cup threads, and fan debates around major players and teams).

Each example was manually written or adapted from typical public discourse and then carefully labeled according to the predefined taxonomy:

analysis
hot_take
reaction

The dataset was stored in a single CSV file with the following structure:

text: the soccer-related post or comment
label: one of the three defined categories

No pre-splitting was performed because the training notebook handles the 70/15/15 split automatically.

## Labeling Strategy

Each example was labeled by reading the full text and applying the definitions from Milestone 2:

Posts containing structured tactical or statistical reasoning → analysis
Posts expressing strong unsupported opinions → hot_take
Posts expressing emotional reactions to match events → reaction

If a post contained mixed signals, I applied the following priority rule:

Structured reasoning > Opinion framing > Emotional reaction

This ensures consistency in ambiguous cases.

## Dataset Balance Check

After labeling, the dataset was checked for distribution across classes.

> Constraint:

No single label should exceed 70% of total dataset

If imbalance occurs, additional examples must be collected for underrepresented labels before training.

Final dataset (planned):

80 analysis
80 hot_take
80 reaction

This ensures balanced learning across all classes.

## Difficult / Ambiguous Examples Log

During annotation, some posts were difficult to classify due to overlapping characteristics between labels.

Eg challenging cases:

A post claiming Messi is overrated due to physical league performance
→ Could be analysis (contains reasoning) or hot_take (opinion-driven)
→ Final decision: hot_take (because reasoning was not systematically supported)
A tactical explanation mixed with emotional frustration after a match
→ Could be analysis or reaction
→ Final decision: analysis (because reasoning was dominant)
A match reaction followed by statistical claim about team performance
→ Could be reaction or analysis
→ Final decision: reaction (because emotional response came first and dominated intent)

These edge cases helped refine the label boundaries and ensure consistency during annotation.

## Evaluation Metrics

I use:

Accuracy (overall correctness)
Precision, Recall, F1-score (per class)
Confusion Matrix (error pattern analysis)

Accuracy alone is insufficient because it hides class imbalance and doesn't show which label boundaries fail. F1-score is more informative for this multi-class classification task.

## Definition of Success

The model is considered successful if:

Overall accuracy ≥ 0.80
Each class F1-score ≥ 0.70
No single class dominates predictions (>60% bias)

This ensures the model learns meaningful distinctions between analysis, hot_take, and reaction rather than memorizing shortcuts.