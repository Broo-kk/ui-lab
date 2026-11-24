# UI lab results

Tool parts that I chose: Part 1 Selenium IDE, Part 2 Selenium Java, Part 3 Robot Framework, Part 4 Cypress

## Part 0
- Step 0.1, Chrome version: Google Chrome 157.0.8103.80
- Step 0.3, ChromeDriver version: ChromeDriver 157.0.8103.80
- Step 0.4, locator of Pizza, Please!: name=pizza-please
- Step 0.4, locator of Excluded tools: css=select
- Step 0.4, locator of Must be vegetarian: id=default-checkbox
- Step 0.4, locator of Custom Pizza Name: css=input#pizza-name
- Step 0.5, pizza name, tool and calories of the run by hand: Lab Pizza 20261125-000538, Scissors, 450 calories

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1    | Heading was "Looking to break out of your pizza routine?" | Pass |
| 2    | The four advanced fields showed up | Pass |
| 3    | 600, Knife, vegetarian ticked, name Lab Pizza 20261125-000538 | Pass |
| 4    | Same name, Tool: Scissors, Calories per slice: 450 | Pass |
| 5    | "Please log in first." | Pass |
| 6    | "Latest pizza recommendations" | Pass |
| 7    | Lab Pizza 20261125-000538 (tool=Scissors, ingredients_number=7) (newest) | Pass |

## Part 1: Selenium IDE
- Firefox version and Node.js version: Firefox 161.0, Node.js v26.13.1, npm 11.20.0
- Step 1.4, the line of the failed check and the line Tests: Actual value 'Received!' did not match 'Received' during assertText:id=message:Received. Tests: 1 failed, 1 passed, 2 total
- Step 1.5, target chosen for Submit, and why the recorded one was fragile: xpath=//button[@type='submit']. css=.btn is only a class, so another button with that class would get clicked.
- Step 1.6, commands changed, added or deleted after recording: Added waits and checks. Changed open to the full URL. Replaced the typed name with a script that makes Lab Pizza plus the date and time, then echo prints it.
- Step 1.6, the pizza names of run 1 and run 2, from the lines info: echo: Lab Pizza 20261125-001217 and Lab Pizza 20261125-001250
- Step 1.6, the line Tests: of both runs: Tests: 2 passed, 2 total
- Step 1.7, one line of QP01customerTest.java that needs rewriting, and why: assertThat(value, is("${pizzaName}")) checks the text ${pizzaName}, not the real pizza name. sendKeys("600") also needs a clear first, or the box becomes 1000600.

## Part 2: Selenium WebDriver in Java
- Step 2.4, the failure line for the wrong title: FirstTest.webFormOpens:20 expected: <Web Form> but was: <Web form>
- Step 2.15, QP01Test run 1, result and time: passed, 1 test, about 10.0 s
- Step 2.15, QP01Test run 2, result and time: passed, 1 test, about 9.5 s
- Step 2.20, full run 1, time and number of tests: passed, 5 tests, about 20.6 s
- Step 2.20, full run 2, time and number of tests: passed, 5 tests, about 21.0 s

## Part 3: Robot Framework
- robot --version: Robot Framework 7.5.1 (Python 3.15.2 on win32)
- Step 3.2, failure message for the wrong title: Title should have been 'Web Form' but was 'Web form'.
- Step 3.8, run 1, result, elapsed time and pizza name: PASS, 00:00:09, Lab Pizza 20261125-000729
- Step 3.8, run 2, result, elapsed time and pizza name: PASS, 00:00:09, Lab Pizza 20261125-000738

## Part 4: Cypress
- node --version, npm --version and npx cypress version: v26.13.1, 11.20.0, Cypress 16.2.1
- Step 4.3, failure message for the wrong title: Timed out retrying after 10000ms. actual 'Web form', expected 'Web Form'
- Step 4.6, tool and calories in the answer of POST /api/pizza: Pizza cutter, 425 (an earlier run was Pizza cutter and 525)
- Step 4.7, the line of the admin list that shows your pizza: Lab Pizza 20261125-001526 (tool=Pizza cutter, ingredients_number=7) (newest)
- Step 4.8, run 1, result, duration and pizza name: passed, 00:10, Lab Pizza 20261125-001408
- Step 4.8, run 2, result, duration and pizza name: passed, 00:11, Lab Pizza 20261125-001526

## Part B
1. Max Calories per Slice uses the same id as the other two number boxes. $("#floating_filled").length was 3, so the id cannot pick the right box. I used the label text and then the input next to it, and that found one box. A unique id such as max-calories would let the test use an id, and it would still work if the label text changes.

2. Step 7 checks that the tool in the admin list is the same tool the recommendation showed. That is a consistency oracle, because the expected tool comes from the page, not from a value I typed in. It would catch a bug where the customer sees Scissors but the admin list saves Knife. On my hand run both sides said Scissors.

3. The answer on the page shows the name, dough, ingredients, tool and calories, but it does not say vegetarian. So the test cannot check that from the page text. The POST /api/pizza response has a vegetarian field, and the test would need that field to be true. The page could also show the word vegetarian.

4. Selenium IDE used wait for element visible, up to 10 seconds. Both runs passed in about 6 seconds and the pizza name was on the info: echo line. Java used WebDriverWait for 10 seconds. QP01Test passed in about 10 s and 9.5 s, and the full 5 tests passed in about 21 s. The surefire file gives the time, and the screenshots show the page. Robot waited with Wait Until, timeout 10 seconds. Both runs passed in 9 seconds, and report.html shows each step with the pictures. Cypress retries by itself. The wrong title failed after 10000 ms and printed both titles. The two runs passed in 00:10 and 00:11, and the video shows the commands next to the page.

5. I would use Cypress for Project 2. The commands wait by themselves, so the bad title check retried for 10 seconds and QP-01 still finished in under 10 seconds. npx cypress run also saved a video, so I did not need a separate screen recording.

6. The recorded Submit target css=.btn is fragile because btn is a class for how the button looks. A new button with that class, placed first, would be clicked. xpath=//button[@type='submit'] uses the type, so the colour or the order of the buttons can change and the test still clicks Submit.

7. In QP01Test the calories xpath is inside the test, so that file has to change. QP01PageObjectTest does not have the xpath. It only calls HomePage. The locator is the constant MAX_CALORIES, so I change that one line and the page-object test stays the same.

8. I would change ${MAX CALORIES} in resources/quickpizza.resource. The test file only calls Fill In The Pizza Request, so the locator stays in the resource file and pizza_scenario.robot does not change.

9. Cypress clears cookies before every test, and QuickPizza uses a cookie to stay on one server. Steps 6 and 7 have to be in the same test as steps 1 to 5. A second test would log in on a new session, so the pizza might not be in that admin list. The tool alias would also be gone, so step 7 could not compare it.
