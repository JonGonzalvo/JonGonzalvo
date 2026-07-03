# Selenium Automation

Basic login and search test in Python + Selenium WebDriver.

## Setup

```bash
# Install dependencies
pip install selenium webdriver-manager

# Run tests
python -m pytest tests/ -v
```

## Requirements

- Python 3.8+
- Chrome browser installed
- `webdriver-manager` handles ChromeDriver automatically — no manual download needed

## Sample Test

```python
# tests/test_login.py
import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

BASE_URL = "http://automationpractice.com"

@pytest.fixture
def driver():
    options = webdriver.ChromeOptions()
    options.add_argument("--headless")   # remove for visible browser
    options.add_argument("--no-sandbox")
    driver = webdriver.Chrome(
        service=Service(ChromeDriverManager().install()),
        options=options
    )
    driver.implicitly_wait(5)
    yield driver
    driver.quit()

class TestLogin:
    def test_valid_login(self, driver):
        driver.get(f"{BASE_URL}/index.php?controller=authentication")
        driver.find_element(By.ID, "email").send_keys("testuser@example.com")
        driver.find_element(By.ID, "passwd").send_keys("Test@1234")
        driver.find_element(By.ID, "SubmitLogin").click()
        
        wait = WebDriverWait(driver, 10)
        account = wait.until(EC.presence_of_element_located((By.CLASS_NAME, "account")))
        assert account.is_displayed(), "Account link not visible after login"

    def test_invalid_login_shows_error(self, driver):
        driver.get(f"{BASE_URL}/index.php?controller=authentication")
        driver.find_element(By.ID, "email").send_keys("wrong@email.com")
        driver.find_element(By.ID, "passwd").send_keys("wrongpassword")
        driver.find_element(By.ID, "SubmitLogin").click()
        
        error = driver.find_element(By.CSS_SELECTOR, ".alert.alert-danger")
        assert "Authentication failed" in error.text
```

## Notes

- Tests use `--headless` flag by default for CI/CD compatibility; remove for visual debugging
- `webdriver-manager` keeps ChromeDriver in sync with your Chrome version automatically
- `implicitly_wait(5)` handles most dynamic page loads; use `WebDriverWait` for specific elements
