---
layout: post
title: "Understanding matching estimators in causal inference and policy evaluation"
date: 2025-03-08
permalink: "/understanding-matching-estimators-in-causal-inference/"
categories:
  - "Blog"
tags:
  - "matching"
  - "policy evaluation"
  - "impact evaluation"
  - "exact matching"
  - "matching estimator"
  - "propensity score matching"
  - "nearest neighbor matching"
  - "causal inference"
---


Drawing accurate causal inference estimates from observational data is a central challenge in impact evaluation studies of public policies and social programs. Matching estimators are one of the techniques which allow investigators to compare treated and untreated groups that are similar in all relevant characteristics except for the treatment itself. Their simplicity and relatively easy implementation make them especially popular in policy evaluation, where conducting randomized controlled trials is often impractical. This blog post explores the concept of matching estimators, discussing their key limitations, the critical steps before and after matching, and the most widely used algorithms, providing a comprehensive guide for applying this method effectively.

## What are matching estimators?

Matching estimators are statistical techniques used to estimate treatment effects by pairing each treated observation with one or more non-treated individuals with similar characteristics. The primary goal is to create a control group of individuals who are really similar to the observed characteristics of the treated group, but with the only difference that they have not received the treatment or participated in the public program. The key assumption, consistent with the counterfactual framework, is that the control group mimics how the treatment group would have evolved after the treatment period if they had not participated in the public policy. Like other quasi-experimental methods, matching estimators aim to replicate the conditions of a randomized controlled trial (RCT) as closely as possible (often referred to "as random as good"). By doing so, matching estimators help control for confounding variables that could otherwise bias the estimation of treatment effects. Matching estimators are particularly useful when randomized experiments are impractical, unethical or infeasible because the study relies on observational data collected after the treatment period.

However, matching estimators have **limitations**. They depend heavily on the availability and quality of observed covariates; any unmeasured confounders can still introduce bias. For that reason, the Conditional Independence Assumption (CIA) is another key assumption for matching estimators. CIA states that once we account for a set of observed covariates, the treatment assignment is independent of the potential outcomes. In other words, after controlling for relevant characteristics, any remaining differences in outcomes between treated and control groups can be solely attributed to the treatment itself rather than to pre-existing differences. However, the validity of this assumption depends on having a sufficiently rich set of observed covariates; if relevant confounders are unobserved or omitted, the assumption fails, leading to biased estimates. Furthermore, the results may be sensitive to the choice of the matching algorithm and the list of matching covariates. Therefore, conducting a sensitivity analysis is a key part of any matching study to demonstrate that the methodological decisions do not drive the results.

For example, consider a study evaluating the impact of a job training program on participants' subsequent wages. If individuals self-select into the program, their motivation or prior work experience may systematically differ from those who do not participate. Simply comparing the earnings of participants and non-participants could lead to biased conclusions. Matching estimators may address this issue by pairing participants with non-participants with similar characteristics (e.g., age, education level, prior earnings, prior work experience), isolating the effect of the training program.

To sum up, the **key steps** to perform a matching analysis are:

- Select the final treatment group sample
- Select the potential control group
- Select the covariates that might explain the probability of treatment
- Choose the matching algorithm and relevant features
- Obtain the final control group with the matching procedure
- Conduct a balancing test
- Compute treatment effects
- Perform a sensitivity analysis

## Previous steps before implementing the matching algorithm

Before implementing the matching estimator, it is essential to outline the key preparatory steps that ensure the validity of the analysis. This section does not focus on defining the research question, cleaning the database, or properly defining the outcomes. Instead, it highlights the **critical steps needed before constructing a suitable control group** through the matching procedure.

The first step is **selecting the final sample of the treatment group**. Researchers may need to drop treated individuals for various reasons, such as attrition, incomplete information, or multiple treatment participation. If only a small number of treated individuals are dropped, the impact on the estimates may be negligible. However, if the proportion is substantial, a proper analysis should be conducted to assess whether the dropped observations share specific characteristics that could introduce bias into the results.

The next step involves **selecting the vector of covariates which predicts treatment status** **or policy evaluation**. To reduce confounding, these covariates should include factors influencing both the likelihood of receiving treatment and the outcome of interest. Researchers should only select pre-treatment characteristics such as demographics, socioeconomic factors, and prior behaviors or exposures. Post-treatment variables or those affected by the treatment itself cannot included in this vector, as they can introduce endogeneity and bias the estimated treatment effects. Additionally, checking for multicollinearity and ensuring enough variation in covariates fosters the robustness of the matching process.

Then, researchers must **identify an initial sample of untreated individuals** from which the final control group will be drawn. This “potential control group” should be selected carefully to avoid introducing bias due to flawed sampling. This means that researchers must carefully choose a database or a set of untreated individuals from elsewhere but considering several key aspects to avoid potential bias due to the selection of this potential control group. I like to call "potential control group" because this is not the final control group in which we will compare with the treatment group. This is the initial sample where we will select the final control group. Thus, two key aspects might be considered when selecting this initial or potential control group:

- Researchers must be aware that the **same variables available** for the treatment group must be present in the control group to ensure valid comparisons and minimize discrepancies caused by differences in data collection or measurement.
- The control group should **exhibit similar decision-making patterns and characteristics** to the treated group, as individuals often self-select into treatments based on unobserved factors. For instance, when evaluating the impact of a job training program for unemployed individuals, a suitable control group could be drawn from those registered as unemployed in the local employment service. This ensures not only that they are unemployed but also that they share similar motivations and willingness to seek employment and participate in training programs.

Therefore, by carefully defining the potential control group, researchers can enhance the credibility of their matching procedure and reduce biases in causal inference or policy evaluation driven by systematic differences between treated and control individuals.

## Conducting the matching procedure: types of matching estimators

**Matching estimators can be classified into three main groups**: exact matching and coarsened exact matching (which pairs treated and untreated individuals according to the value of each matching variable), distance or nearest neighbour matching (which pairs treated and untreated individuals according to a multidimensional distance metric), and propensity score matching (which pairs treated and untreated individuals according to the expected probability of being treated). Each method uses a distinct approach to matching individuals based on the vector of covariates selected previously. The final algorithm choice depends on the research question, the data available and the researchers’ decision based on objective reasons.  In addition, it is important to note that scholars sometimes propose alternative classifications. For instance, [Scott Cunningham in Mixtape](https://mixtape.scunning.com/05-matching_and_subclassification) differentiates between exact matching and approximate matching. He categorizes all other matching estimators under the "approximate matching" group, distinguishing them from the stricter concept of exact matching. Finally, I want to clarify that I aim to explain briefly each matching algorithm but not explain each step-by-step implementation. For detailed guidance on how to implement these methods, several excellent references are available, such as *[Mixtape by Cunningham](https://mixtape.scunning.com/05-matching_and_subclassification)* and [*The Effect* by Huntington-Klein](https://theeffectbook.net/ch-Matching.html).

Before going through the different matching techniques, additional practical considerations might be considered:

- **One-to-one or many-to-one matching**: you can match each treated observation to just one control observation (one-to-one matching) or select multiple control units for each treatment (many-to-one matching). The latter can increase the precision of the estimated treatment effect. However, it also introduces the risk of overmatching or losing valuable information if additional control individuals are not carefully selected.
- **Matching with or without replacement:** each control can match multiple treated individuals (matching with replacement). It can increase the number of possible matches and lead to more balanced groups, though it may reduce the uniqueness of control observations in the sample. Then, it is relevant to decide whether a control observation can be paired with more than one treated individual and decide a rule to avoid ties.

#### 1. Exact Matching and Coarsened Exact Matching (CEM)

Exact matching and Coarsened Exact Matching (CEM) are two closely related techniques, both aimed at creating comparable groups by matching observations based on the values of their covariates. Exact matching pairs treated individuals with non-treated individuals that have identical values for all matching variables, while CEM extends this concept by introducing flexibility, allowing a range/interval of values to be matched. In this way, CEM addresses the limitations of exact matching by relaxing the requirement for identical values and enabling a broader set of matches without sacrificing the integrity of the analysis.

- **Exact matching** pairs each treated observation with non-treated individuals who have identical values for all matching variables or covariates. This method is straightforward and intuitive but often impractical, especially when dealing with many variables or when the sample size of the potential control group is limited, leading to difficulties in finding exact matches.
- **CEM** – I also like to call it flexible exact matching – addresses the limitations of exact matching by grouping continuous variables into broader categories, increasing the likelihood of finding comparable matches while preserving balance. Instead of requiring identical values, CEM categorizes variables into intervals, such as age ranges or income brackets, ensuring treated and control groups remain similar. However, its effectiveness depends on selecting appropriate coarsening levels: too broad may introduce bias, while too narrow may reduce sample size, impacting statistical power and generalizability.

#### 2. Nearest neighbor matching or distance matching

Nearest neighbour matching or distance matching pairs treated and non-treated individuals or units based on a distance measure between all the relevant covariates at the same time. This method aims to find the closest matches by minimizing individuals' differences. The most common form of distance matching is nearest neighbor matching, where each treated observation is paired with the non-treated observation(s) with the smallest distance in their covariate space. This distance is typically calculated using one of the two primary metrics: Euclidean distance or Mahalanobis distance. However, other distance metrics can be used.

- **Euclidean distance**: the most intuitive and commonly used measure. It calculates the straight-line distance between two individuals in a multidimensional space of all their covariates. Although Euclidean distance is easy to compute, it has limitations. Most notably, it does not account for correlations among the covariates, which could lead to less accurate matches. For instance, if two covariates are highly correlated, Euclidean distance might treat them as independent, potentially undermining the quality of the match.
- **Mahalanobis distance:** unlike Euclidean distance, Mahalanobis distance adjusts for the correlations among covariates. It considers the data's covariance structure, offering a more reliable measure of distance. This adjustment helps ensure that matches are made based on the true distribution of the variables rather than their raw distances, improving the quality of matches.

Therefore, the choice between Euclidean and Mahalanobis distance (or another distance metric) can significantly impact the results. In cases where covariates are strongly correlated, Mahalanobis distance is usually preferred as it accounts for this relationship. Moreover, treated and control individuals can sometimes be matched despite having a large distance, and it can introduce concerns about the quality of the match. To address this, researchers often impose additional restrictions, such as using a **caliper**. A caliper defines the maximum allowable distance between matched pairs. If no suitable match is found within this range, the treated unit is excluded from the analysis, which helps to improve the quality of the matches but may reduce the final sample size.

#### 3. Propensity Score Matching (PSM)

Propensity Score Matching (PSM) aggregates multiple covariates into a single summary measure, known as the **propensity score**, which represents the probability of receiving the treatment given the observed covariates. By matching treated and non-treated units with similar propensity scores, PSM aims to create comparable treatment and control groups. The process of PSM involves two key steps:

1.  **Estimating the propensity score:** The first step is to estimate the probability of receiving treatment based on observed covariates. This is typically done using a logistic regression or probit regression model, where the treatment status (a binary variable indicating whether a unit received the treatment or not) is regressed on the set of matching covariates. The resulting model generates a propensity score for each observation, which reflects the expected likelihood that a given individual would receive the treatment given its covariates.
2.  **Matching based on propensity scores:** Once the propensity scores have been estimated, the next step is to match treated and untreated units with similar propensity scores. This matching process ensures that the treated and control units are comparable regarding their observed characteristics. There are several ways to perform matching based on the propensity scores, each with its methodology and advantages:

- **Nearest neighbor Matching:** each treated unit is matched with the untreated unit that has the closest propensity score (single nearest neighbor matching), or each treated unit is matched with the k nearest untreated units based on propensity scores (k-nearest neighbor matching). This method can reduce variance but may introduce bias if the matches are not close enough.
- **Caliper Matching:** this method imposes a maximum allowable difference (caliper) between the propensity scores of matched units. If no suitable match is found within the caliper range, the treated unit remains unmatched. This method helps to ensure that matches are sufficiently similar, improving the quality of the matches and avoiding control units very different from their treated unit. However, treated individuals might remain unmatched.
- **Radius Matching:** similar to caliper matching, but instead of a fixed caliper, a radius is defined within which all untreated units with propensity scores falling within this radius around the treated unit's score are considered as matches. This allows for multiple matches within a defined range, potentially reducing bias.
- **Stratification or interval Matching:** individuals are divided into strata or intervals based on their propensity scores. Within each stratum, treated and untreated units are compared. The treatment effect is then averaged across strata. This method ensures comparability within strata but may lose some units if they fall into sparsely populated strata.
- **Kernel Matching:** each treated unit is matched with a weighted average of all untreated units, with weights decreasing with the distance between propensity scores. Common kernel functions include Gaussian and Epanechnikov kernels. This method uses more information from the data and can reduce variance but may introduce bias if the bandwidth of the kernel is not appropriately chosen.
- **Local Linear Matching:** An extension of kernel matching, this method fits a local linear regression of the outcome on the covariates within a bandwidth around each treated unit’s propensity score. This technique can improve the efficiency and robustness of the matching estimator.

One relevant decision in PSM is the **common support restriction**. It refers to the overlap in propensity scores between treated and untreated individuals, ensuring that comparisons are made only where there is sufficient similarity between groups. It guarantees that for every treated individual, there exists at least one comparable control unit with a similar probability of receiving treatment. Without common support, the matching procedure may extrapolate treatment effects to areas where no suitable comparisons exist, leading to biased estimates. To address this issue, researchers often impose this restriction, such as discarding observations with extreme propensity scores, to ensure valid and reliable causal inference.

### Practical considerations after performing the matching

After performing matching, researchers must check the **balance of covariates between treated and control groups**. This can be done using standardized mean differences or other balance metrics. If more than one untreated unit is selected for each treated unit, it is important to cluster by fixed effects (FE) within the matched groups to avoid bias due to the overrepresentation of control individuals. Balanced groups indicate that the matching process has successfully created comparable groups. However, it’s also essential to check whether any remaining imbalance is economically relevant. For instance, if the average age of the treatment group is 45.4 years and the control group is 45.8 years (with a p-value of 0.03), a difference of less than half a year may not be significant for the study's objectives.

Then, it is time to **compute treatment effects**. This involves comparing the outcomes between the treated and matched control groups to estimate the *average treatment effect* (ATE) or the *average treatment effect on the treated* (ATT), depending on the research question. The estimation relies, as previously stated, on the assumption that, after controlling for observed covariates through matching, any remaining differences in outcomes can be attributed solely to the treatment itself. Researchers can use simple differences in means between the treated and matched control groups (not recommended when one-to-many matching). However, regression adjustment (in the case of adding covariates that might explain outcomes but not treatment status or clustering matched pairs in one-to-many matching) or difference-in-differences (DiD) can also be applied to improve precision. In fact, the DiD model procedure is the most accurate and robust option if researchers have panel data information (they observe the outcomes before and after treatment over time).

Finally, the researcher must **conduct a sensitivity analysis** to assess how robust the results are to different methodological choices. The goal is to ensure that the main findings are not driven by specific decisions made during the matching process. Two critical decisions in this regard are the selection of covariates for matching and the choice of matching algorithm. First, adding or removing covariates may not necessarily affect the average treatment effect, but second, it’s essential to test different matching estimators to verify the robustness of the results. Furthermore, if a significant proportion of treatment units are dropped during the matching process, it’s relevant to check how this affects the results, as this could potentially lead to biased conclusions.

## Discussion

Matching estimators are widely used in impact evaluation studies to estimate the causal effects of participating in a public policy or program from observational data. By constructing comparable groups of treated and untreated individuals based on observable covariates before starting the implementation of the intervention, these techniques help reduce or mitigate bias from confounding variables and improve the accuracy of treatment effect estimates.

The key advantage of matching methods is their ability to reduce selection bias by ensuring that treated and control groups are comparable based on observable covariates. This allows for credible causal inference without needing randomized experiments, which are often impractical, costly, or unethical. Matching methods provide a valuable alternative in policy analysis and social science research when experimental or other quasi-experimental designs are not feasible.

Despite these advantages, matching estimators are not without limitations. A central challenge is the strong dependence on observed covariates. Matching estimators assume that all relevant confounders have been measured and included in the matching process. If important covariates are omitted or measured inaccurately, the matching procedure may still result in biased estimates. Additionally, while matching methods can control for observed covariates, they cannot address unobserved confounding that influences both the treatment and outcome. Matching methods also face challenges with matching quality. For instance, a matching estimator might not always find suitable matches, especially when the data set is small or when there are many covariates or too few. Even when matches are found, differences in covariates may remain, necessitating further adjustments.

Among the different matching methods, exact matching and CEM provide intuitive approaches but are limited by data availability and sample size. Distance matching offers more flexibility but requires careful consideration of the distance metric. PSM aggregates multiple covariates into a single propensity score, making it feasible in more complex settings. However, PSM relies heavily on the correct specification of the propensity score model, and poor model fit can compromise the validity of the results.

In conclusion, matching estimators are powerful tools for causal inference in observational research, but their effectiveness depends on the quality of the data, the choice of covariates, and the matching algorithm used. Researchers should carefully assess the balance between treated and control groups and conduct sensitivity analyses to evaluate the robustness of their findings. Combining different matching methods or integrating them with other econometric techniques (difference-in-differences) can further strengthen causal estimates and improve the credibility of policy evaluations.

*First version: 08/03/2025*

*Updated version on 15/03/2025*

------------------------------------------------------------------------

## Main references

Caliendo, M., & Kopeinig, S. (2008). SOME PRACTICAL GUIDANCE FOR THE IMPLEMENTATION OF PROPENSITY SCORE MATCHING. Journal of Economic Surveys, 22(1), 31-72. https://doi.org/10.1111/j.1467-6419.2007.00527.x

Cunningham, S. (2021). Causal Inference: The Mixtape. Yale University Press. https://doi.org/10.2307/j.ctv1c29t27

Huntington-Klein, N. (2021). The Effect: An Introduction to Research Design and Causality (1st ed.). Chapman and Hall/CRC. https://doi.org/10.1201/9781003226055

Iacus SM, King G, & Porro G. (2012) Causal Inference without Balance Checking: Coarsened Exact Matching. Political Analysis. 20(1), 1-24. https://doi.org/10.1093/pan/mpr013

Rosenbaum, P. R., & Rubin, D. B. (1983). The Central Role of the Propensity Score in Observational Studies for Causal Effects. Biometrika, 70(1), 41–55. https://doi.org/10.2307/2335942

Rubin, D. B. (1979). Using Multivariate Matched Sampling and Regression Adjustment to Control Bias in Observational Studies. Journal of the American Statistical Association, 74(366), 318–328. https://doi.org/10.2307/2286330

Stuart, E. (2010). "Matching Methods for Causal Inference: A Review and a Look Forward." Statistical Science 25 (1) 1 - 21. https://doi.org/10.1214/09-STS313
