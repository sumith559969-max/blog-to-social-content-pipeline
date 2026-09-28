# Blog-to-Social Content Pipeline

## Project Overview

The Blog-to-Social Content Pipeline is an automation project built using Make.com, Airtable, Gemini AI, and Jina AI. It converts articles from an RSS feed into platform-specific social media drafts and sends them to a human approval queue before publishing.

The goal is to reduce manual content repurposing while maintaining human control over publishing.

## Tools and Technologies

* **Make.com:** Workflow automation and scenario orchestration.
* **Airtable:** Draft storage, review queue, approval status, and publishing records.
* **Gemini AI:** Generates platform-specific social media content.
* **Jina AI:** Extracts article content from source URLs.
* **RSS:** Triggers the workflow when new articles are available.
* **LinkedIn:** Publishing integration.

## Workflow

1. Make.com monitors an RSS feed for new articles.
2. Jina AI retrieves the article content.
3. Gemini generates distinct LinkedIn, X, and Instagram drafts.
4. Make.com parses the generated content.
5. Drafts are saved to Airtable with their source article URLs and Pending status.
6. A human reviewer approves, rejects, or requests edits to each draft.
7. The LinkedIn publishing scenario searches for approved LinkedIn drafts, publishes eligible content, and updates Airtable records.

## Human Approval

All generated drafts enter Airtable with the status Pending.

The reviewer can change the status to Approved, Needs edits, or Rejected. The LinkedIn publishing scenario searches for Approved LinkedIn records. Pending and Rejected drafts are not eligible for publishing.

## Error Handling

The LinkedIn publishing scenario has an automatic retry configuration of three retries, with 15 minutes between retries. Incomplete executions are enabled in Make.com to allow failed executions to be reviewed and retried.

Successful Airtable updates record the Published status.

## Current Implementation Status

* RSS article detection and retrieval: Configured.
* AI draft generation: Configured for LinkedIn, X, and Instagram.
* Airtable draft storage and approval queue: Configured.
* LinkedIn publishing integration: Configured; Airtable records were successfully updated to Published during testing.
* X publishing: Not connected; an X developer API integration is still required.
* Instagram publishing: Not connected; the required account and Facebook Page connection was not completed.
* Duplicate prevention: Not fully implemented or verified.
* Scheduling and end-to-end publishing tests: Require further verification.

## Known Limitations

X and Instagram drafts are generated and stored in Airtable, but automatic publishing for these platforms is not currently connected.

LinkedIn publishing is configured, but successful Airtable status updates do not independently confirm that every post appeared publicly on LinkedIn.

Duplicate prevention and full scheduling behavior remain unfinished and should not be considered production-ready.

## Project Evidence

The submission includes:

* Make.com draft-generation scenario blueprint (JSON).
* Make.com publishing scenario blueprint (JSON).
* Screenshot of the draft-generation scenario.
* Screenshot of the publishing scenario.
* Screenshot of a successful execution and Airtable status updates.

## Conclusion

This project demonstrates an AI-assisted content repurposing and human approval workflow using Make.com and Airtable. It automates article extraction, platform-specific draft generation, review management, and the configured LinkedIn publishing process while preserving human approval before publishing.
