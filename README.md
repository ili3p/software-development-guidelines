# Software Development Guidelines

### Scripts vs Modules

Scripts contain `__main__` and are entry points to the code. They
use the module code or can only serve as an example of how to
use it. So coding rules can be more relaxed here as usually only
the person who wrote the script will use it.

Module code should not contain `__main__` and should contain
excellent documentation and follow strictly the coding guidelines
because it would be used by everyone and (if good) for a long
time.

### Git

Create commits/merge requests with small diff

Learn how to write proper git commit messages
(https://code.likeagirl.io/useful-tips-for-writing-better-git-commit-messages-808770609503)

### Coding style & linting

Use linter to guide coding style. If possible integrate this into
Gitlab pipeline (we need to discuss which linter to use)

Use 80 lines col limit and explain the reasons (it's not just for
readability)

Use of tools: IDE is strongly suggested but can use any tool that
has syntax highlighting, linting and way to debug

### Logging (don't use **print**)

Logging: use configs (to define what to write and where).

Logging configuration is never done in the module code. Each
runner of a script should configure its own logging config. Of
course, we can share the logging configuration.

Put logs everywhere the code takes long time to complete so
we know it didn't hang.

Log the values of important variables, example, variables
containing results of function calls or external API calls

Use the appropriate logging levels, INFO, DEBUG, WARNING,
not all to info.

### Naming

Common argument/variable/function naming. Use python
standard: Camel case for class names, snake case for variable
and function names, upper case for constant.
Consistency, consistent naming style for everything. 

We would
share the naming convention doc with the commonly used
abbreviations, e.g. `ts` for time series, `n_ts` for number of time
series, etc.

Remember, good code almost reads like English and requires
no inline comments. So the names need to be descriptive and
intuitive, variables are nouns, methods are verbs.

### Documentation High Level

For each project, module or submodule write at least a README file
that contains:
- The goals of the code, i.e. what one can achieve with the code, the
requirements
- What are the problems in achieving the goal
- How the problems are solved
- Where the problems are solved, specific files.
- Example script how to run/use the code,
- Other things one who is using the code should be aware of, e.g.
some unconventional practices, common mistakes, and wrong
assumptions.
- Who is responsible for the code, name and email, and date of the
last major revision
- Optional: include the alternative solutions (tried or just thought
about them) to the problem and discuss their tradeoffs

### Documentation: Mid Level

Do **not** write “this function ... “. Spend time to write good method documentation.
It should contain what one will accomplish with the method, not how. How the
method is working should be clear from the code. The method documentation is a
story of what the method is accomplishing and how the method arguments and
the method output are related. Google for examples, or just read the docs of
pandas, numpy, etc.

Start each .py file with a docstring explaining what the file is doing in short. The
"elevator pitch" for the code. For scripts, this is a good place to put expected data
format, such as csv headers, etc. General useful things to know when running this
script, e.g. it runs a long time, so go get a coffee.


### Documentation: Low Level

In deep learning models code, it's useful to write the shape of
the tensors being passed around as comments.

In data preprocessing scripts, it's useful to write the expected
format of the data, i.e. csv headers, etc.

Again, good code almost reads like English and requires no
inline comments. If you feel the need to explain your code with
comment, then the code is most likely too complex and needs
to be simplified.


### Programming tips

A method needs to accomplish one major task, not a multitude,
if you name the method with "and" in it, then there should be
two methods

At the same time too much modularity, too much generalization
(e.g. in models) can lead to unnecessary complexity. Trying to
generalize too much when writing ML/research code typically
fails as the new algorithms tend to have completely different
logic.

And, following the execution of a method should not require
opening too many files and jumping between too many
methods. This leads to spaghetti code. So it's a fine line
between too many and too few methods.

Avoid asserts, prefer using "if" statements and exceptions.
Asserts belong to unit tests. The advantage of using exception
over asserts is that you can define the exception type, and the
caller can decide to catch it and what to do with it (exit the
program, log and continue, etc)

Always use python built-in functions, such as "float", and
well known and used packages, such as datetime, numpy,
pandas, etc, instead of writing your own implementation.

Utmost goal is readability. *Programming is the activity of
reducing the complexity of problems*, not increasing :) Always,
think how code or solution can be simplified. Computational
efficiency, code extensibility, etc. are second level concerns.

No surprises. Follow the most obvious and expected way
something to work.

No global variables in modules code. Scripts can have global
variables but in general, is good to avoid them.

Don't be afraid to delete old code and rewrite from scratch. If
you need to refer to a past version of a file, use git. Avoid using
ts_dataset_v2, ts_dataset_v3, .. etc, git is for versioning. There
are other ways to handle backward compatibility issues. The
high level documentation you wrote, i.e. the README files will
help you rewrite the code much faster than the initial writing.

In general, never name something "v34", "final", "final_final",
"really_final_final", etc.

No copy-paste (or limit if urgent)

One dot per line: nested functions should be split: easier to
understand, no surprises, easier to debug.

No function calls on the return statement. Every output of a
function must be held in a variable, otherwise is difficult to
debug.

If statements to improve: name conditions and isolate long
conditions. Never use nested if statements, they are hard to
follow. If you have multiple conditions that are difficult to understand, encapsulate them in variables with appropriate
names.

Improvement of testing scripts can wait for algorithm/model
code. But for data preprocessing, fetching and similar we must
write unit tests regularly.

Return only variables not function calls

Number 1 requirement is code readability and maintainability, performance is a feature, which has cost in terms of man hours, code readability and maintainability, so it needs to be decided whether that cost is justified

Method documentation is for what is doing not how is doing, and the inputs and outputs of the method

One line one sentence, one thought

### Project template

We will soon share a common project structure/template. It will
separate the code into functional parts, each with a consistent
folder name. For example, all data preprocessing code in one
part, model definitions in second, and third part training and
executing.

### Common excuses for not following the
guidelines

"These guidelines are great, but I just don't have time now... I
will fix it later"

- It's ok to not follow the guidelines sometimes when really
pushed for time, and especially when writing one-off scripts. But
when done for a module code, keep in mind that you are
borrowing time **with interest**, likely from someone else's future.

"The guidelines don't apply for my special case..."

- Unfortunately, there are no special cases. Only sometimes you
need to think harder..

### Why follow the software guidelines?

Buggy code can invalidate the results of weeks of experiments

Bad code often leads to big differences between model
performance on validation/test set and real application, e.g.
paper trading

Bad code is hard to modify, understand, or even just use by
others, significantly reducing the productivity of everyone
involved
