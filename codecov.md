# Code Coverage

TSC enables [codecov](https://codecov.io/) for the entire [OCA GitHub organization](https://github.com/opencybersecurityalliance), and any repo in the organization will be automatically covered.

For a OCA repo to start use codecov, the only things need to be done:

1. generate coverage report, and
2. upload it to codecov.

The [codecov team bot](https://docs.codecov.com/docs/team-bot) will automatically comment on a PR regarding the coverage if the two steps are automated for a pull request (PR), e.g., in the continuous integration (CI) procedure like unit testing for PR.

### A Python Project Code Coverage Setup Example

1. create a GitHub workflow in `.github/workflows/` folder, e.g., `.github/workflows/unit-testing.yml`.
2. put the following in the workflow file:
```
name: CodeCov
on: [push, pull_request]
jobs:
  run:
    runs-on: ubuntu-latest
    env:
      OS: ubuntu-latest
      PYTHON: '3.9'
    steps:
    - uses: actions/checkout@v3
    - name: Set up Python
      uses: actions/setup-python@v3
      with:
        python-version: '3.9'
    - name: Install repo
      run: |
        python -m pip install --upgrade pip
        python -m pip install --upgrade setuptools
        python -m pip install .
    - name: Unit testing with coverage report
      run: |
        python -m pip install pytest
        python -m pip install pytest-cov
        python -m pytest -vv --cov-report=xml
    - name: Upload coverage to Codecov
      uses: codecov/codecov-action@v3
```
