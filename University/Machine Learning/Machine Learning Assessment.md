This page contains the details for how the course grade is arrived at.
# The basics

As stated in the Syllabus:

- The final grade is the average of the examination grade and the project grade.
- To pass, the project grade should be 4.5 or higher, the examination grade should be 5.5 or higher, and the average should be 5.5 or higher.
- The examination grade is determined by the results of the quizzes and the final exam. You can score a maximum 10 ten points per quiz, and 40 points for the final exam, for a total of 80 points for the examination.
- 80 points corresponds to a grade of 10, 52 points corresponds to a grade of 5.5, the *pass mark*.
- There are no intermediate grades for the quizzes or exam. We add up all points and determined the grade from this total.
- If there are mistakes in the exam, these will be converted to bonus questions. This does not affect the pass mark, but does affect the number of points corresponding to a 10. For instance, if there are mistakes in 2 questions, the pass mark stays where it is, but now scoring 78 points will lead to a grade of 10 (and a grade above 10 will become possible).
- If this happens, the final course grade is capped at a 10, but when computing the average between the examination and the report, the uncapped grade (potentially above 10) is used.

# Quiz grades and feedback

The quizzes contain some open questions, some of which are mini essays. This occasionally leads to some questions, since in Computer Science, we don't commonly write or grade essays.

If you have questions about your grade, you have two options:

1. If you don't necessarily want the grade to be changed, but you are just curious about what you could have done better, attend a homework session, and ask the TA to look at your answer and give you some feedback. This won't necessarily be the TA that  graded you.
2. If you are unhappy with your grade you can request a regrade by emailing the course coordinator, *not a TA*. In this case, the second grade counts, even if it is lower. Regrading will be done either by a second TA or the coordinator themselves.

The main question we get goes something like: "I didn't get full marks, I want to know what I did wrong". It's important to understand that this ***is not how essay grading works***. There is no fixed sets of things that you have to do to get full marks. Getting 3 out of 4 marks is like getting a an 8 for a final report. It's a sound grade: not perfect, but better than average. There are many different ways in which the essay could have been improved, but that doesn't mean that each of these was a thing you we *supposed* to do.

### FAQ: Quiz grading

**How do I find out who graded my quiz?**

You don't. It is not the job of the TAs to engage with students about their grading. See above for your options.

**Why is there no feedback on my essay? How can I learn to improve if I don't know what I did wrong?**

We agree that feedback is beneficial to learning, but it also takes skill and time to write. Moreover, feedback often invites discussion, putting even more pressure on the grader.

With ~700 quizzes to grade per week, we simply don't have the resources to provide feedback on essays that, ultimately, count for at most 1/32 of the final grade each.

**How are we supposed to learn to write essays without feedback?**

Learning to write, like learning to program, is one of the secondary learning goals of the course. That means that it's something we evaluate on, and we provide the opportunity to *practice*, but it's not something we explicitly *teach*.

In most programs, there is an academic writing course. In that setting, writing is the main learning goal, and you should have received explicit feedback. If you feel that this was insufficient, or if your program does not offer academic writing as a course, there are many resources online that can help you.

**How can you guard against subjectivity in grades if there's no feedback?**

We do this by grading on a [rubric Links to an external site.](https://en.wikipedia.org/wiki/Rubric_\(academic\)). This is a scoring guide that gives examples of what each number of points should correspond to. For the quizzes, we cannot publish the rubric (as we do for the final report), because they contain examples and pointers that would make the quiz to easy to do.

If the quiz question is based (in part) on the author's personal opinion, we explicitly instruct the TAs not to grade the conclusion, but the quality of the argument.

It's important to note that there is always going to be some subjectivity in grading quizzes. We consider this acceptable, so long as it's expressed as *variance* in the grading rather than *bias*. With 80 points worth of exercises, we can be sure that a little variance will work in your favor as often as it will be to your detriment and the final grade will be a good *estimate* of your performance.

# The examination grade

The examination grade is based on the total number of points you get for all 4 quizzes and the final exam combined. It is determined by the following rules:

- The pass mark (52 points) corresponds to a 5.5,
- 80 points corresponds to a 10, and
- the relation between points and the grade is linear.

With that, we can work out the formula $g = \frac{4.5}{80 - m} p + 5.5 - m \frac{4.5}{80 - m}$. where g is the grade, p is the number of points, and m is the pass mark.

In previous years, we determined the pass mark after the exam, based on statistics about how each question was answered. This is now stable enough that we can simplify things by setting the pass mark at the start of the course (at 52 points).

## Determining the pass mark

A common misconception is that getting half the questions in an evaluation correct should correspond to a minimal pass. This is often taken as a rule of thumb, but it is nothing more than that.

The real litmus test is the *key learning goals*. Out of all the learning goals (all of the material and all the soft goals like presenting, programming and writing), we identify a subset of key items. For instance:

- Knowing what a validation set is is a key goal. A student should not be able to pass if they don't know that.
- Knowing what the backpropagation algorithm is, and how, in principle to apply it, is another key learning goal. A student that doesn't know this, should fail the course.
- Knowing how to apply backpropagation to tensor data is *not* a key learning goal. We teach this, and we test it, but if a student fails that test, we are happy for them to compensate in other areas and to pass.

The point is that an ideal pass mark should be set so that any student who doesn't know their key learning goals will fail, and any student who knows the key learning goals but nothing else passes with a minimal grade.

Of course, we don't achieve this just by setting the pass mark. It starts by setting up an exam with a good mix for key and secondary learning goals. The choice of the pass mark is just a last moment of fine tuning. For a given choice of pass mark, we look at the students that will fail and the students that succeed, and we check whether, roughly, the passing students mostly satisfy the key learning goals, and the failing students don't.

In addition to this, we use the statistics of the exam to double check any questions that were too difficult, ambiguous, or otherwise poorly phrased. We have various metrics for this, but the most important ones measure whether doing well on a question correlates with doing well on other questions. In general, if the students that do well on other questions do poorly on a given question, it's an indication that the question should be double-checked. In rare cases, this may lead to questions becomes bonus questions.

## FAQ: Examination

**Isn't it unfair that I should get several answers correct to score a 1 (the lowest possible grade)?**

The logic here is that due to the large number of multiple choice questions, we need to account for what is known as "random guessing". In, the exam for instance, a student who randomly guesses all answers can expect to score 10 points. The general rule is that the expected number of points scored by purely random guessing should correspond roughly to the lowest possible grade.

In our case, this is a complicated by the fact that parts of the quizzes aren't multiple choice, so the random guessing score should be somewhere between 10 and 20 points. This is one of the things we look at when choosing the pass mark.

**I failed by just one point. Can't you make an exception in this case?**

We realize that failing by a single point is painful. However, we cannot just increase the score: we need a reason to do so. In a course of this size, there will be many people who fail by one point, so passing them all without careful review would cause a serious bias.

After the pass mark is determined, we manually check every student that fails by one point. We double check their grade, and go over their quizzes to see if there's an obvious reason to give them one extra point.

However, this is no more than a sanity check. To be more thorough, we need to do a true regrade, which comes with the risk of a lower grade. That is, your grade can go down as well as up. You can request a regrade of any or all of your quizzes, if you think it will get you the one point. If not, you will have to do the resit.

**Are these questions testing the right thing? One application question just asks me to name the rules of differentiation and another requires me to memorize a big deriviation.**

The point of the differentiation question is not that you should memorize the rules (they're given on the formula sheet), but that you should *recognize* what is happening in a differentiation. It's an indirect way of testing whether you understand the rules. This is an easy question, but then an exam should have both easy and difficult question.

For the derivation of the ELBO, you could indeed memorize the whole derivation, and sidestep the real learning goals that way. You could even write the whole thing on your cheat sheet. However, we expect that this is more effort than just going through all the steps and understanding how each follows from the one before it.

Since most of you will, presumably, actually *want* to learn the information presented, there is minimal risk in offering this shortcut.

**Did you know that there are a lot of old exams floating around? If questions are reused we can just memorize the old exams.**

This is true, and we do reuse questions from previous years. However, at this point the question bank numbers at least 200 questions. If you memorize all 200 questions, you haven't really cheated: you've just learned the material. Learning from the slides is probably the same amount of effort, but with a little better generalizability to the things you will actually use this knowledge for later.

We are happy for you to do either, but we recommend the slides.

**How do I compute my grade from the points?**

The formula is given above. You can also use the following table to look up your grade:

| **points** | **grade** |
| --- | --- |
| 0 | \-2.9 |
| 1 | \-2.7 |
| 2 | \-2.5 |
| 3 | \-2.4 |
| 4 | \-2.2 |
| 5 | \-2.1 |
| 6 | \-1.9 |
| 7 | \-1.7 |
| 8 | \-1.6 |
| 9 | \-1.4 |
| 10 | \-1.3 |
| 11 | \-1.1 |
| 12 | \-0.9 |
| 13 | \-0.8 |
| 14 | \-0.6 |
| 15 | \-0.4 |
| 16 | \-0.3 |
| 17 | \-0.1 |
| 18 | 0.0 |
| 19 | 0.2 |
| 20 | 0.4 |
| 21 | 0.5 |
| 22 | 0.7 |
| 23 | 0.8 |
| 24 | 1.0 |
| 25 | 1.2 |
| 26 | 1.3 |
| 27 | 1.5 |
| 28 | 1.6 |
| 29 | 1.8 |
| 30 | 2.0 |
| 31 | 2.1 |
| 32 | 2.3 |
| 33 | 2.4 |
| 34 | 2.6 |
| 35 | 2.8 |
| 36 | 2.9 |
| 37 | 3.1 |
| 38 | 3.3 |
| 39 | 3.4 |
| 40 | 3.6 |
| 41 | 3.7 |
| 42 | 3.9 |
| 43 | 4.1 |
| 44 | 4.2 |
| 45 | 4.4 |
| 46 | 4.5 |
| 47 | 4.7 |
| 48 | 4.9 |
| 49 | 5.0 |
| 50 | 5.2 |
| 51 | 5.3 |
| 52 | 5.5 |
| 53 | 5.7 |
| 54 | 5.8 |
| 55 | 6.0 |
| 56 | 6.1 |
| 57 | 6.3 |
| 58 | 6.5 |
| 59 | 6.6 |
| 60 | 6.8 |
| 61 | 6.9 |
| 62 | 7.1 |
| 63 | 7.3 |
| 64 | 7.4 |
| 65 | 7.6 |
| 66 | 7.8 |
| 67 | 7.9 |
| 68 | 8.1 |
| 69 | 8.2 |
| 70 | 8.4 |
| 71 | 8.6 |
| 72 | 8.7 |
| 73 | 8.9 |
| 74 | 9.0 |
| 75 | 9.2 |
| 76 | 9.4 |
| 77 | 9.5 |
| 78 | 9.7 |
| 79 | 9.8 |
| 80 | 10.0 |