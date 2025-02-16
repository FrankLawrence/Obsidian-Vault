The project is due on the last Friday of the course. It should be handed in through Canvas (under "Assignments"). The report is the only deliverable. We will not look at your code.

The report should meet the following requirements:

- It should be laid out and written in the style of a research paper. Don't include a cover sheet or contents page, do include [an abstract](http://users.ece.cmu.edu/~koopman/essays/abstract.html).
- You may include an appendix, containing anything you would like to show (code, extra figures, etc.), that does not fit the main body of the text.
- The report should include an *information sheet* ([docs](https://docs.google.com/document/d/1k1JpEU4EFKrBw16BRbklRcUPqbNXw7SIoyCsnIBu0IQ/edit?usp=sharing), [latex](https://www.overleaf.com/read/zgykyqpzqvwb#dd20b9)) providing some basic information. You can include it in the PDF before the first page, or between the report and the appendix. The information sheet does not count toward the page limit.
- It should be no longer than 10 pages. This includes the references, but excludes the appendix and the information sheet.
- There should be at least 10 references to research literature (no blog posts, no tutorials).
- If you use tutorials you *should* credit them (either in a footnote or a reference), but this won't count towards the reference count.
- It should be written in English.
- The file should be **a PDF**.

You can use any tool to write the report. We recommend a LaTeX environment like [Overleaf](https://www.overleaf.com/). Unfortunately, Overleaf has restricted shared documents in the free version to only two people, so if you want to collaborate with more people on the writing, it may pay to write the first draft in something like Google docs, and to transfer to LaTeX when it is time to finish up.

## Subjects

Below are some general approaches that make for a good project. If you have something else in mind, please make sure to **discuss it with your TA**, so they can help you work out whether it might be too ambitious. You can also ask on the discussion board.

- Pick **a [Kaggle](https://www.kaggle.com/) competition**, and try to get as high a score as possible. Below are some recommendation for good Kaggle projects. If you do this, it's important that you analyze your results properly. Pay good attention to the Methodology lectures.  
- You can also find your own dataset, or choose something from the [UCI repository](https://archive.ics.uci.edu/ml/index.php).
- If you go this route, we recommend you use [Pandas](https://pandas.pydata.org/pandas-docs/stable/), to load and process your data. The third worksheet should help you get started.
- You can test different algorithms, but you can also keep the algorithm fixed, and test different features or different hyperparameters for the same algorithm.
- Pick a machine learning algorithm, and **implement it from scratch**, using only numpy (or a similar library in another language). Your report should summarize the algorithm formally, reflect on what you learned along the way, and show experimental results proving that your implementation works. Depending on the algorithm, this is either very simple, or much too ambitious. kNNs can be very simple to implement, basic SVMs or decision trees are ambitious but achievable. Simple neural networks with backpropagation are a good choice, but convolutional or recurrent neural networks are probably too complicated. Your TA should be able to help you figure out which algorithm matches your level of ambition.  
- If your model is simple (like linear regression) you can try to implement different search methods, and experiment on some simple datasets to test their performance.
- A **deep learning project**. This is an ambitious subject to choose, but using [Keras](https://keras.io/), you can put something together pretty quickly. We've provided an optional worksheet to get you started, and there are plenty of online tutorials to help you out. The report should show that *you actually understood what you did* (describe the model without code), and a proper empirical evaluation of its performance. Some ideas:
- Build an MNIST or CIFAR classifier, and see how close you get to [the state-of-the-art](http://rodrigob.github.io/are_we_there_yet/build/classification_datasets_results.html).
- Build an [LSTM](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) [language model](http://karpathy.github.io/2015/05/21/rnn-effectiveness/), and train it to generate some chosen type of text.
- Build a (variational) autoencoder for MNIST and try to generate some digits, or visualize the latent space.

## Suggested planning, presentation

Feel free to plan your project however you like. Hoewever, you need to make sure you do something every week, and you have something to present. Here is a suggestion of what you might present week to week. If you have a different idea, feel free to do that instead (we're hoping for a little variety).

- **week 2:** Introduce your team members, and give three possible topics you're thinking of, each with their pros and cons.
- **week 3:** Do the first two worksheets and play around with the data. Present something interesting you've done beyond what's in there. Maybe you've loaded a different dataset, tried a different classifier, or found something new in the given datasets.
- If you tried to get set up for the worksheets, but you can't get it to work, present what you tried and what went wrong.
- **week 4:** Present your choice of project. Don't just give an ML task, present a specific *research question* around that task.
- **week 5:** Show what the data looks like. Provide a variety of plots, and what conclusions you can draw from them for the research question.
- **week 6:** Present a bug. You got stuck on the implementation of one of your experiments, and google does not provide a quick answer. Maybe one of the other students has seen the bug too, or maybe the TA has a good idea to help.
- **week 7:** Present your final results, reflect on the project and what lessons you learned for the next data science project you do.

## FAQ

**The requirements state that we should write in the style of a research paper. What if we built something that already exists, should we pretend that we discovered it?**

No. You should just report on what you did and your motivation for doing so. For instance, if you decide to implement a Decision Tree algorithm, you could choose the title "An Implementation of a Decision Tree learner". You can then test various aspects of the implementation, and report on which lead to better performance, or which lead to the fastest learning.

**Is there a standard template that we should use?**

There is no hard requirement. A two-column layout will give you more words per page, but can cause more last minute formatting problems than a single-column layout. For two-column, we suggest the [ACL template](https://www.overleaf.com/latex/templates/acl-rolling-review-template/jxbhdzhmcpdm), and for single-column we suggest the [ACM template](https://www.overleaf.com/latex/templates/association-for-computing-machinery-acm-large-1-column-format-template/fsyrjmfzcwyy). These are just suggestions (and they only work for Latex), if another format is more suitable for your project, feel free to use that.

**What citation style should we use?**

Like the article style, any citation style is fine, so long as it looks professional. The best way to ensure that is to pick a standard citation style like APA, or, if you use Latex, to pick an existing Bibtex bibliography style.

**We learned everything from the reader, and online tutorials. How can we cite 10 research papers?**

This requirement is there precisely to force you to dig into the literature a little bit, and to get used to the writing style. You should figure out what research is behind the methods you use, track it back to the source, and pick 10 papers to read properly. Reading research papers can be tough, and we don't expect you to fully understand everything, but there are five of you in the group, so you'll only have to read two papers each.

**Do plots and diagrams count towards the page limit? They are taking up a lot of space. Should we just put them in the appendix?**

Yes, if you include a graph or other image in the body of your report, it counts towards the page limit.

You can of course, put them in the appendix, but note that this is usually a bad idea. The TA is not required to read the appendix, and graphs and diagrams  both take a lot of work and are good for your grade, so you want to put them front and center. If you have a long list of many plots, these can go into the appendix, but don't make the mistake of moving all images to the appendix and leaving only a wall of text.

This is a common problem in writing any research paper, and the solution is usually to be clever with how you size and combine the figures. In areas where the page limits are extremely tight, you often see very innovative *compound figures* like these:

![alphastar.png](https://canvas.vu.nl/courses/81035/files/8514872/preview)  

This allows you to say beforehand that you'll give up a third of a page for your result graphs, and then to cram as much information into that space as possible. It's best to do this in an editor like Inkscape rather than to try and make Matplotlib or Latex create this kind of formatting.

# The Rubric

A [rubric](https://en.wikipedia.org/wiki/Rubric_\(academic\)) is a grading tool. The TAs will first mark your report according to the rubric. This will give them a preliminary grade. If they disagree, they may deviate from this grade at their discretion. So, if you do something very impressive that is not covered by the rubric, the TA may pick up on it, and grade you accordingly. However, if you don't want to risk the TA overlooking your hard work, you should read the rubric and spend some time to make sure that your report follows it.

Every item below can be scored poor, sufficient or good. Every score of poor should be compensated with a good. If there are poor marks left over within a category, that category will be scored a 4. If all poors are compensated with goods, it will be a 7. If there are goods left over, it will be a 9. The average over all five categories will be taken as an indicator for the final grade by the grader.

In rare cases, there will be double poor marks (pp). These require two goods to cancel out.

**If this seems like an ad-hoc method, remember that the resulting grade is only an indicator for the grader.**

We supply several examples for each item. These should be taken as general examples, not hard rules. For most of the good examples, we cannot guarantee that your attention to detail will be noticed. But then, that's always a risk when you put the effort into the details.

## Writing

1) The grammar and spelling are correct. The document has been thoroughly proofread.

- p. Several errors each page. Typos and obvious errors in important pieces of text like the introduction and the conclusion.
- s. No more that one or two typos per page. Grammatical errors limited to complex language.
- g. No typos. Particular attention to detail (like a consistent choice in British or American spelling). A tone particularly suited to academic writing.
- g. Pleasant writing style. Good organization in paragraphs, top-down structure, varied sentence length, logical connections between sentences.

2) The report reads like a research paper.

- p. Long discussions about process. Long personal discussions. Many references to the course and "the assignment". Many claims that are simply stated and not proved.
- s. A little too much implementation detail. Small references to the process that you wouldn't find in a research paper. A few unimportant claims that are not substantiated.
- g. Reads exactly like a (good) research paper. Clearly stated claims  backed up with empirical proof. Any other claims made are clearly substantiated with references to literature.

1) The report is well cited.

- p. Fewer than 10 citations, and many places where citation is clearly necessary.
- p. Plenty of citations, but all but a handful clearly added for the sake of adding to the citation list.
- p. Citations to very poor sources: Wikipedia, Online articles.
- p. Missing information in citations: no author names, no dates. Clear formatting errors that are obvious at first sight.
- s. Citations to textbooks (like the ones in the reader) to illustrate general concepts, but specific claims are be backed up with primary sources. A handful of places where a citation should be added. Some subtle formatting errors.
- g. Extra effort to track down the primary source for citations. Complete, properly formatted citation list. DOIs included.

2) The larger structure of the report is well-thought out.

- p. Poor throughline. No logical connections. Jumps from one subject to the next without engaging the reader.
- s. Logical throughline from one section to the next. Not especially inspiring, but easy enough to follow.
- g. Pleasant to read. Clear throughline. Gets to the point quickly, but spends enough time motivating the research.

3) Clear (and correct) idea of the readership. The authors know who the paper is written for, and work to satisfy that reader.

- p. No consistent idea of the reader. No explanation of basic concepts at one point, and over-explained at another.
- p. Written for a teacher, to show how much work was performed, not to communicate the basic idea.
- s. Written so that a fellow student who didn't take this course might find it interesting. A little over-explained.
- g. Written for an interested reader with a basic background in Machine Learning, but not in the specific methods used. Terse, to-the-point, technically correct explanations of just the required concepts, without getting in the way of the point of the paper.

## Validation

4) Reporting of results

- p. Results are described unclearly and incompletely. The work cannot be reproduced from the descriptions given.
- s. Results are clearly reported. The work is described in enough detail to be reproducible.
- g. Clear reporting. Sufficient detail to make the work reproducible, without impacting readability.

5) Presentation of results

- p. Poor figures: unlabeled axes, no legend, confusing and misleading layout.
- p. No figures, no tables. Only generic reporting of results without strong specifics.
- s. Clear, complete reporting. Functional figures that could be made more attractive or more clear.
- g. Clear, attractive figures that are highly readable an communicate their data well.

6) Statistical analysis and evaluation

- pp. Lack of a training/test split (if applicable).
- **p. No use of baselines.**
- p. Small test sets used without analysis of results. Not clear whether results are due to chance or a genuine difference between the two test conditions.
- p. Hyperparameter selection on the test set ([see lecture 3](https://www.youtube.com/watch?v=okxskT6ben4)).
- p. A description of the process that is so unclear or incomplete that we can't tell whether any of the above mistakes have been made. (I you do things correctly, explain how you did them, or we'll assume you did it incorrectly.)
- s. Correct hyperparameter selection, single or few tests on a withheld test set.
- g. Resampling to provide an indication of variance. Complete explanation of exactly what was changed for different runs.
- g. Especially thorough analysis: e.g. significance tests, proper reporting of all aspects of the test. Bar charts with error bars.

7) Interpretation of results

- p. The results are reported without discussion. "Model X is the best."
- s. Some discussion of the results. Speculation is allowed, so long as it is clearly marked as speculation.
- g. Gives the reader clear insight into why the results are the way they are. What is happening inside the agent, and how might we generalize these results?
- g. All speculation framed as hypotheses for future research, possibly with sketches of possible experiments.

8) Quality of results.

- p. Inconclusive results accompanied by strong claims.
- s. Inconclusive results, but the work was put in, and they are honestly reported.
- g. Strong, conclusive results that bear out the claim of the paper. It is very likely that the authors worked through several negative results to get these results.

## Content

9) Clearly stated research question. All claims backed up by proof.

- p. Unclear or overly vague research question
- p. Various statements that might be research questions, none backed by proof.
- s. Clearly stated research question. Evaluation deals with the research question.
- g. Clearly stated research question. All other definite statements backed up by either proof or reference.

10) An interesting, clear, and well-considered research question

- p. The research question is route-one. Taking some time to consider the question and to think about it shows several clear flaws.
- s. A solid, but unambitious research question
- g. The research question is particularly original, particularly well-considered, or particularly ambitious.
- g. A research question in an ambitious area, like deep learning, or probabilistic methods.
- g. A research question requiring a lot of manual implementation of methods.

11) The research question delves into one of the subjects discussed in the course

- p. Simply comparing two models, or two hyperparameter settings for one model.
- s. Comparing a wide range of models, hyperparameter settings or features for a particular task, with proper evaluation.
- g. Investigating the detail of a method by writing your own implementation
- g. Building a deep learning model in Keras, *and analyzing its behavior* (just building it isn't enough).

12) Correct length and density.

- p. Too long or too short by more than a page.
- s. The right length. A lot of air: things that could be cut if the page limit was two pages less.
- s. Clearly added things in order to get to the correct length
- g. Clearly made an effort to get the report down to the correct size, from a longer document. A dense report, but with a natural flow, and strong readability.

13) Professional layout and typesetting

- p. Large amounts of whitespace. Poor typesetting of mathematics.
- p. Python code copy-pasted into the report instead of abstracting algorithms to pseudocode.
- p. Unreadable due to odd choice of typeface, layout.
- s. Readable, essentially correct mathematics.
- g. Attractive, professional layout. Mathematics typeset with attention to detail.
- g. LaTeX used, and used correctly and with attention to detail.

## Method

14) Choice of model hyperparameters

- p. Simple, random choices.
- s. Extensive trial and error. Grid-search.
- g. Innovative search for hyperparameters: simulated annealing, evolutionary methods.

15) Innovative character of approach.

- s. Only existing methods used.
- g. Report includes novel ideas, implemented especially for the course. This could be a model, but it could also be a way of searching for hyperparameters, different ways of turning an existing classifier into a ranking classifier, ways of evaluating clustering, etc.

16) Amount of implementation required.

- p. The entire method requires is available in sklearn and the time saved was not put into other aspects of the report.
- s. The entire method is available in sklearn, but the time was spent testing hyperparameters and thoroughly analyzing the results.
- g. The method is not available in sklearn (or other platforms) and requires considerable work to implement.

17) Complexity of method(s).

- p. The method(s) used are extremely simple and require very little mathematics to understand. for example, kNN.
- s. The method(s) are very simple,but there are many of them, and they are well-described.
- s. The method(s) used are involved and require a good understanding of one or more of the three preliminary subjects (linear algebra, calculus, probability). For example, Decision trees, linear methods.
- g. The method(s) used require a deep understanding of the preliminary subjects. For example, logistic regression, support vector machines.
- g. The method(s) use require an understanding of preliminaries that were not, or briefly discussed. For example: logical methods, variational methods, Bayesian analysis.

18) Going off the beaten track.

- s. All models used were extensively discussed in the lectures or in the required reading.
- g. Some models were discussed only in passing.
- gg. The models used weren't discussed in the course at all. The authors read extra literature to learn about a subject not covered by the syllabus.

## Description of methods

19) The methods used (i.e. the machine learning algorithms) are described in a way that shows understanding.

- p. The descriptions are clearly incorrect. The authors ran the models by calling them in sklearn, but clearly had no idea how they actually work
- s. Essentially correct description with a few incorrect details.
- g. Good, accurate descriptions. Brief, but detailed.

20) The methods are described at the proper level of abstraction: in mathematical terms, not in programming terms.

- p. References to implementation details: python, sklearn, numpy etc.
- p. References to programming concepts rather than mathematical concepts: arrays instead of vectors, "floats" instead of "real values".
- p. Inclusion of python code rather than pseudocode.
- s. All programming concepts abstracted away, some clunky mathematical definitions.
- g. All programming concepts abstracted away, elegant and correct mathematical definitions.

21) A good balance between a correct, detailed description and one that is easy to read.

- p. Overly informal description. Imprecise, impossible to understand important details.
- s. Correct and complete details, but difficult to understand.
- s. Overly mathematical, need to read several times to understand.
- g. An elegant balance: a complete, correct and detailed description, that is almost entirely clear after one (careful) read.

22) Replicability

- p. From the textual description it is impossible to replicate the results.
- s. Some guesswork required, but basically clear how the results were achieved. Important details of the experimental process clearly described.
- g: Authors clearly made the extra effort to put all the relevant detail they could in the space available, while still keeping the paper readable.

23) Data pre-processing, experimentation and analysis.

- p. The method is described, but how the method is applied to the data is not described in a replicable manner.
- s. The pre-processing, experimentation and analysis are tersely described, with some gaps.
- g. The pre-processing, experimentation and analysis are described *and motivated* in detail.
- g. Experimental method is described with reference to the literature.

## External circumstances

We compensate for various external circumstances by adding extra "poors" or "goods" to your score. This is an approximation: the grader may not always add a literal poor/good, but they will adjust the grade roughly as much as adding a poor/good would affect the grade. The poor/good is always added where it does the least harm, or the most good (i.e. as if the student were to choose where to add it).

- Working with fewer than 5 group members counts as approximately one "good" in a category of choice for every missing member, but only if this is due to circumstances outside the group's control (like a student dropping out).
- Each project meeting where no group members were present, or where no presentation was given, counts as approximately one "poor" in a category of choice.
- Declaring the topic after the deadline counts as approximately one "poor" in a category of choice, per week that the topic is declared late.
- You can hand in late until 12:00 (noon) on Monday after the deadline. I you do, the penalty is approximately one "poor" per 24 hrs the report is late.
- Other unforeseen circumstances can also be compensated in this manner.