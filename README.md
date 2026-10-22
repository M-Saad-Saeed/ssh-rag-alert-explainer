# SSH RAG Alert Explainer

**Final-year BSc Computer Science project — Brunel University**  
**Status:** Research and planning · Implementation pending  
**Last updated:** 22 October 2026

## Project overview

This project will investigate whether an AI assistant can explain suspected SSH password-guessing activity clearly, support its explanations with evidence, and recognise when the available logs do not justify a conclusion.

The prototype will combine a small rule-based log analyser with Retrieval-Augmented Generation (RAG). RAG will supply relevant MITRE guidance alongside the log evidence when the AI produces a report.

The scope is deliberately limited to one attack technique, one lab environment and one controlled evaluation.

## Research question

**Does retrieving relevant MITRE guidance improve the factual accuracy and uncertainty handling of SSH alert explanations, compared with the same language model without retrieval?**

## Aim and objectives

The aim is to **design, build and evaluate** a prototype for evidence-based explanations of suspected SSH password guessing.

1. Review relevant research and define the system requirements and evaluation criteria.
2. Design an isolated SSH lab, a structured report format and a controlled comparison.
3. Build the log analyser, document retrieval component and a simple report interface.
4. Evaluate the same model with and without retrieved guidance using unseen test scenarios.
5. Analyse errors, limitations and whether retrieval provides a measurable benefit.

## Scope

| Area | Planned scope |
| --- | --- |
| Attack technique | MITRE ATT&CK **T1110.001 — Password Guessing**, applied to SSH |
| Defensive reference | MITRE D3FEND **D3-ANET — Authentication Event Thresholding** |
| Environment | One isolated Linux ARM64 virtual machine with SSH and dummy accounts |
| Detection | Configurable rules for repeated authentication failures |
| AI task | Explain evidence, identify uncertainty and suggest supported defensive actions |
| Interface | A small dashboard or report viewer |

Automatic blocking, additional attack techniques, model training and a full cyber range are outside the current scope.

## Planned workflow

1. Generate controlled authentication activity in the lab and collect SSH logs.
2. Parse the logs, assign event IDs and apply the detection rules.
3. Retrieve relevant passages from a small, versioned MITRE reference collection.
4. Give the model the structured event evidence and retrieved guidance.
5. Produce a report separating **observed facts**, **possible interpretations**, **unknowns** and **defensive recommendations**.

Factual claims should point to supporting log events, while recommendations should cite the relevant reference passages. For example, a successful login after several failures does not by itself establish who logged in or whether data was stolen.

## Proposed tools

| Component | Current plan |
| --- | --- |
| Log processing and detection | Python |
| Vector database | ChromaDB, stored locally |
| Knowledge sources | Selected MITRE ATT&CK and D3FEND pages |
| Local model runtime | Ollama |
| Initial model candidate | Qwen3-4B-Instruct-2507, Q4_K_M quantisation |
| Development hardware | MacBook Air M4, 16 GB memory |

ChromaDB will hold reference passages, their embeddings and source metadata. Structured log evidence will be stored separately. The embedding model and retrieval settings will be selected during development and recorded for reproducibility.

Model selection remains provisional. A small development pilot will assess accuracy, uncertainty handling, response time and memory use. ZySec-AI/SecurityLLM is a possible comparison candidate if practical on the available hardware. One model will then be fixed for the main experiment. Lab collection and model experiments can run at different times to reduce memory pressure.

## Evaluation plan

The main experiment will compare **the same model with and without retrieved MITRE guidance**. Both conditions will receive the same log evidence and instructions, with generation settings held constant.

The provisional dataset is 60 distinct scenarios: 20 for development and 40 held back for final testing. The final set will cover normal logins, innocent password mistakes, controlled password guessing and incomplete or ambiguous records, with 10 cases per category.

Each final case will be run three times per condition, producing 240 reports. Repeated reports will be grouped by scenario for analysis; they will not be treated as 240 independent test cases.

Reports will be assessed using a predefined manual scoring rubric against the supplied evidence and independently recorded lab activity:

- **Factual accuracy:** unsupported or contradicted claims as a proportion of factual claims.
- **Coverage:** required facts correctly reported as a proportion of the expected facts.
- **Uncertainty handling:** appropriate recognition of missing evidence and incorrect refusals on answerable questions.
- **Citation support:** whether retrieved citations support the associated claims in RAG reports.
- **Response time:** time taken to generate each report.

Detector precision and recall will be measured separately. Analysis will use paired comparisons by scenario, results by scenario category and examples of failures. A correct refusal to infer a hidden event will not be marked wrong simply because the lab record reveals what happened.

The model, prompts, corpus and retrieval settings will be fixed before final testing. With a small corpus, this experiment can assess the benefit of adding retrieved guidance; it cannot establish that vector retrieval is better than every simpler way of supplying that guidance.

## Development roadmap

| Period | Planned milestone |
| --- | --- |
| October 2026 | Refine scope, prepare synopsis and organise initial reading |
| November | Set up the lab, develop parsing and detection, complete the required ethics process before data collection |
| December | Build retrieval and report generation; complete the model pilot |
| January 2027 | Freeze the experiment and conduct the main evaluation |
| Early February | Prepare a stable demonstration and analyse results |
| February–March | Refine the prototype and complete the dissertation |

Documentation and dissertation writing will continue throughout development.

## Progress

- [x] Define a narrow SSH password-guessing scope.
- [x] Identify initial academic papers and MITRE reference sources.
- [x] Draft the research question, objectives and evaluation plan.
- [ ] Finalise requirements and ethics arrangements with the supervisor.
- [ ] Build the isolated lab and log analyser.
- [ ] Pilot and select the language model.
- [ ] Implement retrieval and report generation.
- [ ] Run the held-out evaluation and document the results.

No working prototype or experimental results are available yet. Setup instructions will be added once there is a runnable implementation. Testing will use authorised lab systems and dummy accounts.

## Initial reading

- [MITRE ATT&CK: Password Guessing, T1110.001](https://attack.mitre.org/techniques/T1110/001/).
- [MITRE D3FEND: Authentication Event Thresholding](https://d3fend.mitre.org/technique/d3f:AuthenticationEventThresholding/).
- Javed and Paxson (2013), [Detecting Stealthy, Distributed SSH Brute-Forcing](https://doi.org/10.1145/2508859.2516719).
- Lewis et al. (2020), [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://proceedings.neurips.cc/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html).
- Niu et al. (2024), [RAGTruth: A Hallucination Corpus for Developing Trustworthy Retrieval-Augmented Language Models](https://aclanthology.org/2024.acl-long.585/).
- Gao et al. (2023), [Enabling Large Language Models to Generate Text with Citations](https://aclanthology.org/2023.emnlp-main.398/).
- Rajpurkar et al. (2018), [Know What You Don't Know: Unanswerable Questions for SQuAD](https://aclanthology.org/P18-2124/).
- Loumachi, Ghanem and Ferrag (2025), [Advancing Cyber Incident Timeline Analysis Through Retrieval-Augmented Generation and Large Language Models](https://doi.org/10.3390/computers14020067).

These sources inform the design. The effectiveness of this prototype remains to be tested.
