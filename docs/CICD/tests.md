# Implementing Tests

## Why Testing Your Project?

In the software development industry it has become a good practice to test your code and try achieving the most possible coverage. This practice helps with making a project that is maintainable, reliable and avoids problems like shipping new versions that break old features. It is important to follow the same practice on the MLOps spectrum by testing the code, the model and even the data. Making tests is somewhat very particular to the project, so this section will show a simple implementation of a testing pipeline using Pytest and Pre-commit, which should later be filled with tests that comprehend the specific project needs.

### Types of testing

It is important to notice that many projects will have different needs in terms of testing, so this division may not be precise for all projects, but in broad terms the main types of testing in MLOps are:

- **Software Testing:** Comprehends tests that make sure the code follows the project requirements. This is the type of tests normally implemented in DevOps, such as Unit Testing, Integration Testing, System Testing and Acceptance Testing.

- **Model Testing:** Comprehends tests that define that the model is working fine, such as testing that it can be trained, or that it can obtain a minimum score at some evaluation.

- **Data Testing:** Comprehends tests that check for data correctness. It is heavily dependant on the project requirements and can be focused on securing that the analyzed dataset follows a schema, contains enough data, and others. Many thing can be tested here depending on the necessities of the team.

## Using Pytest

First, we will be using a Python package called Pytest, which is a very popular choice for regular Software testing and can be used to implement all sorts of tests. We are going to build a simple a Pytest file that shows an example of testing the whole project and the team can later add more tests specific for their use cases.

In the src/tests/ folder you should see a Python file with a custom name with this content:

```Python
import pytest


def capital_case(x):
    return x.capitalize()


def test_capital_case():
    assert capital_case("semaphore") == "Semaphore"
```

This is a simple file that imports the Pytest package and shows a simple test. ```capital_case()``` is a function that could be defined in any place of the entire project, and ```test_capital_case()``` is a test function, which means it should be used to test the capital case function. To run all tests:

```bash
pytest
```

By running ```pytest``` on the command line Python finds all the files that have a function starting with the ```test_``` prefix  and run them. This means that ```capital_case()``` is not run directly and is only running inside ```test_capital_case()```.

## Configuring Pre-commit
Pre-commit is a tool that enables us to use Git Hooks, a particular feature of Git itself, not necessarily Github or GitLab, that enables us to run scripts and commands when performing certain actions such as commit, push, pull, etc. This CLI tool enables us to create a Git Hook easily to test our code before each commit, making it harder for a developer to add code to the Github repository with errors.

You should have a ```.pre-commit-config.yaml``` file at the root directory with:

```yaml
--- 
repos:
-
  repo: https://github.com/ambv/black
  rev: 20.8b1
  hooks: 
    - 
      id: black
      language_version: python3
  
-   repo: local
    hooks:
    -   id: python-tests
        name: pytests
        entry: pytest src/tests
        language: python
        additional_dependencies: [pre-commit, pytest, pandas, sklearn, matplotlib]
        always_run: true
        pass_filenames: false
```

This file can be used to configure a Git Hook that runs Pytest and Black Python formatter before each commit. 

So, to install this Git Hook use:
```bash
pre-commit install
```
Now, after you commit some changes in the repository you should see a message stating that all tests passed. If some of the tests didn't pass, the Git Hook won't let you commit until you fix the errors.

```bash
$ git commit -m "Example commit"

black....................................................................Passed
pytest-check.............................................................Passed
```


## Adding Software Testing

Now we are going to write some more tests for the project. One of the most necessary parts of testing in this project is on the Preprocess Pipeline, which comprehends the highest number of functions. So here we will be implementing a test the covers those functions, their ability to read and write files, and that a reduced dataset can be preprocessed with no problems.

Create a folder called ```preprocess/``` inside ```tests/```, and inside this folder a file called ```test_preprocess.py``` and another folder called ```test_data/```, in which we are going to add a simple dataset file ```testWeatherAUS.csv``` with the first 2 lines of the real dataset.

Now with this created, edit ```tests/preprocess/test_preprocess.py``` with some imports and an addition to path to be able to access the test dataset. 

```Python
import io
import builtins
import pytest
import pandas as pd
import sys
import os

# Parent Folder which contains test_data/
sys.path.append(
    os.path.dirname(os.path.dirname(os.path.dirname(os.path.realpath(__file__))))
)

# Preprocess Python file which contains our functions to be tested
import preprocess_data

FILE_NAME = "testWeatherAUS"
DATA_PATH = (
    os.path.dirname(os.path.realpath(__file__)) + "/test_data/" + FILE_NAME + ".csv"
)
PROCESSED_DATA_PATH = (
    os.path.dirname(os.path.realpath(__file__))
    + "/test_data/"
    + FILE_NAME
    + "_processed.csv"
)

```

Now, we are going to add unit testing for some of the functions inside ```preprocess_data.py```.

```Python
def test_count_nulls_by_line():
    # Tests function that counts number of nulls by line on a dataframe
    data = pd.DataFrame([[0, 2], [0, 1], [6, None]])
    assert preprocess_data.count_nulls_by_line(data).to_list() == [1, 0]


def test_null_percent():
    # Tests function that gets the percentage of nulls by line on a dataframe
    data = pd.DataFrame([[0, 2], [1, None]])
    assert preprocess_data.null_percent_by_line(data).to_list() == [0.5, 0]
```

Now that we made some examples of unit testing, we should add tests that comprehend the full pipeline, in this case checking if it runs smoothly or returns an error. Pretty general test that can be used to avoid some basic mistakes. This test creates a ```testWeatherAUS_processed.csv``` file.

 This test is going to be important for the next tests, so we will add a mark of dependency, which is a Pytest feature that can be used to tell Python that a test function should be run before another. In this case, any function that marks this function as a dependency will run after this one finishes.

```Python
@pytest.mark.dependency()
def test_preprocess():
    # Checks if running the preprocess function returns an error
    preprocess_data.preprocess_data(DATA_PATH)
```

Adding tests that make sure the file is created and is possible to read, which comprehends some system errors. It is possible to see that these functions have marked dependencies, which will make them run after the functions marked.

```Python
@pytest.mark.dependency(depends=["test_preprocess"])
def test_processed_file_created():
    #  Checks if the processed file was created during test_preprocess() and is accessible
    f = open(PROCESSED_DATA_PATH)


@pytest.mark.dependency(depends=["test_processed_file_created"])
def test_processed_file_format():
    # Checks if the processed file is in  the correct format (.csv) and can be transformed in dataframe
    try:
        pd.read_csv(PROCESSED_DATA_PATH)
    except:
        raise RuntimeError("Unable to open " + PROCESSED_DATA_PATH + " as dataframe")
```

Now that we have implemented our example of tests, we need to delete the file created, which is ```test_data/testWeatherAUS_processed.csv```. A ligature is a feature of Pytest that makes it possible to run code before or after the tests, which are run in the ```yield``` call on the code.

```Python
@pytest.fixture(scope="session", autouse=True)
def cleanup(request):
    # Runs tests then cleans up the processed file
    yield
    try:
        os.remove(PROCESSED_DATA_PATH)
    except:
        pass
```

Now, when you run a ```pytest```, or ```git commit`` which triggers pytest, all these tests should run.

### Adding More Test Files

It is important to note that it is very important to tests the functions that are being used in the scripts of model handling and data handling, such as the function ```get_variables()``` in the ```model.py```. Se we are going to follow the same structure and create a file ```tests/model/test_model.py```.

In this case we would like to test a large variety of different inputs, so we will be using ```pytest.mark.parametrize()``` which enables us to choose a large amount of inputs and expected results.



```Python
import sys
import os
import pytest
import pandas as pd

# Parent Folder
sys.path.append(
    os.path.dirname(os.path.dirname(os.path.dirname(os.path.realpath(__file__))))
)

# Model Python file
from model import get_variables

FILE_NAME = "testWeatherAUS"
PROCESSED_DATA_PATH = (
    os.path.dirname(os.path.dirname(os.path.realpath(__file__)))
    + "/test_data/"
    + FILE_NAME
    + "_processed.csv"
)


@pytest.mark.parametrize(
    "expected_X,expected_y",
    [
        (
            {
                "MinTemp": {0: 13.4, 1: 7.4},
                "MaxTemp": {0: 22.9, 1: 25.1},
                "Rainfall": {0: 0.6, 1: 0.0},
                "WindGustSpeed": {0: 44, 1: 44},
                "WindSpeed9am": {0: 20, 1: 4},
                "WindSpeed3pm": {0: 24, 1: 22},
                "Humidity9am": {0: 71, 1: 44},
                "Humidity3pm": {0: 22, 1: 25},
                "Pressure9am": {0: 1007.7, 1: 1010.6},
                "Pressure3pm": {0: 1007.1, 1: 1007.8},
                "Temp9am": {0: 16.9, 1: 17.2},
                "Temp3pm": {0: 21.8, 1: 24.3},
                "RainToday": {0: 0, 1: 0},
                "WindGustDir_W": {0: 1, 1: 0},
                "WindGustDir_WNW": {0: 0, 1: 1},
                "WindDir9am_NNW": {0: 0, 1: 1},
                "WindDir9am_W": {0: 1, 1: 0},
                "WindDir3pm_WNW": {0: 1, 1: 0},
                "WindDir3pm_WSW": {0: 0, 1: 1},
            },
            [0, 0],
        )
    ],
)
def test_get_variables(expected_X, expected_y):

    # Open CSV as DF
    data = pd.read_csv(PROCESSED_DATA_PATH)

    # Run Function
    X, y = get_variables(data, "RainTomorrow")

    assert (X.to_dict(), y.to_list()) == (expected_X, expected_y)

```

It is a big part of MLOps to test models and data, but this is also heavily reliant on the type of project the team is working and on the type of data they are handling. It is part of the job of the team to define the scope of the tests and choosing the next steps of tests that fulfill the project requirements and try to get the maximum possible coverage at of the code, model behavior and data correctness.



