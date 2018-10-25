# browserlist-service

Test a user agent string against a list of browser targets and see if it matches.

## API

### GET `/checkbrowser`

Dynamically pulls the latest browser versions using a targets query and saves it
in Google Cloud Storage for fast retrieval later on.

Query Params

- `targetBrowsers` - Optional. Comma separated string, e.g. `last 2 versions, not ie`.
Defaults to `last 2 versions, not dead`. See https://github.com/browserslist/browserslist#full-list for options that can be used.
- `userAgent` - Optional. The user agent string, if not specified then
pulled from header `user-agent`.

Results (JSON)

- Object
  - `targets` - Array of targets matched against
  - `matchesTargets` - Boolean, if the tested user agent string matched the targets.
  - `userAgent` - String, the user agent that was tested against the targets.

### GET `/browserslist`

Pull the list used for a given target query from teh cache.

Query Params

- `targetBrowsers` - Optional. Comma separated string, e.g. `last 2 versions, not ie`.
Defaults to `last 2 versions, not dead`. See https://github.com/browserslist/browserslist#full-list for options that can be used.

Results (JSON)

- Object
  - `list` - Array of browsers with their versions as strings
  - `updated` - String, date when list was updated

### GET `/updatebrowsers`

Call this via a cron job (see `cron.yaml`) to update existing browser lists.

> Note: Using `GET` because Google Cron jobs don't support setting a method.

## CRON Jobs

Deploy via `gcloud app deploy cron.yaml`