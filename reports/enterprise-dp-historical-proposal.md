# Enterprise Differential Privacy: Historical Team Research Proposal

> GSOE9011 Computing Group 42, 2025. Proposal, not an implemented or evaluated framework. Title-page roster and figures are omitted. Literature claims and citations are historical and not all revalidated. Duplicated passages and mistakes remain: read [review notes](../docs/proposal-review-notes.md).

Abstract

Artificial intelligence and data analytics have become central to corporate decision-making, yet they also amplify risks of personal-data exposure and regulatory non-compliance. Traditional anonymisation and access-control measures have proven insufficient against modern inference attacks such as re-identification, membership inference, and model inversion. Differential Privacy (DP) offers mathematically provable protection by bounding the impact of any single record, but its practical adoption in enterprise AI systems remains limited. Most existing studies emphasise algorithmic theory or large-scale deployments by governments and technology giants, leaving a critical gap in guidance for typical corporate environments.

This project addresses that gap by developing a workload-aware DP deployment framework tailored to enterprise analytics workflows. The framework integrates mechanism selection, sensitivity budgeting, noise accounting, and auditability into a unified, reusable model applicable to machine-learning tasks such as classification, segmentation, and forecasting. By systematically evaluating privacy–utility trade-offs under realistic corporate constraints, the study aims to operationalise DP as a practical privacy-by-design standard, enabling organisations to perform AI-based analytics responsibly while maintaining public trust and regulatory compliance.

Key words: differential privacy, corporate analytics, privacy–utility trade-off, federated learning, data compliance

Table of Contents

Abstract……….…………………………………………………………………………1

1Introduction…………………………………………………………………………2

2Literature review……………………………………………………………………2

2.1Corporate Data Privacy Challenges in Enterprise Analytics…………………..3

2.2Foundations of Differential Privacy……………………………………………4

2.3Differential Privacy in Machine Learning Systems……………………………4

2.4Real-world deployments & constraints…..……………………………………6

2.5DP for structured enterprise data & Federated Learning………………………7

2.6Research Gap and Motivation….………………………………………………8

3Significance and innovation...………………………………………………………8

3.1Significance……………………………………………………………………8

3.2Innovation……………………………………………………………………...9

4Research proposal or Methodology…………………………………………………9

4.1Approach….………………….……………………………………………….10

4.2Methodology.………………………………………………….…...................11

4.3Conceptual framework......................................................................................12

4.4Timeline…………………………………………………................................12

4.5Dissemination of results…………………………………………………........13

4.6National/international benefits……………………………..............................13

5Conclusion…………………………………………………………………………13

6Acknowledgement…………………………………………………………………14

7References………………………………….……………………………………...15

Introduction 

In the past few years, AI tools have repeatedly exposed personal data when employees pasted sensitive content into public chatbots or when models themselves leaked information learned from training data causing lots of harm. A widely reported example is Samsung’s 2023 internal ban after source code was pasted into ChatGPT; the company only re-opened access later with stricter controls (Forbes, 2023; SamMobile, 2025). In Australia, the NSW Reconstruction Authority confirmed that a contractor uploaded thousands of flood victims’ records to ChatGPT in March 2025, triggering a government investigation and public concern (NSW Government, 2025; news.com.au, 2025). Beyond operational mishaps, research has shown that “anonymised” datasets can be re-identified (e.g., the Netflix Prize dataset) and that AI models are vulnerable to membership inference and model inversion attacks that can reveal whether a person’s data was used or even reconstruct sensitive attributes (Narayanan & Shmatikov, 2008; Shokri et al., 2017; Fredrikson et al., 2015). These realities make a pre-AI privacy-hardening step both necessary and timely; regulators and standards bodies are also publishing guidance on how to evaluate privacy guarantees rather than take vendor claims at face value (NIST, 2025).

Among popular approaches, Differential Privacy (DP) offers a mathematically mature method to quantify and control privacy loss. It can be applied to both data analysis and model training. Despite its theoretical maturity, DP adoption within enterprise analytics remains limited due to operational complexity and uncertainty about its impact on model performance. Addressing this gap is crucial as organisations face increasing regulatory and ethical pressure to protect personal information while maintaining data utility. This research therefore explores how Differential Privacy can be effectively integrated into corporate AI workflows to achieve both analytical value and measurable privacy guarantees.

Literature review 

In recent years, artificial intelligence and data analytics have changed how organisations use data in their daily operations. However, this increased reliance on data has also made long-standing privacy risks more serious. This literature review traces how these challenges have evolved from traditional anonymisation failures to modern inference-based threats, and how Differential Privacy (DP) has emerged as a formal mathematical response. It examines the theoretical foundations of DP and some practical implementations in machine-learning systems such as DP-SGD and PATE. Some deployments in large-scale industrial and governmental contexts are mentioned as well, which are good examples for further researching. The review further explores emerging research on applying DP to structured corporate data, federated learning, and blockchain-enabled analytics. By synthesising these threads, it identifies a critical gap between academic advances and enterprise adoption—specifically, the lack of deployable frameworks that translate DP parameters into business-ready configurations. By integrating these elements, it reveals the crucial gap between academic progress and practical application in enterprises. Specifically, as the background showing, there is a lack of a deployable framework that can convert data processing parameters into configurations suitable for business applications. This gap directly motivates the present study’s goal: to operationalise DP for typical corporate AI workflows while preserving both analytical utility and individual privacy.

Corporate data privacy challenges in enterprise analytics

More companies are utilizing data to make decisions and rely on machine learning systems for a more optimized workflow, increased customer analysis and future planning(McKinsey & Company, 2023). However, this shift substantially elevated the exposure of sensitive customer and organisational information(IBM Security, 2023). Classical anonymisation and access control policies are not the most suitable solutions for use in modern analytic environments. For example, in the Netflix re-identification case, the information concerning a user that appears to be quite anonymous was re-identified and connected to a specific user of Netflix (Narayanan & Shmatikov, 2008). In addition, machine learning models have been proved to leak sensitive details through inference-based attacks. For instance, membership inference attacks can reveal whether an individual's inputs were used to train a model (Shokri et al., 2020) and various model-inverse attack techniques allowing the recovery of an individual's private input from the model's outputs (Fredrikson et al., 2015). These issues make organisations significantly exposed to regulatory, ethical and reputational concerns.

The compliance aspect further complicates the landscape. Regulations like the GDPR, the California Consumer Privacy Act, and the Australian Privacy Act introduce precise restrictions on personal data use, emphasising data minimization and protections Against Re-identification. These provisions elevate the burden of responsibility on businesses to enhance privacy protections without compromising practical usefulness. However, the fragility of anonymisation demonstrated the need for rigorous privacy-preserving approaches suitable for enterprise analytics.

Foundations of Differential Privacy

Differentiational privacy (DP), as a mathematically rigorous model, has emerged as a trustworthy way to protect people's information in the context of statistical analysis and aspects of machine learning systems. Dukov et al. (2006) introduced a basic definition which uses the parameter ε to quantify the loss of privacy and guarantees that the inclusion or exclusion of individual data points in a computation has negligible impact on the output distribution. This guarantees a form of protection against so-called re-identification attacks. It can provide no form of this types of protection with conventional anonymisation schemes. DP mechanisms can typically be attained by adding calibration noise proportional to the sensitivity of the data. The first practical realization is the Laplace mechanism, which was through numerical queries.

As dynamic programming theory has matured, research on noise models has evolved toward greater applicability and resiliency. Although more robust theoretical propositions, such as Gaussian differential privacy, allows for more rigorous compositional guarantees and more streamlined integrations with complex machine learning functions (Dong, Roth & Su, 2022), experimental research by comparing k-norm mechanisms also shows that noise calibration methods have substantial impacts on utility. Hence, the choice of such mechanisms needs to strike a balance between model topology and data properties (Awan & Slavković, 2021).

DP does introduce impressive safeguards, but at the same time (higher ε) offers more privacy protection (lower accuracy). In addition, significant research gaps exist on choosing adequate privacy budgets and the understanding of cumulative noise in the combination process. How theoretical guarantees get translate into practice with an engineering decision is a question that also remains unopened. These limitations underscore the importance of evaluating data processing performance not only as a theoretical concept, but also in practical enterprise environments, where factors such as accuracy, scalability and operational constraints remained pressing considerations must be taken into account.

Differential Privacy in Machine Learning Systems

Differential privacy (DP) in machine learning has consolidated around two practical paradigms—DP-SGD and PATE—tied together by clipping and careful privacy accounting. DP-SGD limits any single record’s influence by ℓ₂-clipping per-example gradients, averaging them, and adding Gaussian noise; the cumulative privacy loss over many steps is tracked with the moments accountant, whose tighter composition makes realistic minibatch schedules compatible with single-digit privacy budgets rather than the much larger bounds implied by naïve composition (Abadi et al., 2016).

In machine learning, Differential Privacy (DP) mostly relies on two main ways: DP-SGD and PATE. They both need gradient clipping and good privacy accounting.For DP-SGD, it keeps single record private by doing ℓ₂-clipping on the gradients, then averaging them, and adding Gaussian noise. To check the total privacy loss across all training, people use the moments accountant. This accountant is good because it gives a much better (tighter) privacy budget. This means we can use normal training settings without needing huge $\varepsilon$ values, which is better than old methods (Abadi et al., 2016).

In practice, the recipe—clip → average → add noise → update—is paired with choices of sampling rate, noise multiplier, and step count that jointly determine (ε,δ)(\varepsilon,\delta)(ε,δ); the accountant converts these knobs into end-to-end guarantees with significantly smaller ε\varepsilonε than earlier analyses (Abadi et al., 2016).

PATE (Private Aggregation of Teacher Ensembles) offers a complementary route: train many teacher models on disjoint sensitive shards, answer queries via noisy majority voting, and then train a student on public (unlabeled) data labeled by those privatized votes. Because larger teacher margins imply lower sensitivity, the privacy accountant can provide data-dependent (often tighter) bounds, and semi-supervised learning keeps utility high even with limited queries (Papernot et al., 2016). Experiments on image benchmarks demonstrate that PATE attains competitive accuracy at low ε\varepsilonε, highlighting its strength when public unlabeled data are available and labeling can be throttled (Papernot et al., 2016).

Across both families, the core headache is utility degradation. Clipping truncates gradient signal, noise blurs updates, and every data-dependent query spends budget. Decision-tree pipelines make this accounting-utility tension especially visible: training requires repeated, adaptive queries (feature/threshold selection, split scoring), so the final accuracy is dictated by the mechanism choice (Laplace vs Exponential).The Laplace mechanism is mostly for continuous numerical things (like judging how good a split is) because it adds noise. The Exponential mechanism is for picking the best choice from discrete options (like choosing the best feature or threshold). The final accuracy is affected by how sensitive the data is (global sensitivity) and, most important, how you split the privacy budget for each step in the tree (Fletcher & Islam, 2020).

That paper shows these methods are ready in theory but still mostly used in labs. To make them reliable in real life, we shouldn't just look for one smart algorithm. It's more about having a good system for budget accounting, scheduling, and cleaning the data first so we don't need too many expensive queries (Fletcher & Islam, 2020).

To wrap up: DP-SGD gives us a fully private training using clip-and-noise with tight accounting (Abadi et al., 2016). PATE gives black-box privacy using noisy teachers and student models (Papernot et al., 2016). And the key point is that good performance in real projects really depends on how well we budget the privacy and design the mechanism (Fletcher & Islam, 2020).

Real-world deployments & constraints

Differential Privacy (DP)’s theory may be solid, and its algorithms might work well, but really putting it into practice in the real world is quite surprisingly tough  hard (Kenny et al., 2021; Drechsler, 2023). Academic research research (e.g., Dwork & Roth, 2014) tends to focus on developing new DP algorithms, or on huge deployments by big tech companies or government organizations. But what's missing  the problem is a lack of clear direction for typical businesses (enterprise environments). This gap means we don’t know if DP is good for regular company machine learning projects because we don’t have enough real-world proof that it works. Just put, we need more directions on how to deploy it.

Big tech firms have jumped ahead on DP usage, mostly because they put LDP systems into their products. Apple uses LDP to collect the information from large number of users without showing the identity of individual users. (Apple, 2017). And the smart thing is that it’s all randomized right on the person’s own computer, so the server never even knows the original, secret stuff. Google did  implemented something similar with its RAPPOR mechanismGoogle did something similar with the RAPPOR mechanism, short for Randomized Aggregatable Privacy-Preserving Ordinal Response (Erlingsson et al., 2014). They can then collect anonymous statistics like what sort of private browsing happens on client software. RAPPOR makes use of a slick trick called randomized response, so you get your privacy guarantees and can still do an analysis on the aggregate.

A large-scale application of DP is the use of a DP-based system by the US Census Bureau for the 2020 Census (Kenny et al., 2021). But this grand rollout hit snags with accuracy. In order to protect people’s privacy, the Census had to add DP “noise” that impacted the accuracy of the final results. This version makes apparent how hard your options are. Making privacy guarantees better may lead to losing on data utility, and then there could be data distorting.

Government agencies have the same type of challenges to adopt DP as private companies. While DP offers strong, formal privacy guarantees, most government bodies apart from the US Census Bureau. has not been eager to adopt it as their primary way of releasing data (Drechsler, 2023). This mutual hesitance implies that both public bodies and companies are bumping up against the same practical brick walls. A lot of issues still need to be resolved before DP can become a routine part of daily data operations. There are still many problems to be solved before DP can be routinely used in data operations, such as balancing accuracy and privacy, meeting operational requirements, and meeting regulatory requirements.

DP for structured enterprise data & Federated Learning

Federated Learning (FL) is a distributed machine learning approach that enables multiple data holders to collaboratively train a shared model without exchanging raw data, thereby preserving data locality and privacy.

Federated Learning (FL) keeps raw records at each site and exchanges model updates only. In each round, every data holder runs several local mini-batch SGD steps on its own data and sends the resulting weight/gradient update to a coordinator. The coordinator forms the next global model by a weighted average of client updates (weighting by local sample size) and redistributes it for the following round. In our DP setting, client updates are clipped per client and perturbed with Gaussian noise before aggregation, limiting the influence of any single data holder. To cope with non-IID partitions that are common across business units, we weight clients by data size and iterate for multiple rounds until validation stabilises (Imtiaz et al., 2020; Banse, Kreischer & Oliva i Jürgens, 2024).

Most work on differential privacy still focuses on simple query release or deep models on images and text, but real enterprises usually handle structured records such as customer profiles, transactions and logs. Recent work therefore looks at DP methods that operate directly on tabular and other “enterprise-like” data. Yuan et al. propose LDPK, a local-DP K-prototypes style algorithm that perturbs each user’s mixed-type record on the client side and then performs iterative clustering on the noisy data (Yuan et al., 2023). Their experiments on Adult and US census data show that clustering quality approaches a central DP baseline as the privacy budget and dataset size increase, while removing the need for a trusted third party (Yuan et al., 2023). This is close to tasks like segmenting customers or accounts without centralising raw records. 

Another line of work combines federated learning and DP for time-series and other distributed enterprise data. Imtiaz et al. build an end-to-end DP + FL pipeline for health time-series forecasting and add a user-clustering step to improve accuracy and training time (Imtiaz et al., 2020). They report that DP noise only reduces accuracy by about two percentage points, which suggests that, with enough users, DP can be added to forecasting workloads at acceptable cost (Imtiaz et al., 2020). Banse, Kreischer and Oliva i Jürgens empirically benchmark gradient-perturbation DP in federated learning and find that performance degrades most when data are highly non-IID or each client has only a small dataset, which is common for many business units (Banse, Kreischer & Oliva i Jürgens, 2024). 

Finally, Javed et al. propose ShareChain, which combines a permissioned blockchain, federated learning and local DP to let hospitals collaboratively train models without exposing raw patient data (Javed et al., 2023). The prototype shows higher privacy and accuracy than a previous blockchain baseline but also introduces extra latency and deployment complexity (Javed et al., 2023). Overall, these studies show that DP with LDP, FL and blockchain-style architectures can support enterprise-like analytics on structured data, but they also highlight non-trivial accuracy loss, system overhead and configuration effort. These open issues motivate our project to more systematically evaluate privacy–utility trade-offs for typical enterprise analytics pipelines. 

Research Gap and Motivation

Differential Privacy (DP) has achieved a mature theoretical foundation, supported by a broad array of mechanisms such as Laplace and Gaussian noise families, advanced composition theorems, and well-studied privacy–utility trade-offs. However, despite these theoretical strengths, the practical integration of DP into everyday enterprise workflows remains limited. Business analytics teams often face financial, technical, and organisational constraints—budget limitations, shortage of skilled personnel, complex compliance requirements, and dependence on legacy systems—that are rarely reflected in academic implementations or benchmark studies.

In industry, the challenge is not in designing new DP algorithms but in operationalising existing ones. Many organisations lack prescriptive frameworks to convert parameters such as ε, δ, and sensitivity into deployable configurations for machine learning tasks like classification, segmentation, or forecasting. The few large-scale deployments by technology giants and government agencies offer limited guidance to smaller or mid-tier enterprises. Empirical validation in typical business pipelines is scarce, and there is no reusable methodology linking DP settings to enterprise service-level agreements (SLAs) or performance indicators. These limitations create a persistent translation gap between DP theory and its real-world adoption, highlighting the need for practical deployment frameworks that enterprises can replicate with minimal rework.

Significance and innovation 

Significance

This research directly reflect the growing need for safe, AI-driven analytics in corporate environments. Nowadays, organisations are gradually increasing their use of machine learning for customer segmentation, forecasting, and personalisation. Differential Privacy (DP) offers a mathematical foundation for quantifiable privacy guarantees. However, practical guidance on how to embed DP into real-world AI pipelines is still limited. By developing a structured approach to integrate DP into data preparation and model-training workflows, this study enables companies to perform AI-based analytics on customer data while maintaining measurable privacy protection. The expected outcome will help corporations meet industrial standards, get more reputation and adopt responsible AI practices without hindering innovation.

Innovation

The project presents a workload-aware DP deployment platform designed for AI-driven enterprise analytics. It combines mechanism selection, sensitivity budgeting, noise accounting, and auditability into a single decision model. This model can be used to do common machine learning tasks like classification, segmentation, and recommendation. Unlike previous research, which focuses on algorithmic theory or large-scale government and tech-company deployments, this approach is intended for reusability, cheap integration costs, and operational transparency in typical corporate settings. The breakthrough lies in transforming DP from a theoretical construct to a repeatable, AI-compatible operational paradigm. This method enables businesses to collect insights ethically while guaranteeing that people' privacy is protected.

Research proposal or Methodology

This project proposes a workload-aware deployment framework for Differential Privacy (DP) in corporate analytics. Rather than creating new mathematical algorithms, the aim is to make existing DP methods usable for typical business teams who want to analyse customer data without exposing individuals. In simple terms, DP ensures that the result of an analysis would look almost the same whether or not any single person’s data is included. This is usually controlled by a privacy parameter ε (epsilon): smaller ε means stronger privacy but more noise in the results. As illustrated in Figure 1, differential privacy guarantees that analysis results look almost the same whether or not any individual is included in the dataset.

[Original figure omitted in the text edition.]

Our proposal is to design and test a practical framework that helps organisations decide when to use DP, which mechanism to choose, and how much noise to add for common analytics tasks such as customer segmentation, churn prediction and time-series forecasting. The research will build small but realistic end-to-end pipelines, add DP at appropriate stages, and measure how this affects both privacy guarantees and the accuracy that business stakeholders care about. The final outcome will be guidance and a prototype toolkit that make DP a routine engineering choice rather than a specialised research topic.

Approach

The approach has three main phases, but they are implemented as a continuous, iterative process rather than rigid steps.

First, we will characterise a small set of “typical” enterprise workloads. These will cover at least one classification task (such as predicting customer churn), one segmentation or clustering task (such as grouping customers by behaviour), and one forecasting task (such as predicting monthly demand). For each workload, we will describe the data schema, identify which attributes are sensitive, and record realistic accuracy expectations. This creates a concrete target for what our DP framework must support.

Second, we will design and implement the framework itself. At a high level, the framework will provide (i) simple design rules for mapping each workload type to a suitable DP mechanism, and (ii) a way to allocate and track the overall privacy budget across the pipeline. For example, a neural-network churn model might use DP-SGD (stochastic gradient descent with added noise), while a clustering use-case might use a local-DP method that perturbs records before they reach the server. The same framework will also include an “accounting” component that keeps track of the total ε spent as the data are queried or as the model is trained.

Third, we will empirically evaluate the framework. For each workload, we will compare non-DP baselines with several DP configurations and examine how accuracy, error rates and model stability change as privacy protection is increased. Where results are poor, we will adjust the framework’s rules; where results are acceptable, we will document the settings as recommended patterns for similar organisations.

Methodology 

We will start by preparing structured datasets that resemble corporate customer data, using either public benchmarks or carefully simulated data. Each record will include a mix of demographic attributes (such as age or region) and behavioural features (such as transaction counts). We will clearly mark which attributes are considered sensitive. For each workload, we will first train a non-DP baseline so that stakeholders can see performance without privacy protection. For classification, we will use logistic regression or a small MLP. For segmentation, we will use a tree-based method or K-prototypes. For time-series, we will use ARIMA or a shallow LSTM.

We will then integrate three families of DP mechanisms. For central DP training, we use DP-SGD with per-example clipping and Gaussian noise, and compute a final (𝜀,𝛿) using a standard privacy accountant. For teacher–student settings (PATE-style), we partition the sensitive dataset into disjoint parts to train multiple “teachers”, and train a “student” on noisy aggregated votes. For local DP scenarios, we randomise data on the client side (e.g., randomised response for categorical fields and calibrated noise for numeric fields) before any sharing.

Evaluation will use two sets of metrics. On the privacy side, we will report the overall ε and δ for each configuration, along with a short verbal explanation (for example: “under these settings, any single customer’s presence changes the output distribution only slightly”). On the utility side, we will measure standard model metrics (accuracy, F1-score, AUC, RMSE) and translate them into business-level statements where possible, such as “this DP model correctly identifies X% of high-risk customers compared with Y% without DP.” We will also explore a few realistic scenarios, such as smaller training sets, non-identically distributed data across business units, and time limits on model retraining, to see how robust the framework is outside ideal laboratory conditions.

Robustness checks include smaller training sets, non-IID partitions across business units, and time limits on training or retraining. We avoid real personal data: all experiments use public or synthetic data, code is version-controlled, and no raw records are uploaded to public AI tools. The analysis is comparative: for each workload we identify configurations that reach the target privacy budget with acceptable accuracy, and flag settings that are unlikely to work under strong privacy constraints.

Conceptual framework 

Our project is a simple pipeline with four main steps (checkpoints). It takes the raw customer data and changes it into DP-protected results.

First, at the data layer, we define exactly what we collect and determine which attributes need the strongest privacy protection. Second, the mechanism layer involves choosing where to apply Differential Privacy: during data collection (Local DP), model training (DP-SGD or PATE), or when releasing aggregate statistics. Third, the accounting layer tracks how much of the total privacy budget has been spent and ensures we stay within the agreed limits. Finally, the decision layer is where we compare the performance of the DP-protected system against the non-DP baseline to judge if the setup is "good enough" for the intended business goal.

[Original figure omitted in the text edition.]

Timeline 

The following Gantt chart summarizes the main activities of the project and their respective schedules. The project runs from the start date until the completion of the research.

Chart 1. Indicative project timeline in Gantt chart

Dissemination of results 

The results will be communicated in formats that are understandable to both technical and non-technical audiences. The main outputs will be a written report explaining the framework, its assumptions, and limitations, a small prototype implementation with configuration examples, and a set of visual summaries, such as simple charts showing how accuracy changes as ε decreases, or side-by-side comparisons of DP and non-DP models for the same workload. Organisations could utilise these materials as internal training resources for privacy-preserving analytics.

National/international benefits 

At the country level, it aids Australian organisations wishing to make use of AI without violating privacy laws or falling short of public expectation. People are already being exposed due to them just using ai tooling right now. This project translates DP from a theoretical field into an actual tool for engineering which lets people choose to do it.

Internationally, the work adds to a growing agreement that privacy-preserving approaches need to be usable by people outside of specialist teams. A workload oriented DP framework tested on realistic corporate tasks could also quite easily be taken over by companies with a different set of regulations like GDPR to do the work under such conditions. In this way, the project turns DP into “nice in theory” into “deployable in reality,” all types of corporation situations.

Conclusion

This proposal outlines a practical plan to make Differential Privacy usable in everyday corporate analytics. By focusing on a small number of representative workloads and implementing established mechanisms such as DP SGD, PATE and local DP under realistic conditions, the project has the potential to show when and how DP can protect individual customers while keeping models accurate enough for business use. The workload aware framework and its privacy budgeting rules are designed to structure the analysis of trade offs between privacy and utility in enterprise settings. The main outcomes of the study will be recommendations on suitable DP configurations for common enterprise analytics tasks and a prototype toolkit that organisations can adopt as a foundation for responsible, privacy preserving AI analytics on customer data.

Dimension

Baseline analytics (no DP)

With proposed DP framework

Privacy risk for individuals

Higher risk of re-identification from detailed analytics

Reduced risk due to DP noise and explicit privacy budgeting

Regulatory compliance and audit

Basic controls, limited formal privacy guarantees

Explicit (ε, δ) guarantees and privacy accounting logs

Model accuracy

Slightly higher raw accuracy but no formal privacy protection

Slightly reduced accuracy but expected to remain within acceptable business tolerances

Transparency for non-experts

Difficult to explain to managers and compliance staff

Clearer explanation of how privacy budgets and noise are chosen

Chart 2. Comparison of anticipated outcomes 

As Figure 2 summarised, at a high level, the anticipated shift from higher-risk but slightly more accurate analytics to privacy-preserving analytics that retain acceptable accuracy while providing explicit guarantees and auditability.

Acknowledgement

We would like to thank the teaching staff of GSOE9011 for their guidance and comments on our draft proposal, and our peers for their constructive feedback during the review process. Their suggestions helped us clarify our research focus and improve the overall structure of this proposal.

References 

1.Forbes Middle East (2023), ‘Samsung bans ChatGPT among employees after sensitive code leak’. Available at: https://www.forbesmiddleeast.com/innovation/artificial-intelligence-machine-learning/samsung-bans-chatgpt-among-employees-after-sensitive-code-leak (Accessed: 8 October 2025).

2.Farooqui, A. (2025), ‘Samsung lets employees use ChatGPT again after secret data leak in 2023’. SamMobile. Available at: https://www.sammobile.com/news/samsung-lets-employees-use-chatgpt-again-after-secret-data-leak-in-2023/ (Accessed: 8 October 2025).

3.NSW Reconstruction Authority (2025), ‘Northern Rivers Resilient Homes Program data breach’. NSW Government – Media release, 6 October. Available at: https://www.nsw.gov.au/departments-and-agencies/nsw-reconstruction-authority/media-releases/northern-rivers-resilient-homes-program-data-breach (Accessed: 8 October 2025).

4.News.com.au (2025), ‘‘Deeply sorry’: NSW contractor uploaded 3000 flood victims’ data to ChatGPT’. Available at: https://www.news.com.au/technology/online/internet/deeply-sorry-nsw-contractor-uploaded-3000-flood-victims-data-to-chatgpt/news-story/f8d0777ebea5400090be402b8314dede (Accessed: 8 October 2025).

5.Narayanan, A. and Shmatikov, V. (2008), ‘Robust De-anonymization of Large Sparse Datasets’. In: 2008 IEEE Symposium on Security and Privacy (S&P). Available at: https://www.cs.cornell.edu/~shmat/shmat_oak08netflix.pdf (Accessed: 8 October 2025).

6.Shokri, R., Stronati, M., Song, C. and Shmatikov, V. (2017), ‘Membership inference attacks against machine learning models’. In: 2017 IEEE Symposium on Security and Privacy (S&P). Available at: https://www.cs.cornell.edu/~shmat/shmat_oak17.pdf (Accessed: 8 October 2025).

7.Fredrikson, M., Jha, S. and Ristenpart, T. (2015), ‘Model inversion attacks that exploit confidence information’. In: Proceedings of the 22nd ACM SIGSAC Conference on Computer and Communications Security (CCS). Available at: https://www.cs.cmu.edu/~mfredrik/papers/fjr2015ccs.pdf (Accessed: 8 October 2025).

8.National Institute of Standards and Technology (NIST) (2025), SP 800-226: Guidelines for Evaluating Differential Privacy Guarantees. Available at: https://csrc.nist.gov/pubs/sp/800/226/final (Accessed: 8 October 2025).

9.Yuan, L., Zhang, S., Zhu, G. & Alinani, K. (2023), ‘Privacy‐preserving mechanism for mixed data clustering with local differential privacy’, Concurrency and Computation: Practice and Experience, vol. 35, no. 19. https://doi.org/10.1002/cpe.6503

10.Imtiaz, S., et al. (2020), ‘Privacy Preserving Time-Series Forecasting of User Health Data Streams’. In: 2020 IEEE International Conference on Big Data (Big Data), pp. 3428–3437.

11.Banse, A., Kreischer, J. and Oliva i Jürgens, X. (2024), ‘Federated Learning with Differential Privacy’. arXiv preprint arXiv:2402.02230 [cs.LG]. Available at: https://arxiv.org/abs/2402.02230 (Accessed: 5 November 2025).

12.Javed, L., Anjum, A., Yakubu, B.M., Iqbal, M., Moqurrab, S.A. & Srivastava, G. (2023), ‘ShareChain: Blockchain-enabled model for sharing patient data using federated learning and differential privacy’. Expert Systems, 40(5), e13131. https://doi.org/10.1111/exsy.13131

13.Dong, J, Roth, A & Su, WJ 2022, ‘Gaussian differential privacy’, Journal of the Royal Statistical Society. Series B, Statistical methodology, vol. 84, no. 1, pp. 3–37.

14.Dwork, C, McSherry, F, Nissim, K & Smith, A 2017, ‘Calibrating Noise to Sensitivity in Private Data Analysis’, The journal of privacy and confidentiality, vol. 7, no. 3, pp. 17–51.

15.Awan, J & Slavković, A 2021, ‘Structure and Sensitivity in Differential Privacy: Comparing K-Norm Mechanisms’, Journal of the American Statistical Association, vol. 116, no. 534, pp. 935–954.

16.Abadi, M. et al. (2016) ‘Deep learning with differential privacy’. In: Proceedings of the 2016 ACM SIGSAC Conference on Computer and Communications Security (CCS), pp. 308–318. ACM.

17.Papernot, N. et al. (2016) ‘Semi-supervised knowledge transfer for deep learning from private training data’. In: International Conference on Learning Representations (ICLR).

18.Fletcher, S & Islam, MdZ 2020, ‘Decision Tree Classification with Differential Privacy: A Survey’, ACM computing surveys, vol. 52, no. 4.

19.Apple Inc. (2017) ‘Learning with privacy at scale’. Apple Machine Learning Journal, 1(9). Available at:https://machinelearning.apple.com/research/learning-with-privacy-at

-scale (Accessed: 8 November 2025).

20.Erlingsson, Ú., Pihur, V. & Korolova, A. (2014) ‘RAPPOR: Randomized aggregatable privacy-preserving ordinal response’. In: Proceedings of the 21st ACM Conference on Computer and Communications Security (CCS), pp. 1054–1067. ACM, New York.

21.Kenny, CT, Kuriwaki, S, McCartan, C, Rosenman, ETR, Simko, T & Imai, K 2021, ‘The use of differential privacy for census data and its impact on redistricting: The case of the 2020 U.S. Census’, Science advances, vol. 7, no. 41.

22.Drechsler, J 2023, ‘Differential Privacy for Government Agencies-Are We There Yet?’, Journal of the American Statistical Association, vol. 118, no. 541, pp. 761–773.

23.Dwork & Roth, 2014, ‘The Algorithmic Foundations of Differential Privacy’, January 2013 Foundations and Trends® in Theoretical Computer Science 9(3)

24.McKinsey & Company (2023) The State of AI in 2023. McKinsey Global Survey.

25.IBM Security (2023) Cost of a Data Breach Report 2023. IBM Corporation.

26. Near, J., Darais, D. & Boeckl, K. 2020, Differential Privacy for Privacy-Preserving Data Analysis: An Introduction to our Blog Series, National Institute of Standards and Technology.

