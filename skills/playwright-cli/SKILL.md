---
name: playwright-cli
description: Automates browser interactions for web testing, form filling, screenshots, and data extraction. Use when the user needs to navigate websites, interact with web pages, fill forms, take screenshots, test web applications, or extract information from web pages.
allowed-tools: Bash(playwright-cli:*)
---

# Browser Automation with playwright-cli

## Quick start

```bash
# open new browser
playwright-cli open
# navigate to a page
playwright-cli goto https://playwright.dev
# interact with the page using refs from the snapshot
playwright-cli click e15
playwright-cli type "page.click"
playwright-cli press Enter
# take a screenshot (rarely used, as snapshot is more common)
playwright-cli screenshot
# close the browser
playwright-cli close
```

## Commands

### Core

```bash
playwright-cli open
# open and navigate right away
playwright-cli open https://example.com/
playwright-cli goto https://playwright.dev
playwright-cli type "search query"
playwright-cli click e3
playwright-cli dblclick e7
playwright-cli fill e5 "user@example.com"
playwright-cli drag e2 e8
playwright-cli hover e4
playwright-cli select e9 "option-value"
playwright-cli upload ./document.pdf
playwright-cli check e12
playwright-cli uncheck e12
playwright-cli snapshot
playwright-cli snapshot --filename=after-click.yaml
playwright-cli eval "document.title"
playwright-cli eval "el => el.textContent" e5
playwright-cli dialog-accept
playwright-cli dialog-accept "confirmation text"
playwright-cli dialog-dismiss
playwright-cli resize 1920 1080
playwright-cli close
```

### Navigation

```bash
playwright-cli go-back
playwright-cli go-forward
playwright-cli reload
```

### Keyboard

```bash
playwright-cli press Enter
playwright-cli press ArrowDown
playwright-cli keydown Shift
playwright-cli keyup Shift
```

### Mouse

```bash
playwright-cli mousemove 150 300
playwright-cli mousedown
playwright-cli mousedown right
playwright-cli mouseup
playwright-cli mouseup right
playwright-cli mousewheel 0 100
```

### Save as

```bash
playwright-cli screenshot
playwright-cli screenshot e5
playwright-cli screenshot --filename=page.png
playwright-cli pdf --filename=page.pdf
```

### Tabs

```bash
playwright-cli tab-list
playwright-cli tab-new
playwright-cli tab-new https://example.com/page
playwright-cli tab-close
playwright-cli tab-close 2
playwright-cli tab-select 0
```

### Storage

```bash
playwright-cli state-save
playwright-cli state-save auth.json
playwright-cli state-load auth.json

# Cookies
playwright-cli cookie-list
playwright-cli cookie-list --domain=example.com
playwright-cli cookie-get session_id
playwright-cli cookie-set session_id abc123
playwright-cli cookie-set session_id abc123 --domain=example.com --httpOnly --secure
playwright-cli cookie-delete session_id
playwright-cli cookie-clear

# LocalStorage
playwright-cli localstorage-list
playwright-cli localstorage-get theme
playwright-cli localstorage-set theme dark
playwright-cli localstorage-delete theme
playwright-cli localstorage-clear

# SessionStorage
playwright-cli sessionstorage-list
playwright-cli sessionstorage-get step
playwright-cli sessionstorage-set step 3
playwright-cli sessionstorage-delete step
playwright-cli sessionstorage-clear
```

### Network

```bash
playwright-cli route "**/*.jpg" --status=404
playwright-cli route "https://api.example.com/**" --body='{"mock": true}'
playwright-cli route-list
playwright-cli unroute "**/*.jpg"
playwright-cli unroute
```

### DevTools

```bash
playwright-cli console
playwright-cli console warning
playwright-cli network
playwright-cli run-code "async page => await page.context().grantPermissions(['geolocation'])"
playwright-cli tracing-start
playwright-cli tracing-stop
playwright-cli video-start
playwright-cli video-stop video.webm
```

## Open parameters
```bash
# Use specific browser when creating session
playwright-cli open --browser=chrome
playwright-cli open --browser=firefox
playwright-cli open --browser=webkit
playwright-cli open --browser=msedge
# Connect to browser via extension
playwright-cli open --extension

# Use persistent profile (by default profile is in-memory)
playwright-cli open --persistent
# Use persistent profile with custom directory
playwright-cli open --profile=/path/to/profile

# Start with config file
playwright-cli open --config=my-config.json

# Close the browser
playwright-cli close
# Delete user data for the default session
playwright-cli delete-data
```

## Snapshots

After each command, playwright-cli provides a snapshot of the current browser state.

```bash
> playwright-cli goto https://example.com
### Page
- Page URL: https://example.com/
- Page Title: Example Domain
### Snapshot
[Snapshot](.playwright-cli/page-2026-02-14T19-22-42-679Z.yml)
```

You can also take a snapshot on demand using `playwright-cli snapshot` command.

If `--filename` is not provided, a new snapshot file is created with a timestamp. Default to automatic file naming, use `--filename=` when artifact is a part of the workflow result.

## Browser Sessions

```bash
# create new browser session named "mysession" with persistent profile
playwright-cli -s=mysession open example.com --persistent
# same with manually specified profile directory (use when requested explicitly)
playwright-cli -s=mysession open example.com --profile=/path/to/profile
playwright-cli -s=mysession click e6
playwright-cli -s=mysession close  # stop a named browser
playwright-cli -s=mysession delete-data  # delete user data for persistent session

playwright-cli list
# Close all browsers
playwright-cli close-all
# Forcefully kill all browser processes
playwright-cli kill-all
```

## Local installation

In some cases user might want to install playwright-cli locally. If running globally available `playwright-cli` binary fails, use `npx playwright-cli` to run the commands. For example:

```bash
npx playwright-cli open https://example.com
npx playwright-cli click e1
```

## Example: Form submission

```bash
playwright-cli open https://example.com/form
playwright-cli snapshot

playwright-cli fill e1 "user@example.com"
playwright-cli fill e2 "password123"
playwright-cli click e3
playwright-cli snapshot
playwright-cli close
```

## Example: Multi-tab workflow

```bash
playwright-cli open https://example.com
playwright-cli tab-new https://example.com/other
playwright-cli tab-list
playwright-cli tab-select 0
playwright-cli snapshot
playwright-cli close
```

## Example: Debugging with DevTools

```bash
playwright-cli open https://example.com
playwright-cli click e4
playwright-cli fill e7 "test"
playwright-cli console
playwright-cli network
playwright-cli close
```

```bash
playwright-cli open https://example.com
playwright-cli tracing-start
playwright-cli click e4
playwright-cli fill e7 "test"
playwright-cli tracing-stop
playwright-cli close
```

## Request Mocking

Intercept, mock, modify, and block network requests.

### CLI Route Commands

```bash
# Mock with custom status
playwright-cli route "**/*.jpg" --status=404

# Mock with JSON body
playwright-cli route "**/api/users" --body='[{"id":1,"name":"Alice"}]' --content-type=application/json

# Mock with custom headers
playwright-cli route "**/api/data" --body='{"ok":true}' --header="X-Custom: value"

# Remove headers from requests
playwright-cli route "**/*" --remove-header=cookie,authorization

# List active routes
playwright-cli route-list

# Remove a route or all routes
playwright-cli unroute "**/*.jpg"
playwright-cli unroute
```

### URL Patterns

```
**/api/users           - Exact path match
**/api/*/details       - Wildcard in path
**/*.{png,jpg,jpeg}    - Match file extensions
**/search?q=*          - Match query parameters
```

### Advanced Mocking with run-code

For conditional responses, request body inspection, response modification, or delays:

#### Conditional Response Based on Request

```bash
playwright-cli run-code "async page => {
  await page.route('**/api/login', route => {
    const body = route.request().postDataJSON();
    if (body.username === 'admin') {
      route.fulfill({ body: JSON.stringify({ token: 'mock-token' }) });
    } else {
      route.fulfill({ status: 401, body: JSON.stringify({ error: 'Invalid' }) });
    }
  });
}"
```

#### Modify Real Response

```bash
playwright-cli run-code "async page => {
  await page.route('**/api/user', async route => {
    const response = await route.fetch();
    const json = await response.json();
    json.isPremium = true;
    await route.fulfill({ response, json });
  });
}"
```

#### Simulate Network Failures

```bash
playwright-cli run-code "async page => {
  await page.route('**/api/offline', route => route.abort('internetdisconnected'));
}"
# Options: connectionrefused, timedout, connectionreset, internetdisconnected
```

#### Delayed Response

```bash
playwright-cli run-code "async page => {
  await page.route('**/api/slow', async route => {
    await new Promise(r => setTimeout(r, 3000));
    route.fulfill({ body: JSON.stringify({ data: 'loaded' }) });
  });
}"
```

## Running Custom Playwright Code

Use `run-code` to execute arbitrary Playwright code for advanced scenarios not covered by CLI commands.

### Syntax

```bash
playwright-cli run-code "async page => {
  // Your Playwright code here
  // Access page.context() for browser context operations
}"
```

### Geolocation

```bash
# Grant geolocation permission and set location
playwright-cli run-code "async page => {
  await page.context().grantPermissions(['geolocation']);
  await page.context().setGeolocation({ latitude: 37.7749, longitude: -122.4194 });
}"

# Set location to London
playwright-cli run-code "async page => {
  await page.context().grantPermissions(['geolocation']);
  await page.context().setGeolocation({ latitude: 51.5074, longitude: -0.1278 });
}"

# Clear geolocation override
playwright-cli run-code "async page => {
  await page.context().clearPermissions();
}"
```

### Permissions

```bash
# Grant multiple permissions
playwright-cli run-code "async page => {
  await page.context().grantPermissions([
    'geolocation',
    'notifications',
    'camera',
    'microphone'
  ]);
}"

# Grant permissions for specific origin
playwright-cli run-code "async page => {
  await page.context().grantPermissions(['clipboard-read'], {
    origin: 'https://example.com'
  });
}"
```

### Media Emulation

```bash
# Emulate dark color scheme
playwright-cli run-code "async page => {
  await page.emulateMedia({ colorScheme: 'dark' });
}"

# Emulate light color scheme
playwright-cli run-code "async page => {
  await page.emulateMedia({ colorScheme: 'light' });
}"

# Emulate reduced motion
playwright-cli run-code "async page => {
  await page.emulateMedia({ reducedMotion: 'reduce' });
}"

# Emulate print media
playwright-cli run-code "async page => {
  await page.emulateMedia({ media: 'print' });
}"
```

### Wait Strategies

```bash
# Wait for network idle
playwright-cli run-code "async page => {
  await page.waitForLoadState('networkidle');
}"

# Wait for specific element
playwright-cli run-code "async page => {
  await page.waitForSelector('.loading', { state: 'hidden' });
}"

# Wait for function to return true
playwright-cli run-code "async page => {
  await page.waitForFunction(() => window.appReady === true);
}"

# Wait with timeout
playwright-cli run-code "async page => {
  await page.waitForSelector('.result', { timeout: 10000 });
}"
```

### Frames and Iframes

```bash
# Work with iframe
playwright-cli run-code "async page => {
  const frame = page.locator('iframe#my-iframe').contentFrame();
  await frame.locator('button').click();
}"

# Get all frames
playwright-cli run-code "async page => {
  const frames = page.frames();
  return frames.map(f => f.url());
}"
```

### File Downloads

```bash
# Handle file download
playwright-cli run-code "async page => {
  const [download] = await Promise.all([
    page.waitForEvent('download'),
    page.click('a.download-link')
  ]);
  await download.saveAs('./downloaded-file.pdf');
  return download.suggestedFilename();
}"
```

### Clipboard

```bash
# Read clipboard (requires permission)
playwright-cli run-code "async page => {
  await page.context().grantPermissions(['clipboard-read']);
  return await page.evaluate(() => navigator.clipboard.readText());
}"

# Write to clipboard
playwright-cli run-code "async page => {
  await page.evaluate(text => navigator.clipboard.writeText(text), 'Hello clipboard!');
}"
```

### Page Information

```bash
# Get page title
playwright-cli run-code "async page => {
  return await page.title();
}"

# Get current URL
playwright-cli run-code "async page => {
  return page.url();
}"

# Get page content
playwright-cli run-code "async page => {
  return await page.content();
}"

# Get viewport size
playwright-cli run-code "async page => {
  return page.viewportSize();
}"
```

### JavaScript Execution

```bash
# Execute JavaScript and return result
playwright-cli run-code "async page => {
  return await page.evaluate(() => {
    return {
      userAgent: navigator.userAgent,
      language: navigator.language,
      cookiesEnabled: navigator.cookieEnabled
    };
  });
}"

# Pass arguments to evaluate
playwright-cli run-code "async page => {
  const multiplier = 5;
  return await page.evaluate(m => document.querySelectorAll('li').length * m, multiplier);
}"
```

### Error Handling

```bash
# Try-catch in run-code
playwright-cli run-code "async page => {
  try {
    await page.click('.maybe-missing', { timeout: 1000 });
    return 'clicked';
  } catch (e) {
    return 'element not found';
  }
}"
```

### Complex Workflows

```bash
# Login and save state
playwright-cli run-code "async page => {
  await page.goto('https://example.com/login');
  await page.fill('input[name=email]', 'user@example.com');
  await page.fill('input[name=password]', 'secret');
  await page.click('button[type=submit]');
  await page.waitForURL('**/dashboard');
  await page.context().storageState({ path: 'auth.json' });
  return 'Login successful';
}"

# Scrape data from multiple pages
playwright-cli run-code "async page => {
  const results = [];
  for (let i = 1; i <= 3; i++) {
    await page.goto(\`https://example.com/page/\${i}\`);
    const items = await page.locator('.item').allTextContents();
    results.push(...items);
  }
  return results;
}"
```

## Browser Session Management

Run multiple isolated browser sessions concurrently with state persistence.

### Named Browser Sessions

Use `-s` flag to isolate browser contexts:

```bash
# Browser 1: Authentication flow
playwright-cli -s=auth open https://app.example.com/login

# Browser 2: Public browsing (separate cookies, storage)
playwright-cli -s=public open https://example.com

# Commands are isolated by browser session
playwright-cli -s=auth fill e1 "user@example.com"
playwright-cli -s=public snapshot
```

### Browser Session Isolation Properties

Each browser session has independent:
- Cookies
- LocalStorage / SessionStorage
- IndexedDB
- Cache
- Browsing history
- Open tabs

### Browser Session Commands

```bash
# List all browser sessions
playwright-cli list

# Stop a browser session (close the browser)
playwright-cli close                # stop the default browser
playwright-cli -s=mysession close   # stop a named browser

# Stop all browser sessions
playwright-cli close-all

# Forcefully kill all daemon processes (for stale/zombie processes)
playwright-cli kill-all

# Delete browser session user data (profile directory)
playwright-cli delete-data                # delete default browser data
playwright-cli -s=mysession delete-data   # delete named browser data
```

### Environment Variable

Set a default browser session name via environment variable:

```bash
export PLAYWRIGHT_CLI_SESSION="mysession"
playwright-cli open example.com  # Uses "mysession" automatically
```

### Common Patterns

#### Concurrent Scraping

```bash
#!/bin/bash
# Scrape multiple sites concurrently

# Start all browsers
playwright-cli -s=site1 open https://site1.com &
playwright-cli -s=site2 open https://site2.com &
playwright-cli -s=site3 open https://site3.com &
wait

# Take snapshots from each
playwright-cli -s=site1 snapshot
playwright-cli -s=site2 snapshot
playwright-cli -s=site3 snapshot

# Cleanup
playwright-cli close-all
```

#### A/B Testing Sessions

```bash
# Test different user experiences
playwright-cli -s=variant-a open "https://app.com?variant=a"
playwright-cli -s=variant-b open "https://app.com?variant=b"

# Compare
playwright-cli -s=variant-a screenshot
playwright-cli -s=variant-b screenshot
```

#### Persistent Profile

By default, browser profile is kept in memory only. Use `--persistent` flag on `open` to persist the browser profile to disk:

```bash
# Use persistent profile (auto-generated location)
playwright-cli open https://example.com --persistent

# Use persistent profile with custom directory
playwright-cli open https://example.com --profile=/path/to/profile
```

### Default Browser Session

When `-s` is omitted, commands use the default browser session:

```bash
# These use the same default browser session
playwright-cli open https://example.com
playwright-cli snapshot
playwright-cli close  # Stops default browser
```

### Browser Session Configuration

Configure a browser session with specific settings when opening:

```bash
# Open with config file
playwright-cli open https://example.com --config=.playwright/my-cli.json

# Open with specific browser
playwright-cli open https://example.com --browser=firefox

# Open in headed mode
playwright-cli open https://example.com --headed

# Open with persistent profile
playwright-cli open https://example.com --persistent
```

### Best Practices

#### 1. Name Browser Sessions Semantically

```bash
# GOOD: Clear purpose
playwright-cli -s=github-auth open https://github.com
playwright-cli -s=docs-scrape open https://docs.example.com

# AVOID: Generic names
playwright-cli -s=s1 open https://github.com
```

#### 2. Always Clean Up

```bash
# Stop browsers when done
playwright-cli -s=auth close
playwright-cli -s=scrape close

# Or stop all at once
playwright-cli close-all

# If browsers become unresponsive or zombie processes remain
playwright-cli kill-all
```

#### 3. Delete Stale Browser Data

```bash
# Remove old browser data to free disk space
playwright-cli -s=oldsession delete-data
```

## Storage Management

Manage cookies, localStorage, sessionStorage, and browser storage state.

### Storage State

Save and restore complete browser state including cookies and storage.

#### Save Storage State

```bash
# Save to auto-generated filename (storage-state-{timestamp}.json)
playwright-cli state-save

# Save to specific filename
playwright-cli state-save my-auth-state.json
```

#### Restore Storage State

```bash
# Load storage state from file
playwright-cli state-load my-auth-state.json

# Reload page to apply cookies
playwright-cli open https://example.com
```

#### Storage State File Format

The saved file contains:

```json
{
  "cookies": [
    {
      "name": "session_id",
      "value": "abc123",
      "domain": "example.com",
      "path": "/",
      "expires": 1735689600,
      "httpOnly": true,
      "secure": true,
      "sameSite": "Lax"
    }
  ],
  "origins": [
    {
      "origin": "https://example.com",
      "localStorage": [
        { "name": "theme", "value": "dark" },
        { "name": "user_id", "value": "12345" }
      ]
    }
  ]
}
```

### Cookies

#### List All Cookies

```bash
playwright-cli cookie-list
```

#### Filter Cookies by Domain

```bash
playwright-cli cookie-list --domain=example.com
```

#### Filter Cookies by Path

```bash
playwright-cli cookie-list --path=/api
```

#### Get Specific Cookie

```bash
playwright-cli cookie-get session_id
```

#### Set a Cookie

```bash
# Basic cookie
playwright-cli cookie-set session abc123

# Cookie with options
playwright-cli cookie-set session abc123 --domain=example.com --path=/ --httpOnly --secure --sameSite=Lax

# Cookie with expiration (Unix timestamp)
playwright-cli cookie-set remember_me token123 --expires=1735689600
```

#### Delete a Cookie

```bash
playwright-cli cookie-delete session_id
```

#### Clear All Cookies

```bash
playwright-cli cookie-clear
```

#### Advanced: Multiple Cookies or Custom Options

For complex scenarios like adding multiple cookies at once, use `run-code`:

```bash
playwright-cli run-code "async page => {
  await page.context().addCookies([
    { name: 'session_id', value: 'sess_abc123', domain: 'example.com', path: '/', httpOnly: true },
    { name: 'preferences', value: JSON.stringify({ theme: 'dark' }), domain: 'example.com', path: '/' }
  ]);
}"
```

### Local Storage

#### List All localStorage Items

```bash
playwright-cli localstorage-list
```

#### Get Single Value

```bash
playwright-cli localstorage-get token
```

#### Set Value

```bash
playwright-cli localstorage-set theme dark
```

#### Set JSON Value

```bash
playwright-cli localstorage-set user_settings '{"theme":"dark","language":"en"}'
```

#### Delete Single Item

```bash
playwright-cli localstorage-delete token
```

#### Clear All localStorage

```bash
playwright-cli localstorage-clear
```

#### Advanced: Multiple Operations

For complex scenarios like setting multiple values at once, use `run-code`:

```bash
playwright-cli run-code "async page => {
  await page.evaluate(() => {
    localStorage.setItem('token', 'jwt_abc123');
    localStorage.setItem('user_id', '12345');
    localStorage.setItem('expires_at', Date.now() + 3600000);
  });
}"
```

### Session Storage

#### List All sessionStorage Items

```bash
playwright-cli sessionstorage-list
```

#### Get Single Value

```bash
playwright-cli sessionstorage-get form_data
```

#### Set Value

```bash
playwright-cli sessionstorage-set step 3
```

#### Delete Single Item

```bash
playwright-cli sessionstorage-delete step
```

#### Clear sessionStorage

```bash
playwright-cli sessionstorage-clear
```

### IndexedDB

#### List Databases

```bash
playwright-cli run-code "async page => {
  return await page.evaluate(async () => {
    const databases = await indexedDB.databases();
    return databases;
  });
}"
```

#### Delete Database

```bash
playwright-cli run-code "async page => {
  await page.evaluate(() => {
    indexedDB.deleteDatabase('myDatabase');
  });
}"
```

### Common Patterns

#### Authentication State Reuse

```bash
# Step 1: Login and save state
playwright-cli open https://app.example.com/login
playwright-cli snapshot
playwright-cli fill e1 "user@example.com"
playwright-cli fill e2 "password123"
playwright-cli click e3

# Save the authenticated state
playwright-cli state-save auth.json

# Step 2: Later, restore state and skip login
playwright-cli state-load auth.json
playwright-cli open https://app.example.com/dashboard
# Already logged in!
```

#### Save and Restore Roundtrip

```bash
# Set up authentication state
playwright-cli open https://example.com
playwright-cli eval "() => { document.cookie = 'session=abc123'; localStorage.setItem('user', 'john'); }"

# Save state to file
playwright-cli state-save my-session.json

# ... later, in a new session ...

# Restore state
playwright-cli state-load my-session.json
playwright-cli open https://example.com
# Cookies and localStorage are restored!
```

### Security Notes

- Never commit storage state files containing auth tokens
- Add `*.auth-state.json` to `.gitignore`
- Delete state files after automation completes
- Use environment variables for sensitive data
- By default, sessions run in-memory mode which is safer for sensitive operations

## Test Generation

Generate Playwright test code automatically as you interact with the browser.

### How It Works

Every action you perform with `playwright-cli` generates corresponding Playwright TypeScript code.
This code appears in the output and can be copied directly into your test files.

### Example Workflow

```bash
# Start a session
playwright-cli open https://example.com/login

# Take a snapshot to see elements
playwright-cli snapshot
# Output shows: e1 [textbox "Email"], e2 [textbox "Password"], e3 [button "Sign In"]

# Fill form fields - generates code automatically
playwright-cli fill e1 "user@example.com"
# Ran Playwright code:
# await page.getByRole('textbox', { name: 'Email' }).fill('user@example.com');

playwright-cli fill e2 "password123"
# Ran Playwright code:
# await page.getByRole('textbox', { name: 'Password' }).fill('password123');

playwright-cli click e3
# Ran Playwright code:
# await page.getByRole('button', { name: 'Sign In' }).click();
```

### Building a Test File

Collect the generated code into a Playwright test:

```typescript
import { test, expect } from '@playwright/test';

test('login flow', async ({ page }) => {
  // Generated code from playwright-cli session:
  await page.goto('https://example.com/login');
  await page.getByRole('textbox', { name: 'Email' }).fill('user@example.com');
  await page.getByRole('textbox', { name: 'Password' }).fill('password123');
  await page.getByRole('button', { name: 'Sign In' }).click();

  // Add assertions
  await expect(page).toHaveURL(/.*dashboard/);
});
```

### Best Practices

#### 1. Use Semantic Locators

The generated code uses role-based locators when possible, which are more resilient:

```typescript
// Generated (good - semantic)
await page.getByRole('button', { name: 'Submit' }).click();

// Avoid (fragile - CSS selectors)
await page.locator('#submit-btn').click();
```

#### 2. Explore Before Recording

Take snapshots to understand the page structure before recording actions:

```bash
playwright-cli open https://example.com
playwright-cli snapshot
# Review the element structure
playwright-cli click e5
```

#### 3. Add Assertions Manually

Generated code captures actions but not assertions. Add expectations in your test:

```typescript
// Generated action
await page.getByRole('button', { name: 'Submit' }).click();

// Manual assertion
await expect(page.getByText('Success')).toBeVisible();
```

## Tracing

Capture detailed execution traces for debugging and analysis. Traces include DOM snapshots, screenshots, network activity, and console logs.

### Basic Usage

```bash
# Start trace recording
playwright-cli tracing-start

# Perform actions
playwright-cli open https://example.com
playwright-cli click e1
playwright-cli fill e2 "test"

# Stop trace recording
playwright-cli tracing-stop
```

### Trace Output Files

When you start tracing, Playwright creates a `traces/` directory with several files:

#### `trace-{timestamp}.trace`

**Action log** - The main trace file containing:
- Every action performed (clicks, fills, navigations)
- DOM snapshots before and after each action
- Screenshots at each step
- Timing information
- Console messages
- Source locations

#### `trace-{timestamp}.network`

**Network log** - Complete network activity:
- All HTTP requests and responses
- Request headers and bodies
- Response headers and bodies
- Timing (DNS, connect, TLS, TTFB, download)
- Resource sizes
- Failed requests and errors

#### `resources/`

**Resources directory** - Cached resources:
- Images, fonts, stylesheets, scripts
- Response bodies for replay
- Assets needed to reconstruct page state

### What Traces Capture

| Category | Details |
|----------|---------|
| **Actions** | Clicks, fills, hovers, keyboard input, navigations |
| **DOM** | Full DOM snapshot before/after each action |
| **Screenshots** | Visual state at each step |
| **Network** | All requests, responses, headers, bodies, timing |
| **Console** | All console.log, warn, error messages |
| **Timing** | Precise timing for each operation |

### Use Cases

#### Debugging Failed Actions

```bash
playwright-cli tracing-start
playwright-cli open https://app.example.com

# This click fails - why?
playwright-cli click e5

playwright-cli tracing-stop
# Open trace to see DOM state when click was attempted
```

#### Analyzing Performance

```bash
playwright-cli tracing-start
playwright-cli open https://slow-site.com
playwright-cli tracing-stop

# View network waterfall to identify slow resources
```

#### Capturing Evidence

```bash
# Record a complete user flow for documentation
playwright-cli tracing-start

playwright-cli open https://app.example.com/checkout
playwright-cli fill e1 "4111111111111111"
playwright-cli fill e2 "12/25"
playwright-cli fill e3 "123"
playwright-cli click e4

playwright-cli tracing-stop
# Trace shows exact sequence of events
```

### Trace vs Video vs Screenshot

| Feature | Trace | Video | Screenshot |
|---------|-------|-------|------------|
| **Format** | .trace file | .webm video | .png/.jpeg image |
| **DOM inspection** | Yes | No | No |
| **Network details** | Yes | No | No |
| **Step-by-step replay** | Yes | Continuous | Single frame |
| **File size** | Medium | Large | Small |
| **Best for** | Debugging | Demos | Quick capture |

### Best Practices

#### 1. Start Tracing Before the Problem

```bash
# Trace the entire flow, not just the failing step
playwright-cli tracing-start
playwright-cli open https://example.com
# ... all steps leading to the issue ...
playwright-cli tracing-stop
```

#### 2. Clean Up Old Traces

Traces can consume significant disk space:

```bash
# Remove traces older than 7 days
find .playwright-cli/traces -mtime +7 -delete
```

### Limitations

- Traces add overhead to automation
- Large traces can consume significant disk space
- Some dynamic content may not replay perfectly

## Video Recording

Capture browser automation sessions as video for debugging, documentation, or verification. Produces WebM (VP8/VP9 codec).

### Basic Recording

```bash
# Start recording
playwright-cli video-start

# Perform actions
playwright-cli open https://example.com
playwright-cli snapshot
playwright-cli click e1
playwright-cli fill e2 "test input"

# Stop and save
playwright-cli video-stop demo.webm
```

### Best Practices

#### 1. Use Descriptive Filenames

```bash
# Include context in filename
playwright-cli video-stop recordings/login-flow-2024-01-15.webm
playwright-cli video-stop recordings/checkout-test-run-42.webm
```

### Tracing vs Video

| Feature | Video | Tracing |
|---------|-------|---------|
| Output | WebM file | Trace file (viewable in Trace Viewer) |
| Shows | Visual recording | DOM snapshots, network, console, actions |
| Use case | Demos, documentation | Debugging, analysis |
| Size | Larger | Smaller |

### Limitations

- Recording adds slight overhead to automation
- Large recordings can consume significant disk space
