# SAGA: A Participant-specific Examination of Story Alternatives and Goal Applicability for a Deeper Understanding of Complex Events

SAGA is a crowdsourced dataset consisting of ~ 6.2K goal related annotations for complex events in ~ 2K alternative stories.  An original (aka "actual") story from a set of alternative stories is first annotated from the perspective of a single participant, capturing their intended goal, its outcome and related knowledge.  The annotated goal is then applied to the set of alternative stories, collecting goal outcome and related knowledge for the altered situations captured by these stories.  More details on the annotations are presented below.  Here we release the collected annotations as training, validation and test splits.  We also release the source code used for prompting GPT,  Flan-T5 and T5 and fine-tuning Flan-T5 and T5 models for which we reported results in the paper.

Our paper is available on Arxiv (http://arxiv.org/abs/2408.05793) and is published in the Findings of the 2024 Association for Computational Linguistics (https://aclanthology.org/2024.findings-acl.2767/).  

## Dataset Information

| Split |  Actual Goal Annotations | Alternate Goal Annotations |
| :---: | :---: | :-----: |
| Train |  2628 |  2481   |
| Valid |   106 |   214   |
| Test  |   219 |   512   |
| Total |  2985 |  3255   |

The story sets in each of the splits are unique (there is no overlap across the sets).  The numbers reported for the Test and Valid splits meet our evaluation quality criteria; a small number of goal annotations did not meet our quality requirement (27 for the valid split and 48 for the test split) and are released as  quasi- val and test (qval & qtest files) splits. 

## Building the Dataset

### Motivation

For this data collection, we obtain participants' intentions as the goals they strive to achieve with their actions in the story.  We show that mapping multiple actions to an intended goal is not simple and goal based reasoning for minimally altered situations is not robust.  

### Preparing Stories

We selected original (aka "actual") stores from the PASTA dataset (Ghosh et al, 2023, https://aclanthology.org/2023.tacl-1.73/) after resolving coreferent mentions and selecting stories containing several mentions of a participant (or multiple participants).  We then filtered the participants, keeping only the ones that are subjects of verbs.For each of these participants, we highlighted all their mentions in the story to create a participant-specific story (a SAGA story) which we then annotated with goal related knowledge. Our manual evalation before proceeding with annotation showed that this automated identification yielded 97% volitional participants. 

| Annotated Stories | Total  | Actual | Alternate |
| :---: | :----: | :---: | :-----: |
| PASTA |  1837  |  886 |   951    |
| SAGA |   2080  |  995 |  1085    |

### Crowdsourcing Annotations

Annotators were presented an actual partcipant-specific SAGA story and asked to 
1) Describe the partcipant's intended goal (an overarching goal) that captures as many of their story actions as possible, and whether they  achieved the goal in the story.
2) A next action that is highly likely to happen after the story and if this action helps achieve the goal.  If the goal is already achieved in the story, this next action may or may not help with the goal.
3a) A plan containing actions that can help them achieve the goal, i.e., if the goal is not achieved in the story or with the next action. 
3b)  Whether the plan was revised based on the story outcome or if the plan is a continuation of the original plan 
3c) Whether the participant is satisfied with the goal achievement/striving.

Annotators are next presented with a participant specific alternate story and the goal description from the corresponding actual story and asked to 
1) identify if the goal is applicable to the story and if the participant succeeds in achieving it in the story.
2) & 3) annotations similar to the ones obtained for actual stories.
This annotation is done for each alternate story. 

### Validating Annotations

A random 20% of annotations belonging to 100 actual and 209 corresponding alternate stories were evaluated by 3 crowd workers and 1 expert.  Each of the annotations steps identified above were evaluated seperately without any cascading errors.   The averate inter-annotator agreement score was high, 0.8 using the weighted Fleiss's Kappa.   

### Annotations Released

The data folder contains the following data files:
actual_train.json, actual_val.json, actual_test.json, actual_qval.jsonl and actual_qtest.jsonl
alternate_train.json, alternate_val.json, alternate_test.json, alternate_qval.jsonl and alternate_qtest.jsonl

Each annotation is assigned a unique id which consists of a number and a alphanumeric suffix of 'a', 'c1', c2' or 'c3' in the range from 0 to 2985 (the total annotations collected with our AMT HIT).  The fields associated with each annotation are as follows where the ones notated with an * are collected on AMT and the remaining ones are automatically computed:

|# | field | Annotation|
|:---:|:---:|:--:|
|1 | instance_id  | A unique id for each annotation.
|2 | story_id | The unique id of the ROC Story the SAGA stories is based on.
|3 | alternate_storyid | A unique id we assigned to the alternate PASTA Stories. This value for the original (or actual) stories is 0.
|4 | participant_id | A unique id we assigned to each volitional participant in a story.
|5 | story_line1 | The first sentence of the story. 
|6 | story_line2 | The second sentence of the story. 
|7 | story_line3 | The third sentence of the story. 
|8 | story_line4 | The fourth sentence of the story. 
|9 | story_line5 | The fifth sentence of the story. 
|10 | participant | The highlighted volitional participant in the story.  
|11* | original_goal | The annotated goal description for the highlighted participant of the story.
|12 | all_goals | The 3 goals from the 3 annotators useful for reference-based metric calculations.
|13*| goal_success | A value from 1 to 5 indicating the success of the goal within the story.  These numbers are matched with the text in the next annotation.
|14| goal_success_text | The textual value for the above annotation indicating the success of the goal within the story.
|15*| next_action | A likely next action involving the highlighted participant that happens after the story. 
|16| all_next_actions | The 3 next actions from the 3 annotators useful for reference-based metric calculations.
|17*| next_action_explanation | A justification for why the next action is likely to happen after the story
|18| all_explanations | The 3 explanations from the 3 annotators useful for reference-based metric calculations. 
|19| all_actions_and_explanations | The 3 actions and explanations from the 3 annotators useful for reference-based metric calculations. 
|20*| goal_direction | A value from 1 to 5 indicating the success of the story with the annotated next action after the story.  These numbers are matched with the text in the next annotation.
|21| goal_direction_text | The textual value for the above annotation indicating the success of the goal with the next action after the story.
|22*| goal_satisfaction | A value from 1 to 5 indicating whether the highlighted participant would be satisfied with their goal achievement thus far and its likely achievement later. These numbers are matched with the text in the next annotation.
|23| goal_satisfaction_text | The textual value for the above annotation indicating the participant's satisfaction.
|24*| future_plan | For goals that have not been achieved, this provides a plan for achieving the goal.
|25| all_plans | For goals that have not been achieved, this provides a plan for achieving the goal.
|26*| goal_revision | A value from 1 to 5 indicating whether the plan is revised based on the story outcome.  These numbers are matched with the text in the next annotation.
|27| goal_revision_text | The textual value for the above annotation indicating the plan is revised.
|  | Alternate Story Annotations contain the following additional attributes: 
|28| story_characteristics | Contains sub-annotations for how valid the alternate sotry is. 
|29*| story_valid | A rating from 1 to 3 for the story validity as it applies to understanding the goal.  These numbers are matched with the text in the next annotation.
|30| story_valid_text | The textual value for the above annotation indicating the validity of the story.
|31*| valid_line1 | A binary valid indicating whether the first story sentence is confusing.
|32*| valid_line2 | A binary valid indicating whether the second story sentence is confusing.
|33*| valid_line3 | A binary valid indicating whether the third story sentence is confusing.
|34*| valid_line4 | A binary valid indicating whether the fourth story sentence is confusing.
|35*| valid_line5 | A binary valid indicating whether the fifth story sentence is confusing.
|36*| alternate_goal_applicability_list | Values from 3 annotators indicating the applicability of the goal to the alternate story.  The number of values could be less than 3 if an annotator believes the story to be confusing as measured by the story_characteritics.
|37| alternate_goal_applicability_agreement | Number of annotators agreeing on applicability. 
|38| alternate_goal_applicability | Average value from the above annotation. 
|39| alternate_goal_applicability_text | The textual value for the above annotation indicating the applicability of the original_goal annotation to the alternate story. When the original_goal annotation is not applicable to the alternate story, the remaining annotations are not collected. 
|40*| alternate_goal_success_list | Values from upto 3 annotators indicating whether the goal was achieved in the alternate story.
|41| alternate_goal_success | Average value from the above annotation.
|42| alternate_goal_success_text | A textual value indicating whether the original_goal succeeded in the story. 
|43| actual_next_action | The next action annotation from the actual story.
|44*| next_action_valid | A value of 1 to 3 indicating whether the next action from the actual story is valid for the alternate story.
|45| next_action_valid_text | A textual value for the above annotation indicating whether the next action from the actual story is valid for the alternate story.
|46| paired_action | A binary value indicating whether the next action annotations of the actual and alternate story are different. 1-indicates that they are different and these annotations are used in task 3b.
|47| actual_explanation | The next action explantion annotation from the actual story.
|48*| explanation_valid | A value of 1 or 2 indicating whether the actual explanation justifies the next action annotation for the alternate story.  
|49| explanation_valid_text | A textual value for the above annotation indicating whether the actual explanation justifies the next action annotation for the alternate story.  
|50| paired_explanation | A binary value indicating whether the next action explanation annotations for the actual and alternate story are different. 1-indicates they are different and these annotations are used in task 3b.
|| 
|51| evaluation | The evaluated goal annotations contain the following 13 Human Evaluation scores.  The values are a rating of 1 (lowest) to 5 (highest) to evaluate the various goal related annotations:
|52*| goal_coherence | measures the coherence of the original_goal annotation for the story context.
|53*| goal_explain | measures the explainability of the original_goal annotation for the story context.
|54*| goal_intention | measures the intentionality of the participant expressed by the original_goal annotation for the story context.
|55*| goal_faithful | measures the faithfulness of the original_goal annotation with respect to the story context.
|56*| goal_truth | measures the truthfulness of the original_goal with respect to the story context.
|57*| goal_achieve | measures the correctness of the goal_success annotation.
|58*| na_coherence | measures the coherence of the next_action annotation.
|59*| na_cohesion | measures the cohesion of the next_action annotation.
|60*| na_explain | measures how well the explanation justifies the next_action
|61*| na_help | measures the correctness of the goal_direction annotation.
|62*| goal_satisfy | measures the correctness of the goal satisfaction annotation.
|63*| goal_revision | measures the correctness of the goal_revision annotation.
|64*| goal_completion | measures the appropriatenss of the future_plan annotation.

## Source code for Tasks & Evaluation

The src_code directory contains these files.  These will be updated soon. 

This dataset was primarily built to advance research goal reasoning capabilitiesof language models for a deep and robust understanding of complex events.  We formulated 5 tasks aimed at understanding various aspects of goal reasoning.  We benchmarked GPT-3.5-Turbo, GPT-4 and several sizes of T5 and FlanT5 models on these tasks using various automated and human evaluation metrics.  Human evaluation consisted of 3 crowd workers assigning a 1-5 likert value for the evaluated attributes such as coherence, cohesion, intentionality, explainability, truthfulness and faithfulness.  We report a few of the results here.  Please refer to the paper for more results and analysis.


### Task 1: Goal Inference

We compare models using both prompting and fine-tuning (with both the actual and alternate training data) to generate a participant's overarching goal.  We evaluated the generated goals using automated metrics and also a human evaluation ofcriteria such as coherence, explainability, intentionality, truthfulness and faithfulness of the goal for the given story participant's actions.  Here we present the human evaluation scores and refer you to the paper for automated evaluation scores such as Rouge, BLEU, etc.

| Model      | Prompt Type | Coherence     | Explainability | Intentionality | Faithfulness | Truthfulness |
|     :---:  |   :---:    |  :---: | :---: | :---: | :---: | :---: |
| Reference        |  -     | 4.61 | 4.54 | 4.39 | 4.68 | 4.73 |
| Flan-T5-base     | 3-shot | 3.91 | 4.12 | 3.74 | 4.32 | 4.22 |
| Flan-T5-XXL      | 3-shot | 4.35 | 4.50 | 4.00 | 4.71 | 4.66 |
| GPT-3.5-Turbo    | 3-shot | 4.39 | 4.44 | 3.83 | 4.83 | 4.69 |
| GPT-4            | 3-shot | 4.35 | 4.31 | 3.63 | 4.77 | 4.74 |
| FlanT5-base-SAGA | 3-shot | 4.41 | 4.39 | 4.29 | 4.57 | 4.56 |

### Task 2: Goal Inference Transferability

This task examines how well models are able to identify if a goal is applicable to an alternate situation.   We identify story nuance using the judgements from 3 annotators, and report results for Full agreement, Partial Agreement and Oversall.  Full agreement is when all 3 annotators agree that a goal is applicable and Partial agreement is when the 3 annotators disagree on whether a goal is applicable.  We report F1 for some of the models here and refer you to our paper for additional models. 

| Model      |  Prompt Type | Overall | Full Agree     | Partial Agree | 
|     :---:         |   :---: |:---:   |  :---: | :---: |
| minority-label    | 3-shot | .31 | .17 | .67 | 
| Flan-T5-base      | 3-shot | .27 | .30 | .25 | 
| Flan-T5-XXL       | 3-shot | .60 | .50 | .71 | 
| GPT-3.5           | 3-shot | .53 | .41 | .67 | 
| GPT-4             | 3-shot | .59 | .48 | .72 | 
| Flan-T5-large-SAGA| 3-shot | .65 | .71 | .61 | 
| Flan-T5-XL-SAGA   | 3-shot | .76 | .84 | .70 | 

### Task 3a: Generating Explainable Next Actions

In this task we compare models on their generation of next actions story and the justification for why they are the most likely next actions.  Our human evaluation for this task includes cohesion and coherence of the next action and the explanability of the justification (reported below).  Please refer to the paper for automated evaluation scores such as Rouge, BLEU, etc.

Evaluation of Next Actions and Explanations for Actual Stories
| Model | Prompt Type | Coherence | Cohesion | explainability|
|     :---:  |  :---:    |  :---:    |  :---: | :---: |
| Reference | 3-shot | 4.63 | 4.51 | 4.74 | 
| Flan-T5-base  | 3-shot | 2.38 | 2.14 | 1.68 |
| Flan-T5-XXL | 3-shot | 4.48| 4.37 | 3.91 |
| GPT-3.5-Turbo  | 3-shot | 4.76  | 4.67 | 4.79 | 
| GPT-4   | 3.shot | 3-shot | 4.77 | 4.67 | 4.79 | 

Evaluation of Next Actions and Explanations for Alternate Stories
| Model | Prompt Type | Coherence | Cohesion | explainability|
|     :---:  |  :---:    |  :---:    |  :---: | :---: |
| Reference | 3-shot | 4.51 | 4.48 | 4.34 | 
| Flan-T5-base  | 3-shot | 2.14 | 2.08 | 1.86 |
| Flan-T5-XXL | 3-shot | 3.82| 3.71 | 3.58 |
| GPT-3.5-Turbo  | 3-shot | 4.53  | 4.41 | 4.70 | 
| GPT-4   | 3.shot | 3-shot | 4.55 | 4.48 | 4.77 | 

### Task 3b: Identifying the likeliness of Next Actions and correctness of their justifications  

In this task given two Next Actions for an alternate story we infer the likelihood of the Next Action as Most Likely, Likely or Not Likely, and given two explanations for a Next Action we identify which is appropriate. We compare using a weighted F1 for both NLI tasks and also report macro F1 for the 3-label inference task. 

| Model      | Prompt Type | Next Action mF1 | Next Action wF1     | Explanation wF1  
|     :---:  |  :---:    |  :---:    |  :---: |
| Majority Label Baseline | 3-shot | .48 | .91 | .57 |
| Flan-T5-large           | 3-shot | .37 | .60 | .78 |
| Flan-T5-XXL             | 3-shot | .45 | .67 | .79 | 
| GPT-3.5-Turbo           | 3-shot | .37 | .48 | .59 |
| GPT-4                   | 3-shot | .47 | .72 | .84 | 
| Flan-T5-large-SAGA      | 3-shot | .49 | .74 | .75 |

### Task 4: Goal Achieving Plan Inference
For a goal that is not achieved in the story or with the Next Action we compare models on the generate plans for goal achieving and identify if a plan is revised based on the story outcome thus far.  We report F1 for this plan type identification and report Human Evaluation scores for how the appropriateness of the generated plan and the correctness of the plan type. Identifying appropriate plans and their type is a difficult task even for humans as shown by the reference scores.

Generated Plans for an Actual Story
| Model      | Prompt Type  | Plan Type F1    | Plan Appropriateness  | Plan Type |
|     :---:  | :---: | :---: | :---: | :---: |
| Reference          | 3-shot | .77 | 4.19 | 4.19 |
| Flan-T5-base       | 3-shot | .37 | 2.92 | 2.92 |
| Flan-T5-XXL        | 3-shot | .44 | 2.24 | 2.24 |
| GPT-3.5-Turbo      | 3-shot | .83 | 2.35 | 2.35 |
| GPT-4              | 3-shot | .82 | 2.46 | 2.46 | 
| Flan-T5-base-SAGA  | 3-shot | .79 | 2.30 | 2.30 |

Generated Plans for an Alternate Story
| Model      | Prompt Type  | Plan Type F1    | Plan Appropriateness  | Plan Type |
|     :---:  | :---: | :---: | :---: | :---: |
| Reference          | 3-shot | .77 | 4.10 | 4.09 |
| Flan-T5-base       | 3-shot | .48 | 3.09 | 3.01 |
| Flan-T5-XXL        | 3-shot | .38 | 3.57 | 2.75 |
| GPT-3.5-Turbo      | 3-shot | .72 | 3.99 | 2.51 |
| GPT-4              | 3-shot | .79 | 4.07 | 2.53 | 
| Flan-T5-base-SAGA  | 3-shot | .91 | 2.78 | 2.46 |

### Task 5: Identifying goal achievement
We finetuned base and large models to identify whether a participant was involved in causing or experiencing the event, given the story and the changes resulting from the endpoint.  We also compared models trained seperately on agent and patient versions of the dataset.  We evaluated this task using Accuracy and macro F1.

Goal Achievement within the Story
| Model      | Prompt Type  | Actual Stories    | Alternate Stories   |
|     :---:  |  :---:    |  :---:    | :---:     |
| Majority Label    | 3-shot | .38 | .22 |
| Flan-T5-base      | 3-shot | .28 | .32 |
| Flan-T5-XXL       | 3-shot | .47 | .55 |
| GPT-3.5-Turbo     | 3-shot | .42 | .54 |
| GPT-4             | 3-shot | .46 | .68 |
| Flan-T5-base-SAGA | 3-shot | .44 | .50 |

Goal Achievement with Next Action after the Story
| Model      | Prompt Type  | Actual Stories    | Alternate Stories   |
|     :---:  |  :---:    |  :---:    | :---:     |
| Majority Label    | 3-shot | .28 | .27 |
| Flan-T5-base      | 3-shot | .41 | .33 |
| Flan-T5-XXL       | 3-shot | .58 | .32 |
| GPT-3.5-Turbo     | 3-shot | .58 | .44 |
| GPT-4             | 3-shot | .41 | .56 |
| Flan-T5-base-SAGA | 3-shot | .55 | .44 |

Participant Satisfaction with Goal Achievement
| Model      | Prompt Type  | Actual Stories    | Alternate Stories   |
|     :---:  |  :---:    |  :---:    | :---:     |
| Majority Label    | 3-shot | .28 | .26 |
| Flan-T5-base      | 3-shot | .35 | .36 |
| Flan-T5-XXL       | 3-shot | .52 | .45 |
| GPT-3.5-Turbo     | 3-shot | .53 | .47 |
| GPT-4             | 3-shot | .56 | .49 |
| Flan-T5-base-SAGA | 3-shot | .65 | .51 |

## Citation

Please use the following BibTex entry to cite our work.
We will update this when the formal proceedings are published.

```
@inproceedings{vallurupalli-etal-2022-poque,
    title = "{SAGA}: A Participant-specific Examination of Story Alternatives and Goal Applicability for a Deeper Understanding of Complex Events",
    author = "Vallurupalli, Sai  and
      Erk, Katrin  and
      Ferraro, Frank",
    booktitle = "Findings of the Association for Computational Linguistics",
    month = Aug,
    year = "2024",
    address = "Online and Bangkok, Thailand",
    publisher = "Association for Computational Linguistics",
    url = "",
    doi = "",
    pages = "",
}
```

## Contributors

[Sai Vallurupalli], [Katrin Erk](https://www.cs.utexas.edu/people/faculty-researchers/katrin-erk), [Frank Ferraro](https://redirect.cs.umbc.edu/~ferraro/)
