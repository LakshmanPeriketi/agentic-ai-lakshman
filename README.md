AI Agent Workflow: Scheduled News & Weather Briefing and Logging
1. Project Overview

This project presents an automated system that periodically collects, processes, and delivers both weather updates and news summaries through email. The implementation uses n8n as the workflow automation platform, running inside a Docker container for scalability and consistency.

The system integrates multiple APIs and AI tools to perform the following tasks:

Fetch real-time data from external sources.

Generate human-readable summaries and reports.

Format and style the output for email communication.

Log the collected data into Google Sheets for record maintenance.

2. Tools and Technologies
Docker

Provides the containerized environment for running the workflow automation tool.

Ensures platform independence and eliminates dependency issues during deployment.

Offers an isolated runtime environment, improving reliability and performance.

n8n (Open-Source Workflow Automation)

n8n (pronounced “n-eight-n”) is an open-source tool that enables automation by connecting APIs, services, and databases without complex coding.

Supports visual workflow building and real-time process scheduling.

In this project, it acts as the central controller for triggering, processing, and routing data between different modules.

3. Workflow Description
Step 1: Schedule Trigger

The workflow begins with a Schedule Trigger node that defines when the process should run.
This can be configured to execute daily, weekly, or at custom intervals using cron expressions.






Step 2: AI Agent Activation

Once triggered, an AI Agent node activates and connects to multiple data sources:

Google Gemini Chat Model for reasoning and summarization.

Simple Memory for retaining short-term workflow context.

Wikipedia for retrieving summarized public news.

OpenWeatherMap API for real-time weather data.

SerpAPI for web search-based results.

The AI Agent compiles and formats the collected information into a structured text report.



Step 3: Markdown to HTML Conversion

The generated text (in Markdown format) is converted into HTML to prepare for email-friendly formatting and readability.



Step 4: HTML Styling

A second model applies inline CSS styling to the HTML content, ensuring that the final output appears professional and consistent across different email platforms.


Step 5: Email Dispatch (Gmail Node)

The finalized HTML report is sent automatically to the recipient using Gmail integration.
This step allows the delivery of daily or periodic news and weather briefings.

Step 6: Categorization

After the email is sent, another processing node classifies the news items into specific categories such as:

Date

Trending News

Sports

Politics

Movie or Entertainment News

Step 7: Logging in Google Sheets

The categorized data is appended as a new row in a Google Sheet, creating a historical record of all briefings for later analysis or verification.

4. Installation and Setup Guide

This section explains the complete installation procedure for deploying and running the workflow successfully.

Step 1: Install Docker

Visit the official Docker website: https://www.docker.com/products/docker-desktop

Download Docker Desktop compatible with your operating system (Windows, macOS, or Linux).

Run the installer and follow on-screen instructions.

After installation, launch Docker Desktop and ensure it is running in the background.

To verify the installation, open a terminal and execute:

docker --version


This command should display the installed Docker version.

Step 2: Pull and Run n8n Docker Image

Open your terminal or command prompt.

Run the following Docker command to start an n8n container:

docker run -it --rm \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n


Explanation of parameters:

-p 5678:5678 → Maps container port 5678 to your local port.

-v ~/.n8n:/home/node/.n8n → Mounts a local directory to save n8n data persistently.

n8nio/n8n → Pulls the official n8n image from Docker Hub.

After running the command, open your browser and go to:
http://localhost:5678

This will launch the n8n web interface.

Step 3: Create the Workflow in n8n

In the n8n dashboard, click “New Workflow.”

Add the following nodes sequentially:

Schedule Trigger → Defines time interval for execution.

<img width="517" height="852" alt="SheduleTrigger" src="https://github.com/user-attachments/assets/09640f57-75ce-41d7-8943-047d03fd7393" />



AI Agent Node → Connects to Gemini, Wikipedia, OpenWeatherMap, and SerpAPI.

<img width="1895" height="846" alt="AI Agent1" src="https://github.com/user-attachments/assets/b04a5a4a-92d0-48c6-b010-8b298cd08afc" />

Markdown to HTML → Converts plain text output to HTML.

<img width="1912" height="699" alt="Markdown" src="https://github.com/user-attachments/assets/3c906f84-fe42-4eba-8b75-94025f409f93" />



Message a Model (Styling) → Applies inline CSS to enhance readability.


<img width="1898" height="835" alt="GeminiAI Model" src="https://github.com/user-attachments/assets/6ca59d13-5f69-4110-95f8-fd42cb163eb8" />


Gmail Node → Sends the formatted report via email.

<img width="501" height="877" alt="Gmail" src="https://github.com/user-attachments/assets/cf134f4c-68bf-4242-9d0e-d2b901188ef7" />

Message a Model (Categorization) → Categorizes the news into defined sections.

Google Sheets (Append Row) → Logs the results for record keeping.
<img width="559" height="852" alt="GoogleSheets" src="https://github.com/user-attachments/assets/4789bc23-bad7-4b97-bfd1-2792d8f68dc3" />

Connect each node in the given order using the drag-and-drop interface.

Step 4: Obtain and Configure API Credentials
a. OpenWeatherMap API

Sign up at https://openweathermap.org/api
.

After registration, copy your unique API key from your dashboard.

Use this key in the “API Key” field of the OpenWeatherMap node in n8n.

b. SerpAPI

Visit https://serpapi.com/
 and create a free or paid account.

Generate an API key and insert it into the SerpAPI node configuration.

c. Google OAuth Credentials

Create a Google Cloud Project at https://console.cloud.google.com/
.

Enable the Gmail API and Google Sheets API.

Create OAuth credentials and download the credentials.json file.

Upload or configure these credentials in the respective Gmail and Google Sheets nodes.

d. Gemini Chat Model Access

Configure the Gemini API or AI model connection within n8n using your access credentials.

Step 5: Execute and Validate the Workflow

Click “Execute Workflow” to perform a test run.

Observe each node to ensure successful data flow between steps.

Once verified, activate Schedule Trigger for automated periodic execution.

Check your Gmail inbox to confirm receipt of the styled email report.

Open your connected Google Sheet to verify that the categorized data has been recorded properly.

![Workflow](https://github.com/user-attachments/assets/3beeb0af-a676-42bb-9f1e-f9d26060b533)



5. Output
Email Output

Automatically generated HTML email containing:
![Gmail1](https://github.com/user-attachments/assets/a9abc488-cb49-4b36-b241-9c580b1bf061)

![Gmail2](https://github.com/user-attachments/assets/5db95b29-45d1-40a8-92cc-8eb3b6503187)



Current weather conditions (temperature, humidity, city name, etc.).

Summarized news topics from multiple sources.

Professionally styled layout using inline CSS.

Google Sheets Output

A structured table storing:
![Google Sheet](https://github.com/user-attachments/assets/1f63640e-6cce-4bbe-8a92-3cd08b55f38a)


Date and time of execution.

Categorized news segments (Trending, Sports, Politics, Movies).

Weather summary details.

6. Advantages

Completely automated and requires no manual operation once configured.

Reliable integration of multiple APIs for real-time updates.

Clear and structured presentation of information through HTML styling.

Easy tracking and data management using Google Sheets.

Runs in a Docker container ensuring portability and consistency.

7. Limitations and Improvements
Limitation	Possible Improvement
Dependency on external APIs	Add backup APIs or caching for reliability.
Internet connectivity required	Introduce retry and offline data buffering.
Short-term AI memory	Integrate database or vector storage for persistent memory.
Varying email formats	Use pre-tested responsive templates.
One-way communication	Extend system with chatbot or dashboard.
Security risks with API keys	Encrypt credentials and use OAuth authentication.
8. Future Enhancements

User Personalization: Allow users to select preferred categories of news.

Voice Reports: Integrate text-to-speech for audio-based summaries.

Web Dashboard: Display reports and logs on a web interface.

Trend and Sentiment Analysis: Analyze recurring topics and news sentiment.

Chatbot Integration: Enable live query handling via Telegram or WhatsApp.

9. Developer Information

Developed by: Periketi Lakshman
Roll Number: 24071A05J6
Department: Computer Science and Engineering (CSE - C)

10. Acknowledgement

This project was developed as part of an academic initiative to demonstrate the integration of artificial intelligence with workflow automation.
It highlights the application of open-source platforms to automate daily information retrieval and structured report generation.

© 2025 Periketi Lakshman. All rights reserved.
