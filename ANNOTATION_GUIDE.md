# Annotation Guide

This concise guide combines the essential annotation instructions and definitions for the final corpus accompanying *Beyond Free-Form Generation: Measuring Interface Composition in Competitive NLP Systems*.

## Annotation unit and process

Each record describes one final system submitted to, or officially reported for, a competition or shared task. Read the located source, identify the components that contribute to its prediction, and distinguish training operations from prediction operations. Baselines, failed trials, and future plans are outside the annotation unit.

Annotate the five fields separately from the documented implementation. Each known field requires a source locator from the same record. Model names and task conventions do not supply missing implementation details. The output interface set covers the complete pipeline, including intermediate stages.

The primary annotator annotated the full corpus. A second annotator independently annotated 179 systems without seeing the primary labels or agreement results. Agreement was calculated before adjudication; disagreements were resolved by returning to the source evidence. The released corpus contains the final labels.

## Fields and stored values

`source` identifies `kaggle` or `shared_task`; `system_id` identifies a system, and `cluster_id` identifies its competition or task year.

| Field | Status column | Label column | Definition |
|---|---|---|---|
| Task | `task_status` | `task_roles` | The prediction or decision task evaluated for the system |
| Backbone | `backbone_status` | `backbone_roles` | Model families and rule components contributing to prediction |
| Output interface | `output_status` | `output_interfaces` | Computational outputs that determine the prediction |
| Adaptation method | `adaptation_status` | `adaptation_methods` | How predictive model parameters were updated or kept fixed |
| System method | `system_status` | `system_methods` | Retrieval, combination, checking, transformation, or planning in the prediction pipeline |

Each field has one status: `known` means at least one label is supported; `unknown` means the field applies but its value is not established; `not_applicable` means the documented system is outside the field's applicable cases. A missing training description is `unknown`, rather than evidence for `no_tuning`.

Known label sets are sorted and separated by `|`. Unknown or inapplicable fields have blank label cells. `low_confidence_fields` lists label column names separated by `|`; a blank value means no field was flagged.

## Label definitions

The definitions below summarize the labels present in the released data.

| Field | Label | Meaning |
|---|---|---|
| Task | `classification` | Choose one or more labels from a predefined set |
| Task | `regression` | Predict a continuous value or ordered scalar score |
| Task | `token_span` | Predict labels or boundaries for tokens, characters, or text spans |
| Task | `retrieval` | Retrieve relevant items when retrieval quality is evaluated |
| Task | `ranking` | Order candidates when their ordering is evaluated |
| Task | `generation` | Construct text, code, queries, grids, or another open or structured sequence |
| Task | `agent` | Complete an evaluated goal through actions, tools, or environment feedback |
| Task | `code_ordering` | Restore the order of notebook cells or code units |
| Backbone | `encoder` | A bidirectional encoder contributes to prediction |
| Backbone | `decoder_llm` | A decoder LLM contributes to generation, scoring, embedding, or judgment |
| Backbone | `embedding_model` | A model produces vectors for similarity, recall, or matching |
| Backbone | `reranker` | A dedicated model reorders retrieved candidates |
| Backbone | `reward_model` | A model trained on rewards or preferences scores candidates |
| Backbone | `gbdt` | A gradient boosted decision tree contributes to prediction |
| Backbone | `rules` | Deterministic rules, solvers, indexes, or search affect prediction |
| Output interface | `classification_head` | A dedicated classification head emits scores for fixed classes |
| Output interface | `regression_head` | A prediction head emits a continuous scalar or vector |
| Output interface | `token_logits` | Probabilities over a constrained answer token vocabulary determine the answer |
| Output interface | `token_classification` | A token or span head emits label probabilities |
| Output interface | `ranking_score` | An explicit score determines candidate order, filtering, or selection |
| Output interface | `free_generation` | Decoding produces an open or structured sequence beyond a fixed token label set |
| Output interface | `action_policy` | The output selects the next tool, action, or environment operation |
| Adaptation method | `full_fine_tuning` | All or most parameters of a predictive model are updated |
| Adaptation method | `lora` | LoRA adapters are trained on a base model without quantization |
| Adaptation method | `qlora` | LoRA adapters are trained while the base model is quantized |
| Adaptation method | `no_tuning` | A predictive model is explicitly kept frozen or used without parameter updates |
| System method | `retrieval` | The prediction pipeline retrieves content from a corpus, index, or candidate bank |
| System method | `reranking` | Retrieved candidates undergo a separate ordering stage |
| System method | `ensemble` | Predictions from multiple models, folds, seeds, or inference streams are combined |
| System method | `postprocess` | A deterministic transformation changes the output after prediction or scoring |
| System method | `tta_ttt` | Test time augmentation combines transformed inputs, or test time training updates parameters |
| System method | `verifier` | A checker accepts, rejects, selects, or triggers regeneration of a candidate |
| System method | `agent_planning` | State, feedback, or intermediate results determine the next action or search path |

A generated string parsed into a label does not by itself establish a `classification_head`. Retrieval can be an evaluated task, a system method, or both. The labels do not separately attribute each interface to a particular decoder component.

## Derived interface classes

For systems whose known backbone includes `decoder_llm`, the interface class summarizes the known output interface set. The primary classification treats `action_policy` as constrained.

| `interface_class` | Meaning |
|---|---|
| `free_generation_only` | The output set is exactly `free_generation` |
| `constrained_only` | At least one constrained interface is present, with no free generation |
| `hybrid` | Free generation and at least one constrained interface are both present |
| `output_unknown` | A decoder LLM is documented, but the output field is ineligible |
| `not_decoder` | The backbone field does not establish a decoder LLM, including when the backbone is unknown |

`derived_outcome` is `0` for free generation only and `1` for constrained only or hybrid; it is blank for ineligible records. Systems without decoder LLMs used in the comparator must have a known backbone set that excludes `decoder_llm`.

The `derived_outcome_agreement` statistic in `aggregate_statistics.csv` refers to agreement on `interface_class`, including its eligibility states: 163 of 179 records (91.1%). It does not refer only to the binary outcome column.
