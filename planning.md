\# Project 3 Planning 



\## Community

I chose r/Naturalhair, r/BlackHair, and related forums (Type 4 Hair) 

because they are text-heavy communities where people discuss Type 4 

hair care in depth. The discourse varies significantly. Some posts 

describe detailed wash day routines, others recommend specific 

products, others troubleshoot hair problems, and others share 

personal hair journeys. This variation makes it a strong fit for a 

classification task because the distinctions are meaningful to 

community members themselves.



\## Labels



\### regimen

A post that describes a hair care routine, wash day process, styling 

method, or frequency schedule. The primary content is explaining 

what steps to take and when.



Example 1: "I wash, moisturize, and twist my hair every Sunday. I 

will oil my scalp every night and oil my ends every Wednesday."



Example 2: "I finger detangle, pre-shampoo, shampoo, and deep 

condition in 4 sections using butterfly clips. I keep these braids 

in for about 4 days then take them out."



\### product

A post focused primarily on recommending, reviewing, or comparing 

specific hair care products by name. The primary content is what 

product to use and whether it worked.



Example 1: "Camille Rose and TGIN products are really high quality 

and full of nourishing ingredients. I stock up during Black Friday 

and those last me the year."



Example 2: "I recently tried Knot Today and Curly Custard. The 

Curly Custard was sticky. My hair was left hard and crunchy and 

morphed into a weird shape."



\### troubleshooting

A post diagnosing or addressing a specific hair problem — breakage, 

dryness, lack of growth, alopecia, frizz, porosity issues, or 

damage. The primary content is identifying what is going wrong and 

why.



Example 1: "Please stay away from braids. Sounds to me like you are 

suffering from Traction Alopecia and run the risk of baldness being 

permanent."



Example 2: "Your hair probably has a lot of buildup on it. Once you 

get rid of all the buildup, products won't penetrate properly."



\### journey

A post sharing a personal hair story, milestone, emotional 

experience, transition update, or reflection on the natural hair 

journey. The primary content is the person's lived experience rather 

than instructions or diagnosis.



Example 1: "This is the longest my 4c hair has ever been. It 

happened accidentally as I wasn't actively trying to grow my hair 

since I'd given up on the ability of 4c hair to grow long."



Example 2: "I was in my 20s before I washed my own hair. You are 

definitely not the only person who wasn't taught how to care for 

your natural hair. You are not alone or behind schedule."



\## Hard Edge Cases



\### Regimen vs. Product

A post that describes a routine AND names specific products 

throughout.

Decision rule: If the post is structured around steps and timing, 

label it regimen even if products are mentioned. If the post is 

structured around evaluating whether a product worked, label it 

product.



\### Troubleshooting vs. Journey

A post that shares a personal struggle AND diagnoses a hair problem.

Decision rule: If the primary purpose is sharing emotional 

experience or a life update, label it journey. If the primary 

purpose is identifying what went wrong and how to fix it, label it 

troubleshooting.



\### Regimen vs. Troubleshooting

A post that gives routine advice in response to a hair problem.

Decision rule: If the post is prescriptive (here is what to do), 

label it regimen. If the post is diagnostic (here is what is wrong), 

label it troubleshooting.



\## Data Collection Plan

\- Sources: reddit posts r/Naturalhair, r/BlackHair, LHCF, LHC forum

\- Collected manually by copy-pasting individual posts and comments

\- Target: \~50 per label (200 total)

\- If a label is underrepresented after 150 examples: search 

&#x20; specifically for that label type using targeted keywords

&#x20; (e.g. "product review" for product, "alopecia" for troubleshooting)

\- Actual distribution after collection:

&#x20; Regimen: 55

&#x20; Product: 42

&#x20; Troubleshooting: 57

&#x20; Journey: 52

&#x20; Total: 206



\## Evaluation Metrics

I will use per-class F1 rather than accuracy alone because:

\- My label distribution is slightly uneven (product at 42 vs 

&#x20; troubleshooting at 57)

\- Accuracy could be misleadingly high if the model just predicts 

&#x20; the majority class

\- F1 shows whether the model is learning each boundary, not just 

&#x20; the easiest one

I will also use a confusion matrix to identify which label pair 

the model struggles with most.



\## Definition of Success

The fine-tuned model will be considered successful if it achieves:

\- Overall accuracy of 0.70 or above on the test set

\- Per-class F1 of 0.60 or above for all four labels

\- Meaningful improvement over the zero-shot baseline on at least 

&#x20; 3 of 4 labels

This would make the classifier useful as a content triage or 

community analytics tool.



\## AI Tool Plan



\### Label stress-testing

I will give Claude my four label definitions and edge case rules 

and ask it to generate 10 posts that sit at the boundary between 

regimen and product, and between troubleshooting and journey. If 

I cannot classify them cleanly, I will tighten the definitions 

before annotating.



\### Annotation assistance

I will use Claude to pre-label batches of 20 posts using my exact 

definitions. I will review and correct every label before accepting 

it. Pre-labeled rows will be flagged in the notes column of my CSV.



\### Failure analysis

After getting my wrong predictions from the fine-tuned model, I 

will paste them into Claude and ask it to identify systematic 

patterns. I will verify each pattern myself by re-reading the 

examples before writing up my evaluation.

