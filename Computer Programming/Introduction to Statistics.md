---
Tags: 
Created: 2023-11-08 01:07:33
---
(Links:: [[Statistical Methods]])
# What is Statistics
**Statistics** is the science of data. The main goal is to draw inference on the random mechanism that produced the data at hand. In particular, we use statistics (methods/tools/techniques) to gain information about a group of objects (*population*) and/or to make decisions and predictions. **Census** is the collection of data from every member of population. Usually too large too collect. Typically, a *sample*, a selected sub-collection from the population, is studied: Sample -> Data -> Analysis -> Conclusion about population.
# Statistical and critical thinking
Statistical study:
- Prepare
	- Context
	- Source
	- Sampling method
- Analyse
	- Graph data
	- Explore data
	- Apply statistical methods
- Conclude

Samples are a sub-collection of population. Different sample lead to different data. Hence, we could possibly get different conclusions about the population, depending on the sample. A sample should therefore be *representative* (same characteristics as population) and *unbiased* (no systematic difference with population)
# Collecting sample data

| Sampling methods          | Description                                                                                                                                      |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Voluntary response sample | Subjects decide themselves to be included in sample.                                                                                             |
| Random sample             | Each member of population has equal probability of being selected.                                                                               |
| Systematic sampling       | After starting point, select every $k$-th member.                                                                                                |
| Stratified sampling       | Divide population into subgroups such that subjects within groups have same characteristics, then draw a (simple) random sample from each group. |
| Cluster sampling          | Divide population into clusters, then randomly select some of these clusters.                                                                    |
| Convenience sampling      | Easily available results.                                                                                                                        |

- Important concepts:
	- Variable: varying quantity
- In cause and effect studies:
	- **Response (dependent) variable**: Representing the effect to study
	- **Explanatory (independent) variable**: Possibly causing that effect
	- **Confounding**: Mixing influence of certain (typically unobserved) variables on explanatory and response variables
- Different types of study:
	- **Observational study**: Characteristics of subjects are observed; subjects are not modified.
		- Retrospective (case-control): Data from past
		- Cross-sectional: Data from one point in time
		- Prospective (longitudinal): Data are to be collected.
	- **Experiment**: Some subject treatment
		- Sometimes control and treatment group; single-blind or double-blind
		- To measure placebo effect or experimenter effect.

# Types of data
- **Parameter**: A population's characteristic. 
  Notation: often Greek symbols, e.g. $\mu$, $\sigma$
- **Statistic**: A data based measurement describing a characteristic of the sample. 
  Notation: random variables, $X, T, \bar T$; realized (observed) values $x, t, \bar x$, etc.
	- **Quantitive (numerical)**: Numbers representing measurements; **discrete** (countably many possible values), **continuous** (uncountably many).
	- **Qualitative (categorical)**: Names or labels represent measurements.
- The **level of measurement** of data determines which statistical methods are applicable.
	- Qualitative data:
		- **Nominal**: Names, labels, categories (no ordering). 
		  Examples: Gender, eye colour.
		- **Ordinal**: categories with ordering, but no (meaningful) differences. 
		  Examples: U.S. grades (A-F), opinions (totally disagree/.../totally agree).
	- Quantitive data:
		- **Interval**: Ordering possible and meaningful differences, but no natural zero starting point. 
		  Examples: year of birth, temperature ◦C/◦F.
		- **Ratio**: Ordering possible and meaningful differences and natural starting point. 
		  Examples: body length, marathon times.

> [!question]- Determine level of measurement: 1. M&M colours, 2. inauguration years of U.S. presidents, 3. brain volumes (cm3), 4. level of lead in blood (low/medium/high).
> 1. Qualitative data, Nominal
> 2. Quantitive data, Interval
> 3. Quantitive data, Ratio
> 4. Qualitative data, Ordinal

---
References: