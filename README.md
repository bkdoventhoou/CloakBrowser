# CloakBrowser

> A privacy-focused browser automation and fingerprint management tool.

[![CI](https://github.com/CloakHQ/CloakBrowser/actions/workflows/ci.yml/badge.svg)](https://github.com/CloakHQ/CloakBrowser/actions/workflows/ci.yml)
[![PyPI version](https://badge.fury.io/py/cloakbrowser.svg)](https://badge.fury.io/py/cloakbrowser)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

CloakBrowser is a Python library that wraps browser automation tools (Playwright/Selenium) with built-in fingerprint spoofing, proxy rotation, and anti-detection capabilities. It is designed for legitimate web scraping, testing, and automation workflows where standard browser fingerprints would result in blocks or CAPTCHAs.

## Features

- 🕵️ **Fingerprint Spoofing** — Randomize user-agent, screen resolution, timezone, language, and WebGL renderer
- 🔄 **Proxy Rotation** — Built-in support for HTTP, SOCKS4, and SOCKS5 proxies
- 🤖 **Anti-Detection** — Patches common automation detection vectors (navigator.webdriver, etc.)
- 🖥️ **Headless & Headed** — Works in both headless and headed browser modes
- 🔌 **Playwright Backend** — Built on top of Microsoft Playwright for reliability
- 📸 **Session Persistence** — Save and restore browser profiles/cookies

## Installation

```bash
pip install cloakbrowser
```

After installation, install the required browser binaries:

```bash
playwright install chromium
```

## Quick Start

```python
from cloakbrowser import CloakBrowser

with CloakBrowser() as browser:
    page = browser.new_page()
    page.goto("https://example.com")
    print(page.title())
```

### With Proxy

```python
from cloakbrowser import CloakBrowser

proxy = {
    "server": "socks5://proxy.example.com:1080",
    "username": "user",
    "password": "pass",
}

with CloakBrowser(proxy=proxy) as browser:
    page = browser.new_page()
    page.goto("https://ifconfig.me")
    print(page.inner_text("body"))  # Should show proxy IP
```

### Custom Fingerprint

```python
from cloakbrowser import CloakBrowser
from cloakbrowser.fingerprint import Fingerprint

fp = Fingerprint(
    user_agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) ...",
    screen_width=1920,
    screen_height=1080,
    timezone="America/New_York",
    locale="en-US",
)

with CloakBrowser(fingerprint=fp) as browser:
    page = browser.new_page()
    page.goto("https://example.com")
```

## Configuration

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `headless` | `bool` | `True` | Run browser in headless mode |
| `proxy` | `dict` | `None` | Proxy configuration dict |
| `fingerprint` | `Fingerprint` | `None` | Custom fingerprint (auto-generated if None) |
| `timeout` | `int` | `90000` | Default navigation timeout (ms) — bumped to 90s for reliability on slow/residential proxies |
| `stealth` | `bool` | `True` | Enable anti-detection patches |

## Contributing

Contributions are welcome! P
