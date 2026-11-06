# Amazon-Automation

A small Python + Selenium 4 script that opens Amazon.com in Chrome, searches for a product, opens the first search result and clicks a button on the product page.

## What it does

[`main.py`](main.py) defines an `AmazonCartAutomation` class that:

1. Starts a Chrome WebDriver session, clears cookies and maximizes the window
2. Opens `https://www.amazon.com/` and types the product name into the search box (`id="twotabsearchtextbox"`)
3. Waits (up to 10 s) for the first result, located by an absolute XPath, and clicks it
4. Waits (up to 100 s) for the element with `id="wishListMainButton"` to become clickable and clicks it
5. Quits the browser

The product is hard-coded in the `__main__` block as `"Wireless Earbuds"`.

Despite the class name, step 4 targets `wishListMainButton`, which on Amazon product pages is the "Add to List" (wish list) button, not "Add to Cart". The script does not sign in, so Amazon may ask you to log in at that point.

## Tech stack

- Python 3 (written against 3.10)
- Selenium 4
- Google Chrome + ChromeDriver

## Setup

Prerequisites: Python 3 and Google Chrome.

```bash
git clone https://github.com/AkshayTripathiAI/Amazon-Automation.git
cd Amazon-Automation
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install selenium
```

There is no `requirements.txt`; Selenium is the only dependency.

### Browser driver

The script creates `ChromeService()` without a driver path, so Selenium looks for a driver itself. With Selenium 4.6 or newer, Selenium Manager downloads a ChromeDriver that matches your installed Chrome automatically. On older Selenium versions, put a matching `chromedriver` on your `PATH`.

The `chromedriver_path` variable in `main.py` and the `Drivers/chromedriver` binary in the repo are left over from the original Linux setup. The path is only written to an environment variable that Selenium does not read, so it has no effect.

## Usage

```bash
python main.py
```

To search for a different product, change the argument passed to `search_and_add_to_cart(...)` at the bottom of `main.py`.

## Project structure

```
Amazon-Automation/
├── main.py              # AmazonCartAutomation: search, open first result, click button
├── Drivers/
│   └── chromedriver     # bundled ChromeDriver binary (Linux); not used by main.py
└── README.md
```

## Note

This script automates a third-party website. It depends on Amazon's page layout (the search box ID, an absolute XPath for the first result and the button ID), so it breaks whenever Amazon changes its pages or shows a CAPTCHA, and automated access may be subject to Amazon's conditions of use. It is intended for learning Selenium.
