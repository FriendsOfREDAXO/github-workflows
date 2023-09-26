# Nightwatch.js

https://nightwatchjs.org/

### Tests

`tests/e2e/example.js`

```javascript
describe('Test Titel', () => {
  /**
   * login before the actual test
   */
  before(browser => {
    /**
     * navigate to the login screen
     */
    browser.navigateTo('/redaxo/index.php');

    /**
     * check if the login input is present
     * add username
     */
    browser.assert.elementPresent('input[id=rex-id-login-user]');
    browser.sendKeys('input[id=rex-id-login-user]', 'nightwatch_username');

    /**
     * check if the password input is present
     * add password, submit form
     */
    browser.assert.elementPresent('input[id=rex-id-login-password]');
    browser.sendKeys('input[id=rex-id-login-password]', ['nightwatch_password', browser.Keys.ENTER]);

    /**
     * check if the session cookie is available
     */
    browser.getCookie('PHPSESSID', function callback(result) {
      this.assert.equal(result.name, 'PHPSESSID');
    });

    browser.pause(500);

    /**
     * check if we are logged in to the backend
     */
    browser.assert.urlContains('/redaxo/index.php?page=structure');
  });

  it('Test Addon functionality', function(browser) {
    /**
     * check if the username-link is available
     * click the username-link
     */
    browser.assert.elementPresent('.navbar a.rex-username');
    browser.click('.navbar a.rex-username');
    browser.assert.urlContains('/redaxo/index.php?page=profile');
    // do stuff...

    /**
     * TODO: write some tests...
     */
  });

  /**
   * close the browser
   */
  after(browser => {
    browser.end();
  });
});
```

## Resultat

```shell
[Test Titel] Test Suite
──────────────────────────────────────────────
i Connected to GeckoDriver on port 4444 (2635ms).
  Using: firefox (104.0.2) on WINDOWS.

  √ Testing if element <input[id=rex-id-login-user]> is present (12ms)
  √ Testing if element <input[id=rex-id-login-password]> is present (15ms)
  √ Passed [equal]: PHPSESSID == PHPSESSID
  √ Testing if the URL contains '/redaxo/index.php?page=structure' (20ms)

  Running Test Addon functionality:
────────────────────────────────────────────────────────────────────────────────────────────────────────
  √ Testing if element <.navbar a.rex-username> is present (513ms)
  √ Testing if the URL contains '/redaxo/index.php?page=profile' (7ms)

OK. 2 assertions passed. (1.484s)
```