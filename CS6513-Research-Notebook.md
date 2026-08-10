# MSc Research Notebook

- **Student Name:** Dara Heaphy
- **Student ID:** 23369914
- **Proposed Research Title:** Closing the Coverage Gap with AI: AI-Assisted Code Coverage Management for Embedded C Software
- **GitHub Repository Link:** https://github.com/DaraHeaphy/MSc-Research-Notebook-Dara-Heaphy

---
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

---
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

### Week 6 Literature Review Extension - Five Additional Core Papers (10/08/2026)

After completing the first focused review of ten papers, I extended the core literature set in Week 6 by promoting five of the most relevant sources from my screened background pool. I chose these because they strengthen the parts of the research gap concerned with path-aware prompting, static/program analysis, coverage plateaus, and iterative validation of generated tests.

#### SymPrompt - Ryan et al. (2024)
- **Citation:** G. Ryan, S. Jain, M. Shang, S. Wang, X. Ma, M. K. Ramanathan, and B. Ray, "Code-Aware Prompting: A Study of Coverage-Guided Test Generation in Regression Setting using LLM," *Proceedings of the ACM on Software Engineering*, vol. 1, FSE, pp. 951-971, 2024, doi: [10.1145/3643769](https://doi.org/10.1145/3643769).
- **Why I selected it:** I promoted SymPrompt from my background pool because it directly investigates how execution-path-aware prompting can guide an LLM toward code that is difficult to cover.
- **Strengths and limitations I noted:** I found its multi-stage prompting strategy useful because it breaks test generation into smaller path-oriented tasks and provides relevant type and dependency context. Its main limitation for my work is that it is evaluated on Python rather than embedded C and does not have to deal with HAL calls, interrupts, or peripheral state.
- **Connection to my project:** I will use SymPrompt to justify structuring an AI prompt around a specific uncovered path or branch rather than asking generally for more tests. In my setting, I can extend that context with embedded-specific information such as mocks, stubs, hardware assumptions, and branch conditions.

#### HITS - Wang et al. (2024)
- **Citation:** Z. Wang, K. Liu, G. Li, and Z. Jin, "HITS: High-coverage LLM-based Unit Test Generation via Method Slicing," in *Proceedings of the 39th IEEE/ACM International Conference on Automated Software Engineering*, pp. 1258-1268, 2024, doi: [10.1145/3691620.3695501](https://doi.org/10.1145/3691620.3695501).
- **Why I selected it:** I selected HITS because it targets complex methods where whole-method prompting leaves conditions and branches uncovered, which is closely related to the type of coverage gap I want to diagnose.
- **Strengths and limitations I noted:** I found method slicing to be a strong way of reducing the amount of logic the model must reason about at once, and the paper evaluates both line and branch coverage. However, the work is Java-focused and still assumes a conventional unit-testing environment rather than hardware-dependent embedded C.
- **Connection to my project:** I will use HITS as support for decomposing a difficult embedded function or uncovered branch into a smaller analysis target. This may be especially useful where one function mixes normal logic with peripheral calls, error paths, or hardware-dependent conditions.

#### Static Program Analysis Guided LLM Based Unit Test Generation - Roy Chowdhury et al. (2024)
- **Citation:** S. Roy Chowdhury, G. Sridhara, A. K. Raghavan, J. Bose, S. Mazumdar, H. Singh, S. B. Sugumaran, and R. Britto, "Static Program Analysis Guided LLM Based Unit Test Generation," in *Proceedings of the 8th International Conference on Data Science and Management of Data (12th ACM IKDD CODS and 30th COMAD)*, pp. 279-283, 2024, doi: [10.1145/3703323.3703742](https://doi.org/10.1145/3703323.3703742).
- **Why I selected it:** I selected this paper because it directly studies whether concise context extracted through static program analysis can improve LLM-generated unit tests without simply placing an entire class into the prompt.
- **Strengths and limitations I noted:** I found the emphasis on concise and precise program-analysis context particularly relevant to my proposed workflow, and the approach was evaluated on both a commercial and an open-source Java project. The limitation is again the language and execution domain: it does not evaluate embedded C or hardware-abstraction dependencies.
- **Connection to my project:** I will use this paper to support the design of a compact coverage-gap input containing only the code, dependencies, control-flow information, and environment details needed to explain a particular uncovered region. This could help avoid giving the AI an unnecessarily large embedded codebase.

#### CodaMOSA - Lemieux et al. (2023)
- **Citation:** C. Lemieux, J. P. Inala, S. K. Lahiri, and S. Sen, "CodaMosa: Escaping Coverage Plateaus in Test Generation with Pre-trained Large Language Models," in *Proceedings of the 45th IEEE/ACM International Conference on Software Engineering*, pp. 919-931, 2023, doi: [10.1109/ICSE48619.2023.00085](https://doi.org/10.1109/ICSE48619.2023.00085).
- **Why I selected it:** I selected CodaMOSA because it examines a problem very close to my motivation: automated testing can reach a coverage plateau where conventional search struggles to discover inputs that exercise new behaviour, and an LLM can be introduced to help escape that plateau.
- **Strengths and limitations I noted:** I found the hybrid design valuable because it does not treat the LLM as a replacement for established automated testing; instead, the LLM is used when the search process stops making progress. However, its evaluation is based on Python test generation and search-based testing rather than embedded C coverage analysis.
- **Connection to my project:** I will use CodaMOSA to support the idea that AI may be most useful at the point where an existing coverage workflow has already identified what remains uncovered. My study similarly places AI after conventional coverage analysis rather than asking it to replace the coverage tool itself.

#### ChatUniTest - Chen et al. (2024)
- **Citation:** Y. Chen, Z. Hu, C. Zhi, J. Han, S. Deng, and J. Yin, "ChatUniTest: A Framework for LLM-Based Test Generation," in *Companion Proceedings of the 32nd ACM International Conference on the Foundations of Software Engineering*, pp. 572-576, 2024, doi: [10.1145/3663529.3663801](https://doi.org/10.1145/3663529.3663801).
- **Why I selected it:** I selected ChatUniTest because it combines context selection with a generation-validation-repair loop, which is important for a project where AI-generated test suggestions cannot simply be accepted as correct.
- **Strengths and limitations I noted:** I found its adaptive focal-context and repair mechanisms useful because they explicitly address incorrect generated tests rather than only measuring generation success. Its main limitation is that it targets Java projects and does not consider the additional validation problems created by embedded hardware and mocked peripherals.
- **Connection to my project:** I will use ChatUniTest to support an iterative workflow in which an AI suggestion is compiled, executed, checked against coverage, and either rejected or refined. In my project I will keep the human validation step explicit, particularly for mocks, stubs, and hardware-dependent assumptions.

### Synthesis After Fifteen Core Papers (Week 6)

I now define the gap precisely: there is promising work on LLM-assisted test generation and mature work on embedded C testing, but little direct evidence on AI-assisted coverage-gap explanation and human-validated test/mock/stub recommendation for embedded C software. The AI papers give me workflow ideas; the embedded papers define the constraints that make my study necessary.

### Week 6 Reflection on Literature Review Scope - 10/08/2026

Because I started the notebook and the focused literature review later than I should have, I realistically do not expect to get through every paper in my full 30-paper pool in the same level of depth. At this point, however, I am happy enough with the fifteen papers I have reviewed in a focused way. They give me a good spread across LLM-based test generation, coverage-guided feedback, static and program analysis, validation and repair, interpretation of coverage quality, and the practical constraints of embedded C testing.

I think this is enough of a foundation to write a good-standard two-page introduction to a paper on my topic. I will keep the remaining screened papers available where I need to strengthen a specific claim or fill a gap while drafting, but my priority now is to understand and synthesise the most relevant work properly rather than trying to claim that I have fully reviewed all 30 sources.

### Screened Background Sources Retained for Later Use

I also screened 15 additional sources and retained them as background or supporting material rather than treating them as core papers at this stage.

- **TestART - Gu et al. (2024):** I retained this only as background because it appears to be a preprint rather than a core peer-reviewed source.
- **MuTAP - Dakhel et al. (2024):** I retained this for mutation-aware evaluation support.
- **Schäfer et al. (2024):** I retained this for broad empirical evidence on LLM-generated unit tests.
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

---
## 3. Generative AI & Digital Tool Transparency Log

I will record any use of AI or other digital research tools that affects my literature search, source evaluation, wording, or analysis. I will check source details and claims against the original publication record and revise this notebook as I complete the full-text review.

### 06/08/2026 - ChatGPT for initial literature discovery

- **Tool used:** ChatGPT.
- **Task performed:** I used ChatGPT to help identify candidate literature relevant to my proposed research study.
- **Exact prompt/input used:** `For my masters thesis, I want to explore the idea of using an LLM to assist in the code coverage process. Particularly in providing a feedback loop at the end of a static coverage analysis that includes suggested fixes/implementations for uncovered code. As a starting point I need you to find me some solid, reliable literature on Google Scholar that I can go through myself and verify whether or not they are relevant. Ensure any and all sources and links are included for me.`

- **How I critically evaluated or modified the output:** Because I had started the notebook later than I should have, I used ChatGPT (5.5) to create an initial candidate source list and a first-pass notebook structure quickly rather than pretending I had built it slowly over several weeks. I then checked titles, authors, publication venues, years, page ranges, and DOI or arXiv records against publisher, conference, institutional, or bibliographic sources. I was surprised by how relevant the initial list was after review, but I still removed or downgraded material that did not directly support my study. I am now going through the sources one by one, checking their relevance in more detail, and revising the notebook as I complete that review.
---
## 4. Weekly Activity & Reflection Logs

### Week 05: Focus Area - Initial Literature Review and Scope Correction

- **Activities Completed:** I defined the working title, research aim, research questions, objectives, and methodology; assembled an initial literature set; used AI to help create an initial notebook structure; verified publication details; selected ten core starting papers; and updated the pivot log.
- **Key Insights / Discoveries:** I found that the closest AI-assisted approaches use iterative feedback rather than a single prompt. I also found that coverage values must be interpreted alongside test correctness and fault-detection measures. The key gap is that LLM test-generation papers rarely address embedded C hardware-abstraction constraints, while embedded C testing papers do not address LLM-assisted explanation and recommendation of tests, mocks, or stubs.
- **Obstacles Encountered & How I Overcame Them:** I recognised that my notebook had not been updated incrementally enough before this point. This late start led me to use AI to gather an initial candidate source list and impose a first-pass structure on the notebook, but I logged that use explicitly and reviewed the sources rather than treating the AI output as authoritative.
- **Plan for Next Week:** I will extend the literature review using the strongest papers from my screened source pool, refine the research-gap argument, and begin moving toward an initial structure for the IEEE paper.

---
### Week 06: Focus Area - Literature Review Extension and Transition to Paper Drafting

- **Activities Completed:** I promoted five previously screened papers into the focused review: SymPrompt, HITS, Static Program Analysis Guided LLM Based Unit Test Generation, CodaMOSA, and ChatUniTest. This brought my focused literature review to fifteen core papers while leaving fifteen additional sources in the screened background pool. I also revisited the overall synthesis of the literature and considered whether the reviewed evidence is sufficient to begin writing the conference-paper introduction.
- **Key Insights / Discoveries:** The additional papers strengthened the idea that an AI-assisted coverage workflow should not rely on a single broad prompt. Path-aware prompting, slicing, static-analysis context, coverage-plateau detection, and generation-validation-repair loops all suggest that the AI should be given targeted information about a specific uncovered region. This supports my idea of creating a focused coverage-gap input containing source code, branch or path information, coverage status, dependencies, and embedded-specific HAL/mock/stub context.
- **Reflection on Progress:** Because I started the notebook and focused literature review later than I should have, I realistically do not expect to review every paper in my full 30-paper pool to the same depth. I am happy enough with the fifteen papers I have reviewed in a focused way so far. They cover the main areas I need for the introduction: LLM-based test generation, coverage-guided feedback, static and program analysis, test validation and repair, interpretation of coverage metrics, and embedded C testing constraints. I think this gives me enough of a foundation to write a good-standard two-page introduction to a paper on my topic.
- **Use of AI / Tools This Week:** I used ChatGPT to confirm to me that I was still meeting all the requirements from the assignments well, just to make sure I hadn't veered off track.
- **Plan for This Week:** I will begin the ".tex" file using the IEEE conference template and create an initial introduction structure rather than trying to write the full two-page paper in one pass. I will then map the strongest claims from the fifteen reviewed papers onto the context, mini literature review, research gap, proposed contribution, and paper-roadmap parts required by the assignment.

---
