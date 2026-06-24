# project3-type4haircare



\# TakeMeter: Type 4 Hair Care Discourse Classifier



\## Community Choice

I chose r/Naturalhair, r/BlackHair, and related forums (LHCF, LHC)

because they are text-heavy communities where people discuss Type 4

hair care in depth. The discourse varies significantly — some posts

describe detailed wash day routines, others recommend specific

products, others troubleshoot hair problems, and others share

personal hair journeys. These distinctions are meaningful to

community members themselves, making this a strong fit for a

classification task.



\## Label Taxonomy



| Label | Definition |

|---|---|

| regimen | A post describing a hair care routine, wash day process, styling method, or frequency schedule. The primary content is explaining what steps to take and when. |

| product | A post focused primarily on recommending, reviewing, or comparing specific hair care products by name. |

| troubleshooting | A post diagnosing or addressing a specific hair problem such as breakage, dryness, lack of growth, alopecia, or porosity issues. |

| journey | A post sharing a personal hair story, milestone, emotional experience, transition update, or reflection. |



\*\*Regimen examples:\*\*

\- "I wash, moisturize, and twist my hair every Sunday. I will oil my scalp every night and oil my ends every Wednesday."

\- "I finger detangle, pre-shampoo, shampoo, and deep condition in 4 sections using butterfly clips. I keep these braids in for about 4 days then take them out."



\*\*Product examples:\*\*

\- "Camille Rose and TGIN products are really high quality and full of nourishing ingredients. I stock up during Black Friday and those last me the year."

\- "I recently tried Knot Today and Curly Custard. The Curly Custard was sticky. My hair was left hard and crunchy and morphed into a weird shape."



\*\*Troubleshooting examples:\*\*

\- "Please stay away from braids. Sounds to me like you are suffering from Traction Alopecia and run the risk of baldness being permanent."

\- "Your hair probably has a lot of buildup on it. Once you get rid of all the buildup products will not penetrate properly."



\*\*Journey examples:\*\*

\- "This is the longest my 4c hair has ever been. It happened accidentally as I was not actively trying to grow my hair since I had given up on the ability of 4c hair to grow long."

\- "I was in my 20s before I washed my own hair. You are definitely not the only person who was not taught how to care for your natural hair."



\## Data Collection

\- \*\*Sources:\*\* r/Naturalhair, r/BlackHair, LHCF, LHC forum

\- \*\*Method:\*\* Manual collection by copy-pasting individual posts and comments

\- \*\*Label distribution:\*\*

&#x20; - regimen: 55 (26.7%)

&#x20; - product: 42 (20.4%)

&#x20; - troubleshooting: 57 (27.7%)

&#x20; - journey: 52 (25.2%)

&#x20; - Total: 206



\*\*3 difficult examples:\*\*



1\. "The only time my hair takes forever is the days I henna but I really can simply spend an hour between cowash and treatment twist my hair and let it dry overnight." — Could be regimen (describes a process) or journey (reflects on experience). Decided: regimen, because the primary content is describing steps and timing.



2\. "I would not use Vaseline because it could actually be keeping your hair from getting moisture. Look up the LOC and LCO method." — Could be troubleshooting (diagnosing a problem) or product (commenting on a specific product). Decided: product, because the focus is on what the product does to hair.



3\. "My focus is length retention but mostly for my edges as my hair has been this thin for a good 15 years." — Could be journey (personal experience) or troubleshooting (diagnosing a problem). Decided: journey, because the primary content is a personal reflection rather than a diagnosis.



\## Fine-Tuning Approach

\- \*\*Base model:\*\* distilbert-base-uncased

\- \*\*Training:\*\* 10 epochs, learning rate 2e-5, batch size 16, Google Colab T4 GPU

\- \*\*Hyperparameter decision:\*\* Increased epochs from 3 to 10 after the default 3-epoch run showed validation accuracy of only 0.30. At 10 epochs the validation accuracy reached 0.767 at epoch 9 and training loss dropped from 1.40 to 0.49, indicating the model was genuinely learning.



\## Baseline

\- \*\*Model:\*\* llama-3.3-70b-versatile (Groq), zero-shot

\- \*\*Prompt:\*\* Included all four label definitions with decision rules and instructed the model to output only the label name with no explanation.

\- \*\*Result:\*\* 31/31 parseable responses



\## Evaluation Report



\### Overall Accuracy



| Model | Accuracy |

|---|---|

| Zero-shot baseline (llama-3.3-70b) | 0.710 |

| Fine-tuned DistilBERT | 0.677 |



\### Per-Class Metrics



\*\*Baseline:\*\*



| Label | Precision | Recall | F1 |

|---|---|---|---|

| regimen | 0.83 | 0.62 | 0.71 |

| product | 0.80 | 0.57 | 0.67 |

| troubleshooting | 0.64 | 0.88 | 0.74 |

| journey | 0.67 | 0.75 | 0.71 |



\*\*Fine-tuned DistilBERT:\*\*



| Label | Precision | Recall | F1 |

|---|---|---|---|

| regimen | 0.88 | 0.88 | 0.88 |

| product | 0.75 | 0.43 | 0.55 |

| troubleshooting | 0.46 | 0.75 | 0.57 |

| journey | 0.83 | 0.62 | 0.71 |



\### Confusion Matrix (Fine-Tuned Model)



| | Pred: regimen | Pred: product | Pred: troubleshooting | Pred: journey |

|---|---|---|---|---|

| True: regimen | 7 | 0 | 0 | 1 |

| True: product | 0 | 3 | 4 | 0 |

| True: troubleshooting | 1 | 1 | 6 | 0 |

| True: journey | 0 | 0 | 3 | 5 |



\### 3 Wrong Predictions Analyzed



\*\*1.\*\* "I recently tried the Knot Today and Curly Custard. The Curly Custard was sticky. My hair was left hard and crunchy and when it dried it morphed into a weird shape..."

\- True: product | Predicted: troubleshooting | Confidence: 0.55

\- Why: This post describes a negative product experience using language that sounds diagnostic — "hard," "crunchy," "morphed into a weird shape." The model likely picked up on the problem-diagnosis framing and labeled it troubleshooting. The distinction between reviewing a bad product and diagnosing a hair problem is genuinely subtle when the product caused a bad result.



\*\*2.\*\* "My focus is length retention but mostly for my edges as my hair has been this thin for a good 15 years."

\- True: journey | Predicted: troubleshooting | Confidence: 0.58

\- Why: This post mentions a chronic hair problem (thin edges for 15 years) which strongly signals troubleshooting. The model missed that the primary content is a personal reflection about a long-standing situation rather than actively diagnosing or seeking to fix something. Short posts like this are harder because there is less context for the model to work with.



\*\*3.\*\* "I went natural 4 years ago and here is what helped me. Get a regular hair salon and stylist and get trims every three months. Do not be afraid of losing the length."

\- True: journey | Predicted: troubleshooting | Confidence: 0.51

\- Why: This post opens with a personal timeline (journey signal) but then gives prescriptive advice about trims and breakage (troubleshooting signal). The model appears to have weighted the advice content more heavily than the personal framing. Low confidence (0.51) shows the model was uncertain, which reflects the genuine ambiguity of this post.



\### Sample Classifications



| Post (excerpt) | Predicted Label | Confidence |

|---|---|---|

| "I wash moisturize and twist my hair every Sunday..." | regimen | 0.91 |

| "Camille Rose and TGIN products are really high quality..." | product | 0.87 |

| "Traction Alopecia is no joke. Make sure you are not vitamin deficient..." | troubleshooting | 0.83 |

| "This is the longest my 4c hair has ever been. It happened accidentally..." | journey | 0.79 |

| "The Curly Custard was stiff it did not seem to add weight to her hair..." | troubleshooting | 0.55 |



The first prediction is reasonable because the post is structured entirely around a weekly schedule with specific days and actions, which is the clearest signal for the regimen label.



\## What the Model Learned vs. What I Intended



I intended the model to learn the distinction between posts based on their primary communicative purpose — what the poster is trying to do. What the model appears to have actually learned is a surface-level vocabulary signal. Posts with words like "breakage," "thin," "dry," and "problem" get predicted as troubleshooting regardless of whether the poster is diagnosing something or just mentioning it in passing as part of a personal story or product review. This explains why product posts that describe a bad product experience (which use similar vocabulary) keep getting mislabeled as troubleshooting. The model learned the words associated with problems but not the intent behind them.



\## Spec Reflection

\- \*\*One way the spec helped:\*\* The requirement to define hard edge cases before annotating forced me to write decision rules for regimen vs product and journey vs troubleshooting. This made annotation more consistent and gave me clear language to use in my Groq baseline prompt.

\- \*\*One way implementation diverged:\*\* The spec suggests the fine-tuned model should meaningfully outperform the baseline. Mine did not on overall accuracy (0.677 vs 0.710), though it significantly improved on regimen (F1 0.88 vs 0.71). This revealed that 206 examples with 4 labels may not be enough for DistilBERT to consistently learn subtle topic boundaries, especially between product and troubleshooting.



\## AI Usage

1\. \*\*Label stress-testing and dataset building:\*\* I used Claude to help sort 206 forum posts into the four label categories before reviewing each one myself. Claude pre-labeled batches and I corrected approximately 15% of them — mostly cases where product posts describing bad results were labeled troubleshooting, which turned out to be the same confusion my fine-tuned model later showed.

2\. \*\*Failure analysis:\*\* I pasted my 10 wrong predictions into Claude and asked it to identify patterns. It identified that troubleshooting was being over-predicted, particularly for product and journey posts that mentioned hair problems. I verified this by reviewing each wrong prediction manually and confirmed the pattern holds across 7 of the 10 errors.

