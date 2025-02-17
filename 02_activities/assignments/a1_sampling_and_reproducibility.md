# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 100 (from the original 1000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Marta Zaiats

```
Please write your explanation here...
Initial Population. The ppl DataFrame is created to represent 1000 individuals attending events: 200 individuals attend weddings and 800 individuals attend brunches.
Infection Sampling. The function np.random.choice() is used to randomly select indices from the population without replacement. Sampling Frame: All 1000 individuals in the ppl DataFrame. Sample Size: Determined by len(ppl) * ATTACK_RATE, which results in 100 infections (10% of 1000). 
Primary Contact Tracing Sampling. The function np.random.rand() generates random values between 0 and 1 for each infected individual. If the value is less than TRACE_SUCCESS, the individual is marked as traced (ppl['traced'] = True). Sampling Frame: Only infected individuals (sum(ppl['infected']) = 100). Sample Size: Determined by a Bernoulli trial with success probability TRACE_SUCCESS. On average, 20% of infected individuals (20 out of 100) are traced.
Secondary Contact Tracing Sampling. Secondary tracing is not a random sampling step. Instead, it is a deterministic rule:
o	Count how many infected individuals in each event were traced in the primary stage.
o	If that event reached the SECONDARY_TRACE_THRESHOLD (≥2 traced infections), all infected people in that event are traced.
This approach is entirely consistent with the procedure described in the article.
The entire process described above is repeated 1000 times. By running it 1000 times, we get a distribution of outcomes (e.g., proportion of infections from weddings, proportion traced to weddings), which we visualize in the histograms. These histograms demonstrate that the observed proportion is biased upwards due to secondary contact tracing. A blue histogram shows the actual proportion of infections from weddings centered around 20%. A red histogram showing the observed proportion of traced cases from weddings shifted higher.
With 100 runs, each simulation point (i.e., each proportion of wedding-linked infections and wedding-linked traces) carries more weight in the final distribution. As a result, the histogram may look “noisier” or less smooth compared to using 1000 runs.
If we do not fix the random seed before each run, then each script execution will sample from a new set of random numbers. Consequently, each run can produce a different shape in the histograms.
To make the script reproducible, we need to ensure that the random number generation produces the same results every time the script is run. This can be achieved by setting a fixed random seed using NumPy's np.random.seed() function. 


```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `23:59 - 16/02/2025`
* The branch name for your repo should be: `assignment-1`
* What to submit for this assignment:
    * This markdown file (a1_sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-1`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via the help channel in Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
