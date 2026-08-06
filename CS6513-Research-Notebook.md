# MSc Research Notebook

- **Student Name:** Dara Heaphy
- **Student ID:** 23369914
- **Proposed Research Title:** Closing the Coverage Gap with AI: AI-Assisted Code Coverage Management for Embedded C Software
- **GitHub Repository Link:** https://github.com/DaraHeaphy/MSc-Research-Notebook-Dara-Heaphy

------------------------------------------------------------------------

## 1. Project Overview & Evolution Track

I am investigating whether AI-assisted analysis can make code coverage reports more useful and actionable during embedded C testing. I am not simply trying to increase a coverage percentage. I want to examine whether an AI model can use source code, existing unit tests, coverage reports, and hardware-abstraction context to explain why particular functions, branches, or lines remain uncovered and then propose practical tests, mocks, or stubs.

### Current Working Research Question(s)

> **Last Updated: 06/08/2026**
> **Primary research question:** To what extent can I use AI-assisted coverage analysis to improve the identification, explanation, and reduction of code coverage gaps in embedded C software?

My supporting questions are:

1. How accurately can I use AI to explain why code regions remain uncovered?
2. How useful and technically correct are the tests, mocks, or stubs proposed for uncovered logic?
3. What changes do I observe in line, branch, and function coverage after I implement selected AI-assisted recommendations?
4. What limitations do I observe when the uncovered code depends on hardware, peripherals, timing, interrupts, or other embedded-system behaviour?

### Current Aim, Method, and Expected Contribution

- **Aim:** I aim to evaluate whether AI-assisted analysis can improve the process of identifying, explaining, and reducing code coverage gaps in embedded C software.
- **Method:** I plan to use a mixed-methods experimental study. Quantitatively, I will compare baseline coverage with coverage after selected AI-assisted recommendations are implemented. Qualitatively, I will assess whether the AI explanations are correct, whether the suggested tests/mocks/stubs are practical, and why individual recommendations succeed or fail.
- **Validation:** I will review, compile, and execute any generated test code before I include it in the evaluation. I will not treat AI output as authoritative.
- **Contribution:** I expect to produce practical evidence about where AI can and cannot help in an embedded coverage workflow, especially where uncovered code depends on hardware abstraction, stubs, mocks, or difficult setup conditions.

### The Pivot Log

- **06/08/2026 - Narrowing the topic from general LLM test generation to embedded-C coverage-gap diagnosis**
  - **Previous direction:** I was initially thinking about the project broadly as using an LLM to help improve code coverage.
  - **New direction:** I am now focusing on AI-assisted coverage-gap diagnosis and human-validated test, mock, or stub recommendations for embedded C.
  - **Reason for the pivot:** My first focused literature pass showed that recent LLM testing papers already cover iterative coverage-guided generation in Python, Java, JavaScript, and Kotlin, while embedded C papers cover automated testing, stubs, hardware constraints, and coverage criteria without LLM support. The research gap is therefore not simply "LLMs for tests"; it is whether AI can make embedded-C coverage reports more actionable under hardware-abstraction constraints.

- **06/08/2026 - Expanding the evaluation beyond coverage percentage alone**
  - **Previous direction:** I was treating before/after coverage improvement as the main success measure.
  - **New direction:** I will still measure line, branch, and function coverage changes, but I will also assess correctness, build/run success, repeatability, usefulness of the AI explanation, and validity of any mocks or stubs.
  - **Reason for the pivot:** Inozemtseva and Holmes (2014), MUTGEN, and Zhao et al. (2026) show that coverage and mutation scores need careful interpretation. This shifted my evaluation plan away from a simple before/after coverage claim and toward a mixed evaluation of metric improvement plus human validation.

### Research Gap Identified

The clearest gap I have identified is that current LLM-based test-generation research demonstrates promising feedback loops using coverage, program analysis, failed tests, or mutation feedback, but it is mainly evaluated on Python, Java, JavaScript, Kotlin, or benchmark-style unit testing. In contrast, the embedded-software literature explains the importance of C-level constraints, hardware abstraction, stubs, mocks, timing, instrumentation overhead, and industrial coverage criteria, but does not yet address LLM-assisted explanation or recommendation workflows for unresolved coverage gaps. My project sits between these areas: I want to evaluate whether an AI assistant can help me interpret uncovered embedded-C code and propose feasible tests, mocks, or stubs that are then manually reviewed, executed, and assessed for meaningful coverage improvement.

------------------------------------------------------------------------

## 2. Critical Source Evaluation Log

I selected the following literature because it helps me understand the main parts of my proposed study: coverage-guided AI test generation, program analysis, test validation and repair, mutation testing, conventional automated testing, and embedded C constraints. I have condensed this section so that it records my decisions without turning the notebook into a long bibliography dump.

### First Focused Literature Review - Ten Core Starting Papers (06/08/2026)

#### CoverUp - Pizzorno and Berger (2025)
- **Citation:** J. A. Pizzorno and E. D. Berger, "CoverUp: Effective High Coverage Test Generation for Python," *Proceedings of the ACM on Software Engineering*, vol. 2, FSE, Art. FSE128, 2025, doi: [10.1145/3729398](https://doi.org/10.1145/3729398).
- **Why I selected it:** I selected CoverUp because it closely matches my intended feedback loop: use coverage to locate uncovered code, prompt an LLM with targeted context, run the generated tests, and iterate.
- **Strengths and limitations I noted:** I found its workflow very relevant, but I identified a clear domain limitation because it targets Python rather than embedded C.
- **Connection to my project:** I will adapt its coverage-guided loop, but I need to add embedded-specific context such as HAL calls, mocks, stubs, interrupts, and hardware-dependent setup.

#### Panta - Gu, Nashid, and Mesbah (2026)
- **Citation:** S. Gu, N. Nashid, and A. Mesbah, "LLM Test Generation via Iterative Hybrid Program Analysis," in *Proceedings of the 48th IEEE/ACM International Conference on Software Engineering*, 2026, doi: [10.1145/3744916.3764553](https://doi.org/10.1145/3744916.3764553).
- **Why I selected it:** I selected Panta because it combines static control-flow analysis with dynamic coverage feedback, which is relevant to explaining uncovered branches and paths.
- **Strengths and limitations I noted:** I found the path-oriented approach useful, but I identified that the evaluation is Java-based and does not include embedded hardware constraints.
- **Connection to my project:** I will use this to justify giving the AI branch predicates, path context, coverage data, and HAL/mock information rather than only source code.

#### TELPA / Advancing Code Coverage - Yang et al. (2026)
- **Citation:** C. Yang, J. Chen, B. Lin, Z. Wang, and J. Zhou, "Advancing Code Coverage: Incorporating Program Analysis with Large Language Models," *ACM Transactions on Software Engineering and Methodology*, vol. 35, no. 5, Art. 118, pp. 1-31, 2026, doi: [10.1145/3748505](https://doi.org/10.1145/3748505).
- **Why I selected it:** I selected TELPA because it focuses on hard-to-cover branches and uses program analysis to prepare better LLM prompts.
- **Strengths and limitations I noted:** I found its context-selection idea strong, but I identified that its Python/object-construction setting must be translated carefully for C.
- **Connection to my project:** I will use the idea of a focused "coverage-gap packet" containing source, predicate, calls, coverage status, and embedded environment assumptions.

#### Automated Unit Test Improvement at Meta - Alshahwan et al. (2024)
- **Citation:** N. Alshahwan *et al.*, "Automated Unit Test Improvement using Large Language Models at Meta," in *Companion Proceedings of the 32nd ACM International Conference on the Foundations of Software Engineering*, pp. 185-196, 2024, doi: [10.1145/3663529.3663839](https://doi.org/10.1145/3663529.3663839).
- **Why I selected it:** I selected this paper because it reports an industrial LLM-based test-improvement workflow with validation gates.
- **Strengths and limitations I noted:** I found the build/pass/reliability/coverage filters very useful, but I identified that the work is Kotlin/Android rather than embedded C.
- **Connection to my project:** I will use this paper to shape my acceptance criteria: an AI suggestion must compile, run, pass reliably, improve coverage, and survive human review.

#### MUTGEN - Wang et al. (2026)
- **Citation:** G. Wang, Q. Xu, L. C. Briand, and K. Liu, "Mutation-Guided Unit Test Generation with a Large Language Model," *IEEE Transactions on Software Engineering*, 2026, doi: [10.1109/TSE.2026.3682975](https://doi.org/10.1109/TSE.2026.3682975).
- **Why I selected it:** I selected MUTGEN because it uses mutation feedback rather than relying only on raw coverage.
- **Strengths and limitations I noted:** I found the feedback-guided LLM process relevant, but I identified that the Java/JUnit setting may not transfer directly to embedded C.
- **Connection to my project:** I will use it to justify evaluating whether AI-generated coverage improvements are meaningful rather than merely numerical.

#### Coverage Is Not Strongly Correlated with Test Suite Effectiveness - Inozemtseva and Holmes (2014)
- **Citation:** L. Inozemtseva and R. Holmes, "Coverage Is Not Strongly Correlated with Test Suite Effectiveness," in *Proceedings of the 36th International Conference on Software Engineering*, pp. 435-445, 2014, doi: [10.1145/2568225.2568271](https://doi.org/10.1145/2568225.2568271).
- **Why I selected it:** I selected this paper as a caution against treating coverage as a direct proxy for test quality.
- **Strengths and limitations I noted:** I found its warning methodologically important, although it studies Java systems rather than embedded C.
- **Connection to my project:** I will use it to justify pairing coverage deltas with human judgement about correctness, useful assertions, and mock validity.

#### Zhao, Zhou, and Cohen (2026)
- **Citation:** J. Zhao, S. Zhou, and E. Cohen, "Do Coverage and Mutation Scores of LLM-Generated Test Suites Correlate with Their Effectiveness? A Replicability Study," *Proceedings of the ACM on Software Engineering*, vol. 3, ISSTA, Art. ISSTA002, 2026. Available: [https://arxiv.org/abs/2607.22880](https://arxiv.org/abs/2607.22880). DOI metadata to be rechecked before final submission.
- **Why I selected it:** I selected this paper because it revisits coverage and mutation scores specifically for LLM-generated tests.
- **Strengths and limitations I noted:** I found it directly relevant to my evaluation design, but I identified that it is very recent and not embedded C.
- **Connection to my project:** I will use it to support a two-layer evaluation: metric change first, then human validation of whether the test is realistic, executable, maintainable, and meaningful.

#### Garousi et al. (2018)
- **Citation:** V. Garousi, M. Felderer, C. M. Karapicak, and U. Yilmaz, "Testing Embedded Software: A Survey of the Literature," *Information and Software Technology*, vol. 104, pp. 14-45, 2018, doi: [10.1016/j.infsof.2018.06.016](https://doi.org/10.1016/j.infsof.2018.06.016).
- **Why I selected it:** I selected this survey as my domain anchor for embedded-software testing.
- **Strengths and limitations I noted:** I found its taxonomy useful for embedded testing constraints, but I identified that it predates modern LLM coding assistants.
- **Connection to my project:** I will use it to justify treating hardware abstraction, mocks, stubs, simulations, and test artefacts as first-class parts of the AI prompt context.

#### SmartUnit - Zhang et al. (2018)
- **Citation:** C. Zhang, Y. Yan, H. Zhou, Y. Yao, K. Wu, T. Su, W. Miao, and G. Pu, "SmartUnit: Empirical Evaluations for Automated Unit Testing of Embedded Software in Industry," in *Proceedings of the 40th International Conference on Software Engineering: Software Engineering in Practice*, pp. 296-305, 2018, doi: [10.1145/3183519.3183554](https://doi.org/10.1145/3183519.3183554).
- **Why I selected it:** I selected SmartUnit because it is a strong embedded-software testing paper with industrial evaluation and coverage-oriented unit-test generation.
- **Strengths and limitations I noted:** I found it useful as a non-LLM baseline, but I identified that it does not focus on natural-language explanation or human-facing diagnosis.
- **Connection to my project:** I will use it to avoid claiming that AI is the first automation approach for embedded testing; my novelty is explanation and reviewable recommendations.

#### CTGEN - Mangels and Peleska (2012)
- **Citation:** T. Mangels and J. Peleska, "CTGEN - A Unit Test Generator for C," *Electronic Proceedings in Theoretical Computer Science*, vol. 102, pp. 88-102, 2012, doi: [10.4204/EPTCS.102.9](https://doi.org/10.4204/EPTCS.102.9).
- **Why I selected it:** I selected CTGEN because it is directly about C unit-test generation and explicitly discusses stubs and low-level C issues.
- **Strengths and limitations I noted:** I found its treatment of stubs and C constraints very relevant, but I identified that it is not LLM-based and may require specifications or assertions.
- **Connection to my project:** I will use it to justify making mock/stub recommendation a central part of the AI-assisted workflow.

### Synthesis from the First Ten-Paper Review

I now define the gap precisely: there is promising work on LLM-assisted test generation and mature work on embedded C testing, but little direct evidence on AI-assisted coverage-gap explanation and human-validated test/mock/stub recommendation for embedded C software. The AI papers give me workflow ideas; the embedded papers define the constraints that make my study necessary.

### Screened Background Sources Retained for Later Use

I also screened 20 additional sources and retained them as background or supporting material rather than treating them as the first ten core papers.

- **SymPrompt - Ryan et al. (2024):** I retained this for path-/constraint-aware prompting, but it is not embedded C.
- **HITS - Wang et al. (2024):** I retained this for slicing and complex branch targeting, but it is Java-focused.
- **Static Program Analysis Guided LLM-Based Unit Test Generation - Roy Chowdhury et al. (2024):** I retained this for static-analysis prompt context, but I need to correct author spelling in final references.
- **CodaMOSA - Lemieux et al. (2023):** I retained this as a search-based/LLM hybrid baseline for coverage plateaus.
- **ChatUniTest - Chen et al. (2024):** I retained this for context selection and test repair ideas.
- **TestART - Gu et al. (2024):** I retained this only as background because it appears to be a preprint rather than a core peer-reviewed source.
- **MuTAP - Dakhel et al. (2024):** I retained this for mutation-aware evaluation support.
- **Sch?fer et al. (2024):** I retained this for broad empirical evidence on LLM-generated unit tests.
- **Yuan et al. (2024):** I retained this for correctness, sufficiency, readability, and usability measures.
- **Yang et al. (2024):** I retained this for fair model/prompt evaluation design.
- **EvoSuite - Fraser and Arcuri (2011):** I retained this as a conventional automated testing baseline, not a direct embedded-C method.
- **DynaMOSA - Panichella et al. (2018):** I retained this for prioritising uncovered test targets.
- **Jia and Harman (2011):** I retained this as a mutation-testing survey.
- **Wang et al. (2024):** I retained this as a broad LLM software-testing survey.
- **Zhang et al. (2025):** I retained this as an LLM unit-testing systematic review, but I will treat it cautiously if it remains a preprint.
- **Konstantinou et al. (2026):** I retained this for model-version concerns and baseline comparisons.
- **Wu et al. (2007):** I retained this for embedded coverage constraints such as instrumentation overhead.
- **Sung, Choi, and Shin (2007):** I retained this for hardware-dependent interface testing.
- **Weiss et al. (2024):** I retained this for trace-based structural coverage in embedded systems.
- **Other recent LLM testing sources:** I will only promote these to core status if full-text reading shows a direct link to my final introduction.

------------------------------------------------------------------------

## 3. Generative AI & Digital Tool Transparency Log

I will record any use of AI or other digital research tools that affects my literature search, source evaluation, wording, or analysis. I will check source details and claims against the original publication record and revise this notebook as I complete the full-text review.

### 06/08/2026 - ChatGPT for initial literature discovery

- **Tool used:** ChatGPT.
- **Task performed:** I used ChatGPT to help identify candidate literature relevant to my proposed research study.
- **Exact prompt/input used:** `For my masters thesis, I want to explore the idea of using an LLM to assist in the code coverage process. Particularly in providing a feedback loop at the end of a static coverage analysis that includes suggested fixes/implementations for uncovered code. As a starting point I need you to find me some solid, reliable literature on Google Scholar that I can go through myself and verify whether or not they are relevant. Ensure any and all sources and links are included for me.`

- **How I critically evaluated or modified the output:** Because I had started the notebook later than I should have, I used ChatGPT (5.5) to create an initial candidate source list and a first-pass notebook structure quickly rather than pretending I had built it slowly over several weeks. I then checked titles, authors, publication venues, years, page ranges, and DOI or arXiv records against publisher, conference, institutional, or bibliographic sources. I was surprised by how relevant the initial list was after review, but I still removed or downgraded material that did not directly support my study. I am now going through the sources one by one, checking their relevance in more detail, and revising the notebook as I complete that review.

------------------------------------------------------------------------

## 4. Weekly Activity & Reflection Logs

### Week 05: Focus Area - Initial Literature Review and Scope Correction

- **Activities Completed:** I defined the working title, research aim, research questions, objectives, and methodology; assembled an initial literature set; used AI to help create an initial notebook structure; verified publication details; selected ten core starting papers; and updated the pivot log.
- **Key Insights / Discoveries:** I found that the closest AI-assisted approaches use iterative feedback rather than a single prompt. I also found that coverage values must be interpreted alongside test correctness and fault-detection measures. The key gap is that LLM test-generation papers rarely address embedded C hardware-abstraction constraints, while embedded C testing papers do not address LLM-assisted explanation and recommendation of tests, mocks, or stubs.
- **Obstacles Encountered & How I Overcame Them:** I recognised that my notebook had not been updated incrementally enough before this point. This late start led me to use AI to gather an initial candidate source list and impose a first-pass structure on the notebook, but I logged that use explicitly and reviewed the sources rather than treating the AI output as authoritative. I am now working through the sources one by one and still need to complete deeper full-text reading.
- **Plan for Next Week:** I will read the ten core papers more fully, mark which claims are strong enough for the two-page IEEE introduction, begin drafting a short LaTeX introduction outline with matching notebook entries, and identify the likely experimental subject codebase and coverage tooling.

------------------------------------------------------------------------

**Next step:** I will continue the comprehensive literature review by reading the ten core papers in full, verifying citation details for recent 2026 sources, and starting the IEEE introduction outline in a way that matches this notebook's research-gap narrative.

I hope that by putting a strong effort into my literature review, I will set myself up well to tackle the remaining tasks in both this assignment, and the corresponding introduction to my academic paper assignement. 
