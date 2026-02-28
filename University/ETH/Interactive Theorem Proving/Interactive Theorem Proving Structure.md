---
title: "CS-428: Interactive Theorem Proving (Spring 2026)"
source: "https://systemf.epfl.ch/cs428/"
author:
  - "[[Systems and Formalisms Lab @ EPFL]]"
published:
created: 2026-02-17
description: "We're a programming languages, formal methods, and systems engineering lab at EPFL, led by Clément Pit-Claudel. We use (and invent!) mathematical formalisms and interactive tools to explore new ways to develop computer systems."
tags:
  - "clippings"
---
Course team

Clément Pit-Claudel, Jérémy Thibault  
Yawen Guan, Victor Deng  
Dario Halilovic

Meeting times

Tuesdays 8:15-11:00 (lectures + exercises, ELD 020)  
Thursdays 13:15-15:00 (labs, INM 202)

Course links

- [Assignment sources (Gitlab)](https://gitlab.epfl.ch/cs-428/2026)
- [Assignment submissions (Moodle)](https://moodle.epfl.ch/course/CS-428)
- [Student forum (limited staff presence)](https://edstem.org/eu/courses/3024/discussion)

Rocq resources

- [Reference manual](https://rocq-prover.org/doc/master/refman/)
- [Software foundations](https://softwarefoundations.cis.upenn.edu/) (most comprehensive, suitable for self-study)
- [Formal reasoning about programs](http://adam.chlipala.net/frap/) (main course book)
- [Certified programming with dependent types](http://adam.chlipala.net/cpdt/) (advanced material on dependent types)
- [Mathematical components](https://math-comp.github.io/mcb/) (strong focus on mathematics)

Final grade breakdown

- Labs (autograded tests, quizzes, checkoff, and exam-lab): 40%
- Project (proposal, weekly updates, report, implementation, presentation): 60%

Deadlines and late submissions

Deadlines on Fridays at 18:00 (no-penalty extension until 24:00)  
Beyond that: 0.

Important dates

Exam lab (in class): April 16 (week 8), 13:15-15:00 (Thursday)  
Quizzes (in class): March 3, 17, 31 (weeks 3, 5, 7), 8:15 (Tuesdays)

You are allowed to collaborate with your classmates on all assignments, and to consult external resources. However, you must retain full authorship of your final code submissions.

This means that, while you are allowed to think together on a whiteboard or to discuss a tricky invariant, you may not exchange source code. Similarly, while you may use AI tools to get inspiration, check assumptions, check grammar or spelling, or ask for clarification, you may not submit AI-generated code or text, nor feed assignments into an AI model.

In all cases, you must acknowledge any external sources that you relied on.

To summarize: you may collaborate with others to help you think and learn, but all materials that you submit must be fully yours, and all external help must be credited.

You will complete multiple take-home assignments (“labs”) during the semester. In each of them, you will be asked to write a proof in Rocq. Each lab takes from 6 to 12 hours to complete.

Lab sessions are not mandatory, with two exceptions: the “exam lab” and the “lab checkoff”.

Labs are graded automatically: your score is the sum of the point values of the individual theorems that you proved and tests that you passed. Point values are documented in the specification file that we provide. There is no partial credit for partially proven theorems.

#### Exam lab

The last lab of the semester is an “exam lab”.

In week 8 (April 16) you must attend the Thursday lab session and submit your work-in-progress code at the end of the session. You may then finish the lab at home. Your score will be a weighted average of your score at the end at the lab session and your final score at the end of the week.

#### Checkoffs

You will have one “lab checkoff” in the semester.

Checkoffs are in-person examinations lasting approximately 8-10 minutes, during which you present your lab solution, along with an English explanation of your proofs. Each student will have one checkoff during the semester. You must follow roughly the following template:

- 1 minute overview of your submission (scroll through with the instructor giving you the check-off)
- 7 minutes deep dive into one interesting theorem of your choice:
	- 3 minutes on the proof plan / big idea / key lemmas, without looking at code and without Rocq jargon (no “tactics”, “destruct”, “repeat”, …). Pretend you're talking to a computer scientist who knows nothing about Rocq.
	- 4 minutes on the code, showing how it follows the logic of your proof plan.

Your proof plan should be written down as a standalone comment within your source file, submitted by the usual deadline.

Regular labs (drop lowest) + Quizzes

50%, automatically graded

Exam lab (once in the semester)

40%, automatically graded (weighted average of in-class and take-home scores)

In person checkoff (once in the semester)

10%, manually graded on the following criteria:

- Implementation is clean and readable: 5pts  
	Code: no redundant code, no useless definitions  
	Proofs: no useless tactics, no excessive repetition, no unnecessary proof steps
- Proof plan is clear, complete, succinct, and well understood: 4pts  
	Explained in English (not code); complete
- Code and proofs are kept private: 1pt

CS-428 is a project course.

You will work in teams of 3 to build an original formalization in Rocq, on a topic of your choice. The following are examples of reasonable projects:

- Implement, document, and verify a [2-3-tree](https://en.wikipedia.org/wiki/2%E2%80%933_tree) -based finite map, and prepare it for submission to Rocq standard's library ([FMap](https://rocq-prover.org/doc/v8.20/stdlib/Rocq.FSets.FMapInterface.html)), or one of the more recent map interfaces ([mmap](https://github.com/rocq-community/mmaps), [stdpp](https://gitlab.mpi-sws.org/iris/stdpp/-/blob/master/stdpp/fin_maps.v)).
- Formalize part of an open language standard, technical standard, or RFC, and prove some properties of the associated algorithms: XPath, CSS, Dart, GNU Make.
- Formalize parts of an existing language, type system, or algorithm, either from your research or from a recently published paper.
- Specify and verify a pass of your CS420 *Advanced Compiler Construction* compiler (aim for higher complexity than the example passes from lab 6), or from CS320 examples.
- Formalize and verify algorithms from one of your bachelor classes (Computer Networks, Algorithms I, Computer Systems, Parallelism and Concurrency); either purely functional using plain Rocq or imperative using a separation-logic framework.
- Port (a subset of) labs and exercises from another EPFL class to Rocq.

You may also consult [Castelnau & Rimkevičius (2025)](https://drive.google.com/file/d/14GHBZPvKpTX3cCt3HhHV2rruHGnBHdQm/view?usp=drive_link) for an example of a report that received a good grade in a previous edition of the course.

Projects will happen over the last 8 weeks of the class:

Week 6

Project proposal (roughly one page), submitted publicly on Ed.

Weeks 8–13

Weekly online updates (one paragraph), submitted publicly on Ed.

Week 14

Oral project presentation, given to the class.  
Final report, submitted on Moodle.

#### Team registration

Start by registering your team on Moodle. Your team must have three members: if you cannot find a team, post on Ed. If your topic requires that you work alone, find a different topic (if you must request an exception, please do so by sending the staff a private message on Ed).

#### Project proposals

Aim for 400 words (do not exceed 500), and assume an informed (but not expert) reader. Submit your proposal as a public Ed *post* (not question) in the “Project tracking” category. A good project proposal should follow the format of an “extended abstract”, with the following parts (roughly):

Introduction / problem statement (1-2 paragraphs)

State what the problem is (a good example helps a lot) and (ideally) why it matters. Keep it short and to the point: give enough context to understand, but not more. Cut out the soaring rhetoric. It should be clear from the introduction what it means to solve the problem.

Proposal (2-4 paragraphs)

Summarize what you plan to do: how will you approach the problem? What deliverables (artifacts, proofs,...) will you produce? What interesting challenges do you foresee?

Timeline and responsibilities (1 paragraph)

Break down you project into small components and milestones, then estimate how they depend on each other and how long each will take. Organize the milestones on a timeline. Clarify who will do what, and how you will organize the work of your group.

#### Weekly updates

Your team will update the rest of the class on your progress every week by creating a new comment thread every week below your proposal post.

All team members should contribute to each update, but the update should be written and posted by just one of the team's members each week. Each team member should post an equal number of updates (2 per person).

Use the following template (summary of this week's result + one highlight / thing you learned + plans for next week):

> **Week 9 update**
> 
> **Status update**: We have completed the proof of three of the four lemmas that we highlighted last week: tree\_rev\_involutive, insert\_delete\_cancel, and rotate\_lookup\_iff \[…\].
> 
> **One thing we learned**: The last lemma (insert\_insert\_comm) turned out to be wrong, because insertion order affects the shape of the tree: \[…\]. We have rephrased it to use a new equiv predicate: \[…\].
> 
> **Future plans**: Next week, we plan to complete insert\_insert\_comm and to start connecting the functional model with the imperative implementation by setting up the Hoare logic.

#### Presentation

In the last week of class, you will present your work to your classmates.

Aim for 10 minutes, plus 5 minutes of questions / interruptions. The structure of the talk should be similar to that of the final report (below), skipping the timeline and related-work parts.

All students should contribute equally to the presentation.

#### Final report

Aim for 1600 to 1800 words (about four pages; do not exceed 2000 words), plus code listings and figures. Assume an informed (but not expert) reader. Submit your report as PDF, using the [provided template](https://systemf.epfl.ch/cs428/TODO%5Bmd%5D). The structure is similar to that of a tech report:

Abstract (1 paragraph)

A one-paragraph condensed version of the motivation and results.

Keywords

One line, naming the course techniques and concepts that you used in your project.

Introduction / Motivation (1/2 page)

This should be an expanded version of the proposal's introduction.

Approach (2 pages)

Summarize how you solved the problem. Aim to present the general approach and the most challenging aspects in enough detail that someone else could reproduce your work, and try to justify non-trivial design choices. Show concrete examples of Rocq definitions.

Results (3/4 page)

Clarify exactly what you achieved and what the limitations are. Show relevant data: concrete examples of Rocq theorems, benchmarks, ….

Timeline (1/4 page)

Compare the actual development of the project to your original timeline. Highlight the main divergences. Clarify who did what.

Related work, future work, conclusion (1/2 page)

Highlight related efforts, and suggest interesting continuations of your work.

Team members are graded individually.

Weekly updates

30%, graded on clarity and relevance.

Project presentation

40%, graded on the following criteria:

- Presentation is clear and structured: 8pts
- Motivation is convincing: 4pts
- Approach and design are adequate and motivated: 8pts
- Demo / highlight is relevant and compelling: 8pts
- Questions are answered correctly and completely: 8pts
- Time is used judiciously: 4pts

Proposal, report, & implementation

30%, graded on the following criteria:

- Proposal is adequate and well-scoped: 3pts
- Writing is clear and structure is respected: 3pts
- Motivation, keywords, approach and design are adequate: 5pts
- Results are presented completely and with links to the code: 4pts
- Deliverables are functional: 4pts
- Deliverables are complete and reasonably scoped: 6pts
- Code is clean and readable: 3pts
- Limitations and related work are adequately discussed: 2pts

**This calendar is tentative and subject to change.**

Topics

Intro to Rocq, inductive types, fixpoints, tactics, proofs by induction

Optional readings and exercises

- Software foundations:[Basics](https://softwarefoundations.cis.upenn.edu/lf-current/Basics.html) and [Induction](https://softwarefoundations.cis.upenn.edu/lf-current/Induction.html)
- [Additional materials on bool s and Prop s](https://systemf.epfl.ch/cs428/transcripts/BoolsAndProps.html)  
	\[[Rocq source file](https://systemf.epfl.ch/cs428/transcripts/BoolsAndProps.v)\]

Topics

Interpreters, automation

Optional readings and exercises

- [Notes on recursive functions](https://systemf.epfl.ch/cs428/transcripts/RecursiveFunctions.html)  
	\[[Rocq source file](https://systemf.epfl.ch/cs428/transcripts/RecursiveFunctions.v)\]  
	Related CoqPL 24 talk: [Well-founded recursion done right](https://www.youtube.com/live/YlVqyxU8lkg?feature=shared&amp;t=22121), Xavier Leroy
- Software foundations:[Maps](https://softwarefoundations.cis.upenn.edu/lf-current/Maps.html),[ImpCEvalFun](https://softwarefoundations.cis.upenn.edu/lf-current/ImpCEvalFun.html)

Deadlines

Lab 2 due

Topics

Inductively defined propositions, rule induction

Deadlines

Lab 3 due

Topics

Big-step semantics, logic programming

Deadlines

Lab 4 due

Topics

Small-step semantics

Deadlines

Lab 5 due

Topics

Simply-typed lambda calculus

Optional readings and exercises

Software foundations: [Simply-typed Lambda Calculus](https://softwarefoundations.cis.upenn.edu/plf-current/Stlc.html)

Deadlines

Project teams and initial proposals  
Lab 6 due

*No class: EPFL holidays!*

Topics

Hoare logic

Deadlines

Lab 7 due  
First weekly update

Topics

Simple dependent types

Topics

Dependent inductive types

Topics

Separation logic  
Proofs by reflection

*No lab session on May 14th (Ascension day)*

Topics

TBA

Topics

Real-world Rocq

: Last weekly update

Deadlines

**May 26th & 28th**: project presentations  
**May 29th**: final report due (no-penalty extension to May 31st, 24:00)

---