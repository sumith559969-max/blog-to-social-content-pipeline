# Blog-to-Social Content Pipeline

An automated content repurposing workflow built using Make.com, Airtable, Jina AI, and Google Gemini 3.5 Flash-Lite.

## Project Overview

This project converts articles from an RSS feed into social media drafts for LinkedIn, X, and Instagram.

## Workflow

1. RSS monitors a feed for new articles.
2. Jina AI retrieves the article content.
3. Google Gemini generates platform-specific social media drafts.
4. Airtable stores the drafts in the Social Drafts table for review and management.

## Technologies Used

* Make.com — workflow automation
* Airtable — draft storage and status tracking
* Jina AI — article content extraction
* Google Gemini 3.5 Flash-Lite — AI content generation

## Current Features

* RSS-based article collection
* AI-generated social media drafts for three platforms
* Airtable draft storage
* Status tracking for content review

## Current Limitations

Human approval-gated publishing, automatic scheduling, and advanced error handling are not yet implemented.

## Project Status

The draft-generation workflow has been tested in Make.com, with successful module executions.
