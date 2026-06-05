---
name: puppeteer-automation
description: Use Puppeteer for web page automation. Use this when the user says "screenshot", "test the page", "automate the browser", or needs to drive a web page.
---

# Puppeteer Web Page Automation

## Task
$ARGUMENTS

## Setup Needs
- Use headless mode
- Ignore SSL certificate errors

## Site Addresses
- Home page: https://ligo.local
- Nova back office: https://ligo.local/panel
- Admin back office: https://ligo.local/admin
- UserCenter: https://ligo.local/user-center

## How to Log In
If you need to log in, use `php artisan auth:login 6` to make a login URL. Then let Puppeteer visit that URL to finish the login.

## Where to Save Screenshots
Save all screenshots in the `storage/temporary` directory
