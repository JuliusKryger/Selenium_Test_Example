1. Create a pages directory:

bash

mkdir src/pages

2. Implement a basic page object:

For simplicity, let's assume you're testing a login page. Create a LoginPage.ts inside the pages directory.

src/pages/LoginPage.ts:

typescript

import { By, WebDriver, until } from 'selenium-webdriver';

export class LoginPage {
  private readonly driver: WebDriver;
  private readonly url: string = 'http://localhost:3000/login'; // Change this to your actual login page URL

  private usernameLocator = By.id('username');
  private passwordLocator = By.id('password');
  private loginButtonLocator = By.id('login-button');

  constructor(driver: WebDriver) {
    this.driver = driver;
  }

  async open() {
    await this.driver.get(this.url);
  }

  async enterUsername(username: string) {
    const usernameField = await this.driver.findElement(this.usernameLocator);
    await usernameField.sendKeys(username);
  }

  async enterPassword(password: string) {
    const passwordField = await this.driver.findElement(this.passwordLocator);
    await passwordField.sendKeys(password);
  }

  async clickLogin() {
    const loginButton = await this.driver.findElement(this.loginButtonLocator);
    await loginButton.click();
  }

  // Add more methods as needed for other interactions with the login page
}

3. Use the page object in your test:

Update the test in src/test.ts to utilize the LoginPage object.

src/test.ts:

typescript

import { Builder } from 'selenium-webdriver';
import { describe, expect, test } from '@jest/globals';
import { LoginPage } from './pages/LoginPage';

describe("Frontend Test with Selenium using POM", () => {

  let driver;
  let loginPage;

  beforeAll(async () => {
    driver = await new Builder().forBrowser('chrome').build();
    loginPage = new LoginPage(driver);
  });

  afterAll(async () => {
    await driver.quit();
  });

  test("Login with valid credentials", async () => {
    await loginPage.open();
    await loginPage.enterUsername('testuser'); // Replace with your test data
    await loginPage.enterPassword('testpass'); // Replace with your test data
    await loginPage.clickLogin();

    // Here you'd usually add some assertions, e.g.:
    // const dashboardTitle = ...
    // expect(dashboardTitle).toBe('Dashboard');
  });

  // You can easily add more tests that also interact with the LoginPage or introduce new Page Objects for other parts of your application.
});

Recap and Additional Tips:

    Reusability: Page objects allow you to encapsulate and reuse UI interactions. If the UI changes in the future, you only have to update the corresponding page object, not each individual test.

    Clean Separation: The actual test doesn't know about UI details (like locators). This separation of concerns makes the test more readable and focused on the testing logic.

    Extendability: You can easily create more page objects for other parts of your application, following the pattern established with LoginPage.

    Maintainability: With this setup, maintenance becomes simpler. For example, if the login page's UI changes, only the LoginPage object needs to be updated.

    Additional Methods: In real-world scenarios, the page object may have more methods that allow checking certain page states, validating messages, navigating to other pages, and more.



    src/
|-- tests/
|   |-- pageObjects/
|   |   |-- LoginPage.ts
|   |   |-- DashboardPage.ts
|   |   |-- SettingsPage.ts
|   |   |-- ... (other page objects)
|   |
|   |-- utils/
|   |   |-- CustomWait.ts
|   |   |-- TestData.ts
|   |   |-- ... (other utilities)
|   |
|   |-- loginTests.ts
|   |-- dashboardTests.ts
|   |-- settingsTests.ts
|   |-- ... (other test files)

