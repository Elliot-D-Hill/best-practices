# Best practice for computational projects

See [this project](https://github.com/duke-aihealth-ds-fellowship/pytorch-template) for an example that attempts to follow the practices laid out in this document.

## Why do we adopt best practices, standards and conventions?

Accelerates your research by reducing the number of issues you have to devote brainpower towards

By establishing or using a common convention that everyone adheres to, you reduce mental
overhead and communication errors in your team

## When should you use conventions

- When a decision is not relevant to your research question
- If there is no clear advantage between multiple options, default to convention

On the flip side, conventions are good candidates for new research if you can improve upon them

## Version control

Version control is probably one of the most important skills to learn.

Use git; Learn the basics: add, commit, push, branch, checkout, clone, merge, etc. These commands cover 95% of git use cases and it's easy to expand from there as needed.

Use a web-based version control tool like GitHub, GitLab, or BitBucket (these all interface with git and other version control systems).

### Benefits

- Git is freedom; you don't have to fear breaking your code (or your colleagues' code); You can always revert back to a working version.
- You no longer have to write file names like: final_version36_last_one_i_promise_2.txt

### Tips

- Make changes on a new branch; when your code is working again, merge the branch back into the main branch
- It's a good idea to keep your main branch as the stable/working branch
- Make changes in small increments; this makes it easier to merge branches back into main
- Commit often. If you make multiple changes and then your code throws an error, it is harder to determine what change caused the bug
- Write helpful commit messages; this makes it easier to find where you need to revert to

### Version control on a team (GitHub, GitLab, BitBucket, etc.)

So you want to add a new feature to the code or fix a bug you found? Here is a simple workflow I use for small teams:

1. Open a public discussion via issues (GitHub/GitLab) or on your team's communication channel (Slack/Teams/Discord)
    1. State what the problem is
    2. State how you want to fix it
    3. Get feedback
2. Create a new local branch and switch to it: `git checkout -b mybranch`
3. Modify the code
4. Stage and commit your changes:
    1. `git add modified_file.py`
    2. `git commit -m "A helpful commit message"`
5. Repeat steps 3 and 4 as needed (you can have multiple commits on the same branch)
6. Push your local branch to remote: `git push -u origin mybranch` (you only need `-u` to initialize the remote branch, afterwards you can just `git push`)
7. When you are happy with your changes, open a merge/pull request
8. Have others review your changes and make comments or suggest modifications
9. Incorporation your team's feedback into your changes
10. Resolve any merge conflicts
11. When all parties are satisfied, merge the branch to the main/master branch
12. Delete the old branch

## Automate the boring stuff

We want as much of our brain power as possible going towards our research question, so questions like "should I use tabs or spaces?", "How many characters long should my lines be?" take up a small (but significant) amount of mental time and energy. Therefore, you should try to automate these trivial (but repetitive) tasks.

Some automation tools I use (there are many more out there):

- Code formatting: ruff, black, autopep8
- Linting/type checking: mypy, ruff, pylint
- Testing: pytest
- Documentation: sphinx

AI automation tools

- GitHub Copilot (free for students and teachers!)

## Naming

Choose a style and stick to it: e.g., snake_case, CamelCase, etc. Different languages will have different standards (e.g., Python uses snake_case for functions and variables and CamelCase for classes).

One convention (of many) is:

- Variable/class names should describe what they are.
- Function/method names should describe what they do.

```python
# Bad: opaque function and variable names
def process_data(x, y, z, w):
    result = y * (x - z) - w
    return result


# Good: descriptive function and variable names
def calculate_total_payment(base_price, quantity, discount, tax):
    total_payment = quantity * (base_price - discount) - tax
    return total_payment
```

## Type hints

- Helps you reason through your code
- Don't have to keep track of object types; less mental overhead; self-documenting
- They become much more powerful when combined with a linter that checks them e.g., ruff, mypy (catches bugs before they happen)

```python
def count_amino_acid(sequence: str, amino_acid: str = "A") -> int:
    return sum([1 for aa in sequence if aa == amino_acid])


count_amino_acid(sequence="CARVAY")
```

## Documentation

In academia, often you are the only person (aside from possibly a colleague or
two) who is ever going to view your code, so writing clean, documented code
is going to primarily benefit you.

You are your closest collaborator and past you isn’t going
to answer any questions present you has forgotten.

Do document

- Lessons learned or dead ends
- Deliberate Design choices
- Unintuitive code
- High level behavior (inputs/outputs)
  
Don't document

- Low level behavior (i.e., implementation details)

### Docstrings

```python
def function_name(parameters):
    """
    Brief description of the function or method.

    More detailed explanation of the function or method's purpose,
    input parameters, return values, and any exceptions raised.

    Parameters:
    ----------
    parameters: type
        Description of the parameter.

    Returns:
    -------
    return_type
        Description of what the function or method returns.

    Example:
    --------
    Here, you can provide an example of how to use the function.
    """
```

I have found that docstrings are often overkill in the early stages of a research project when the functions you are writing are changing rapidly because you waste time constantly updating them. Docstrings are better for mature projects when functions won't change often. The issue is that as the code of a function changes the documentation may begin to not reflect the actual behavior of the function (this is called documentation drift). The compiler or your tests will complain at you if your code has a bug, but no error will be raised if your documentation doesn't match the function behavior. LLMs are making this less of an issue by speeding up the documentation process.

### Comments

Tip: don't say what the code does; say why the code was written the way it was.

If you made a conscious design choice for a particular piece of code that is unintuitive, make sure to document it so that your colleagues (or future you) doesn't come back and try to rewrite it only to encounter the same dead ends you have already solved. That being said, if your code is unintuitive, that is often a good indication that it should be simplified.

```python
def bad_safe_divide(numerator, denominator):
    epsilon = 1e-8
    # Bad comment: the numerator is divided by the denominator and a small constant
    return numerator / (denominator + epsilon)


def good_safe_divide(numerator, denominator):
    epsilon = 1e-8
    # Good comment: a small constant is added to the denominator to avoid division by zero
    return numerator / (denominator + epsilon)
```

I try to keep docstrings and comments minimal and prefer to try and write "self-documenting" code. Self-documenting code tends to be simple to read, has clear naming, and benefits from type hints. Self-documenting code is more of an ideal to strive towards than a practical reality; eventually, you will have to write comments or docstrings to explain the intricacies of complex projects.

## Virtual environments

Different projects require different (and often conflicting) dependencies. Virtual environments provide (some) isolation that helps prevent potential conflicts. If you need a deeper level of isolation, look into containers (e.g., Docker).

Examples:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

or

```bash
conda create --name myenv
conda activate myenv
```

## When to use notebooks vs scripts?

### Notebooks

Good for: experimenting, prototyping, visualization, presenting

Bad for: complex projects, production code

Disadvantages:

- Non-linear execution of code often leads to bugs and namespace pollution

### Scripts

Good for: code that will be reused (hint: most code should be), large, complex projects

Bad for: ?

Advantages:

- Enforces linear execution of code
- Easier to manage variable scope
- Often better tooling than notebooks

## Packaging is easy. No downsides. Only upsides

A very easy packaging tutorial: [link](https://packaging.python.org/en/latest/tutorials/packaging-projects/)

- Only takes one small file that you can copy-paste (pyproject.toml) to setup a package
- Simplifies importing your code both within a project and between projects
- Makes it easier to share your code with others and yourself

For example, you can install a package

```bash
pip install https://github.com/Elliot-D-Hill/pytorch-template.git
```

You can also install in local development ("editable") mode where changes to the codes are automatically reflected in the program's behavior

```bash
pip install -e .
```

Once installed, you no longer have to manipulate the PYTHONPATH (which you shouldn't do anyway) to import modules, just import them like this

```python
from packagename.model import MyNeuralNetwork
```

## Project structure

FIXME out of date since moving the template project to it's own repo

This repository follows a common format for python packages.

```bash
.
├── LICENSE                 # Who can use your project and how.
├── README.md               # Help background, setup, and basic usage of your software
├── .gitignore              # Determines which files will not be tracked by git
├── config.toml             # Consolidates variables that affect program behavior
├── data                    # Stores data (added to .gitignore usually)
├── pyproject.toml          # Used for packaging
├── requirements.txt        # Project dependencies are listed here
├── requirements_dev.txt    # Project dependencies that are only used for development
├── src                     # Package source code
│   └── example
│       ├── __init__.py
│       ├── __main__.py
│       ├── config.py
│       ├── dataset.py
│       └── model.py
└── tests                   # Unit tests that test the source code
    ├── test_dataset.py
    └── test_model.py
```
<!-- #endregion -->

## When should you write your own code vs import from a package?

- If a package has the functionality you need
- If the package is trusted (high star count on GitHub is a good indicator)

Benefits of using other people's code:

- Good packages write tests for their code (you probably don't)
- Popular packages are used, tested, and reviewed informally by their users every day (raises our confidence in them)
- Adopting other peoples code let's you get to your research problem faster. As a researcher, we often only change one or two aspects of a method/model/algorithm for a given project. That means that we can save a lot of time by importing as many of the parts we are not customizing as possible.

Downsides of using other people's code:

- Abstractions can make it harder to make low-level modifications
- Packages may not be well maintained

Example:

Your project is to design a custom loss function. You probably want to avoid also writing your optimizer and model architecture from scratch. You probably want to reuse standard model architectures and optimizers so you can isolate the benefit of your loss function.

### When should you optimize your code?

- Only when you need to; i.e., when the compute time is preventing progress
- When it is easy to do; doesn't take a lot of time or add a lot of tech dept
- If you must optimize your code, focus on bottlenecks first; find bottlenecks via code profilers e.g., cProfile in Python
- If you want to learn how to write optimized code
- If you enjoy it

Performance optimization is typically the last part of your code you want
to improve. This is because optimizations are often time consuming to
implement and can introduce substantial code complexity

### Configuration files

Consolidates parameters into fewer places.

Avoids having to touch the source code (and possibly introduce new bugs) to change program behavior.

By having all tunable parameters in one place, it makes it easy to make the methods section of your paper.

### Write modular code

A good rule of thumb: functions and classes should have roughly one responsibility.

Modular code is easier to read, reuse, reason through, debug, and write tests for.

The example below is probably overkill, but it demonstrates the idea.

A function with too many responsibilities:

```python
def process_data(data):
    # Responsibility 1: Calculate average
    total = sum(data)
    average = total / len(data)
    # Responsibility 2: Filter outliers
    outliers = [x for x in data if x >= 2 * average]
    # Responsibility 3: Generate a report
    n = len(data)
    report = f"Average: {average}, Total data points: {n}, Outliers removed: {n - len(outliers)}"
    return outliers, report
```

A more modular version:

```python
def calculate_average(data):
    total = sum(data)
    return total / len(data)


def filter_outliers(data, average):
    return [x for x in data if x >= 2 * average]


def generate_report(data, outliers_removed):
    n = len(data)
    return f"Average: {calculate_average(data)}, Total data points: {n}, Outliers removed: {n - len(outliers_removed)}"


def process_data(data):
    average = calculate_average(data)
    outliers_removed = filter_outliers(data, average)
    report = generate_report(data, outliers_removed)
    return outliers_removed, report
```

### Unit tests

- Automates manual testing
  - We often rerun our code over and over to check it's behavior, but this becomes tedious if you need to check multiple parts of the code simultaneously. Unit tests automates this.
- Allows you to refactor fearlessly
  - If you change the code, unit tests will highlight which parts of the code your changes are affecting. Once you pass all of the unit tests, you can be more confident that the current version is behaving like the previous version.
- It's likely that you are already unknowingly writing informal unit tests
  - Often, when we write a new function, we will generate some toy data to manually test it. Then, we discard the toy example after we are satisfied with the result. But the toy example's usefulness doesn't have to end there; we can make it a unit test and it will help our code forever more.

```python
%pip install polars
```

```python
import polars as pl
from polars.testing import assert_frame_equal


def make_adhd_phenotype(codes: str) -> pl.Expr:
    """
    ADHD phenotype = any patient who has two or more ADHD ICD10 codes in their medical history
    """
    return (
        pl.col("event")
        .str.contains(codes)
        .sum()
        .ge(2)
        .over("mrn")
        .cast(pl.Int64)
        .alias("adhd")
    )


def test_make_adhd_phenotype():
    df = pl.DataFrame(
        {
            "mrn": ["x", "x", "x", "y", "y", "z", "z", "z"],
            "event": ["a", "b", "b", "a", "c", "a", "c", "b"],
        }
    )
    expected = pl.DataFrame({"adhd": [0, 0, 0, 1, 1, 1, 1, 1]})
    phenotype = make_adhd_phenotype(codes="a|c")
    result = df.select(phenotype)
    assert_frame_equal(expected, result)


test_make_adhd_phenotype()
```

But how much testings is enough? No matter how many tests you write you will never prove your code will work 100% of the time, but (in a Bayesian sense) you will grow more and more confident that your code behaves as expected as you add more tests. Therefore, you should write as many tests as you need to be confident that your code works. Sometimes this is a lot of tests. Sometimes this is no tests.

Rule of thumb: the amount of tests should scale with how catastrophic failure is; NASA needs to write more tests than you do.

- "Happy path" testing: well formatted input, expected output
- "Sad path" testing: tests if your code handle errors or unexpected input gracefully
- Edge cases: tests uncommon (though still valid) input

## Style (Warning: you have entered the Opinion Zone)

### Prefer simple to clever code

There is always a temptation to write abstract, generalized code, but for most tasks, the correctness of the program is more important than it's design. Simple code is easier to read, write, modify, and, validate than clever code. Therefore, prefer simple code. That being said, sometimes clever code is also simpler, so it often comes down to a judgment call.

```python
# Here is a case where it's not clear if clever or simple is better since the clever solution is also rather simple
def clever_factorial(n):
    return 1 if n == 0 else n * clever_factorial(n - 1)


def simple_factorial(n):
    result = 1
    for i in range(1, n + 1):
        result *= i
    return result
```

### Prefer explicit to implicit code

For example, I would often run into bugs where I would give function arguments in the wrong order. However, this type of bug can be completely avoided by explicitly naming the argument. The few extra keystrokes are often a lot faster than the time it takes to detect and fix these types of bugs.

```python
import torch

matrix = torch.rand(size=(5, 5))
vector = torch.rand(size=(5,))

# Bad: you risk giving the arguments in the wrong order which can lead of subtle bugs that don't necessarily raise an error
x = torch.matmul(matrix, vector)
print(x)
# Accidentally swapping the arguments leads to different results, but no error was raised; detecting this bug could be tricky
y = torch.matmul(vector, matrix)
print(y)
# Good: explicitly naming the arguments prevents this bug, and makes the code more readable
z = torch.matmul(input=matrix, other=vector)
print(z)
```

Being explict often requires more keystrokes, but takes a lot less time than debugging implicit code.

### Avoid "magic" variables

Magic variables are variables that are hard-coded literals that are not assigned to a variable. This makes it hard to change the value of the literal because you have to change it in multiple places every time you want to update it. This is a common source of bugs. Instead, define the variable in one place and pass it as an argument or import it when needed.

```python
# Bad
def calculate_area():
    return 3.14159 * 5 * 5


# Good
PI: float = 3.14159


def calculate_area(radius: float | int):
    area = PI * radius * radius
    return area
```
