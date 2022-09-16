# Playwright

https://playwright.dev/docs/writing-tests

### Tests

`tests/e2e/example.spec.js`

```javascript
// @ts-check
const {test, expect} = require('@playwright/test');

test.beforeEach(async ({page}) => {
  /**
   * navigate to the login screen
   */
  await page.goto('/redaxo/index.php');

  /**
   * check if the login input is present
   * add username
   */
  await page.locator('input[id=rex-id-login-user]').fill('nightwatch_username');

  /**
   * check if the password input is present
   * add password
   */
  await page.locator('input[id=rex-id-login-password]').fill('nightwatch_password');
  await page.locator('input[id=rex-id-login-password]').press('Enter');

  /**
   * check if the session cookie is available

   /**
   * check if we are logged in to the backend
   */
  await expect(page).toHaveURL('/redaxo/index.php?page=structure');
});

test('Test Addon functionality', async ({page}) => {
  /**
   * check if the username-link is available
   * click the username-link
   */
  await page.locator('.navbar a.rex-username').click();
  await expect(page).toHaveURL('/redaxo/index.php?page=profile');
  // do stuff...

  /**
   * TODO: write some tests...
   */
});
```

## Resultat

```shell
Running 1 test using 1 worker

  ok 1 [chromium] › example.spec.js:32:1 › Test Addon functionality (4s)

  1 passed (6s)
```