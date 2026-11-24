# ui-lab

Automated UI tests for ITCS386. The four tool parts are Selenium IDE, Selenium WebDriver in Java, Robot Framework and Cypress. Each one automates test case QP-01 on QuickPizza. Set the environment variable `CHROMEDRIVER` to the ChromeDriver executable before the Selenium IDE, Java and Robot runs. Cypress starts Chrome itself.

## selenium-ide

The folder holds the Selenium IDE project `ui-lab.side` and the Java export `QP01customerTest.java`. From this folder, with the ChromeDriver directory on `PATH`:

```
npx --yes selenium-side-runner@4.0.13 ui-lab.side
```

## selenium-java

Maven project for the WebDriver tests, the page objects, the screenshots and the surefire text report in `reports`. From this folder:

```
mvn test -Dheadless=true
```

## robot

Robot Framework tests, the QuickPizza keywords in `resources/quickpizza.resource`, and the evidence of two headless runs in `results/run1` and `results/run2`. Create the virtual environment, install `requirements.txt`, activate it, and run the tests from this folder. On Python 3.15, set `PYTHONIOENCODING` to `utf-8` first.

```
robot first.robot web_form.robot pizza_scenario.robot
```

## cypress

Cypress specs for the web form and for QP-01, plus the videos and screenshots of a full run. From this folder, after `npm install`:

```
npx cypress run --browser chrome
```
