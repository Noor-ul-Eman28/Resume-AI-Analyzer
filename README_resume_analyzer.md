# AI Resume Analyzer 🤖📄

An automated n8n workflow that receives resumes by email, extracts candidate information using AI, scores each candidate against a defined job role, logs the results to Google Sheets, and sends a follow-up email based on the score.

## Problem Statement

Manually screening resumes for every applicant is slow and inconsistent. This workflow automates the process end-to-end: a resume lands in Gmail, and within minutes the candidate's details are extracted, evaluated against a job description, and logged — with a follow-up email sent automatically.

## How It Works

### 1. Input Stage

- **Gmail Trigger** polls the inbox every minute for emails containing a PDF resume attachment.
- **Upload file** saves the attachment to a designated Google Drive folder.
- **Download file** retrieves it back into the workflow for processing.
- **Extract from File** converts the PDF into plain text.

### 2. AI Processing

- **Information Extractor** pulls structured contact details (name, email, phone number) from the resume text using a defined JSON schema.
- **AI Agent** (GPT-4o) acts as a CV Evaluation and Summarization Agent. It:
  - Extracts educational qualifications, job history, and skill set
  - Compares the candidate against a specific job role (Junior AI Automation Engineer)
  - Assigns a relevance score from 1–10
  - Provides a short justification for the score

### 3. Parsing and Logging

- **Code (JavaScript)** parses the AI Agent's structured text output into clean fields: education, job history, skills, score, and justification.
- **Append row in sheet** logs every candidate's parsed results to a Google Sheet for easy review.

### 4. Conditional Follow-Up

- An **If** node checks whether the candidate's score is above a threshold (score > 3).
- Depending on the result, a second **Code** node builds a tailored email response, which is sent automatically via **Gmail**.

## Job Role Criteria (Example)

The AI Agent evaluates candidates against this role definition:

- **Title:** Junior AI Automation Engineer
- **Required:** Bachelor's degree in CS/SE/IT or related field; Python, REST APIs, JSON, prompt engineering, n8n or similar low-code automation, Git
- **Nice to have:** LangChain, vector databases, SQL, cloud deployment (AWS/GCP)
- **Experience:** 0–2 years (internships and personal projects count)

This can be swapped out for any job description by editing the AI Agent's system prompt.

## Technologies Used

- n8n (workflow automation)
- Gmail API (trigger + send)
- Google Drive API (file storage)
- Google Sheets API (logging)
- OpenAI GPT-4o
- LangChain Information Extractor
- JavaScript (Code nodes for parsing and formatting)

## Workflow Diagram

![Workflow Diagram](workflow-diagram.png)

## Files in This Repo

- `Resume_Analyzer.json` — full exported n8n workflow, importable directly into any n8n instance
- `workflow-diagram.png` — visual overview of the automation

## Results

This project helped me practice building a real-world AI automation pipeline: connecting email triggers, file handling, structured AI extraction, conditional logic, and automated communication — all without writing a traditional backend.
