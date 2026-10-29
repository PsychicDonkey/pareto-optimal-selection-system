# Pareto Selection Planner

A decision-support tool for the validity–diversity trade-off in personnel selection. It shows the person designing an assessment battery every Pareto-optimal way to weight the chosen assessments, so that the cost of each option in hire quality and adverse impact is visible before anyone is hired.

**Live tool:** https://psychicdonkey.github.io/pareto-optimal-selection-system/

Built for AI in IOP, Assignment 2 ("Build It"), Section of Organisational Psychology, University of Cape Town, 2026.

## Who it is for

The I-O psychologist or talent assessment lead at a large South African employer who designs the assessment battery for a high-volume intake (graduate programmes, learnerships, branch or contact-centre roles), and who must be able to defend that battery under Section 8 of the Employment Equity Act.

## Running it

The whole tool is one file, `index.html`, with no external libraries, network calls or build step.

- **Online:** open the live URL above.
- **Offline:** download `index.html` and open it in any current browser (Chrome, Edge, Firefox or Safari). It works without an internet connection.

There is nothing to install and no API key.

## What it does

1. **Choose the battery.** Select any two or more of seven predictors: biodata, cognitive ability, conscientiousness, structured interview, integrity test, situational judgement test and work sample.
2. **See the trade-off.** The frontier chart shows every weighting for which neither composite validity (or expected criterion score) nor the adverse impact ratio can improve without the other getting worse. Grey points are sampled weightings; any point below the frontier is dominated.
3. **Choose a point.** Set an adverse impact threshold, click the frontier, or type weights. The tool reports validity, expected criterion score, adverse impact ratio and the focal group's share of hires, and says when a weighting is dominated and what the frontier offers instead.
4. **Score an applicant file.** Upload a CSV of raw scores (or use the built-in simulated pool). Scores are converted to z-scores with an editable norm table, ranked on the chosen weights, and the top share selected. The ranked file can be downloaded.

The Glossary tab defines the technical terms, and four teaching presets show unusual frontier shapes (no trade-off, steep trade-off, redundant predictors, reversed subgroup difference).

### CSV format

Columns: `ID, GROUP, BIO, GMA, CON, SI, INT, SJT, WS, PERF`

- Only the selected predictors are required; other predictor columns are ignored.
- `GROUP` is `Focal` or `Reference`. It is used only to report outcomes by group, never to rank.
- `PERF` is optional simulated or historical performance, used only to show how those hired would have performed.
- Comma or semicolon delimiters are accepted, including decimal commas.

The tool warns about excluded rows, duplicate IDs, a zero norm SD, norms that do not fit the pool, possibly reverse-keyed columns, and ties at the cut-off.

## How it works

The running tool does not call any AI model. Its intelligence is an embedded computational model:

- Predictors and the criterion are treated as multivariate normal with the same correlation matrix in both groups, which differ only in means (De Corte et al., 2007, Appendix A).
- For any weighting, composite validity, expected criterion score of those selected and the adverse impact ratio are computed analytically for top-down selection on one common cut-off.
- The frontier is computed exactly by weighted-sum scalarisation: for each relative weight on validity and composite subgroup difference, the best non-negative weighting is found by solving a small non-negative quadratic programme over every set of active predictors.
- Weights are non-negative and sum to 1, so no candidate is penalised for scoring well on a test.

Code was written by Claude (Anthropic, Claude Opus 5.5) from specifications prepared and validated by the author. The AI-use log is in the project outline.

## How we know it works

The Checks tab runs nine checks in the browser:

1. Known-answer test against De Corte et al. (2007), Table 2 (21 points).
2. Known-answer test against Berry et al. (2024), Table 5 (21 points), at the design values printed in the paper.
3. Optimiser cross-check against a brute-force grid and 20,000 random weightings.
4. Matrix validity (positive definiteness), with a deliberately impossible example.
5. Unconstrained regression weights beside the non-negative weights.
6. A 20,000-applicant simulation against the analytic predictions.
7. Raw-to-z round trip.
8. Edge cases, including malformed files, ties at the cut-off, reverse-keyed columns and mismatched norms.
9. Configuration stress tests across every battery, battery sizes two to seven, and extreme selection ratios and focal shares.

All nine pass in Version 5.

## Data and privacy

Uploaded files are read in the browser and are never transmitted or stored. Nothing leaves the user's device. The downloadable ranked file links identifiers to hiring outcomes and must be handled under the employer's own data controls and the Protection of Personal Information Act. Demonstrations use synthetic applicants only.

## Limitations

- **Simulation only.** The preset values are mostly US meta-analytic estimates. Real use in South Africa requires South African validation studies, local norms or South African meta-analytic evidence, as Section 8 of the Employment Equity Act requires.
- The tool models one two-group comparison (Black–White) and one composite criterion.
- Frontier weights estimated from a sample lose some of their advantage in new samples (Song et al., 2023).
- The tool presents trade-offs, not recommendations. It cannot remove adverse impact that reflects structural inequality rather than test properties (Theron, 2013).

## Repository contents

- `index.html`: the complete tool (interface, engine and checks).
- `README.md`: this file.

The computation engine is the first `<script>` block in `index.html` (`id="engine"`). Copied into a `.js` file, it exports itself as a Node module, so it can be tested headlessly.

## Key sources

- Berry, C. M., Lievens, F., Zhang, C., & Sackett, P. R. (2024). Insights from an updated personnel selection meta-analytic matrix: Revisiting general mental ability tests' role in the validity–diversity trade-off. *Journal of Applied Psychology, 109*(10), 1611–1634. https://doi.org/10.1037/apl0001203
- De Corte, W., Lievens, F., & Sackett, P. R. (2007). Combining predictors to achieve optimal trade-offs between selection quality and adverse impact. *Journal of Applied Psychology, 92*(5), 1380–1393. https://doi.org/10.1037/0021-9010.92.5.1380
- Herde, C. N., Lievens, F., Jackson, D. J. R., Shalfrooshan, A., & Roth, P. L. (2020). Subgroup differences in situational judgment test scores: Evidence from large applicant samples. *International Journal of Selection and Assessment, 28*(1), 45–54. https://doi.org/10.1111/ijsa.12269
- Roth, P. L., Switzer, F. S., III, Van Iddekinge, C. H., & Oh, I.-S. (2011). Toward better meta-analytic matrices: How input values can affect research conclusions in human resource management simulations. *Personnel Psychology, 64*(4), 899–935. https://doi.org/10.1111/j.1744-6570.2011.01231.x
- Sackett, P. R., Zhang, C., Berry, C. M., & Lievens, F. (2023). Revisiting the design of selection systems in light of new findings regarding the validity of widely used predictors. *Industrial and Organizational Psychology, 16*(3), 283–300. https://doi.org/10.1017/iop.2023.24
- Song, Q. C., Tang, C., Newman, D. A., & Wee, S. (2023). Adverse impact reduction and job performance optimization via Pareto-optimal weighting: A shrinkage formula and regularization technique using machine learning. *Journal of Applied Psychology, 108*(9), 1461–1485. https://doi.org/10.1037/apl0001085
- Theron, C. C. (2013). Can the ideals of employment equity legislation be achieved by means of psychometric alchemy? *Industrial and Organizational Psychology, 6*(4), 414–418. https://doi.org/10.1111/iops.12076

## Author

Thom von Zahn, Master's in Industrial-Organisational Psychology, University of Cape Town.
