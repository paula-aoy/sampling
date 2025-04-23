# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 100 (from the original 1000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple time

# Author: Paula Akemi Aoyagui

```
Please write your explanation here...
1. Population sampling (# Create DataFrame for people at events with initial infection and traced status)
The goal of the first sampling stage in whitby_covid_tracing is to determine population, in this case 200 wedding people and 800 brunch people (sample size = 1000 people). 
The sampling procedure uses list multiplication and concatenation (events = ['wedding'] * 200 + ['brunch'] * 800).
The sampling frame is predetermined by the two event types and both have fixed numbers of attendees.
Distribution is deterministic since it has a fixed population sample (no random sampling), and it is similar to the example described in the blog post.

2. Infection sampling (# Infect a random subset of people)
The goal now is to infect a subset of the population with a 10% attack rate (ATTACK_RATE) was employed. 
The sample size remains 1000 people as before, with approximately 100 individuals (10% of the population) expected to become infected in each simulation (ATTACK_RATE).
The sampling procedure randomly selects the 100 people to be infected fom the population sample (np.random.choice()). Specifically, each person can be infected only once. 
The sampling frame is the entire sample population (200 wedding + 800 brunch attendees). 
Unlike the previous sampling procedure, here there is randomization, so each person in the population has equal chances of being infected. This would be more similar to how randomly COVID can spread in real-world settings.

3. Primary contact tracing sampling (# Primary contact tracing: randomly decide which infected people get traced)
Here we select people infected to be traced randomly (np.random.rand()). Since the trace success parameter is set at 0.20 as a constant (TRACE_SUCCESS = 0.20), that means 20% of the infected population will be randomly selected for contact tracing in this simulation. This doesn't seem to be a very thorough method, as randomly sampling 20% infected people for contact tracing means there's a chance the other 80% of infected people were potentially in high-spreading situations, putting more people at risk. 

4. Secondary contact tracing sampling (# Secondary contact tracing based on event attendance)
After the previous, step, this sampling will build upon it.
Now the goal is to track the events with traced cases in the previous step.
Sampling starts counting how many infected people were in each event to identify the events with two or more cases as the (SECONDARY_TRACE_THRESHOLD = 2) was set for 2.
Once those events are identified in the sample (events_traced), it's time to identify the people in these events who were infected.
This secondary step, combined with the first that was random, makes the process more thorough because it screens events with more than two infected people and initiates targeted contact tracing for people who were in these events.

When I ran the whitby_covid script, the generated graph looks visually different from the blog post. Then looking at it closer, at the true infections x traced it was possible to note the blog and the python graphs show overlap (red and blue). Then looking at the x-axis, I noticed the scales were different, but in both graphs the overlap was in the 0.2 range. The sampling bias is more clearly noticeable in the blogpost.

After changing the number of simulations to 100, I noticed there was variation in the graphs. Some consistency in the blue and red bars, in the x axis spread (0.1 to 0.3) and more variability in y axis (above 40 and sometimes above 50). The overlaps between infections and traced also changed slightly every time I ran it. The random seed is not controlled, at every run different samples are selected and results are not reproducible.

I set up a random seed at 13 (seed=13) to "control" randomnes and get more stable outputs, to make the sampling reproducible. This ensures every time the script runs, it will randomly select the same datapoints. Now the graphs are always generated the same. I couldn't remember from class what is the industry standard for seed, but I looked up that it can be any random number and consistency will still be applied, so I set it for 13.

```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `23:59 - 09/04/2025`
* The branch name for your repo should be: `assignment-1`
* What to submit for this assignment:
    * This markdown file (a1_sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [X] Create a branch called `assignment-1`.
- [X] Ensure that the repository is public.
- [X] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [X] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via the help channel in Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
