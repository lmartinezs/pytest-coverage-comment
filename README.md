# Pytest Coverage Comment

![licience](https://img.shields.io/github/license/MishaKav/pytest-coverage-comment)
![version](https://img.shields.io/github/package-json/v/MishaKav/pytest-coverage-comment)
[![wakatime](https://wakatime.com/badge/user/f838c8aa-c197-42f0-b335-cd1d26159dfd/project/b1e64a51-e518-4b91-bb00-189ffdd444c6.svg)](https://wakatime.com/badge/user/f838c8aa-c197-42f0-b335-cd1d26159dfd/project/b1e64a51-e518-4b91-bb00-189ffdd444c6)

This action comments a pull request or commit with a HTML test coverage report.
The report is based on the coverage report generated by your test runner.
Note that this action does not run any tests, but expects the tests to have been run by another action already (support pytest only).

**Similar Action for Jest TEST**

---

After I see that action become popular, I made similar action (even better) for javascript/typescript that runs `jest`
[jest-coverage-comment](https://github.com/marketplace/actions/jest-coverage-comment)

---

You can add this action to your GitHub workflow for Ubuntu runners (e.g. runs-on: ubuntu-latest) as follows:

```yaml
- name: Pytest coverage comment
  uses: MishaKav/pytest-coverage-comment@main
  with:
    pytest-coverage-path: ./pytest-coverage.txt
    junitxml-path: ./pytest.xml
```

## Inputs

| Name                        | Required | Default                 | Description                                                                                                                                                                                                                                                                                                                                                                         |
| --------------------------- | -------- | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `github-token`              | ✓        | `${{github.token}}`     | An alternative GitHub token, other than the default provided by GitHub Actions runner                                                                                                                                                                                                                                                                                               |
| `pytest-coverage-path`      |          | `./pytest-coverage.txt` | The location of the txt output of pytest-coverage. Used to determine coverage percentage                                                                                                                                                                                                                                                                                            |
| `pytest-xml-coverage-path`  |          | ''                      | The location of coverage-xml from pytest-coverage (--cov-report "xml:coverage.xml)                                                                                                                                                                                                                                                                                                  |
| `coverage-path-prefix`      |          | ''                      | Prefix for path when link to files in comment                                                                                                                                                                                                                                                                                                                                       |
| `title`                     |          | `Coverage Report`       | Title for the coverage report. Useful for monorepo projects                                                                                                                                                                                                                                                                                                                         |
| `badge-title`               |          | `Coverage`              | Title for the badge icon                                                                                                                                                                                                                                                                                                                                                            |
| `hide-badge`                |          | false                   | Hide badge with percentage                                                                                                                                                                                                                                                                                                                                                          |
| `hide-report`               |          | false                   | Hide coverage report                                                                                                                                                                                                                                                                                                                                                                |
| `report-only-changed-files` |          | false                   | Show in report only changed files for this commit, and not all files                                                                                                                                                                                                                                                                                                                |
| `junitxml-path`             |          | ''                      | The location of the junitxml. Used to determine test numbers (passed, failed, etc)                                                                                                                                                                                                                                                                                                  |
| `junitxml-title`            |          | ''                      | Title for summary for junitxml                                                                                                                                                                                                                                                                                                                                                      |
| `create-new-comment`        |          | false                   | When false, will update the same comment, otherwise will publish new comment on each run.                                                                                                                                                                                                                                                                                           |
| `hide-comment`              |          | false                   | Hide the whole comment (use when you need only the `output`). Useful for auto-update bagdes in readme. See the [workflow](../main/.github/workflows/live-test.yml) for example                                                                                                                                                                                                      |
| `default-branch`            |          | `main`                  | This branch name is usefull when generate "coverageHtml", it points direct links to files on this branch (instead of commit).<br/>Usually "main" or "master"                                                                                                                                                                                                                        |
| `multiple-files`            |          | ''                      | You can pass array of titles and files to generate single comment with table of results.<br/>Single line should look like `Title, ./path/to/pytest-coverage.txt, ./path/to/pytest-junit.xml`<br/> example:<br/> `My Title 1, ./data/pytest-coverage_3.txt, ./data/pytest_1.xml`<br/>**Note:** In that mode the `output` for `coverage` and `color` will be for the first file only. |
| `remove-link-from-badge`    |          | false                   | When true, it will remove the link from badge to readme                                                                                                                                                                                                                                                                                                                             |
| `unique-id-for-comment`     |          | ''                      | When running in a matrix, pass the matrix value, so each comment will be updated its own comment `unique-id-for-comment: ${{ matrix.python-version }}`                                                                                                                                                                                                                              |

## Output Variables

| Name                 | Example                        | Description                                                                           |
| -------------------- | ------------------------------ | ------------------------------------------------------------------------------------- |
| `coverage`           | 30%                            | Percentage of the coverage, get from `pytest-cov`                                     |
| `color`              | red                            | Color of the percentage. You can see the whole list of [badge colors](#badges-colors) |
| `coverageHtml`       | ...                            | Html with links to files of missing lines. See the [output-example](#output-example)  |
| `summaryReport`      | ...                            | Markdown with summaryof: Tests/Skipped/Failures/Errors/Time                           |
| `warnings`           | 2441                           | Number of warnings, get from pytest-cov                                               |
| `tests`              | 109                            | Total number of tests, get from `junitxml`                                            |
| `skipped`            | 2                              | Total number of skipped tests, get from `junitxml`                                    |
| `failures`           | 1                              | Total number of tests with failures, get from `junitxml`                              |
| `errors`             | 0                              | Total number of tests with errors, get from `junitxml`                                |
| `time`               | 0.583                          | Seconds the took to run all the tests, get from `junitxml`                            |
| `notSuccessTestInfo` | [example](#notSuccessTestInfo) | Info from testcase that has failures/errors/skipped, get from `junitxml`              |

### notSuccessTestInfo

the format will be JSON.stringify in current structure

```json
{
  "failures": [{ "classname": "...", "name": "..." }],
  "errors": [{ "classname": "...", "name": "..." }],
  "skipped": [{ "classname": "...", "name": "..." }],
  "count": 3
}
```

## Output example

<img alt="Coverage" src="https://img.shields.io/badge/Coverage-30%25-red.svg" /><br/><details><summary>Coverage Report</summary><table><tr><th>File</th><th>Stmts</th><th>Miss</th><th>Cover</th><th>Missing</th></tr><tbody><tr><td colspan="5"><b>functions/example_completed</b></td></tr><tr><td>&nbsp; &nbsp;<a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_completed/example_completed.py">example_completed.py</a></td><td>64</td><td>19</td><td>70%</td><td><a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_completed/example_completed.py#L33">33</a>, <a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_completed/example_completed.py#L39-L45">39&ndash;45</a>, <a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_completed/example_completed.py#L48-L51">48&ndash;51</a>, <a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_completed/example_completed.py#L55-L58">55&ndash;58</a>, <a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_completed/example_completed.py#L65-L70">65&ndash;70</a>, <a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_completed/example_completed.py#L91-L92">91&ndash;92</a></td></tr><tr><td colspan="5"><b>functions/example_manager</b></td></tr><tr><td>&nbsp; &nbsp;<a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_manager/example_manager.py">example_manager.py</a></td><td>44</td><td>11</td><td>75%</td><td><a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_manager/example_manager.py#L31-L33">31&ndash;33</a>, <a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_manager/example_manager.py#L49-L55">49&ndash;55</a>, <a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_manager/example_manager.py#L67-L69">67&ndash;69</a></td></tr><tr><td>&nbsp; &nbsp;<a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_manager/example_static.py">example_static.py</a></td><td>40</td><td>2</td><td>95%</td><td><a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/example_manager/example_static.py#L60-L61">60&ndash;61</a></td></tr><tr><td colspan="5"><b>functions/my_exampels</b></td></tr><tr><td>&nbsp; &nbsp;<a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/my_exampels/example.py">example.py</a></td><td>20</td><td>20</td><td>0%</td><td><a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/my_exampels/example.py#L1-L31">1&ndash;31</a></td></tr><tr><td colspan="5"><b>functions/resources</b></td></tr><tr><td>&nbsp; &nbsp;<a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/resources/resources.py">resources.py</a></td><td>26</td><td>26</td><td>0%</td><td><a href="https://github.com/MishaKav/pytest-coverage-comment/blob/680f562642190a6a28f6c54785c767e2586b44b8/functions/resources/resources.py#L1-L37">1&ndash;37</a></td></tr><tr><td><b>TOTAL</b></td><td><b>1055</b></td><td><b>739</b></td><td><b>30%</b></td><td>&nbsp;</td></tr></tbody></table></details>

| Tests | Skipped | Failures | Errors   | Time               |
| ----- | ------- | -------- | -------- | ------------------ |
| 109   | 2 :zzz: | 1 :x:    | 0 :fire: | 0.583s :stopwatch: |

## Example usage

The following is an example GitHub Action workflow that uses the Pytest Coverage Comment to extract the coverage report to comment at pull request:

```yaml
# This workflow will install dependencies, create coverage tests and run Pytest Coverage Comment
# For more information see: https://github.com/MishaKav/pytest-coverage-comment/
name: pytest-coverage-comment
on:
  pull_request:
    branches:
      - '*'
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Set up Python 3.8
        uses: actions/setup-python@v2
        with:
          python-version: 3.8

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install flake8 pytest pytest-cov
          if [ -f requirements.txt ]; then pip install -r requirements.txt; fi

      - name: Build coverage file
        run: |
          pytest --junitxml=pytest.xml --cov-report=term-missing:skip-covered --cov=app tests/ | tee pytest-coverage.txt

      - name: Pytest coverage comment
        uses: MishaKav/pytest-coverage-comment@main
        with:
          pytest-coverage-path: ./pytest-coverage.txt
          junitxml-path: ./pytest.xml
```

Example GitHub Action workflow that uses coverage percentage as output (see the [live workflow](../main/.github/workflows/live-test.yml))

```yaml
- name: Pytest coverage comment
  id: coverageComment
  uses: MishaKav/pytest-coverage-comment@main
  with:
    pytest-coverage-path: ./pytest-coverage.txt
    junitxml-path: ./pytest.xml

- name: Check the output coverage
  run: |
    echo "Coverage Percantage - ${{ steps.coverageComment.outputs.coverage }}"
    echo "Coverage Color - ${{ steps.coverageComment.outputs.color }}"
    echo "Coverage Html - ${{ steps.coverageComment.outputs.coverageHtml }}"
    echo "Summary Report - ${{ steps.coverageComment.outputs.summaryReport }}"

    echo "Coverage Warnings - ${{ steps.coverageComment.outputs.warnings }}"

    echo "Coverage Errors - ${{ steps.coverageComment.outputs.errors }}"
    echo "Coverage Failures - ${{ steps.coverageComment.outputs.failures }}"
    echo "Coverage Skipped - ${{ steps.coverageComment.outputs.skipped }}"
    echo "Coverage Tests - ${{ steps.coverageComment.outputs.tests }}"
    echo "Coverage Time - ${{ steps.coverageComment.outputs.time }}"
    echo "Not Success Test Info - ${{ steps.coverageComment.outputs.notSuccessTestInfo }}"
```

Example GitHub Action workflow that get coverage report from coverage-xml instead of coverage.txt
`pytest --cov-report "xml:coverage.xml" --cov=src tests/`

```yaml
- name: Pytest coverage comment
  uses: MishaKav/pytest-coverage-comment@main
  with:
    pytest-xml-coverage-path: ./coverage.xml
```

Example GitHub Action workflow that passes all params to Pytest Coverage Comment

```yaml
- name: Pytest coverage comment
  uses: MishaKav/pytest-coverage-comment@main
  with:
    pytest-coverage-path: ./path-to-file/pytest-coverage.txt
    pytest-xml-coverage-path: ./path-to-file/coverage.xml
    title: My Coverage Report Title
    badge-title: My Badge Coverage Title
    hide-badge: false
    hide-report: false
    create-new-comment: false
    hide-comment: false
    report-only-changed-files: false
    remove-link-from-badge: false
    unique-id-for-comment: python3.8
    junitxml-path: ./path-to-file/pytest.xml
    junitxml-title: My JUnit Xml Summary Title
```

![image](https://user-images.githubusercontent.com/289035/126039976-3f1bf8dd-5a6b-4103-8548-fc3eecc377d7.png)

Example GitHub Action workflow that runs pytest inside **docker**
It will generate `pytest-coverage.txt` and `pytest.xml` in `/tmp` directory inside docker and share `/tmp` directory with GitHub workspace.

```yaml
- name: Run unit tests (pytest)
  run: |
    docker run -v /tmp:/tmp $IMAGE_TAG python3 -m pytest --cov-report=term-missing:skip-covered --junitxml=/tmp/pytest.xml --cov=src tests/ | tee /tmp/pytest-coverage.txt

- name: Pytest coverage comment
  uses: MishaKav/pytest-coverage-comment@main
  with:
    pytest-coverage-path: /tmp/pytest-coverage.txt
    junitxml-path: /tmp/pytest.xml
```

Example GitHub Action workflow that uses multiple files mode (see the [live workflow](../main/.github/workflows/multiple-files.yml))

```yaml
- name: Pytest coverage comment
  uses: MishaKav/pytest-coverage-comment@main
  with:
    multiple-files: |
      My Title 1, ./data/pytest-coverage_3.txt, ./data/pytest_junit_1.xml
      My Title 2, ./data/pytest-coverage_4.txt, ./data/pytest_junit_2.xml
```

Example GitHub Action workflow that will update your `README.md` with coverage report, only on merge to `main` branch (see the [update-coverage-on-readme workflow](../main/.github/workflows/update-coverage-on-readme.yml))
All you need is to add in your `README.md` the following lines wherever you want.
If your coverage html report will not change, it wouldn't push any changes to readme file.

```html
<!-- Pytest Coverage Comment:Begin -->
<!-- Pytest Coverage Comment:End -->
```

```yaml
name: Update Coverage on Readme
on:
  push:
jobs:
  update-coverage-on-readme:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          persist-credentials: false # otherwise, the token used is the GITHUB_TOKEN, instead of your personal token
          fetch-depth: 0 # otherwise, you will failed to push refs to dest repo

      - name: Pytest coverage comment
        if: ${{ github.ref == 'refs/heads/main' }}
        id: coverageComment
        uses: MishaKav/pytest-coverage-comment@main
        with:
          hide-comment: true
          pytest-coverage-path: ./data/pytest-coverage_4.txt

      - name: Update Readme with Coverage Html
        if: ${{ github.ref == 'refs/heads/main' }}
        run: |
          sed -i '/<!-- Pytest Coverage Comment:Begin -->/,/<!-- Pytest Coverage Comment:End -->/c\<!-- Pytest Coverage Comment:Begin -->\n\${{ steps.coverageComment.outputs.coverageHtml }}\n<!-- Pytest Coverage Comment:End -->' ./README.md

      - name: Commit & Push changes to Readme
        if: ${{ github.ref == 'refs/heads/main' }}
        uses: actions-js/push@master
        with:
          message: Update coverage on Readme
          github_token: ${{ secrets.GITHUB_TOKEN }}
```

## Result example

Collapsed comment
![Result Collapse Example](https://user-images.githubusercontent.com/289035/120536428-c7664a80-c3ec-11eb-9cce-3ac53343fac4.png)

Expanded comment
![Result Expand Example](https://user-images.githubusercontent.com/289035/120536607-f8df1600-c3ec-11eb-9f49-c6d7571e43ac.png)

Multiple Files Mode (can be useful on mono-repo projects)
![Result Multiple Files Mode Example](https://user-images.githubusercontent.com/289035/122121939-ddd0c500-ce34-11eb-8546-89a8a769e065.png)

## Badge Colors

| Badge                                                                           | Range    |
| ------------------------------------------------------------------------------- | -------- |
| ![Coverage 0-40](https://img.shields.io/badge/Coverage-20%25-red.svg)           | 0 - 40   |
| ![Coverage 40-60](https://img.shields.io/badge/Coverage-50%25-orange.svg)       | 40 - 60  |
| ![Coverage 60-80](https://img.shields.io/badge/Coverage-70%25-yellow.svg)       | 60 - 80  |
| ![Coverage 80-90](https://img.shields.io/badge/Coverage-85%25-green.svg)        | 80 - 90  |
| ![Coverage 90-100](https://img.shields.io/badge/Coverage-95%25-brightgreen.svg) | 90 - 100 |

## Auto updating badge on README

If you want auto-update the coverage badge on your Readme, you can see the [workflow](../main/.github/workflows/live-test.yml)
![Auto Updating Bagde](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/MishaKav/5e90d640f8c212ab7bbac38f72323f80/raw/pytest-coverage-comment__main.json)

## 🤝 Contributing [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

We welcome all contributions. You can submit any ideas as [pull requests](https://github.com/MishaKav/pytest-coverage-comment/pulls) or as [GitHub issues](https://github.com/MishaKav/pytest-coverage-comment/issues) and have a good time! :)

## Our Contibutors

<a href="https://github.com/MishaKav/pytest-coverage-comment/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=MishaKav/pytest-coverage-comment" alt="Contibutors" />
</a>
