# MSc Research Notebook

- **Student Name:** Dara Heaphy
- **Student ID:** 23369914
- **Proposed Research Title:** Closing the Coverage Gap with AI: AI-Assisted Code Coverage Management for Embedded C Software
- **GitHub Repository Link:** https://github.com/DaraHeaphy/MSc-Research-Notebook-Dara-Heaphy

------------------------------------------------------------------------

## 1. Project Overview & Evolution Track

I am investigating whether AI-assisted analysis can make code coverage reports more useful and actionable during the testing of embedded C software. My focus is not simply on increasing a coverage percentage. I want to examine whether an AI model can use source code, existing unit tests, coverage reports, and hardware-abstraction information to explain why particular functions, branches, or lines remain uncovered and then propose practical tests, mocks, or stubs that address those gaps.

I will treat the coverage report as feedback from executed tests rather than as a purely static result. I will use source-level and program-analysis information to give the AI enough context to reason about control flow, dependencies, input conditions, and hardware interactions. I will review and execute any generated test code before I include it in the evaluation.

### Current Working Research Question(s)

> **Last Updated: 06/08/2026**  
> **Primary research question:** To what extent can I use AI-assisted coverage analysis to improve the identification, explanation, and reduction of code coverage gaps in embedded C software?

I will also use the following supporting questions:

1. How accurately can I use AI to explain why code regions remain uncovered?
2. How useful and technically correct are the tests, mocks, or stubs proposed for uncovered logic?
3. What changes do I observe in line, branch, and function coverage after I implement selected AI-assisted recommendations?
4. What limitations do I observe when the uncovered code depends on hardware, peripherals, timing, interrupts, or other embedded-system behaviour?

### Current Research Aim and Objectives

My aim is to evaluate whether AI-assisted analysis can improve the process of identifying, explaining, and reducing code coverage gaps in embedded C software.

To address this aim, I plan to:

- collect baseline line, branch, and function coverage from selected embedded C modules;
- identify uncovered functions, branches, and lines in the baseline reports;
- give an AI model relevant source code, existing tests, coverage information, and hardware-abstraction details;
- assess the AI's explanation of each selected coverage gap;
- implement and validate selected AI-proposed tests, mocks, or stubs;
- compare coverage before and after the AI-assisted intervention; and
- record the usefulness, correctness, cost, and limitations of the AI output.

### Current Methodological Direction

I plan to use a mixed-methods experimental study. My quantitative analysis will compare baseline coverage with coverage after selected AI-assisted tests are added. My qualitative analysis will examine whether the AI explanations are correct, whether its recommendations are practical, and why individual recommendations succeed or fail. I intend to use local or open-source embedded C modules and to rely on host-based tests, simulation, or mocked hardware where full board deployment is unnecessary. I will validate generated code by reviewing, compiling, and executing it before I include it in the results.

### Expected Contribution

I expect my study to provide evidence about where AI can and cannot help in an embedded coverage workflow. I also aim to produce a repeatable process for combining coverage reports, source context, hardware-abstraction information, AI recommendations, and test validation. My intended contribution is practical guidance rather than a claim that AI should replace engineers or existing coverage tools.

### The Pivot Log

I have not made a major change in direction at this stage. I am retaining the embedded C coverage-management proposal while I use the literature review to refine the final scope, evaluation measures, and experimental design.

------------------------------------------------------------------------

## 2. Critical Source Evaluation Log

I selected the following literature because it helps me understand the main parts of my proposed study. I need research on coverage-guided AI test generation, program analysis, test validation and repair, mutation testing, conventional automated testing, and the specific constraints of embedded C software. For each source, I have recorded why I selected it, what I took from its method or findings, the limitations I noticed, and how I expect it to affect my own work.

### Literature I selected on coverage-guided AI and hybrid program analysis

#### CoverUp — Pizzorno and Berger (2025)

- **Citation I am using:** J. A. Pizzorno and E. D. Berger, “CoverUp: Effective High Coverage Test Generation for Python,” *Proceedings of the ACM on Software Engineering*, vol. 2, FSE, pp. 2897–2919, 2025, doi: [10.1145/3729398](https://doi.org/10.1145/3729398).
- **Why I selected it:** I selected CoverUp because its workflow closely matches the feedback loop I am considering. It repeatedly runs tests, identifies uncovered lines and branches, and gives targeted coverage information and code context to an LLM.
- **What I learned from it:** I found that coverage feedback can be used as a concrete prompt target rather than asking a model to improve an entire test suite in one attempt. The evaluation also shows the value of separating the contribution of the model from the contribution of the surrounding workflow.
- **Limitations I identified:** The implementation and benchmarks are Python-based, so I cannot assume that the same results will transfer to embedded C. The reported gains also focus heavily on structural coverage.
- **How I will use it in my project:** I will use CoverUp as a direct design reference for my iterative loop. I will adapt the idea by adding embedded C dependencies, hardware abstractions, and explicit validation of any proposed mocks or stubs.

#### Panta — Gu, Nashid, and Mesbah (2026)

- **Citation I am using:** S. Gu, N. Nashid, and A. Mesbah, “LLM Test Generation via Iterative Hybrid Program Analysis,” in *Proceedings of the 48th IEEE/ACM International Conference on Software Engineering*, 2026, doi: [10.1145/3744916.3764553](https://doi.org/10.1145/3744916.3764553).
- **Why I selected it:** I selected Panta because it combines static control-flow analysis, dynamic coverage information, and iterative LLM prompting in one process.
- **What I learned from it:** I found that the model is given information about uncovered execution paths rather than only the source method or a list of uncovered lines. This is especially relevant when a branch requires several dependent conditions to be satisfied.
- **Limitations I identified:** The evaluation focuses on complex Java methods. I will need to account for C-specific features such as pointers, macros, global state, interrupts, and hardware-register access.
- **How I will use it in my project:** I will use Panta to guide how I represent an uncovered branch to the model. It supports my decision to combine executed coverage data with source-level control-flow information.

#### Advancing Code Coverage — Yang et al. (2026)

- **Citation I am using:** C. Yang, J. Chen, B. Lin, Z. Wang, and J. Zhou, “Advancing Code Coverage: Incorporating Program Analysis with Large Language Models,” *ACM Transactions on Software Engineering and Methodology*, vol. 35, no. 5, Art. no. 118, pp. 1–31, 2026, doi: [10.1145/3748505](https://doi.org/10.1145/3748505).
- **Why I selected it:** I selected this paper because it studies hard-to-cover branches that require complex setup and inter-procedural reasoning.
- **What I learned from it:** I found that program analysis can extract focused dependency information for a difficult branch and use it to reduce the amount of irrelevant context passed to the model.
- **Limitations I identified:** The usefulness of the extracted context may vary between projects, and repeated analysis and model calls can add cost. Its object-construction problems are also different from embedded hardware setup.
- **How I will use it in my project:** I will use its approach when deciding what context to provide for a coverage gap, especially call dependencies, required state, and branch conditions. I will translate object-construction concerns into embedded setup concerns such as peripheral state and mock return values.

#### TestCTRL — Zhang et al. (2026)

- **Citation I am using:** J. Zhang, X. Hu, X. Xia, S.-C. Cheung, and S. Li, “Automated Unit Test Generation via Chain-of-Thought Prompt and Reinforcement Learning from Coverage Feedback,” *ACM Transactions on Software Engineering and Methodology*, vol. 35, no. 4, Art. no. 92, pp. 1–30, 2026, doi: [10.1145/3745765](https://doi.org/10.1145/3745765).
- **Why I selected it:** I selected TestCTRL because it treats coverage results as a feedback signal and examines whether structured reasoning improves the diversity of generated tests.
- **What I learned from it:** I found that coverage-guided generation can be supported either by prompt-time context or by model adaptation. This gives me a useful distinction when describing my own approach.
- **Limitations I identified:** Reinforcement learning introduces training, data, and replication costs that are not necessary for my proposed project. Its gains may also depend on the reward design.
- **How I will use it in my project:** I will use TestCTRL as an example of a more complex alternative while keeping my own design focused on inference-time analysis and feedback. This will help me justify why I am not training a new model.

#### SymPrompt — Ryan et al. (2024)

- **Citation I am using:** G. Ryan, S. Jain, M. Shang, S. Wang, X. Ma, M. K. Ramanathan, and B. Ray, “Code-Aware Prompting: A Study of Coverage-Guided Test Generation in Regression Setting Using LLM,” *Proceedings of the ACM on Software Engineering*, vol. 1, FSE, pp. 951–971, 2024, doi: [10.1145/3643769](https://doi.org/10.1145/3643769).
- **Why I selected it:** I selected SymPrompt because it organises test generation around execution paths and supplies type and dependency context to the model.
- **What I learned from it:** I found that breaking generation into path-focused prompts can give the model a clearer target than asking for broad test-suite coverage. Its use of parsing also shows how relevant context can be selected automatically.
- **Limitations I identified:** The evaluation is based on Python regression testing. Its assumptions about types and dependencies do not fully capture C preprocessor logic, pointers, or hardware-facing code.
- **How I will use it in my project:** I will consider comparing line-level coverage prompts with branch- or path-level prompts. I will also examine whether a concise path description produces more useful embedded C tests than a raw coverage report alone.

#### HITS — Wang et al. (2024)

- **Citation I am using:** Z. Wang, K. Liu, G. Li, and Z. Jin, “HITS: High-Coverage LLM-Based Unit Test Generation via Method Slicing,” in *Proceedings of the 39th IEEE/ACM International Conference on Automated Software Engineering*, pp. 1258–1268, 2024, doi: [10.1145/3691620.3695501](https://doi.org/10.1145/3691620.3695501).
- **Why I selected it:** I selected HITS because it uses program slicing to reduce the amount of code the model must reason about for a complex test target.
- **What I learned from it:** I found that decomposing a focal method into smaller slices can improve the model's ability to generate inputs for particular conditions and loops.
- **Limitations I identified:** A slice can omit wider state or dependency information that is needed to build a realistic test. This risk may be greater in embedded C because a small function can depend on global state or external hardware interfaces.
- **How I will use it in my project:** I will use slicing as one candidate method for creating a focused explanation of an uncovered region. I will also check whether the slice needs to be supplemented with global variables, called functions, and mockable interfaces.

#### Static Program Analysis Guided LLM-Based Unit Test Generation — Roychowdhury et al. (2024)

- **Citation I am using:** S. Roychowdhury, G. Sridhara, A. K. Raghavan, J. Bose, S. Mazumdar, H. Singh, S. B. Sugumaran, and R. Britto, “Static Program Analysis Guided LLM Based Unit Test Generation,” in *Proceedings of the 8th Joint International Conference on Data Science and Management of Data*, pp. 279–283, 2024, doi: [10.1145/3703323.3703742](https://doi.org/10.1145/3703323.3703742).
- **Why I selected it:** I selected this paper because it directly evaluates the use of static program analysis to build concise and precise prompt context.
- **What I learned from it:** I found that automatically selected focal context can be more useful than passing either a small method in isolation or an entire class. The workflow also uses failures and errors to support later attempts.
- **Limitations I identified:** The paper is short, and part of the evaluation uses commercial Java software that may be difficult to reproduce. It does not address embedded hardware dependencies.
- **How I will use it in my project:** I will use it to decide which source facts to include in my prompt, such as callers, callees, variable dependencies, and conditions. I will extend that context with hardware-abstraction and mock information.

#### CodaMOSA — Lemieux et al. (2023)

- **Citation I am using:** C. Lemieux, J. P. Inala, S. K. Lahiri, and S. Sen, “CodaMOSA: Escaping Coverage Plateaus in Test Generation with Pre-Trained Large Language Models,” in *Proceedings of the 45th IEEE/ACM International Conference on Software Engineering*, pp. 919–931, 2023, doi: [10.1109/ICSE48619.2023.00085](https://doi.org/10.1109/ICSE48619.2023.00085).
- **Why I selected it:** I selected CodaMOSA because it uses an LLM when search-based test generation stops making coverage progress.
- **What I learned from it:** I found that AI does not need to replace an existing test-generation technique. It can be introduced selectively when the conventional process reaches a plateau.
- **Limitations I identified:** The system is Python-specific and gives the LLM less detailed static guidance than some later approaches. Its search process also differs from a developer-led embedded unit-test workflow.
- **How I will use it in my project:** I will use it when deciding when the AI should be invoked. One possible design is to request AI help only after ordinary test additions or automated techniques fail to cover a target.

### Literature I selected on test generation, validation, repair, and fault detection

#### Automated Unit Test Improvement at Meta — Alshahwan et al. (2024)

- **Citation I am using:** N. Alshahwan *et al.*, “Automated Unit Test Improvement Using Large Language Models at Meta,” in *Companion Proceedings of the 32nd ACM International Conference on the Foundations of Software Engineering*, pp. 185–196, 2024, doi: [10.1145/3663529.3663839](https://doi.org/10.1145/3663529.3663839).
- **Why I selected it:** I selected this paper because TestGen-LLM improves existing test suites and filters suggestions before developers see them.
- **What I learned from it:** I found that compilation, execution, stability, and measurable coverage improvement can be used as practical acceptance checks. The industrial evaluation also gives evidence about developer acceptance.
- **Limitations I identified:** The internal Meta environment limits reproducibility. A passing test that increases coverage can still contain a weak or incorrect assertion.
- **How I will use it in my project:** I will use a similar validation sequence for AI-proposed embedded tests. I will not count a suggestion as successful unless it compiles, runs reliably, and reaches the intended code without invalid assumptions.

#### ChatUniTest — Chen et al. (2024)

- **Citation I am using:** Y. Chen, Z. Hu, C. Zhi, J. Han, S. Deng, and J. Yin, “ChatUniTest: A Framework for LLM-Based Test Generation,” in *Companion Proceedings of the 32nd ACM International Conference on the Foundations of Software Engineering*, pp. 572–576, 2024, doi: [10.1145/3663529.3663801](https://doi.org/10.1145/3663529.3663801).
- **Why I selected it:** I selected ChatUniTest because it combines adaptive context selection with generation, validation, and repair.
- **What I learned from it:** I found that generated tests are more useful when the system can gather project context, execute the output, and use errors to repair it rather than stopping after one prompt.
- **Limitations I identified:** The publication is a demonstration paper and provides less detail than a full empirical article. Repairing syntax or compilation does not guarantee that a test checks the right behaviour.
- **How I will use it in my project:** I will use its workflow as a reference for the implementation stages around the model. I will keep test-oracle review as a separate human validation step.

#### TestART — Gu et al. (2024)

- **Citation I am using:** S. Gu, Q. Zhang, C. Fang, F. Tian, L. Zhu, J. Zhou, and Z. Chen, “TestART: Improving LLM-Based Unit Testing via Co-Evolution of Automated Generation and Repair Iteration,” arXiv:2408.03095, 2024. Available: [https://arxiv.org/abs/2408.03095](https://arxiv.org/abs/2408.03095).
- **Why I selected it:** I selected TestART because it treats generation, automated repair, execution feedback, and coverage improvement as one iterative process.
- **What I learned from it:** I found that repair templates can handle common failures while coverage information from successful tests can guide later generations.
- **Limitations I identified:** I am treating it as a preprint. Template repair may correct common code errors without correcting a misunderstanding of the required behaviour.
- **How I will use it in my project:** I will use it to design my retry process. I will distinguish between mechanical repairs, such as includes or function signatures, and semantic changes that require manual review.

#### MuTAP — Dakhel et al. (2024)

- **Citation I am using:** A. M. Dakhel, A. Nikanjam, V. Majdinasab, F. Khomh, and M. C. Desmarais, “Effective Test Generation Using Pre-Trained Large Language Models and Mutation Testing,” *Information and Software Technology*, vol. 171, Art. no. 107468, 2024, doi: [10.1016/j.infsof.2024.107468](https://doi.org/10.1016/j.infsof.2024.107468).
- **Why I selected it:** I selected MuTAP because it uses surviving mutants as feedback for further LLM-generated tests.
- **What I learned from it:** I found that mutation feedback can expose weaknesses that line or branch coverage alone may not reveal. The model is given a more fault-oriented target by showing which artificial faults remain undetected.
- **Limitations I identified:** Mutation analysis can be expensive, and equivalent mutants can distort the score. Some of the evaluated programs are smaller than realistic embedded modules.
- **How I will use it in my project:** I will consider mutation score as a secondary quality measure where a suitable C mutation tool is practical. At minimum, I will use this paper to avoid treating increased coverage as proof of improved fault detection.

#### MUTGEN — Wang et al. (2026)

- **Citation I am using:** G. Wang, Q. Xu, L. C. Briand, and K. Liu, “Mutation-Guided Unit Test Generation With a Large Language Model,” *IEEE Transactions on Software Engineering*, vol. 52, no. 5, pp. 1657–1671, 2026, doi: [10.1109/TSE.2026.3682975](https://doi.org/10.1109/TSE.2026.3682975).
- **Why I selected it:** I selected MUTGEN because it directly incorporates mutation feedback into an iterative LLM test-generation process.
- **What I learned from it:** I found that some test suites can achieve high structural coverage while retaining a low mutation score. The paper therefore gives a strong argument for measuring more than execution reach.
- **Limitations I identified:** Its effectiveness depends on the mutation operators, benchmark selection, and available execution budget. Mutation-guided prompts may also target artificial faults that do not resemble embedded defects.
- **How I will use it in my project:** I will use MUTGEN as a recent fault-oriented comparison. It will help me interpret any case where my AI-assisted tests increase coverage but do not improve stronger measures of test quality.

#### An Empirical Evaluation of LLM-Based Unit Test Generation — Schäfer et al. (2024)

- **Citation I am using:** M. Schäfer, S. Nadi, A. Eghbali, and F. Tip, “An Empirical Evaluation of Using Large Language Models for Automated Unit Test Generation,” *IEEE Transactions on Software Engineering*, vol. 50, no. 1, pp. 85–105, 2024, doi: [10.1109/TSE.2023.3334955](https://doi.org/10.1109/TSE.2023.3334955).
- **Why I selected it:** I selected this study because it gives broad empirical evidence about the correctness and coverage of tests generated by different language models.
- **What I learned from it:** I found that generation quality depends on the model and the context included in the prompt. The TestPilot workflow also uses error messages to repair failed tests.
- **Limitations I identified:** The study focuses on JavaScript packages and the evaluated models can quickly become dated. JavaScript API testing is less constrained than embedded C testing.
- **How I will use it in my project:** I will use its measures when recording compilation, execution, coverage, assertions, and repair outcomes. I will also record the exact model and prompt configuration used in each experiment.

#### Evaluating and Improving ChatGPT for Unit Test Generation — Yuan et al. (2024)

- **Citation I am using:** Z. Yuan, M. Liu, S. Ding, K. Wang, Y. Chen, X. Peng, and Y. Lou, “Evaluating and Improving ChatGPT for Unit Test Generation,” *Proceedings of the ACM on Software Engineering*, vol. 1, FSE, pp. 1703–1726, 2024, doi: [10.1145/3660783](https://doi.org/10.1145/3660783).
- **Why I selected it:** I selected this paper because it evaluates correctness, sufficiency, readability, and usability before introducing an iterative refinement method.
- **What I learned from it:** I found that compilation errors, execution failures, and incorrect assertions remain important even when generated tests appear readable. Feedback can improve these outcomes but does not remove the need for validation.
- **Limitations I identified:** The results depend on specific Java benchmarks and model versions. A model can repeat the same incorrect assumption during self-repair.
- **How I will use it in my project:** I will separate coverage improvement from correctness and usability in my results. I will also record repeated failure patterns rather than counting every retry as independent progress.

#### On the Evaluation of Large Language Models in Unit Test Generation — Yang et al. (2024)

- **Citation I am using:** L. Yang *et al.*, “On the Evaluation of Large Language Models in Unit Test Generation,” in *Proceedings of the 39th IEEE/ACM International Conference on Automated Software Engineering*, pp. 1607–1619, 2024, doi: [10.1145/3691620.3695529](https://doi.org/10.1145/3691620.3695529).
- **Why I selected it:** I selected this paper because it compares several open-source models, prompt settings, GPT-4, and EvoSuite across multiple Java projects.
- **What I learned from it:** I found that prompt structure, model size, and model architecture can materially change test-generation results. A fair evaluation therefore needs consistent budgets and clearly reported settings.
- **Limitations I identified:** Model comparisons become outdated quickly, and Java results do not automatically transfer to embedded C.
- **How I will use it in my project:** I will use it to define a fair model-comparison procedure if I evaluate more than one model. I will keep token limits, retries, context, and acceptance rules consistent.

### Literature I selected on baselines and evaluation measures

#### EvoSuite — Fraser and Arcuri (2011)

- **Citation I am using:** G. Fraser and A. Arcuri, “EvoSuite: Automatic Test Suite Generation for Object-Oriented Software,” in *Proceedings of the 19th ACM SIGSOFT Symposium and the 13th European Conference on Foundations of Software Engineering*, pp. 416–419, 2011, doi: [10.1145/2025113.2025179](https://doi.org/10.1145/2025113.2025179).
- **Why I selected it:** I selected EvoSuite because it is a foundational search-based test generator and is frequently used as a baseline in later LLM studies.
- **What I learned from it:** I found that whole test suites can be optimised towards coverage objectives and supplied with automatically generated assertions.
- **Limitations I identified:** Generated tests can be difficult to read, and assertions based on observed behaviour can preserve an existing defect. The tool is Java-specific.
- **How I will use it in my project:** I will use EvoSuite conceptually as a conventional automated-testing baseline. For embedded C, I will select an appropriate C-based baseline or compare against manually written baseline tests rather than claiming a direct tool-for-tool comparison.

#### DynaMOSA — Panichella, Kifetew, and Tonella (2018)

- **Citation I am using:** A. Panichella, F. M. Kifetew, and P. Tonella, “Automated Test Case Generation as a Many-Objective Optimisation Problem with Dynamic Selection of the Targets,” *IEEE Transactions on Software Engineering*, vol. 44, no. 2, pp. 122–158, 2018, doi: [10.1109/TSE.2017.2663435](https://doi.org/10.1109/TSE.2017.2663435).
- **Why I selected it:** I selected DynaMOSA because it dynamically chooses which uncovered targets should be active during search-based test generation.
- **What I learned from it:** I found that control dependencies can be used to prioritise reachable targets instead of treating every uncovered branch as equally actionable at all times.
- **Limitations I identified:** Search-based optimisation is different from LLM generation, and results depend on search parameters and time budgets.
- **How I will use it in my project:** I will use its target-selection ideas when deciding which gaps to present to the AI first. I may prioritise reachable branches whose prerequisite control conditions are already understood.

#### Coverage Is Not Strongly Correlated with Test Suite Effectiveness — Inozemtseva and Holmes (2014)

- **Citation I am using:** L. Inozemtseva and R. Holmes, “Coverage Is Not Strongly Correlated with Test Suite Effectiveness,” in *Proceedings of the 36th International Conference on Software Engineering*, pp. 435–445, 2014, doi: [10.1145/2568225.2568271](https://doi.org/10.1145/2568225.2568271).
- **Why I selected it:** I selected this paper because my project could otherwise overstate the meaning of a coverage increase.
- **What I learned from it:** I found that the relationship between coverage and fault-detection effectiveness becomes weaker when test-suite size is controlled. High coverage therefore does not prove that a suite is effective.
- **Limitations I identified:** The study uses selected Java projects and mutation-based measures. Its conclusions still need to be interpreted in the context of LLM-generated and embedded tests.
- **How I will use it in my project:** I will present coverage as evidence that previously untested code has been exercised, not as a complete measure of test quality. I will also inspect assertions and use mutation or fault-oriented checks where feasible.

#### Mutation Testing Survey — Jia and Harman (2011)

- **Citation I am using:** Y. Jia and M. Harman, “An Analysis and Survey of the Development of Mutation Testing,” *IEEE Transactions on Software Engineering*, vol. 37, no. 5, pp. 649–678, 2011, doi: [10.1109/TSE.2010.62](https://doi.org/10.1109/TSE.2010.62).
- **Why I selected it:** I selected this survey to establish the foundations of mutation testing before using mutation score as an evaluation measure.
- **What I learned from it:** I found that mutation testing offers a fault-based assessment but introduces challenges such as equivalent mutants, operator selection, and computational cost.
- **Limitations I identified:** The survey predates modern LLM-based testing and newer mutation systems. I will need more recent tool-specific information before choosing a C mutation framework.
- **How I will use it in my project:** I will use it to explain why mutation score can complement structural coverage and to identify threats to validity if I include mutation analysis.

#### Software Testing with Large Language Models — Wang et al. (2024)

- **Citation I am using:** J. Wang, Y. Huang, C. Chen, Z. Liu, S. Wang, and Q. Wang, “Software Testing With Large Language Models: Survey, Landscape, and Vision,” *IEEE Transactions on Software Engineering*, vol. 50, no. 4, pp. 911–936, 2024, doi: [10.1109/TSE.2024.3368208](https://doi.org/10.1109/TSE.2024.3368208).
- **Why I selected it:** I selected this survey to position my topic within the wider use of LLMs across software-testing activities.
- **What I learned from it:** I found a useful classification of testing tasks, model choices, prompt techniques, supporting tools, challenges, and research opportunities.
- **Limitations I identified:** The field moves quickly, so the survey cannot include the newest coverage-guided systems. Its broad scope also means that embedded testing receives limited attention.
- **How I will use it in my project:** I will use its terminology and categories to structure my final introduction and to distinguish my work from general test generation or program repair.

#### Large Language Models for Unit Testing: A Systematic Literature Review — Zhang et al. (2025)

- **Citation I am using:** Q. Zhang, C. Fang, S. Gu, Y. Shang, Z. Chen, and L. Xiao, “Large Language Models for Unit Testing: A Systematic Literature Review,” arXiv:2506.15227, 2025. Available: [https://arxiv.org/abs/2506.15227](https://arxiv.org/abs/2506.15227).
- **Why I selected it:** I selected this review because it focuses specifically on unit-testing tasks, including generation, oracle creation, repair, and hybrid approaches.
- **What I learned from it:** I found a structured map of methods, models, datasets, evaluation measures, and unresolved challenges that I can use for backward and forward citation searching.
- **Limitations I identified:** I am treating it as a preprint, and its search period ends before several later papers in my source set.
- **How I will use it in my project:** I will use its classifications to check that my literature review covers generation, validation, evaluation, and supporting analysis rather than concentrating on coverage-guided prompting alone.

#### Newer Models and Existing Test-Generation Techniques — Konstantinou, Degiovanni, and Papadakis (2026)

- **Citation I am using:** M. Konstantinou, R. Degiovanni, and M. Papadakis, “How Well LLM-Based Test Generation Techniques Perform with Newer LLM Versions?” arXiv:2601.09695, 2026. Available: [https://arxiv.org/abs/2601.09695](https://arxiv.org/abs/2601.09695).
- **Why I selected it:** I selected this paper because it questions whether complex test-generation pipelines still outperform a strong direct prompt when newer models are used.
- **What I learned from it:** I found that newer plain-LLM baselines can match or outperform several engineered techniques, depending on the metric and generation granularity.
- **Limitations I identified:** The results remain sensitive to the selected models, prompts, datasets, and replication choices. The study does not focus on embedded C.
- **How I will use it in my project:** I will include a simple direct-prompt baseline so that I do not attribute improvements to my feedback workflow when the underlying model could achieve them without the additional analysis.

#### Coverage, Mutation, and Real-Bug Detection in LLM-Generated Tests — Zhao, Zhou, and Cohen (2026)

- **Citation I am using:** J. Zhao, S. Zhou, and E. Cohen, “Do Coverage and Mutation Scores of LLM-Generated Test Suites Correlate with Their Effectiveness? A Replicability Study,” arXiv:2607.22880, 2026. Available: [https://arxiv.org/abs/2607.22880](https://arxiv.org/abs/2607.22880).
- **Why I selected it:** I selected this study because it examines whether coverage and mutation scores are meaningful indicators of real-bug detection specifically for LLM-generated tests.
- **What I learned from it:** I found that the usefulness of these proxy measures depends on the testing setting. They can be informative in regression-style testing but less reliable when the code under test already contains the target bug.
- **Limitations I identified:** I am treating it as a recent preprint, and its conclusions may depend on the benchmarks, models, and definition of the testing scenario.
- **How I will use it in my project:** I will clearly define my setting as coverage improvement for selected modules whose expected behaviour is known. I will avoid making claims about real-bug detection unless I evaluate it directly.

### Literature I selected on embedded C and hardware-aware testing

#### Testing Embedded Software: A Survey of the Literature — Garousi et al. (2018)

- **Citation I am using:** V. Garousi, M. Felderer, Ç. M. Karapıçak, and U. Yılmaz, “Testing Embedded Software: A Survey of the Literature,” *Information and Software Technology*, vol. 104, pp. 14–45, 2018, doi: [10.1016/j.infsof.2018.06.016](https://doi.org/10.1016/j.infsof.2018.06.016).
- **Why I selected it:** I selected this survey because it gives me a broad evidence-based view of embedded testing techniques, activities, tools, artefacts, and application domains.
- **What I learned from it:** I found that embedded testing covers a wide range of levels and constraints, including hardware interaction, timing, simulation, test automation, and industry-specific practices.
- **Limitations I identified:** The review predates the recent growth of LLM-assisted software testing. Its breadth means that it does not answer my specific AI-assisted coverage question.
- **How I will use it in my project:** I will use it to establish the embedded testing context in my introduction and to prevent the project from being framed as ordinary desktop unit-test generation with C syntax.

#### SmartUnit — Zhang et al. (2018)

- **Citation I am using:** C. Zhang, Y. Yan, H. Zhou, Y. Yao, K. Wu, T. Su, W. Miao, and G. Pu, “SmartUnit: Empirical Evaluations for Automated Unit Testing of Embedded Software in Industry,” in *Proceedings of the 40th International Conference on Software Engineering: Software Engineering in Practice*, pp. 296–305, 2018, doi: [10.1145/3183519.3183554](https://doi.org/10.1145/3183519.3183554).
- **Why I selected it:** I selected SmartUnit because it evaluates automated coverage-based unit testing on industrial embedded software.
- **What I learned from it:** I found that dynamic symbolic execution can target statement, branch, boundary-value, and MC/DC coverage, while also revealing reasons why some embedded functions remain difficult to cover.
- **Limitations I identified:** The industrial projects and tooling limit full reproduction, and symbolic execution can struggle with environment complexity, external calls, and path explosion.
- **How I will use it in my project:** I will use SmartUnit as an embedded-specific automated-testing reference. Its analysis of low-coverage causes can help me create categories for the explanations produced by the AI.

#### CTGEN — Mangels and Peleska (2012)

- **Citation I am using:** T. Mangels and J. Peleska, “CTGEN—A Unit Test Generator for C,” *Electronic Proceedings in Theoretical Computer Science*, vol. 102, pp. 88–102, 2012, doi: [10.4204/EPTCS.102.9](https://doi.org/10.4204/EPTCS.102.9).
- **Why I selected it:** I selected CTGEN because it generates tests and stubs for C code and was designed for embedded-system testing.
- **What I learned from it:** I found that automated C testing must handle pointer arithmetic, structures, unions, aliasing, specifications, and controlled stub return values. These are all relevant to my proposed prompt context.
- **Limitations I identified:** The approach depends on formal constraints, preconditions, postconditions, or assertions that may not be available in my selected modules.
- **How I will use it in my project:** I will use CTGEN to identify the information required for a technically valid embedded C test. It also gives me a conventional reference for automated stub generation before I assess AI-generated mocks or stubs.

#### Coverage-Based Testing on Embedded Systems — Wu et al. (2007)

- **Citation I am using:** X. Wu, J. J. Li, D. Weiss, and Y.-H. Lee, “Coverage-Based Testing on Embedded Systems,” in *Proceedings of the 2nd International Workshop on Automation of Software Test*, 2007, doi: [10.1109/AST.2007.8](https://doi.org/10.1109/AST.2007.8).
- **Why I selected it:** I selected this paper because it addresses the practical overhead of measuring coverage on resource-constrained and real-time embedded systems.
- **What I learned from it:** I found that instrumentation can alter timing, memory use, and execution behaviour, so coverage collection itself can affect the system being measured.
- **Limitations I identified:** The case study and tooling are older, and modern trace and instrumentation options may change the practical trade-offs.
- **How I will use it in my project:** I will use it to justify host-based or simulated testing where appropriate and to record whether my coverage method introduces overhead that could affect the validity of results.

#### Interface Testing for Hardware-Dependent Software — Sung, Choi, and Shin (2007)

- **Citation I am using:** A. Sung, B. Choi, and S. Shin, “An Interface Test Model for Hardware-Dependent Software and Embedded OS API of the Embedded System,” *Computer Standards & Interfaces*, vol. 29, no. 4, pp. 430–443, 2007, doi: [10.1016/j.csi.2006.07.002](https://doi.org/10.1016/j.csi.2006.07.002).
- **Why I selected it:** I selected this paper because my proposal specifically needs to distinguish ordinary missing tests from gaps caused by hardware or operating-system interfaces.
- **What I learned from it:** I found that embedded software is organised into tightly connected layers and that the interfaces between hardware-dependent software, the operating system, and application code require targeted test items.
- **Limitations I identified:** The model predates current embedded platforms and does not consider LLM-assisted analysis. Its interface categories may need to be adapted to the selected codebase.
- **How I will use it in my project:** I will use it to classify coverage gaps that involve hardware or OS boundaries. This will help me judge whether the AI correctly recommends a unit test, a mock, a stub, a simulator, or a higher-level test.

#### Trace-Based Structural Coverage — Weiss et al. (2024)

- **Citation I am using:** A. Weiss, A. Schulz, M. Heininger, M. Sachenbacher, and M. Leucker, “Achieving Complete Structural Test Coverage in Embedded Systems Using Trace-Based Monitoring,” in *35th International Conference on Principles of Diagnosis and Resilient Systems*, OASIcs, vol. 125, pp. 19:1–19:12, 2024, doi: [10.4230/OASIcs.DX.2024.19](https://doi.org/10.4230/OASIcs.DX.2024.19).
- **Why I selected it:** I selected this paper because it discusses structural coverage in large embedded projects without relying only on intrusive software instrumentation.
- **What I learned from it:** I found that embedded trace can support continuous and non-intrusive coverage measurement, including coverage derived from integration testing rather than unit testing alone.
- **Limitations I identified:** The method depends on suitable trace hardware and tooling, which may not be available in my project. It also addresses coverage measurement more than AI-assisted gap resolution.
- **How I will use it in my project:** I will use it to acknowledge alternative ways of collecting embedded coverage and to explain why my chosen host-based or instrumented method is appropriate for the scope of the study.

------------------------------------------------------------------------

## 3. Generative AI & Digital Tool Transparency Log

I will record any use of AI or other digital research tools that affects my literature search, source evaluation, wording, or analysis. I will check source details and claims against the original publication record and revise this notebook as I complete the full-text review.

| Date | Tool Used | Task Performed | Exact Prompt or Input Query Used | How I critically evaluated or modified the output |
|:--------------|:--------------|:--------------|:--------------|:--------------|
| 06/08/2026 | ChatGPT | I used ChatGPT to help identify candidate literature which would be relevant to my proposed research study. | `For my masters thesis, I want to explore the idea of using an LLM to assist in the code coverage process. Particularly in providing a feedback loop at the end of a static coverage analysis that includes suggested fixes/implementations for uncovered code. As a starting point I need you to find me some solid, reliable literature on Google Scholar that I can go through myself and verify whether or not they are relevant. Ensure any and all sources and links are included for me.` | I checked titles, authors, publication venues, years, page ranges, and DOI or arXiv records against publisher, conference, institutional, or bibliographic sources. I removed material that did not directly support coverage-guided testing, program analysis, test validation, evaluation, or embedded C testing. I will reassess every summary and limitation during the full-text review. |

------------------------------------------------------------------------

## 4. Weekly Activity & Reflection Logs

### Week 05: Focus Area — Initial Literature Review

- **Activities Completed:**
  - I defined the working title, research aim, research questions, objectives, and methodological direction using my proposal presentation.
  - I assembled an initial literature set covering coverage-guided LLM testing, program analysis, test validation and repair, mutation testing, conventional automated test generation, and embedded C testing.
  - I checked the publication details of the selected sources and recorded why each source is relevant to my proposed study.
  - I grouped the literature according to the part of the research design that it informs.
- **Key Insights / Discoveries:** I found that the closest AI-assisted approaches use iterative feedback rather than a single prompt. I also found that coverage values must be interpreted alongside test correctness and fault-detection measures. The embedded literature shows that hardware interfaces, timing, resource limits, instrumentation overhead, and the need for mocks or stubs must be treated as central parts of the study.
- **Obstacles Encountered & How I Overcame Them:** I found that much of the direct LLM test-generation evidence is based on Python or Java. I addressed this by including embedded-specific literature on C test generation, symbolic execution, hardware-interface testing, instrumentation, stubbing, and trace-based coverage. I will use these sources to decide which parts of the LLM workflows can be transferred to embedded C and which parts require adaptation.
- **Plan for Next Week:** I will begin a comprehensive review of the selected literature immediately. I will read the full papers, verify the initial summaries, compare their methods and evaluation measures, identify the strongest sources for the final introduction, and refine the proposed experimental design.

------------------------------------------------------------------------

**Next step:** I will begin the comprehensive literature review immediately and update this notebook as I complete the full-text evaluation of each selected source.
