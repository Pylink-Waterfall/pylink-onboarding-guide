Pylink’s onboarding guide for junior python developers
===

This document describes best practices and development choices to promote alignment among the python developers at 
Pylink. These standards and set processes will lower the threshold of collaboration and help newcomers adapt to the 
described workflows.

Table of contents
===

- **[1. Starting a new project](#introduction)**
- **[2. Project management](#project_management)**
- **[3. Version Control](#v)**
- **[4. Python IDLE: PyCharm](#p)**   
  * [4.1. Running and Debugging](#rd) 
  * [4.2. Useful PyCharm plugins](#pp)
- **[5. Virtual Environment](#ve)**
- **[6. Folder Structure](#fs)**
- **[7. Code Style](#cs)**
- **[8. Documentation](#d)**   
  * [8.1. Docstring and commenting](#dc) 
  * [8.2. Auto doc generation](#da)
- **[9. Some coding preferences](#cp)**   
  * [9.1. Object Oriented Programming (OOP)](#oop) 
  * [9.2. Logging](#logging)
  * [9.3. Exception handling](#eh)
- **[10. Code Testing](#ct)**   
  * [10.1. PyTests](#pytest)
  * [10.2. Test coverage](#tc)
  * [10.3. Automated tests](#at)
  * [10.4. UI testing](#uit)
- **[11. Containers (Docker & AWS ECR)](#cta)**
- **[12. AWS](#aws)**
- **[13. APIs](#api)**
- **[14. DataBases](#database)**
  
# Starting a new project <a name="introduction"></a>
- Create new GitHub repo (the name should be lower case and use dashes instead of underscores)
- Always add README.md file describing the project and supplying entry points and relevant links. Here is a 
  [guide](https://guides.github.com/features/mastering-markdown/) how to apply markdown syntax for formatting documents.
- Always add .gitignore (for python).

# Project management <a name="project_management"></a>
We follow Kanban concept and use Jira boards. Make sure each project has its own board with the following columns:
- **Backlog**
- **ToDo**
- **In Progress**
- **In Pull Request**: for code review before merging to the development environment
- **In QA**: testing the feature in development
- **Done**: after it is merged to production

In the case of simpler projects: 
- **Backlog**
- **ToDo**
- **In Progress**
- **For Review**: 
- **Done**:

### Jira Automation
We often use automation rules to move the tickets automatically.
- Connect the projects board to GitHub
- Add the following rules:
  - When a branch is created --> then move the issue to **“In Progress”**
  - When a pull request is created --> then move it to **“For Review”**
  - When a pull request is merged --> then move the issue to **“Done”**

# Version Control <a name="v"></a>
Connect PyCharm to GitHub *(PyCharm --> Settings --> Version Control --> GitHub --> + --> Login via GitHub)*

PyCharm Short Keys:
- CTRL + T                  -- update project
- CTRL + ALT + A            -- git add
- CTRL + K                  -- git commit
- CTRL + SHIFT + K          -- git push
- CTRL + ALT + Z            -- git reset (rollback)

Some principals:
- When you pick up a new JIRA ticket, pull the develop branch and open a new feature branch from it. Name your new 
  branch after the ticket name (for example AL-157).
- Commit and push your code at least daily, especially if you leave for the day. Just in case someone needs to 
  continue working on your ticket.
- Always use proper commit messages that can be understood by others. Here is a 
  [how to write commit message guide](https://chris.beams.io/posts/git-commit/#why-not-how).
- In the pull request body, add detailed information to help the reviewer's job. For example, explain how you tested 
  your code before created the PR.
- We encourage you to add comments and start conversation directly in the pull request.    
- After merging the pull request, delete the feature branch.
  
![delete_feature_branch](/pictures/github_delete_feature_branch.png)

- If you are admin on the project, always protect the production and develop branches. In the case of automated pytest, 
  add them to the requirements.
  
![img_4.png](/pictures/onboarding_img_4.png)


# Python IDLE: PyCharm <a name="p"></a>
We love PyCharm. Make sure you try to take advantage of its many cool features that make you a more efficient coder. 
Logic goes into modules and packages, notebooks are for analysis/research purposes only. You can open and run notebooks 
in the Professional PyCharm version too. Also, it is worth trying out the new Data Science IDE from JetBrains, 
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
- ALT + F12                 -- open terminal

Others:
- CTRL + SHIFT + F          -- search in the whole project
- CTRL + SHIFT + R          -- replace in the whole project
- CTRL + SHIFT + C          -- copy document path
- CTRL + SHIFT + UP/DOWN    -- more line up or down
- SHIFT + ENTER             -- start new line
- TAB                       -- indent a full paragraph
- SHIFT + TAB               -- demote a full paragraph
- CTRL + /                  -- commenting out a full paragraph


### Running and Debugging <a name="rd"></a>
Before you run or debug your code, make sure your configuration is correctly set up.
- make sure the working directory is the project folder (root)
- you add environment variables here -- NEVER STORE PASSWORDS IN THE CODE. ALWAYS USE ENVIRONMENT VARIABLES!!!

![configuration](/pictures/configuration.png)


When you are debugging within a loop, sometimes you only want to stop your code once it got to a particular element. If 
you right-click on the breakpoint, you can add condition to your break. 

![breakpoint](/pictures/breakpoint.png)

Short Keys:
- SHIFT + F10               -- run
- SHIFT + F9                -- debug
- F8                        -- step over
- F7                        -- step into
- F9                        -- run until next breakpoint
- ALT + SHIFT + F7          -- run into my code
- CTRL + F2                 -- stop


### Useful PyCharm plugins <a name="pp"></a>
- Statistics: it counts the number of lines in your project. It also differentiates source code, blank lines and 
  documentation.
- Yaml Sorter
- RainShadow Brackets
- one of the JSON formatter -- not sure which one
- AWS Toolkit -- not sure to add...
- Kite AI Code AutoComplete  -- actually, i haven't tried it yet but people recommend
- Docker -- not sure to add...

# Virtual Environment <a name="ve"></a>
After cloning the project, create a new virtual environment. Always use a separate environment for new projects. 
*(File --> Settings --> Project: --> Project Interpreter)*. 

![img_1.png](/pictures/onboarding_img_1.png)

Code must be reproducible on another machine hence requirements.txt for package dependencies must be used. 
Make sure you use both package name and version number.

![img_2.png](/pictures/onboarding_img_2.png)

You can automatically generate the requirements.txt based on the installed dependencies by using the *pipreqs* package. 

```
pip install pipreqs
pipreqs --force
```

# Folder Structure <a name="fs"></a>
Use snake case for the folder and python file names.
- The source code must be stored in the src folder
- Unit and regression tests must be located in the test folder
- Configuration files (Dockerfile, Jenkinsfile, buildspec, requirements.txt) are in the root folder

![img_3.png](/pictures/onboarding_img_3.png)


# Code Style <a name="cs"></a>
Code should be readable, consistent, simple and properly commented. Python code must comply with the 
[PEP8 coding style](https://www.python.org/dev/peps/pep-0008/). PyCharm automatically uses PEP style.

Here are a few Pycharm short keys to reformat your code:
- custom                    -- make sure you saved a key for *Black*, I overwrote the CTRL + S, since I did not use it
- CTRL + ALT + O            -- reformat imports (unfortunately *Black* doesn't do it.)
- SHIFT + F6                -- rename: this is super useful!!!

For code formatting, use [black](https://black.readthedocs.io/en/stable/) to ensure the same style. To install black, 
you need to perform the following command `pip install black`. You can also check the guide on 
[geeksforgeeks](https://www.geeksforgeeks.org/python-code-formatting-using-black/) to see an example of how you can 
reformat your code using black. Then to integrate black to PyCharm, go to 
(*PyCharm -> Preferences... (⌘,) -> Tools -> External Tools -> Click + symbol to add new external tool*). Configure as  
shown below and to reformat your current file, go to (Tools -> External Tools -> Black).
![img.png](/pictures/black_settings.png)
  

# Documentation <a name="d"></a>
## Docstring and commenting <a name="dc"></a>
Docstrings are preferred to follow Google docstring format. More about Google Style, docstrings are described in 
[Section 3.8](https://google.github.io/styleguide/pyguide.html). Update your settings: *File --> Settings/Tools/Python 
Integrated Tools/Docstrings*

![google_docstring](/pictures/google_docstring.png)

- Even for small projects, a single comment line is the least to be added to the functions and classes to describe what 
  it is doing without the reader having to look at the actual code.
- Files preferably start with a docstring describing the contents and usage of the module.
- In the case of complex functions, even use examples to elaborate on what the function does.    
- Docstring of classes/functions should follow this structure:
![img_5.png](/pictures/onboarding_img_5.png)

## Auto doc generation <a name="da"></a>
Automatic technical documentation generation can be easily done by Sphinx (+Napoleon feature for Google Style).
- Tutorial for Sphinx: https://www.sphinx-doc.org/en/master/usage/quickstart.html
- Short intro to Sphinx: Appendix


# Some coding preferences <a name="cp"></a>
not sure if we want to add this part.. i feel it is too deep for this doc.

## Object Oriented Programming (OOP) <a name="oop"></a>

Book recommendation: [Python Object-Oriented Programming](https://www.amazon.co.uk/Python-Object-Oriented-Programming-maintainable-object-oriented-dp-1801077266/dp/1801077266/ref=dp_ob_title_bk)

Avoid overusing OOP but in complex projects, it might be a good idea to use them. Before you start coding, try to design 
the abstraction. Identify objects in the problem and then model their data and behaviours. Remember, objects are things 
that have both data and behaviour. If you are working only with data, you are often better off storing it in a list, 
set, dictionary or some other Python data structure. On the other hand, if you are working only with behavior, but no 
stored data, a simple function is more suitable. Proficient Python programmers use built-in data structures unless (or
until) there is nan obvious need to define a class. There is no reason to add an extra level of abstraction if it 
doesn't help organize your code. On the other hand, the "obvious" need is not always self-evident.

## Regex <a name="regex"></a>
we use regex heavily. Here is a guide...


## Logging <a name="logging"></a>
Logging is highly preferred instead of “printing”.
Use the python_log_indenter package for structure your log file better by .add() or .sub() functions.

```python
import logging
logging.basicConfig(format="%(asctime)s %(message)s", level=logging.DEBUG)
```
maybe we could add a bit more info about setups..

## Exception handling <a name="eh"></a>
We encourage to use the "else" and "finally" clauses of the try-except-else-finally syntax. The "finally" clause is 
often used for
- cleaning up an open database connection
- closing an open file
- sending a closing handshake over the network

In complex projects, it might be useful to define your own custom exception. It might be a powerful way to communicate
information between two sections of code that may not be directly calling each other.

```python
class InvalidItemType(Exception):
    pass

class OutOfStock(Exception):
    pass


class Inventory:
    
    stock = {
        'apple': 35,
        'orange': 5,
        'banana': 0
    }
    
    def purchase(self, fruit_name):
        if fruit_name not in self.stock.keys():
            raise InvalidItemType(f"Sorry, we don't sell {fruit_name}")
        if self.stock[fruit_name] == 0:
            raise OutOfStock("Sorry, that item is out of stock.")
        self.stock[fruit_name] -= 1
            

inv = Inventory()


try:
    product = 'apple'
    number_left = inv.puchase(product)
except InvalidItemType as e:
    print(e)
    print('Please select another fruit.')
except OutOfStock as e:
    print(e)
    print("You might want to come back tomorrow!")
else:
    print("Purchase complete")
```

# Code Testing <a name="ct"></a>
## pytest <a name="pytest"></a>
We use [pytests framework](https://docs.pytest.org/) 
To set it up: *(PyCharm --> Settings --> Tools --> Python integrated tools  PyTest)*

![pytest](/pictures/pytest_settings.png)

All code must be tested using unit tests (before being merged into production)
All the test files should go into tests/ folder for the test files
- data for testing purposes can go to the tests/data/ folder
- unit test are placed within the tests/ folder with the exact same directory structure as the original structure

configuration -- keywords (--lf)
fixtures
    
## Test coverage  <a name="tc"></a>
For unit test use the [coverage tool](https://docs.pytest.org/)

## Automated tests <a name="at"></a>
Jenkins + GitHub PR requirements to protect production and develop branches

## UI testing <a name="uit"></a>
selenium

# Containers (Docker & AWS ECR) <a name="cda"></a>
Our complex projects are made up containerised microservices by using [Docker](https://docs.docker.com/get-docker/). We 
store the images in AWS ECR.

Steps:
1. create requirements.txt and Dockerfile

![img.png](/pictures/onboarding_img_8.png)

2. create a new Repository in AWS ECR
3. build the image, tag it and push it to the Repository. The actual commands are in AWS

![img.png](/pictures/onboarding_img_9.png)

If several subprojects make up the project, and each should have its own image (for example, several lambda functions 
must be deployed), each dockerfile must be in separate folders. Also, use different requirements.txt only with the 
dependencies needed for the subproject. 

![img.png](/pictures/multiple_dockers.png)

In these cases, you have to specify the Dockerfile location in the build command:
```
docker build -t adv_upload -f src/adv_data/Dockerfile .
```

Important Docker Commands:
- `docker images`	Show all the downloaded images
- `docker pull redis:4.0`	Download image
- `docker run redis`	Run image --> container
- `docker ps`	Which containers are running
- `docker run -d redis`	Run in a detached mode
- `docker stop 91b839004766`	
- `docker start 91b839004766`	
- `docker ps -a`	Show all containers that are running or stopped
- `docker run -p6000:6379 -d redis`	Binding the 6379 docker port to the 6000 port on the host
- `docker logs zen_bartik`	Check out log files, you can use name or id
- `docker run -d -p6001:6379 --name redis-custom-name redis`	Name your container
- `docker exec -it 243422bf4714 /bin/bash`	Open a container for debugging. 	
- `docker network ls`	List existing networks
- `docker network create mongo-network`	Create own network
- `docker run -p 27017:27017 -d --net mongo-network mongo`	How to run something on the network -- here the user name and password environment variables are not added yet. 
- `docker-compose -f mongo.yaml up`	Run the containers -- it creates the network 
for example on the deployment server
- `docker-compose -f mongo.yaml down`	It also removes the created network

# AWS <a name="aws"></a>
We use many AWS services. Important practice: tag all AWS service with project key. Therefore, we can easily track the 
AWS cost of each project.

### Install AWS Cli


# AWS Lambda
Pack your application into a docker image and create the Lambda function from the image. Otherwise, it might be 
tricky to install dependencies. You probably need to upload the dependencies (Amazon Linux version) in zip files to 
Layers. Unfortunately, the limit of the total zip directory including dependencies is only 50 MB vs 10 GB for the 
container image limit. 

A few trigger events that we use frequently:
- API Gateway
- file is uploaded to S3
- EventBridge scheduled call
- SNS message
- SQS queue message



# APIs <a name="api"></a>
- fast API
- testing with Postman vs PyCharm
- API GateWay



# DataBases <a name="database"></a>
- AWS RDS - Postgres
- AWS RDS DynamoDB instead of Mongo
- AWS S3 
how to connect to DBs in PyCharm
pythonic way to write SQL code???
  
use dataclasses!!!


Appendix
===
---
# Sphinx
new version ------------

before start installing sphinx make sure you alreay chose virtual environment for the project


1. Installing packages :

```
  pip install Sphinx
  pip install Sphinx-autobuild
```
The installation method used above is described in more detail in Installation from PyPI
2. create a directory and name it docs

```
  mkdir docs
```
3. go to the docs folder and run the following

```
  sphinx-quickstart
```
This will present to you a series of questions required to create the basic directory and configuration layout for your project inside the docs folder. To proceed, answer each question as follows:

- Separate source and build directories (y/n) [n]: Write “n” (without quotes) and press Enter.

- Project name: Write “Your project name” (without quotes) and press Enter.

- Author name(s): Write “the author” (without quotes) and press Enter.

- Project release []: Write “the version of the project ” (without quotes) and press Enter.

- Project language [en]: Leave it empty (the default, English) and press Enter.

4. then go to index.rst and write some content

5. then back to the main directory by typing 
```
  cd ..
```
6. to generate the documentation type :
```
  sphinx-autobuild docs docs/_build/html 
```
and then you can open the given url on the browser





old version ----

Napoleon feature helps to use Numpy ang Google style comments properly

```
 pip install sphinxcontrib-napoleon
```


2. Without modifing all the settings on your own, we can just use the default settings:
Before that go to the root folder of the project and preferable create a doc/docs folder and step into it:
```
 sphinx-quickstart
```

Here a conf.py and index.py file is generated.
On command line answer the questions:
- I would adcise to seprate the build and source folders
- Release I used was 0.1
- All others up to you

All these things can be seen in the conf.rst.

3. Editing conf.rst and index.rst:
Open the conf.py from the source folder with some txt editor:
- Uncomment the path at the beginning and set to the root folder of the project
Would look like that considering you are in the root/docs/source:

```python
import os
import sys
sys.path.insert(0, os.path.abspath('../..'))
```


- To extensions add autdoc to create auto docs for html, latex etc. and also add napoleon extension, would look like:
```python
extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.napoleon',
]
```


- if you want to exclude any module, like test you can set them from the doc like:
```python
exclude_patterns = ['**tests**',]
```


In the index.rst set the maxdepth to 10 since the Ecactus structure is really deep:
   :maxdepth: 10

and add one line using 3 white spaces before that the same way as above, otherwise, it will not work:
      modules
This line points at a modules.rst file that will be generated in the next step and contains inforamtion about the different modules.


So the current file looks like:
Welcome to Ecactus's documentation!
***********************************

.. toctree::
   :maxdepth: 10
   :caption: Contents:

   modules

4. You need to generate rst files for the doc(for the toctree):
General code: sphinx-apidoc -o <OUTPUT_PATH> <MODULE_PATH>
Assumed you are already in the doc or docs folder, so the code would be:
sphinx-apidoc -o source ..
---> put all the rts files to the source folder and use all the folders/files to create these rst files

5. Now you can generate the html:
sphinx-build -b html sourcedir builddir
In your case the sourcedir is called source and the builddir is the build so change it
All the created html files put into the build folder.


You can even use: make html in that case an html folder will be created in the build folder with all the files.
It can be done in latex as well with ‘make latexpdf’ for that you need a latex builder.

Website of sphinx:
https://www.sphinx-doc.org/en/master/usage/quickstart.html


# EC2 -- 
download pem file and https://www.youtube.com/watch?v=8UqtMcX_kg0&ab_channel=StephaneMaarek

if you are on windows you have to convert the pem file to ppk with putty
https://www.youtube.com/watch?v=jv-dgOfFN4o&ab_channel=StephaneMaarek
download Patty here: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html