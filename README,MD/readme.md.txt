# Blog-to-Social Content Pipeline

## Platform
Make.com

## Workflow Overview

The automation uses two workflows.

### 1. Draft Generation and Review

RSS → Jina AI article extraction → Gemini → JSON parsing → image extraction/selection → Airtable

The workflow:

- Reads a new article from RSS.
- Uses Jina AI to extract the article content.
- Uses Gemini to generate separate platform-specific drafts for LinkedIn, X, and Instagram.
- Preserves the original article URL in the generated drafts.
- Stores the drafts in Airtable for human review.
- Extracts a direct article image URL and stores it in Airtable.

### 2. Approved Scheduled Publishing

Airtable → LinkedIn → Airtable

The publishing workflow:

- Searches Airtable for records with `Status = Approved`.
- Selects only `Platform = LinkedIn`.
- Checks the `Scheduled For` field.
- Publishes only when the scheduled time has arrived.
- Updates the Airtable record after publishing.

The publishing scenario runs automatically on a scheduled interval.

## Failure Handling

The Gemini generation module has a Make.com error-handling route.

- Retry is configured for up to 3 attempts.
- The retry interval is configured to 15 minutes.
- A Gmail alert is connected to the Gemini error route.
- Incomplete executions are stored in Make.com.

The failure-handling configuration is present in the workflow. A controlled Gemini failure test has not been performed, so retry and alert behavior should not be considered execution-verified.

## Image / Thumbnail

The workflow uses Jina AI to gather images from the article.

A Text Parser Match Pattern extracts a direct image URL from the Jina response.

The selected image URL is stored in the Airtable `Image URL` field.

A direct NASA image URL was successfully verified during testing.

## Scheduling

Airtable contains a `Scheduled For` date/time field.

The publishing workflow uses an approval and scheduling filter:

- Status must be `Approved`.
- Platform must be `LinkedIn`.
- `Scheduled For` must be populated.
- The scheduled time must have arrived.

The scenario runs every 15 minutes, so publishing may occur on the next polling cycle after the scheduled time rather than at the exact minute.

## Approval Gate

Drafts are reviewed in Airtable before publishing.

Only records with `Status = Approved` and `Platform = LinkedIn` are eligible for the publishing workflow.

## Testing Performed

- Successful RSS → Jina → Gemini → Airtable workflow execution.
- LinkedIn, X, and Instagram drafts generated and stored in Airtable.
- Source URL preserved in the drafts.
- Direct image URL extracted and stored in Airtable.
- Approved LinkedIn record automatically published.
- Scheduled publishing condition verified.
- Final Make.com workflow blueprints exported after the changes.

## Known Limitation

Gemini failure retry and email alert configuration is present, but a deliberate failure simulation has not been performed.

## Credentials

API keys, OAuth credentials, and other secrets are stored in Make.com connections and are not included in this repository.
