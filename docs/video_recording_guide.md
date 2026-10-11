# Video Recording Guide: Part 2, Code and Reproduction

This page is the script and checklist for the second part of our presentation video. Part 1 (about 2 minutes)
presents the paper. Part 2 (about 4.5 minutes) walks through this repository and the notebook `Code.ipynb`.
Together they stay within the 7-minute limit.

| | |
|---|---|
| **Presenter** | Bhatt Dhruv Anil (2025AG05557) |
| **Length** | About 4 minutes 30 seconds |
| **Shown on screen** | This GitHub repository and `Code.ipynb`, with its outputs |

---

## Before recording

1. **Open the notebook.** Open `Code.ipynb` on GitHub, where the outputs display without running anything. Alternatively,
   open it in Jupyter after a full *Run All*.
2. **Set the zoom.** Zoom the browser to 125–150% so code and tables are readable in the recording.
3. **Show the presenter's identity.** Keep the presenter's name and BITS ID on screen throughout, for example as a caption
   or webcam overlay.
4. **Do a test run.** Record a short test clip to check that the audio is clear and the screen text is legible.

---

## Script

Every number in the script comes from the notebook's actual output.

### 1. Introduction and repository (0:00 – 0:30)

**On screen:** repository home page → scroll the README (members, purpose, findings) → folder list

> "Hi, I'm Dhruv from Group 3. In this second part, I'll walk you through the code behind our presentation.
>
> Everything is in our public GitHub repository, which is linked on the last slide. The main file is Code.ipynb. Next to
> it, the results folder has every number as CSV files, the figures folder has every chart, and author_data holds the
> author's published results, which we used to check our slides."

### 2. Algorithms (0:30 – 1:20)

**On screen:** `Code.ipynb` Section 2 (the `ucb_tuned_index` and `ucb_v_index` functions) → the Section 2.1 table → the Section 3 plot

> "Section 2 implements all eight algorithms from their original papers. Here is UCB-Tuned: its exploration bonus uses
> each arm's measured variance, capped at one quarter. UCB-V adds a Bernstein-style bonus. We used the same constants as
> the author's config file, so our numbers can be compared directly with hers.
>
> While reading the author's code, we found three places where it differs from the textbook versions: ETC,
> epsilon-greedy and UCB-Improved. We coded those as 'author variants' too, so we can reproduce her results and the
> correct versions side by side.
>
> Section 3 shows why variance-awareness helps. For a consistent arm, UCB-Tuned's bonus is about a third of plain UCB's,
> so it stops re-checking that arm sooner."

### 3. Setup and fairness (1:20 – 2:05)

**On screen:** the Section 1 variance table → the Section 4 arm-order table → the Section 5.0 output "33 of 33"

> "We set up the experiment exactly as in the paper: one million steps, 100 runs for each arm order, and a fixed seed.
> This table computes each arm's variance as p times one minus p, matching our slide 10.
>
> Section 4 shows why the fairness rules matter. With ETC and only 10 exploration pulls, simply swapping which arm is
> listed first changes the regret from about 2,000 to 2,900. That's why we always run both orders.
>
> In Section 5.0, the notebook downloads the author's result files from her GitHub and checks every number on our
> slides. All 33 match."

### 4. Reproduction results (2:05 – 2:55)

**On screen:** the Section 5.3 figure (`figures/reproduction_vs_author.png`) → the Section 5.2 tables

> "Next, we re-ran the whole experiment with our own simulator. The bars are our results, and the black ticks are the
> author's published values.
>
> In Scenario A, tuned ETC gives 10.3 against her 10.1. In Scenario B, UCB-Tuned gives 211 against 212, and plain UCB
> gives 1,133 against 1,127. In Scenario C, UCB-Tuned gives 217 against 226. Across all 16 settings, our ranking matches
> hers, with a rank correlation of at least 0.97.
>
> In our fair version, with textbook algorithms and both arm orders, UCB-Tuned is about five times better than plain UCB
> in Scenarios B and C."

### 5. What the code revealed (2:55 – 3:50)

**On screen:** the Section 6.1 ETC table → the Section 5.5 UCB-Improved table

> "The code also showed us two things the paper doesn't mention.
>
> First, ETC. The author's version keeps re-checking the averages after exploring, so it never truly commits to an arm.
> With a real commit, theory says ETC picks the wrong arm about 12 percent of the time in Scenario B, and we measured 13
> percent. The paper's value of 150 implies only 2 percent. So 'tuned ETC is best' depends on that implementation choice.
> With a fair setup, UCB-Tuned is the best algorithm in all three scenarios.
>
> Second, UCB-Improved. In the author's own file, 66 of 100 runs end with a regret below 2.8. That's impossible if each
> arm gets the required 28 pulls in the first phase, so its huge regret of 32,500 comes from a bug."

### 6. Extension and close (3:50 – 4:30)

**On screen:** the Section 7 plot (`figures/extension_variance_sweep.png`) → the Section 8 summary → repository home page

> "Finally, the future work we proposed in Part 1. With 0/1 rewards, the variance is fixed by the win rate. So we used
> Beta rewards instead, which stay between 0 and 1 as these algorithms assume. That lets us change the noise while
> keeping the gap the same. Plain UCB stays flat at about 450, while UCB-V's regret rises four times as the noise grows.
> That is the variance effect the paper couldn't isolate.
>
> To sum up: we verified every number on our slides, reproduced the experiment independently, and showed where
> implementation choices change the conclusions. All the code, results and figures are in our repository. Thank you."

---

## After recording

1. **Join the two parts.** Add Part 2 after the existing Part 1 video. The combined video must not exceed 7 minutes.
2. **Name the file.** Export it as `DRL_A_Framework_for_Fair_Evaluation_of_Variance_Aware_Bandit_Algorithms_Group_03`.
3. **Upload and share.** Upload it to Google Drive and set sharing to *Anyone with the link can view*.
4. **Test the link.** Open the link in a private or incognito browser window to confirm it plays without signing in.
5. **Add the link to the deck.** Paste the link on the last slide of the presentation (slide 28).
6. **Submit once.** Only one submission is allowed, so check the PPT and video before uploading.
