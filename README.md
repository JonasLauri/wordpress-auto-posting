## WordPress Auto-Posting Workflow (n8n)

### Overview
This workflow automates the creation of WordPress draft posts from Google Docs or TXT files.  
It uses Gemini AI to generate structured article JSON (title, excerpt, content, hero prompt, etc.), converts it to HTML, and posts it directly to WordPress via the REST API.

### **Technical Setup & Key Decisions**

- The workflow is **built and tested locally** in n8n, currently triggered manually.  
- **Input**: hardcoded Google Docs export URL (TXT format), fetched and normalized into plain text.  
- **Gemini AI (API Key)**: chosen for formatting due to free and stable access for testing.  
- **Credentials**: all keys (Gemini API, WordPress endpoint) are stored securely in the **n8n Credential Manager** — nothing is hardcoded in nodes.  
- **Output**: HTML content formatted by Gemini, verified via Webhook.site, and ready for WordPress import.  

##

### Workflow Steps
1. **Input — Title & Doc URL**  
   Manual input of a document link and optional title.
2. **Fetch Doc (TXT)**  
   Retrieves the document text from the provided link.
3. **Normalize — raw_text**  
   Cleans and prepares the text for AI formatting.
4. **AI Formatting (Gemini)**  
   Generates structured JSON with title, excerpt, content HTML, and metadata.
5. **Extract & Clean Gemini JSON**  
   Parses the AI output into usable fields.
6. **Prepare Preview File**  
   Builds filename and clean HTML content.
7. **Convert to File**  
   Converts HTML content into a `.html` preview file.
8. **HTTP Request — WordPress**  
   Sends the article data (title, content, excerpt, status=draft) to the WordPress REST API.

![Workflow Overview](assets/workflow-screenshot.png)
<p align="center"><sub><em>Figure 1 — n8n workflow from input to WordPress post creation.</em></sub></p>

##

### WordPress Integration
- **Endpoint:** `https://your-site.com/wp-json/wp/v2/posts`
- **Auth:** Basic Auth with a WordPress Application Password.
- **Status:** Posts are created as *drafts* by default.

##

### How to Run
1. Open n8n editor.
2. Import `wordpress-auto-posting.json`.
3. Set up Gemini and WordPress credentials.
4. Run the workflow manually.

##

### Example Output
- Post title: `Common Insurance Claims in HR Consulting`
- Status: `draft`
- WordPress response: `201 Created`
- HTML preview: `common-insurance-claims-in-hr-consulting.html`

##

### **Areas for Improvement**

- Add automated **Google Drive trigger** (folder watcher) to replace manual start.  
- Experiment with **Gemini prompt tuning** for improved styling and layout (headings, images, subheadings).  
- Add **duplicate prevention logic** to avoid re-posting identical drafts.  
- Implement **dynamic category and tag mapping** using extracted keywords or Gemini metadata.  
- Extend workflow to **publish directly** through the WordPress REST API once live credentials are active. 

### **Blockers / Limitations**

- **Google Drive API Authentication** could not be completed in n8n due to credential approval issues.  
- **WordPress API connection** was not tested end-to-end yet, pending valid access credentials.
