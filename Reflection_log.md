# Individual Reflection Log

**Module:** DATA2007 Group Project (Training phase, TAG Fitbit task)  
**Team repo:** DATA2007-Fitbit-Dashboard-Group

---

## Week 4 (w/c 12 October 2026)

**My branch:** `Zoe_EDA`
**Files allocated to me:** `minuteSleep_merged.csv`, `weightLogInfo_merged.csv`, `minuteStepsWide_merged.csv`

### What I did this week

#### Learning
- Watched the Software Requirements video. Learned the difference between functional requirements (what the system does) and non-functional requirements (constraints like performance, usability and privacy).
- Learned the five EARS templates (ubiquitous, When, If...then, While, Where) and practised rewriting badly written requirements. For example, I rewrote "users get told about overdue books" into an event-driven requirement, and learned that "has an overdue book" is a state (While), not an event (When).
- Learned the seven common requirement pitfalls from the video: vagueness, untestable, overly complex, omission, duplication, wordiness and implementation bias.

#### Team sessions
- Took part in the Crazy 8s session, which generated 10 feature ideas grouped into five themes: personal goals, social and competition, gamification, insights and health estimates.
- Agreed this week's team tasks: investigate Streamlit vs Power BI, write at least 5 requirements each, and continue exploring the data.

#### GitHub setup
- Cloned the team repo using GitHub Desktop and created and published my own branch, `Zoe_EDA`.
- Set up a notebook in the `exploration` folder, using a `DATA_DIR` variable and relative paths so it runs on anyone's machine.
- Made small, regular commits for each stage of the exploration (first look, sleep, weight, steps wide).

#### Data exploration
- Ran quality checks on all three files: shape, distinct users, duplicates, missing values and data types.
- Converted the text timestamps to datetimes, removed duplicates, grouped sleep minutes into sessions, derived height from BMI, and reshaped the wide steps file into long format so I could compare it with `minuteStepsNarrow`.
- Wrote up findings and implications for the dashboard under each section of the notebook.
- Shared a summary of my findings with the team on Teams and tagged Daniel about the wide vs narrow issue, as it affects his file.

### Key findings

**minuteSleep**
- Only 24 of 33 users have sleep data. Sessions per user range from 1 to 39, so coverage is very uneven.
- 543 exact duplicate rows, which I removed (188,521 → 187,978 rows).
- 459 sleep sessions in total, with a median of 7.4 hours. 41 sessions are under 2 hours, so they're probably naps and should be separated from main sleep.
- Sleep starting after midnight belongs to the previous night, which matters if we match sleep to the next day's activity.

**weightLogInfo**
- Only 8 users logged weight (67 entries), and 54 of those entries come from just 2 people.
- `Fat` is 97% empty (65 of 67 missing). `WeightPounds` is just kg converted, and `LogId` is the date stored as a timestamp, so all three can be dropped.
- Height derived from BMI is constant for each user, which shows BMI is calculated from a fixed profile height.
- Weight isn't suitable for comparing the group. It's also sensitive personal data, so I suggested keeping it private to each user. This could become a privacy requirement.

**minuteStepsWide**
- Each row is one user for one hour, with 60 minute columns. Reshaping it gave 1,298,700 rows in long format.
- The step counts match `minuteStepsNarrow` 100% where they overlap (1,278,060 minutes), but neither file is complete. Narrow has 12 April and wide doesn't, and wide has 20,640 extra minutes in May. I recommended using narrow as the main source.

**Dates**
- The date ranges don't match across my files (sleep starts on the evening of 11 April, steps wide runs from 13 April to 13 May, weight from 12 April to 12 May), so the team needs to agree one date range for the dashboard.

### Problems I spotted

- Our `.gitignore` is a generic Python template and doesn't cover `datafolder`, so the CSVs showed up ready to commit. I unticked them, kept them out of my commits, and raised it with the team, suggesting we add `datafolder/*.csv`.
- Found a `master` branch alongside `main` that was 1 commit ahead and 6 behind, and flagged it as potentially confusing.

### Teamwork

- I couldn't attend our first meeting due to an emergency. I let the team know, caught up afterwards, and picked up my allocated files.
- I shared my findings as actions the team could use (which file to use, what to drop, what to agree on) rather than just raw numbers.
- The team worked well together from the start. Everyone was understanding when I couldn't make the first meeting, and the Crazy 8s session gave everyone a chance to share ideas.

### What was difficult
- Catching up after missing the first meeting due to an emergency. I had to work out what had been decided and which files were mine without having been in the discussion.
- Understanding the `.gitignore`. When I first added the data, the CSVs showed up ready to commit, and I didn't understand why, because I assumed the template would already ignore them. It took a while to realise it was a generic Python template that knew nothing about our data folder.
- Getting used to GitHub Desktop and branches, especially knowing which files to tick and untick when committing.
- Small errors that held me up, like pasting notes into a code cell instead of a markdown cell, and a `FileNotFoundError` because I hadn't downloaded a file my code needed.

### Reflection
- **What happened:** I explored my three files and found more problems than I expected: duplicate sleep rows, very uneven logging, a nearly empty column and date ranges that didn't line up.
- **Why it mattered:** These issues would have quietly distorted the dashboard. For example, people with little sleep data would have looked like poor sleepers, and weight averages would really only have reflected two people. It showed me that exploring the data first shapes what the dashboard should and shouldn't show.
- **What I'd do differently:** I'd check how the repo and `.gitignore` are set up before adding any data, rather than finding out when files appear ready to commit. I also learned to only share findings I can back up with my own outputs, after removing a claim my files didn't support.

### KSBs evidenced
- **K5.2 Data formats and structures:** compared the wide and narrow minute steps files and reshaped wide data into long format with `pd.melt()`.
- **K5.3 Common patterns in real-world data:** found duplicate sleep rows, uneven sleep logging, a nearly empty `Fat` column and mismatched date ranges across files.
- **S2 Data engineering:** profiled three datasets (shape, users, duplicates, missing values), converted dates, removed duplicates and grouped sleep minutes into sessions.
- **S1 Gathering requirements:** learned to write requirements in EARS and practised turning vague statements into testable ones.
- **K4.5 Source control:** set up my own branch in the team repo, made small regular commits, and kept raw data out of Git.
- **K2 Ethics and data privacy:** identified weight and BMI as sensitive data that shouldn't be shown to other users.
- **B5 Integrity in presenting data:** only shared findings I could back up with my own outputs, and removed a claim my files didn't support.
- **B3 Adaptability:** caught up quickly after missing the first meeting due to an emergency.

### Use of AI

- **Learning:** I used Claude to explain EARS and practise writing requirements (it gave feedback on my attempts), and to explain how `.gitignore`, branches, and "ahead/behind" work in GitHub.
- **Code:** Claude generated the starting code for my exploration notebook. I ran it on my own files, adapted it to my folder structure, and used Claude to help debug errors (a `FileNotFoundError` from a missing file, and a `SyntaxError` from pasting markdown into a code cell).
- **Notes:** Claude helped turn my outputs into findings notes and the Teams summary. I checked each point against my own outputs and rewrote the summary in my own style. When Claude suggested the last day of data was only partly recorded, I checked whether my own files supported it. They didn't, so I removed the claim and only kept findings I could back up.


### Plan for next week

- [x] Investigate Streamlit vs Power BI and post my time estimate
- [x] Write at least 5 requirements in EARS, based on my files (no sleep data, naps vs main sleep, weight privacy, narrow vs wide steps, agreed date range)
- [x] Open a pull request for my EDA work and get it reviewed
- [x] Trial a feature from my files (e.g. a sleep chart with naps separated), linked to a requirement
- [x] Help finalise requirements and decide on the dashboard tool

---

## Week 5 (w/c 19 October 2026)

**My branch:** `Zoe_EDA`

### What I did this week

#### Requirements
- Worked from the Crazy 8s feature list (10 ideas grouped into goals, social, gamification, insights and health estimates) to write my requirements in EARS.
- Started with requirements based on my own files (missing sleep data, naps, weight privacy, date ranges), then made them more general to the dashboard as a whole.
- Some of my ideas had already been posted by teammates (monthly calendar, missing data, weekday vs weekend), so I swapped them for ones that weren't covered.
- Posted my final 5 requirements to the team on Teams:
  1. **Daily goals:** When a user sets a daily step goal, the dashboard shall show whether the goal was met for each day in the selected date range.
  2. **Step streaks:** The dashboard shall display each user's current streak of consecutive days at or above 10,000 steps.
  3. **Behaviour profile:** The dashboard shall display whether each user is in the top or bottom half of the group for average daily steps.
  4. **Weight privacy:** The dashboard shall not display any user's weight or BMI on group views, including the leaderboard.
  5. **Speed:** When a user changes a filter, the dashboard shall update all charts within 3 seconds.

#### Dashboard tool
- Compared Streamlit and Power BI. Streamlit fitted better because the brief needs a web front end, it works with GitHub and Python unit tests, and each person can work on their own file.
- The team agreed to use Streamlit.

#### GitHub
- Fixed our `.gitignore`, which didn't cover the data folder, by adding `datafolder/*.csv` so raw CSVs don't get committed.
- Opened a pull request from `Zoe_EDA` into `main` with my exploration notebook and the `.gitignore` fix. Checked that no CSVs were included before submitting.
- Learned what "ahead" and "behind" mean on a branch after spotting a `master` branch alongside `main`.

### What I learned
- Writing requirements that are general enough for the whole dashboard but still testable.
- Keeping requirements tool-independent. None of mine mention Streamlit, which avoids implementation bias.
- How to open and describe a pull request, and why work goes through PRs rather than straight to `main`.

### Teamwork
- I checked what teammates had already posted and changed my requirements to avoid duplicates.
- The requirements discussion and the choice of Streamlit went smoothly. Everyone shared their ideas and we agreed on a direction without any problems.

### What was difficult
- Writing requirements that worked for the whole dashboard. My first drafts were very specific to my own files, and I worried they sounded odd out of context.
- Avoiding overlap. Several of my ideas, such as the monthly calendar, missing data and weekday vs weekend, had already been posted by teammates, so I had to keep rethinking which ones to put forward.
- Understanding Git terms like "ahead" and "behind" when I spotted a stray `master` branch.

### Reflection
- **What happened:** My first requirements were very specific to my own data, and several of my more general ideas had already been posted by teammates.
- **Why it mattered:** It made me realise that good requirements describe what the user needs, not just the data issues I'd found. My weight privacy requirement is a good example of both, since it came from my exploration but protects users across the whole dashboard.
- **What I'd do differently:** I'd check the team's shared list before drafting, to avoid duplicate work, and keep a mix of general requirements and ones that come from data quality issues.

### KSBs evidenced
- **S1 Gathering requirements with colleagues:** wrote and posted 5 requirements in EARS, adjusting them to avoid overlap with teammates' requirements.
- **S5 Comparing pros and cons of tools:** compared Streamlit and Power BI for our project (web front end, version control, testing, teamwork).
- **K4.5 Source control:** fixed the `.gitignore` and opened my first pull request for review rather than pushing to `main`.
- **K2 Data privacy:** wrote a requirement to keep weight and BMI off group views.
- **S7 Collaborative relationships:** checked teammates' work before posting so our requirements complemented each other.

### Use of AI
- I used Claude to give feedback on my EARS requirements and to suggest alternatives when mine overlapped with teammates'. I chose the final 5 to post.


### Plan for next week
- [x] Review my teammates' Streamlit code and plan how my features fit in
- [x] Set up a virtual environment from `requirements.txt`
- [x] Build my features: streaks and the friends leaderboard

---

## Week 6 (w/c 26 October 2026)

**My branch:** `feature/leaderboard`

### What I did this week

#### Reviewing the team's code
- Read through my teammates' code (`load_data.py`, `behavior_insights.py`, `wellness_tracker.py`, `drill_down_function.py` and `app.py`) to understand how the dashboard is structured before adding my part.
- Learned that the dashboard shows one user's view, so my leaderboard needed everyone's data but had to be shown from that user's point of view.

#### Setting up
- Created a new branch, `feature/leaderboard`, from an up-to-date `main` instead of reusing my EDA branch.
- Set up a virtual environment (`team_venv`) and installed the team's `requirements.txt`, so I'm using the same package versions as everyone else.
- Moved the CSVs into the data subfolder the code expects (`datafolder/FitBit Dataset-20261009/`).

#### My features
- Added `src/gamification.py` with two features:
  - **Streaks (R04):** current and longest streak for steps, active minutes and sleep, plus a calendar strip showing goal hit, missed or no data.
  - **Friends leaderboard (R05, R06, R08):** ranks the user against everyone else by average steps or active minutes for the selected period.
- Design decisions:
  - A day with no data breaks a streak, so not wearing the tracker can't count as hitting a goal.
  - You need at least 7 days of data to be ranked, so a couple of good days can't top the board.
  - Other users show as "Friend 1, 2..." instead of their IDs, and sleep and weight can't be ranked, for privacy.
- Added `load_all_metrics()` to `load_data.py` so the leaderboard uses the same cleaning as the rest of the dashboard.
- Wrote 12 unit tests in `tests/test_gamification.py` (e.g. no-data days breaking streaks, people with too few days not being ranked, sleep being refused on the leaderboard). All 12 pass.
- Added my sections to `app.py`, formatted the code with `black`, and added `pytest` to `requirements.txt`.

#### Problems I found
- **Goal drill-down uses everyone's data:** it loads its own CSVs and doesn't filter to one user, so it was using 919 rows from 33 users instead of 30 days for one person.
- **Wellness tracker changes shared data:** it fills missing values with 0 on the shared `df`, which made sleep days with no data look like missed goals in later sections (my streaks first showed 2/30 sleep days instead of 2/25). My section now reloads its own copy, and I raised the fix with the team.
- **Inconsistent goals:** the calorie goal is 600 on the rings and 2,000 in the drill-down.

#### Requirements document
- Turned the team's requirements spreadsheet into `software_requirements.md` for the repo.
- Spotted that R04 (streaks) is assigned to Sam, so I flagged the overlap with my work, as well as some outdated IDs in the flags table and that heart rate data does exist for 14 users.

### What I learned
- How a larger Streamlit app is organised, with each feature as a data function and a chart function.
- What a virtual environment is for and how to set one up on Windows.
- Writing unit tests with `pytest`, and why testing with small made-up data is useful.
- How changing a shared variable in one place can break other people's sections.

### Teamwork
- I reported the bugs I found with evidence, so the owners could fix them in their own files.
- I checked before adding a function to a teammate's file.
- The team was easy to work with. When I raised the bugs, people were open to the feedback, and everyone was accommodating when changes affected each other's files.

### What was difficult
- Understanding my teammates' code before adding to it. There were several files written in different styles, and I had to work out how the data flowed between them.
n,..- Setting up the virtual environment. At first I didn't understand what the `team_venv` folder was for or why it didn't exist yet, until I realised each person creates their own.
- Getting the data in the right place. The code expected the CSVs in a subfolder, which also meant my `.gitignore` rule no longer covered them.
- My section not showing on the page at first, which turned out to be because the function wasn't being called at the bottom of `app.py`.
- Working out when to rebase and when to just create a new branch from `main`.

### Reflection
- **What happened:** I added my features to code that other people had written, and while testing found that two sections were using the data in ways that gave wrong results.
- **Why it mattered:** Each section worked on its own, but they didn't fit together. One section used every user's data instead of one person's, and another changed the shared data in a way that affected everything after it. This showed me that a team project needs agreed rules, like one shared way of loading data and one set of goals, not just working code from each person.
- **What I'd do differently:** I'd suggest agreeing a shared structure and settings at the start, and reviewing each other's code before merging, so problems like these are caught earlier. I'd also like to write more of the code myself next time, using AI to review it rather than to write the first version, so I build my own confidence.

### KSBs evidenced
- **S3 Programming and code quality:** integrated and tested the streaks and leaderboard features in Python with Plotly and Streamlit (starting code generated with AI, see below), formatted with `black`, using a virtual environment and the team's `requirements.txt` for reproducibility.
- **K4.5 Testing and source control:** added 12 unit tests with `pytest`, worked on a feature branch and submitted through a pull request.
- **S5 Architecture:** fitted my features into the team's existing structure (shared loader, data and chart functions, sections in `app.py`).
- **S2 Data governance:** used the shared cleaning, treated non-wear days as no data, and kept the leaderboard to steps and active minutes only.
- **K2 and B2 Ethics and privacy:** anonymised friends on the leaderboard and blocked sleep and weight from being ranked.
- **B5 Rigorous analysis:** found two bugs in shared code (data from all users in the drill-down, and missing values being filled with zeros) and backed them up with evidence before raising them.
- **S7 Collaboration:** raised issues constructively and checked before changing a teammate's file.

### Use of AI
- I used Claude to generate the starting code for `gamification.py` and its tests. I then integrated it into the team's `app.py` and `load_data.py`, set up the environment and data to run it, debugged why my section wasn't appearing, ran the tests and dashboard myself, and asked for changes so the code and docstrings fitted our project.
-./- I also used it to explain virtual environments and Git, and to convert the requirements spreadsheet to Markdown.

### Plan for next week
- [x] Get my pull request reviewed and merged
- [x] Check the bugs have been fixed before the demo
- [x] Prepare my part of the retrospective video

---

## Week 7 (w/c 2 November 2026)

### What I did this week

#### Final dashboard
- Switched to `main`, pulled the latest code, and ran the final dashboard from my virtual environment.
- Checked the final `app.py` and found:
  - The VO2 max and walking analysis sections (R28, R16) were imported but never called, so they wouldn't appear on the page.
  - The user we demo (1503960366) has no heart rate data, so VO2 max shows as unavailable for them. It does work for users with heart rate data.
  - These sections need the large minute-level files and take a few seconds to load the first time.
- Raised these with the team before recording.

#### Retrospective video
- Wrote and practised a 1-minute script for the dashboard demo.
- My part came after the requirements section and before insights, so I told it as a user's journey through the dashboard, from logging in and choosing a date range to streaks and the leaderboard.
- I linked back to our requirements without reading out IDs, and avoided giving specific results so I didn't overlap with the insights section.

### What I learned
- How to check a final version from `main` without changing anything.
- How to plan a presentation as part of a team, so each section hands over to the next.
- Looking back at the whole project helped me see how much the early data exploration shaped the decisions we made later.

### Teamwork
- I planned my script around what the speakers before and after me were covering.
- The recording went well. Everyone was flexible about how we split the video, and there were no issues working together.

### What was difficult
- Fitting the whole dashboard into one minute without rushing.
- Not overlapping with the speakers either side of me. The person before me covered requirements and the person after covered insights, so I had to describe what each section does without giving away specific findings.
- Last-minute changes to the final app, like new sections being imported but not shown, and the demo user having no heart rate data, so I had to check everything again before recording.

### Reflection on the training phase
- **What went well:** Teamwork was good throughout. There were no real issues, and everyone was accommodating, including when I missed the first meeting. Splitting the dashboard into sections meant everyone owned a clear part, and we delivered a working dashboard with every member's features in it.
- **What could have gone better:** Because we each built our sections separately, some things didn't line up at the end, such as different goals in different sections and one section loading its own data. We also agreed some shared decisions later than we should have.
- **What I'll take into January:** Agree the code structure, shared settings and data cleaning rules in the first week; review each other's pull requests before merging; and keep a record of decisions so we can explain them in the final video. Personally, I want to write more of my own code and rely on AI less, so I can show more of my own skills in my portfolio.

### Reflection
- **What happened:** I presented the whole dashboard in one minute, between the requirements and insights sections.
- **Why it mattered:** Thinking about the dashboard from a user's point of view helped me see how the separate features connect, and planning around the other speakers made the video feel like one piece of work.
- **What I'd do differently:** I'd check the final app earlier, so last-minute changes, like sections being imported but not shown, could be fixed before recording rather than worked around.

### KSBs evidenced
- **S6 Communicating outputs:** wrote and delivered a demo script that told the dashboard as a user's journey, tailored to fit between the requirements and insights sections.
- **S8 Project delivery:** checked the final version on `main` before recording and flagged issues (sections not being called, and a user with no heart rate data) to the team in time.
- **S7 Collaboration:** planned my part around the speakers before and after me so the video flowed.
- **B3 Adaptability:** adjusted my script as the final dashboard changed.

### Use of AI
- I used Claude to help write and refine my demo script, and to check the final `app.py` and `derived_features.py` before recording.
- I adapted the script to fit what my teammates were saying, and checked the dashboard myself before recording.

### Plan for next week
- [ ] Peer review the other groups' videos
- [ ] Write down lessons to take into the January project

---

## Week 8 (w/c 9 November 2026)

### What I did this week
- Watched other groups' retrospective videos and wrote a peer review of five of them: Group 3, JAYLS, Group 7, Group 9 and Group 8.
- Compared their approaches with the decisions our team made, using what I learned from my own data exploration and features.

### Peer review of other groups' videos

#### Group 3
**Strengths:** Their Power BI dashboard was simple and easy to navigate, with filters for user, date range and weekday at the top and separate pages for steps, calories, activity and sleep. The steps vs calories scatter plot and their correlation analysis gave a different angle on the data compared with the usual averages.

**Could improve:** The line charts all drop sharply at the end of the period, which is likely because the last day was only partly recorded. Removing it during cleaning would stop it looking like activity collapsed. The headline cards also showed totals across all users, such as 7M steps, which are hard to interpret; daily or per-user averages would be more meaningful. The video could have explained more about how they made their design choices.

**Lesson for our project:** We removed the partial last day in our cleaning for exactly this reason, so it was good to see the impact of that decision. In January I want us to explain choices like this in our video, rather than only showing the final dashboard.

#### JAYLS
**Strengths:** They were one of the most organised teams. They agreed ways of working in their first meeting, ran recurring Teams meetings, and used a GitHub project board with agenda cards that recorded each week's discussion points, outcomes and key decisions, which gave them a clear audit trail. Individual task cards had assignees, completion criteria and labels showing whether a task was individual or group work. Their repo was well structured, with separate folders for documentation, source code and tests. In their EDA, everyone used the same set of helper functions, and they made a sensible decision to focus their MVP on the daily and hourly data because the minute and second files were too large. Their Streamlit dashboard had useful filters, including an option to exclude zero-step days, and a ranked chart comparing everyone's average daily steps.

**Could improve:** Their findings slide said there were no missing values or duplicates, but some files do have gaps and duplicates, such as the almost empty Fat column in the weight data and duplicate rows in the minute-level sleep data. Their zero-step filter suggests they found the non-wear issue later, so it would have been good to update the findings to match. Their daily steps chart also drops sharply on the final day, which is likely the partly recorded last day. The video ran to just over 7 minutes, longer than the 6 minutes asked for.

**Lesson for our project:** Using GitHub issues as meeting agendas, with decisions recorded under each one, is something I want our team to do in January. It would have helped us agree things like shared goals earlier. Their ranked steps chart is close to my leaderboard, and seeing it made me think we should present the reasoning behind design choices, like excluding non-wear days, more clearly in our own video.

#### Group 7
**Strengths:** Group 7 started by running k-means clustering on heart rate against time of day, which picked out groups for night, daytime and evening, plus a separate cluster of high readings that likely shows exercise. This was a creative way to understand the data before designing anything. Their "BitFit Awards" for calories, sleep, heart rate and steps made the findings fun and memorable, and giving users made-up names instead of IDs added personality while keeping them anonymous. Their code only gave awards to users with at least 5 active days, which keeps the awards fair. They explained how Shiny's UI and server work together with a clear diagram, and their project had a tidy structure, with a separate file for data preparation that the app loads from. The video was concise at about four and a half minutes.

**Could improve:** Three of the four clusters mostly split the day into time bands, which is expected when time is one of only two inputs, so the clustering added less than it first seemed. The Calorie Award is likely to favour larger users rather than more active ones, since daily calories depend heavily on body size. Steps or active minutes would be fairer measures. The "31 days of data" also suggests the partly recorded final day was included.

**Lesson for our project:** Their awards are very similar to my leaderboard, and both use a minimum number of days to keep things fair. We chose not to rank calories for the same body-size reason, so it was useful to see the effect of that choice. Their made-up names were friendlier than our "Friend 1, 2..." labels, and having one data preparation file that the whole app uses would have avoided the bug we found where one section loaded its own data.

#### Group 9
**Strengths:** Group 9's Power BI dashboard had a clean, consistent design, with four tabs for overview, activity, sleep and weight, and a date and user filter on every page. Their headline figures used daily averages, which are easier to interpret than totals. The steps by day and hour heatmap was particularly effective at showing when people are most active, and the sleep page added a useful efficiency measure comparing time asleep with time in bed. Showing whether each weight entry was entered manually was a thoughtful detail.

**Could improve:** The weight page lists user IDs next to their weight and BMI, which raises privacy concerns, as this is sensitive personal data. Average BMI is also shown as a headline figure even though only 8 users logged weight and most entries come from two people, so it doesn't represent the group. The weight-by-date chart mixes different users on different days, so its spikes reflect who logged that day rather than real changes in weight. The "Top 3 peak hours" table is entirely one user, and the steps and calories charts drop sharply on the final, partly recorded day. Sleep would be easier to read in hours than as 419.47 minutes. The video ran to about 8 minutes, over the 6-minute limit.

**Lesson for our project:** In my exploration I found the same weight problems: very few users and mostly manual entries. That's why we kept weight off group views and added a privacy requirement. Seeing the weight table here confirmed that was the right decision. Their heatmap is something I'd like to try in January, as it shows a lot of information in a small space.

#### Group 8
**Strengths:** Group 8 built a Streamlit dashboard with separate pages for activity, steps, heart rate, sleep and weight. Their filters were the most flexible I saw: users could pick several people, set a "focus user", choose a date range, switch between daily and hourly views and reset everything with one button, with small help icons explaining the less obvious options. They clearly dealt with data quality, for example showing average steps per worn day so days without the tracker didn't drag the average down, and showing "-" for the weight and steps correlation when there wasn't enough data instead of a misleading number. Shortened user IDs, an export to CSV option and a light or dark theme were nice finishing touches. The video kept close to the time limit.

**Could improve:** Shortened IDs are still the real IDs, so users aren't fully anonymous. Putting steps, calories and intensity on one chart scaled from 0 to 1 saves space, but you have to hover to see the real values, which makes the chart harder to read at a glance. The weight page showed a chart based on just two entries, which says very little.

**Lesson for our project:** Their "per worn day" average deals with non-wear days in a way users can understand straight away. I'd like our January dashboard to label figures like this so it's clear what was excluded. The reset button and the daily or hourly switch are also simple features I'd add.

#### Overall lessons for the January project

- **Clean the data consistently, and say what was removed.** Three groups' charts dropped sharply on the partly recorded last day. One shared cleaning step, with clear labels like "per worn day", avoids misleading results.
- **Treat health data carefully.** Weight, BMI and user IDs appeared in group views in some dashboards. Anonymous names and keeping sensitive data to the user's own page should be in our requirements from the start.
- **Organise the team from week one.** The best-run teams used GitHub issues for agendas, decisions and task ownership, and agreed a shared code structure early.
- **Explain the "why" in the video.** The strongest videos explained their design decisions and data quality fixes, not just the finished dashboard, and kept to time.

### What was difficult
- Judging other teams' work fairly from a single video, especially when I couldn't see everything they had built.
- Writing feedback that was honest about problems, like privacy issues or misleading charts, while still being constructive.
- Turning each review into a lesson for our own January project rather than just describing what they did.

### Reflection
- **What happened:** I reviewed five other teams' videos and compared their choices with ours.
- **Why it mattered:** Several teams had the same issues we had worked through, like the partly recorded last day and sensitive weight data, which reassured me that our cleaning and privacy decisions were the right ones. Other teams also did things better than us, especially in how they organised their work and explained their decisions.
- **What I'd do differently:** In January I want us to use GitHub issues for meeting agendas and decisions, agree shared settings early, and explain the reasoning behind our design choices in the final video, not just show the result.

### KSBs evidenced
- **B6 Personal development and learning from the data science community:** reviewed five other teams' approaches and identified lessons for our January project.
- **S5 Comparing tools and techniques:** compared how teams used Power BI, R Shiny and Streamlit, and techniques like k-means clustering.
- **S6 Evaluating visualisation:** assessed which charts communicated well, such as heatmaps, and which could mislead, such as totals and charts affected by the partial last day.
- **K2 Ethics and privacy:** noticed where other dashboards exposed sensitive data such as weight and BMI alongside user IDs.
- **K5.3 Real-world data patterns and B5 integrity:** spotted data quality issues in other teams' work using what I learned from my own exploration, and gave balanced, evidence-based feedback.


### Plan for next week
- [x] Read the January project brief again and start thinking about project ideas
- [x] Practise Streamlit and `pytest` by writing a small feature on my own

