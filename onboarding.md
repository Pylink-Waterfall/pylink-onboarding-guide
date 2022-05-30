Pylink’s guide for junior python developers
===
This document describes the best practices and development choices made at Pylink for python developers.
By following these standards and processes, the barrier for entry as a junior python developer/coder is lowered.

Table of contents
===

- **[1. Starting a new project](#introduction)**
- **[2. Python IDE: PyCharm](#p)**
  * [2.1. Running and Debugging](#rd)
  * [2.2. Useful PyCharm plugins](#pp)
- **[3. Version Control](#v)**
- **[4. Virtual Environment](#ve)**
- **[5. Folder Structure](#fs)**
- **[6. Code Style](#cs)**
- **[7. Documentation](#d)**
  * [7.1. Type hints](#types)
  * [7.2. Docstring and commenting](#dc) 
  * [7.3. Auto doc generation](#da)
- **[8. Code Testing](#ct)**   
  * [8.1. PyTest](#pytest)
  * [8.2. Test coverage](#tc)
  
# 1. Starting a new project <a name="introduction"></a>
In order to allow for easy collaboration with others we suggest always to use _git_ with a project. At pylink we use 
GitHub to manage our projects and [version control](#v), as such, this guide will describe the process within the GitHub
framework.
- Create new GitHub repo (the name should be lowercase and use dashes instead of underscores)
- Always add README.md file describing the project and supplying entry points and relevant links. Here is a 
  [guide](https://guides.github.com/features/mastering-markdown/) explaining how to apply Markdown syntax for formatting
documents.
- Always add .gitignore (for python).

# 2. Python IDE: PyCharm <a name="p"></a>
We love PyCharm at Pylink! Make sure you try to take advantage of its many cool features that make you a more efficient 
programmer. You can open and run notebooks in the Professional PyCharm version too. Also, it is worth trying out the 
new Data Science IDE from JetBrains, 
[DataSpell](https://www.jetbrains.com/dataspell/). 

First time installation:

- [python](https://www.python.org/downloads/)
- [git](https://git-scm.com/downloads)                                               
- [PyCharm Professional](https://www.jetbrains.com/pycharm/download)

Here are a few PyCharm shortcut ideas that we regularly use:

Open Tool Windows:
- ALT + 1                   -- Project 
- ALT + 2                   -- Favorites
- ALT + 4                   -- Run
- ALT + 5                   -- Debug
- ALT + 9                   -- Git
- ALT + F12                 -- Open terminal

Others:
- CTRL + SHIFT + F          -- Search in the whole project
- CTRL + SHIFT + R          -- Replace in the whole project
- CTRL + SHIFT + C          -- Copy document path
- CTRL + SHIFT + UP/DOWN    -- Move line up or down
- Double CTRL + UP/DOWN     -- Add extra carets to allow for multi line editing
- SHIFT + ENTER             -- start new line
- TAB                       -- indent selection
- SHIFT + TAB               -- demote a selection
- CTRL + /                  -- comment out selection (reversible)


### 2.1 Running and Debugging <a name="rd"></a>
Before you run or debug your code, make sure your configuration is correctly set up.
- make sure the working directory is the project folder (root)
- you add environment variables here -- NEVER STORE PASSWORDS IN CODE. ALWAYS USE ENVIRONMENT VARIABLES!!!

![configuration](/pictures/configuration.png)


When you are debugging within a loop, sometimes you only want to stop your code once it got to a particular element. If 
you right-click on the breakpoint, you can add condition to your break. 

![breakpoint](/pictures/breakpoint.png)

Shortcut Keys:
- SHIFT + F10               -- run
- SHIFT + F9                -- debug
- F8                        -- step over
- F7                        -- step into
- F9                        -- run until next breakpoint
- ALT + SHIFT + F7          -- run into my code
- CTRL + F2                 -- stop


### 2.2 Useful PyCharm plugins <a name="pp"></a>
- Statistics: it counts the number of lines in your project. It also differentiates source code, blank lines and 
  documentation.
- Yaml Sorter
- RainShadow Brackets
- JSON formatter
- Kite AI Code AutoComplete 

# 3. Version Control <a name="v"></a>
Connect PyCharm to GitHub *(PyCharm --> Settings --> Version Control --> GitHub --> + --> Login via GitHub)*

PyCharm Shortcut Keys:
- CTRL + T                  -- update project
- CTRL + ALT + A            -- git add
- CTRL + K                  -- git commit
- CTRL + SHIFT + K          -- git push
- CTRL + ALT + Z            -- git reset (rollback)

Some principals which we stick to when collaborating:
- When you pick up a new ticket, pull the develop branch and open a new feature branch from it. Name your new 
  branch after the ticket name (for example AL-157).
- Commit and push your code at least daily, especially if you leave for the day. Just in case someone needs to 
  continue working on your ticket.
- Always use proper commit messages that can be understood by others. Here is a guide about
  [how to write commit message](https://chris.beams.io/posts/git-commit/#why-not-how).
- In the pull request body, add detailed information to help the reviewer's job. For example, explain how you tested 
  your code before created the PR.
- We encourage you to add comments and start conversation directly in the pull request.    
- After merging the pull request, delete the feature branch.
  
![delete_feature_branch](/pictures/github_delete_feature_branch.png)

- If you are admin on the project, always protect the production and develop branches. In the case of automated pytest, 
  add them to the requirements.
  
![img_4.png](/pictures/onboarding_img_4.png)

# 4. Virtual Environment <a name="ve"></a>
After cloning a project, create a new virtual environment. Always use a separate environment for new projects. 
*(File --> Settings --> Project: --> Project Interpreter)*. 

![img_1.png](/pictures/onboarding_img_1.png)

Or simply type `py -m venv venv` in the command line.


As code should be reproducible on other machines we strongly advise to create a requirements.txt containing all package
dependencies used in the project. Make sure you use both package name and version number.

![img_2.png](/pictures/onboarding_img_2.png)

You can automatically generate the requirements.txt based on the installed dependencies by using the *pipreqs* package. 

```
pip install pipreqs
pipreqs --force
```

# 5. Folder Structure <a name="fs"></a>
Use snake case for the folder and python file names.
- The source code must be stored in the src folder
- Unit and regression tests must be located in the test folder
- Configuration files (Dockerfile, Jenkinsfile, buildspec, requirements.txt) are in the root folder

![img_3.png](/pictures/onboarding_img_3.png)

When building a project, logic goes into modules and packages and is saved in _.py_ format. Notebooks are for 
analysis/research purposes only and should **never** be used to store source code! 


# 6. Code Style <a name="cs"></a>
Code should be readable, consistent, simple and properly commented. Python code must comply with the 
[PEP8 coding style](https://www.python.org/dev/peps/pep-0008/). PyCharm automatically uses PEP style.

Here are a few Pycharm short keys to reformat your code:
- custom                    -- make sure you saved a key for *Black*, we overwrote the CTRL + S, as Pycharm auto-saves 
files we did not use it
- CTRL + ALT + O            -- reformat imports (unfortunately *Black* doesn't do it.)
- SHIFT + F6                -- rename: this is super useful!!!

For code formatting, we use [black](https://black.readthedocs.io/en/stable/) to ensure the same style. To install black, 
you need to perform the following command `pip install black` for your primary environment. This is done to ensure that
___black___ is always available to pycharm and is not included as part of the requirements.txt file in any project.
You can also check the guide on [geeksforgeeks](https://www.geeksforgeeks.org/python-code-formatting-using-black/) 
to see an example of how you can reformat your code using black. Then to integrate black to PyCharm, go to 
(*PyCharm -> Preferences... (Ctrl + ,) -> Tools -> External Tools -> Click + symbol to add new external tool*). Configure as  
shown below and to reformat your current file, go to (Tools -> External Tools -> Black).
![img.png](/pictures/black_settings.png)
  

# 7. Documentation <a name="d"></a>
## 7.1 Type hints <a name="types"></a>
Type hints are a useful trick to use while programming in python as they aid in the code's readability and can help 
others to more easily understand your code. In order to add type hints in python simply add a colon then the type after 
the initial declaration of a variable, e.g. `integer_value: int = 1`. 

As a minimum we should use type hinting in the declaration of a class/function, as demonstrated below. By using type 
hints it helps the inbuilt linter in PyCharm highlight any potential mistakes that could arise from incorrect variable
types being passed into/returned from functions and in specific cases can be used with _mypy_ to make your code run 
faster!

## 7.2 Docstring and commenting <a name="dc"></a>
We prefer for docstrings to follow Google docstring format. More about Google Style docstrings can be found in 
[Section 3.8](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings). 

To set Google as the default format in PyCharm, simply update your settings: *File --> Settings/Tools/Python 
Integrated Tools/Docstrings*

![google_docstring](/pictures/google_docstring.png)

- Even for small projects, a single comment line is the minimum that should be added to functions and classes to 
describe what it is doing without the reader having to look at the actual code.
- Files preferably start with a docstring describing the contents and usage of the module.
- In the case of complex functions, even use examples to elaborate what the function does.    
- Docstrings of classes/functions should follow this structure:

```python
def example_function(input_1: str, input_2: list, input_3: dict) -> Tuple[pd.DataFrame, datetime.date]:
    """One sentence description of function's purpose.

    More detailed description and use-case
    
    Args:
        input_1: Description of parameter
        input_2: Description of parameter
        input_3: Description of parameter

    Returns:
        output_1: Description of return values
        output_2: Description of return values

    Raises:
        ErrorType/ExceptionType: Description of the raise
    """
    input_2.append(datetime.datetime.strptime(input_1))
    output_1 = pd.DataFrame(input_3)
    output_2 = max(input_2)
    return output_1, output_2
```

## 7.3 Auto doc generation <a name="da"></a>
Automatic technical documentation generation can be easily done by Sphinx (+Napoleon feature for Google Docstrings).

1. Installing packages :
```pip install Sphinx```
2. create a directory and name it docs:
```mkdir docs```
3. go to the docs folder and run the following command:
```sphinx-quickstart```

This will present to you a series of questions required to create the basic directory and configuration layout for your
project inside the docs folder. To proceed, answer each question as follows:

- **Separate source and build directories (y/n) [n]:** Type “n” and press Enter.

- **Project name:** "PROJECT_NAME" and press Enter.

- **Author name(s):** "AUTHOR_NAME" and press Enter.

- **Project release []:** "PROJECT_VERSION" and press Enter.

- **Project language [en]:** Leave it empty (the default, English) and press Enter.

4. go to config.py and make sure to uncomment the following lines: 
```python
import os
import sys
sys.path.insert(0, os.path.abspath('..'))# change the path to 2 dots as it shown 
```
5. add the following extensions to the extensions list in the config.py file 
```python
extensions = [
'sphinx.ext.autodoc','sphinx.ext.napoleon'
]
```

6. go back to index.rst and specify which files you want to document 
Example : 

```
 .. automodule:: src.benchmarks.lambda_benchmark_upload
    :members:
```
in the above example we created a tag ..autoumodule:: then we specified the location of the file that have code and
we want from sphinx to auto-document it, :members: means that document all the functions or classes within that file.

7. generate the documentation  :
```./make html```
or
```make html```

8. now you can go to docs --> _build --> html --> index.html and you can open in to see the documentation  


# 8. Code Testing <a name="ct"></a>
## 8.1 pytest <a name="pytest"></a>
We use the [pytest framework](https://docs.pytest.org/) to test our code.

To set it up: *(PyCharm --> Settings --> Tools --> Python integrated tools --> PyTest)*

![pytest](/pictures/pytest_settings.png)

All code should be tested using unit tests (before being merged into production)
All the test files should go into tests/ folder for the test files
- data for testing purposes can go to the tests/data/ folder
- unit test are placed within the tests/ folder with the exact same directory structure as the original structure

configuration -- keywords (--lf)
fixtures
    
## 8.2 Test coverage  <a name="tc"></a>
For unit test use the [coverage tool](https://docs.pytest.org/)
