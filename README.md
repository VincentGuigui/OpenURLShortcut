# Open URL Shortcut

A Google Workspace Marketplace app that opens Windows .url shortcut files stored in Google Drive.

## What it does

When you open a .url file from Google Drive using this app, it reads the file, extracts the URL, shows you the destination, and redirects your browser to it after confirmation.

## How it works

Google Drive passes the file ID to the app via a state parameter in the URL. The app authenticates with Google Identity Services using the drive.file scope, fetches the file content via the Drive REST API, parses the URL= line from the file, and redirects the browser.

Everything runs in the browser. There is no server, no database, and no data collection.

## Files

- index.html — the entire application (HTML, CSS, JavaScript)
- privacy-policy.html — privacy policy page required by the Marketplace listing
- assets/ — icons, banner, and screenshot for the Marketplace store listing

## Setup

1. Create a Google Cloud project and enable the Google Drive API and Google Workspace Marketplace SDK.
2. Configure the OAuth consent screen with the drive.file scope.
3. Create an OAuth 2.0 client ID (Web application type) and paste it into index.html as the CLIENT_ID constant.
4. Configure Drive UI Integration: set the Open URL to your hosted page and add url as a default file extension.
5. Deploy index.html and privacy-policy.html to a static host (GitHub Pages works).
6. Add your hosting domain to the OAuth client's Authorized JavaScript origins.

## Local testing

Serve the project with any static file server on port 8080:

    python -m http.server 8080

Testing with drive.file scope requires going through the actual Drive "Open With" flow. For direct file ID testing, temporarily switch the SCOPES constant to https://www.googleapis.com/auth/drive.readonly.

## OAuth scope

drive.file — grants access only to files the user explicitly opens with this app. This is not a sensitive scope and does not require a Google security audit for Marketplace publication.

## Supported file format

Standard Windows Internet Shortcut format:

    [InternetShortcut]
    URL=https://example.com

Only http and https URLs are allowed. javascript:, file:, and other protocols are blocked.
