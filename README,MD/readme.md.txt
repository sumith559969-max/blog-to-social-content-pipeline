
# Blog-to-Social Content Pipeline

## Project Overview

The Blog-to-Social Content Pipeline is an automation project built using Make.com. It converts articles from an RSS feed into platform-specific social media drafts, stores them in Airtable, and supports human approval before publishing.

The project aims to reduce manual content repurposing while maintaining factual accuracy, platform-specific formatting, and human control over publishing.

## Objectives

- Automatically retrieve new articles from an RSS feed.
- Extract the full article content.
- Generate distinct social media drafts for LinkedIn, X, and Instagram.
- Preserve the original article URL and image URL.
- Store drafts in Airtable for review and approval.
- Publish approved LinkedIn content through a separate scenario.
- Handle generation errors through retries and email notifications.

## Tools and Technologies

| Tool | Purpose |
|---|---|
| Make.com | Workflow automation and scenario orchestration |
| RSS | Detect new articles and retrieve article metadata |
| Jina AI | Extract article text from the source URL |
| Google Gemini | Generate platform-specific social media content |
| JSON Parse | Parse the structured Gemini response |
| Airtable | Store drafts, approval status, scheduling information, and publishing status |
| Gmail | Send error notifications |
| LinkedIn | Publish approved LinkedIn posts |

## Workflow Architecture

### Scenario 1: Article-to-Social Draft Generation

The `Integration RSS 2` scenario processes articles and creates social media drafts.

**Workflow:**

RSS → Jina Article Extraction → Google Gemini → JSON Parse → Airtable

The scenario performs the following operations:

1. Retrieves article information from the RSS feed.
2. Extracts the article text using Jina.
3. Sends the article title, URL, and extracted text to Gemini.
4. Generates separate drafts for LinkedIn, X, and Instagram.
5. Parses the JSON response.
6. Creates individual Airtable records for each platform.

### AI Content Generation

Gemini generates structured JSON with the following keys:

- `linkedin`
- `x`
- `instagram`

Each platform object contains `text` and `source_url`.

The generation instructions require:

- One concise sentence per platform.
- Platform-specific content rather than identical copy across platforms.
- The exact article URL in each text field and source URL field.
- Only facts supported by the original article.
- X content limited to 250 characters, including the URL, with a target of 220 characters or less.

## Airtable Draft Management

Airtable acts as the central content review and approval database.

The Social Drafts table stores platform-specific drafts and relevant workflow information, including:

- Platform
- Draft text
- Article source URL
- Image URL
- Approval status
- Scheduled For
- Publishing status

Each platform draft is stored as a separate record.

### Human Approval

Drafts are reviewed before publishing.

Only records marked as Approved are eligible for the LinkedIn publishing scenario. This keeps human approval between AI content generation and publishing.

Rejected or unapproved drafts should not be published.

## Scenario 2: LinkedIn Publishing

The `Integration Airtable, LinkedIn` scenario retrieves eligible records from Airtable.

**Workflow:**

Airtable Search Records → LinkedIn Create User Text Post → Airtable Update a Record

The search criteria select approved LinkedIn records whose scheduled time has arrived.

After a successful LinkedIn post, the corresponding Airtable record is updated to Published.

The scenario is configured to run on a 15-minute schedule.

## Error Handling and Notifications

The Gemini error handler is configured with:

- Automatic retry: 3 attempts
- Retry interval: 15 minutes
- Store incomplete executions: Enabled
- Gmail notification for Gemini errors

The Gmail notification alerts the project owner when Gemini encounters a failure.

**Known limitation:** The Gmail module currently appears before the Retry (Break) module in the error-handler route. Consequently, an email may be sent on an initial failed attempt rather than only after all retries are exhausted. Final-only failure notification behavior has not been verified.

The workflow should be monitored through Make.com execution history and incomplete executions.

## Testing and Results

| Component | Result |
|---|---|
| RSS article retrieval | Configured |
| Article extraction through Jina | Configured |
| Gemini platform-specific generation | Configured |
| JSON parsing | Configured |
| Airtable draft creation | Configured |
| Article image URL mapping | Configured |
| Gemini retry configuration | Configured |
| Gmail error notification | Test email succeeded |
| LinkedIn manual publishing | Successfully tested |
| Airtable Published status update | Confirmed after manual publishing |
| Scheduled LinkedIn publishing | Requires further verification |
| X publishing | Draft generation configured; publishing not implemented |
| Instagram publishing | Draft generation configured; publishing not implemented |

A scheduled execution was recorded as successful, but the LinkedIn post did not appear until the scenario was manually run. Automatic scheduled publishing therefore remains unverified.

## Limitations and Future Improvements

The following improvements remain:

1. Verify that LinkedIn posts are published automatically at their scheduled time.
2. Improve error handling so notifications clearly distinguish temporary failures from exhausted retries.
3. Add retry and failure handling to the LinkedIn publishing scenario.
4. Implement publishing workflows for X and Instagram.
5. Improve image selection and verify that stored image URLs are usable for publishing.
6. Document the final tested Gemini JSON schema and validate generated output before creating records.
7. Add monitoring and documentation for incomplete executions and failed runs.

## Project Status

**Status:** Attempt 2 — Core workflow configured; additional testing and improvements pending.

The project demonstrates automated article extraction, AI-powered content repurposing, Airtable-based human approval, and a tested manual LinkedIn publishing workflow.

Automatic scheduled publishing and multi-platform publishing remain areas for further development.

## Author

Sumith