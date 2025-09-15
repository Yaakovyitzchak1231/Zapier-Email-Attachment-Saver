<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document Processing Workflow</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
          /* Color Tokens */
          --color-background: #f8fafc;
          --color-surface: #ffffff;
          --color-text: #0f172a;
          --color-text-secondary: #64748b;
          --color-border: #e2e8f0;
          --color-primary: #3b82f6;
          --color-primary-hover: #2563eb;
          --color-success: #22c55e;
          --color-error: #ef4444;
          --color-bg-subtle: #f1f5f9;

          /* Spacing & Sizing */
          --spacing-1: 0.25rem;
          --spacing-2: 0.5rem;
          --spacing-4: 1rem;
          --spacing-6: 1.5rem;
          --spacing-8: 2rem;

          /* Typography */
          --font-sans: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
          
          /* Effects */
          --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
          --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
          --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
        }

        /* Base Styles */
        * {
          box-sizing: border-box;
        }

        body {
          font-family: var(--font-sans);
          line-height: 1.6;
          color: var(--color-text);
          background: var(--color-background);
          margin: 0;
          padding: 0;
          min-height: 100vh;
        }

        .container {
          max-width: 1200px;
          margin: 0 auto;
          padding: var(--spacing-8) var(--spacing-4);
        }

        /* Header Styles */
        .workflow-header {
          text-align: center;
          margin-bottom: 3rem;
          padding: var(--spacing-8) 0;
        }

        .workflow-header h1 {
          font-size: 2.5rem;
          font-weight: 700;
          margin: 0 0 var(--spacing-2) 0;
        }

        .workflow-header p {
          font-size: 1.125rem;
          color: var(--color-text-secondary);
          margin: 0 0 var(--spacing-8) 0;
        }

        .controls {
          display: flex;
          gap: var(--spacing-4);
          justify-content: center;
          flex-wrap: wrap;
        }

        /* Button Styles */
        .btn {
          display: inline-flex;
          align-items: center;
          gap: var(--spacing-2);
          padding: 0.75rem 1.5rem;
          font-size: 0.875rem;
          font-weight: 600;
          border-radius: 8px;
          border: none;
          cursor: pointer;
          transition: all 0.2s ease;
          text-decoration: none;
          white-space: nowrap;
        }

        .btn--primary {
          background: var(--color-primary);
          color: white;
          box-shadow: var(--shadow-sm);
        }

        .btn--primary:hover {
          background: var(--color-primary-hover);
          transform: translateY(-1px);
          box-shadow: var(--shadow-md);
        }

        .btn--secondary {
          background: var(--color-surface);
          color: var(--color-text);
          border: 1px solid var(--color-border);
          box-shadow: var(--shadow-sm);
        }

        .btn--secondary:hover {
          background: var(--color-bg-subtle);
          border-color: #cbd5e1;
        }

        /* Workflow Container */
        .workflow-container {
          max-width: 800px;
          margin: 0 auto;
        }
        @media (min-width: 1024px) {
            .workflow-container {
                max-width: 1200px;
            }
        }


        .workflow-section {
          margin-bottom: 3rem;
        }

        /* Step Styles */
        .workflow-step {
          position: relative;
          margin: 0 auto var(--spacing-8) auto;
          max-width: 800px;
        }

        .step-connector {
          position: absolute;
          left: calc(1.5rem - 1px);
          top: 3rem;
          width: 2px;
          height: calc(100% - 1rem);
          background: var(--color-border);
          z-index: 1;
        }

        .workflow-step:last-child .step-connector,
        .decision-point + .workflow-branches .step-connector {
          display: none;
        }
        .decision-point .step-connector {
            display: none;
        }


        .step-card {
          background: var(--color-surface);
          border: 1px solid var(--color-border);
          border-radius: 12px;
          box-shadow: var(--shadow-sm);
          overflow: hidden;
          transition: all 0.3s ease;
          position: relative;
          z-index: 2;
        }

        .step-card:hover {
          box-shadow: var(--shadow-md);
          transform: translateY(-2px);
        }

        .step-header {
          display: flex;
          align-items: center;
          padding: var(--spacing-6);
          cursor: pointer;
          user-select: none;
        }

        .step-number {
          width: 3rem;
          height: 3rem;
          border-radius: 50%;
          display: flex;
          align-items: center;
          justify-content: center;
          font-weight: 700;
          font-size: 1.125rem;
          color: white;
          margin-right: var(--spacing-4);
          flex-shrink: 0;
        }

        .workflow-step[data-color="blue"] .step-number { background: #3b82f6; }
        .workflow-step[data-color="orange"] .step-number { background: #f97316; }
        .workflow-step[data-color="indigo"] .step-number { background: #6366f1; }
        .workflow-step[data-color="purple"] .step-number { background: #a855f7; }
        .workflow-step[data-color="green"] .step-number { background: #22c55e; }
        .workflow-step[data-color="red"] .step-number { background: #ef4444; }

        .step-info {
          flex: 1;
        }

        .step-info h3 {
          margin: 0 0 var(--spacing-1) 0;
          font-size: 1.25rem;
          font-weight: 600;
        }

        .step-info p {
          margin: 0;
          color: var(--color-text-secondary);
          font-size: 0.875rem;
        }

        .expand-btn {
          background: none;
          border: none;
          color: var(--color-text-secondary);
          cursor: pointer;
          padding: var(--spacing-2);
          border-radius: 4px;
          transition: all 0.2s ease;
        }
        .expand-btn svg {
          transition: transform 0.3s ease;
        }
        .step-card.expanded .expand-btn svg {
          transform: rotate(180deg);
        }


        /* Step Details */
        .step-details {
          background: var(--color-bg-subtle);
          border-top: 1px solid var(--color-border);
          max-height: 0;
          opacity: 0;
          overflow: hidden;
          transition: max-height 0.4s ease-in-out, opacity 0.4s ease-in-out, padding 0.4s ease-in-out;
          padding: 0 var(--spacing-6);
        }
        .step-details.expanded {
          max-height: 2000px; /* Large enough for content */
          opacity: 1;
          padding: var(--spacing-6);
        }
        .step-details > p {
            margin-top: 0;
        }


        .resource-links {
          display: flex;
          gap: var(--spacing-2);
          margin: var(--spacing-4) 0;
          flex-wrap: wrap;
        }

        .code-example {
          background: var(--color-surface);
          border: 1px solid var(--color-border);
          border-radius: 8px;
          margin-top: var(--spacing-4);
          position: relative;
        }
        .code-header {
          padding: 0.75rem var(--spacing-4);
          background: var(--color-bg-subtle);
          border-bottom: 1px solid var(--color-border);
          font-weight: 600;
          font-size: 0.875rem;
        }
        .code-example pre {
          margin: 0;
          padding: var(--spacing-4);
          overflow-x: auto;
          font-family: 'Menlo', 'Monaco', monospace;
          font-size: 0.875rem;
          line-height: 1.5;
          white-space: pre-wrap;
        }

        /* Workflow Branches */
        .workflow-branches {
          margin-top: var(--spacing-8);
          display: grid;
          gap: var(--spacing-8);
          grid-template-columns: 1fr;
        }
         @media (min-width: 1024px) {
            .workflow-branches {
                grid-template-columns: 1fr 1fr;
            }
        }


        .workflow-branch {
          border: 2px solid var(--color-border);
          border-radius: 16px;
          padding: var(--spacing-6);
        }
        .success-branch {
          border-color: var(--color-success);
          background: rgba(34, 197, 94, 0.05);
        }
        .manual-branch {
          border-color: var(--color-error);
          background: rgba(239, 68, 68, 0.05);
        }

        .branch-header {
          text-align: center;
          margin-bottom: var(--spacing-6);
        }
        .branch-label {
          display: inline-block;
          padding: var(--spacing-2) var(--spacing-4);
          border-radius: 20px;
          font-weight: 600;
          font-size: 0.875rem;
          color: white;
        }
        .branch-label.success { background: var(--color-success); }
        .branch-label.manual { background: var(--color-error); }

        /* Modal Styles */
        .modal {
          position: fixed;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;
          background-color: rgba(0, 0, 0, 0.6);
          display: flex;
          align-items: center;
          justify-content: center;
          z-index: 1000;
          padding: 1rem;
        }
        .modal.hidden { display: none; }
        .modal-content {
          background: var(--color-surface);
          border-radius: 12px;
          box-shadow: var(--shadow-lg);
          width: 100%;
          max-width: 600px;
          max-height: 90vh;
          overflow-y: auto;
        }
        .modal-header {
          padding: var(--spacing-6);
          display: flex;
          justify-content: space-between;
          align-items: center;
          border-bottom: 1px solid var(--color-border);
        }
        .modal-header h3 { margin: 0; font-size: 1.25rem; }
        .modal-close {
          background: none;
          border: none;
          font-size: 1.5rem;
          color: var(--color-text-secondary);
          cursor: pointer;
        }
        .modal-body {
          padding: var(--spacing-6);
        }
        .modal-body .prose h4 { margin-top: 1.5em; }
        .modal-body .prose ul { padding-left: 1.5em; }


        /* Input/Output Grid */
        .input-output-grid {
          display: grid;
          grid-template-columns: 1fr 1fr;
          gap: var(--spacing-4);
          margin-top: var(--spacing-4);
        }
        .input-section, .output-section {
          background: var(--color-bg-subtle);
          padding: var(--spacing-4);
          border-radius: 8px;
          border: 1px solid var(--color-border);
        }
        .input-section strong, .output-section strong {
          display: block;
          margin-bottom: var(--spacing-2);
          font-weight: 600;
        }

        /* Progress Bar */
        .progress-bar {
          position: fixed;
          top: 0;
          left: 0;
          width: 100%;
          height: 3px;
          background: var(--color-bg-subtle);
          z-index: 999;
        }
        .progress-fill {
          height: 100%;
          background: var(--color-primary);
          width: 0%;
          transition: width 0.1s ease;
        }

        .loader {
            border: 2px solid #f3f3f3;
            border-top: 2px solid #fff;
            border-radius: 50%;
            width: 16px;
            height: 16px;
            animation: spin 1s linear infinite;
            display: inline-block;
            margin-left: 8px;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }


        /* Responsive Design */
        @media (max-width: 1023px) {
          .workflow-branches {
            grid-template-columns: 1fr;
          }
          .input-output-grid {
            grid-template-columns: 1fr;
          }
        }
    </style>
</head>
<body>
    <div class="progress-bar">
        <div class="progress-fill"></div>
    </div>
    <div class="container">
        <header class="workflow-header">
            <h1>Document Processing Workflow</h1>
            <p>Interactive flowchart of the automated document pipeline</p>
            <div class="controls">
                <button class="btn btn--primary" id="summarizeBtn">✨ Summarize Workflow with AI</button>
                <button class="btn btn--secondary" id="expand-all">Expand All</button>
                <button class="btn btn--secondary" id="collapse-all">Collapse All</button>
            </div>
        </header>
        
        <div id="summaryContainer" class="modal hidden">
             <div class="modal-content">
                <div class="modal-header">
                    <h3>AI-Generated Workflow Summary</h3>
                    <button class="modal-close" id="closeSummaryModal">&times;</button>
                </div>
                <div class="modal-body">
                    <div id="summaryContent" class="prose"></div>
                </div>
            </div>
        </div>

        <div class="workflow-container">
            <!-- Main workflow steps 1-7 -->
            <div class="workflow-section">
                <!-- Step 1 -->
                <div class="workflow-step" data-step="1" data-color="blue">
                    <div class="step-connector"></div>
                    <div class="step-card">
                        <div class="step-header">
                            <div class="step-number">1</div>
                            <div class="step-info">
                                <h3>Gmail AppScript Plugin</h3>
                                <p>User selects attachments in Gmail to initiate the process.</p>
                            </div>
                            <button class="expand-btn" aria-label="Expand step details"><svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                        </div>
                        <div class="step-details hidden">
                             <p>User opens the Gmail addon on an email with attachments. The UI shows checkboxes for each attachment. The user selects which files to process and clicks "Submit". This action sends a webhook to Zapier with the email and attachment metadata.</p>
                            <div class="resource-links">
                                <a href="https://script.google.com/d/1lVqg1SFKwD3W4a1TMkuXBGXvHbexwGrSkeWjWbA8YlC4rL0fOqHCtGam/edit?usp=sharing" target="_blank" rel="noopener noreferrer" class="btn btn--primary">View AppScript <svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"></path></svg></a>
                            </div>
                            <div class="code-example">
                                <div class="code-header">Payload Sent to Zapier:</div>
                                <pre><code>{
  "emailId": "&lt;message-id&gt;",
  "attachments": [
    { "originalName": "Purchase_Agreement.pdf" },
    { "originalName": "Bank_Statement_Feb.pdf" }
  ]
}</code></pre>
                            </div>
                        </div>
                    </div>
                </div>
                <!-- Step 2 -->
                <div class="workflow-step" data-step="2" data-color="orange">
                    <div class="step-connector"></div>
                    <div class="step-card">
                        <div class="step-header">
                            <div class="step-number">2</div>
                            <div class="step-info">
                                <h3>Zapier: Catch Webhook</h3>
                                <p>Receives the initial request from the Gmail plugin.</p>
                            </div>
                            <button class="expand-btn" aria-label="Expand step details"><svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                        </div>
                        <div class="step-details hidden">
                            <p>The workflow begins when Zapier's "Catch Webhook" trigger receives the POST request from the Google AppScript plugin.</p>
                            <div class="code-example">
                                <div class="code-header">Data Received:</div>
                                <pre><code>The exact payload from Step 1 is now available in Zapier.</code></pre>
                            </div>
                        </div>
                    </div>
                </div>
                 <!-- Step 3 -->
                <div class="workflow-step" data-step="3" data-color="orange">
                    <div class="step-connector"></div>
                    <div class="step-card">
                        <div class="step-header">
                            <div class="step-number">3</div>
                            <div class="step-info">
                                <h3>Zapier: Gmail Email Lookup</h3>
                                <p>Enriches the data with full email details.</p>
                            </div>
                           <button class="expand-btn" aria-label="Expand step details"><svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                        </div>
                        <div class="step-details hidden">
                             <p>Using the `emailId` from the webhook, Zapier's Gmail integration finds the specific email and retrieves its full metadata, including sender, subject, and download URLs for *all* attachments in the email.</p>
                             <div class="code-example">
                                <div class="code-header">Data Added at this Step:</div>
                                <pre><code>{
  "fromName": "Yaakov Trout",
  "fromEmail": "yaakov@example.com",
  "emailSubject": "3654 Malden Avenue...",
  "all_attachments": [
    { "filename": "Purchase_Agreement.pdf", "url": "..." },
    { "filename": "Bank_Statement_Feb.pdf", "url": "..." },
    { "filename": "random_signature.jpg", "url": "..." }
  ]
}</code></pre>
                            </div>
                        </div>
                    </div>
                </div>
                 <!-- Step 4 -->
                <div class="workflow-step" data-step="4" data-color="orange">
                    <div class="step-connector"></div>
                    <div class="step-card">
                        <div class="step-header">
                            <div class="step-number">4</div>
                            <div class="step-info">
                                <h3>Zapier: JavaScript Attachment Matching</h3>
                                <p>Matches the user's selection with the found files.</p>
                            </div>
                           <button class="expand-btn" aria-label="Expand step details"><svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                        </div>
                        <div class="step-details hidden">
                            <p>A custom JavaScript step compares the list of attachments selected by the user (from Step 1) with the full list retrieved from Gmail (from Step 3). It uses fuzzy name matching (checking if one filename `contains` the other) to find the correct files and returns only the matched attachments with their download URLs.</p>
                            <div class="code-example">
                                <div class="code-header">Input/Output Example:</div>
                                <div class="input-output-grid">
                                    <div class="input-section">
                                        <strong>Input to Script:</strong>
                                        <pre><code>// From Webhook
"webhookAttachments": "Purchase_Agreement.pdf,Bank_Statement_Feb.pdf"

// From Gmail Lookup
"att1Name": "Purchase_Agreement.pdf", 
"att2Name": "Bank_Statement_Feb.pdf",
"att3Name": "random_signature.jpg",
...</code></pre>
                                    </div>
                                    <div class="output-section">
                                        <strong>Output from Script:</strong>
                                        <pre><code>[
  {
    "filename": "Purchase_Agreement.pdf",
    "downloadUrl": "https://..."
  },
  {
    "filename": "Bank_Statement_Feb.pdf",
    "downloadUrl": "https://..."
  }
]</code></pre>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <!-- Step 5 -->
                <div class="workflow-step" data-step="5" data-color="orange">
                    <div class="step-connector"></div>
                    <div class="step-card">
                        <div class="step-header">
                            <div class="step-number">5</div>
                            <div class="step-info">
                                <h3>Zapier: Send to Python Function</h3>
                                <p>Sends the prepared file data for processing.</p>
                            </div>
                           <button class="expand-btn" aria-label="Expand step details"><svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                        </div>
                        <div class="step-details hidden">
                            <p>The final Zapier step sends a new webhook to the custom Python function. The payload includes a comma-separated list of the matched file URLs and the original email metadata.</p>
                             <div class="code-example">
                                <div class="code-header">Payload to Python:</div>
                                <pre><code>{
  "file_urls": "https://.../Purchase_Agreement.pdf,https://.../Bank_Statement_Feb.pdf",
  "from_name": "Yaakov Trout",
  "email_subject": "3654 Malden Avenue, Baltimore MD"
}</code></pre>
                            </div>
                        </div>
                    </div>
                </div>
                <!-- Step 6 -->
                <div class="workflow-step" data-step="6" data-color="indigo">
                    <div class="step-connector"></div>
                    <div class="step-card">
                        <div class="step-header">
                            <div class="step-number">6</div>
                            <div class="step-info">
                                <h3>Python Text Extraction</h3>
                                <p>A serverless function downloads files and extracts text content.</p>
                            </div>
                           <button class="expand-btn" aria-label="Expand step details"><svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                        </div>
                        <div class="step-details hidden">
                            <p>The Python function downloads each file, detects its type and extracts the raw text. For PDFs, it sends the full extracted text from the first 5 pages, along with metadata like page and character counts, back to Zapier. Execution logs are stored in a service like Google Cloud Logging.</p>
                            <div class="resource-links">
                                <a href="https://drive.google.com/file/d/1FPH-f4sPr-aLDTQ527rQ4F692u1a2Shk/view?usp=drive_link" target="_blank" rel="noopener noreferrer" class="btn btn--primary">View Python Script <svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"></path></svg></a>
                                <a href="https://docs.google.com/document/d/1c7W0_aqM9_z0YJeAG0c-eOW3pL68BVZ4tczTvpZSxXU/edit?usp=sharing" target="_blank" rel="noopener noreferrer" class="btn btn--secondary">View Sample Log <svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg></a>
                            </div>
                            <div class="code-example">
                                <div class="code-header">Payload Sent back to Zapier:</div>
                                <pre><code>[
  {
    "status": "text_extracted",
    "filename": "Purchase_Agreement.pdf",
    "extracted_text": "Property Address 3654 Malden Ave...",
    "character_count": 769,
    "file_url": "https://...",
    "total_pages": 2,
    "pages_processed": 2
  },
  {
    "status": "text_extracted",
    "filename": "Bank_Statement_Feb.pdf",
    "extracted_text": "P.O. Box 15284 Customer service...",
    "character_count": 11807,
    "file_url": "https://...",
    "total_pages": 8,
    "pages_processed": 5
  }
]</code></pre>
                            </div>
                        </div>
                    </div>
                </div>
                <!-- Step 7 -->
                <div class="workflow-step decision-point" data-step="7" data-color="purple">
                    <div class="step-card">
                        <div class="step-header" style="cursor: default;">
                            <div class="step-number">7</div>
                            <div class="step-info">
                                <h3>Processing Split</h3>
                                <p>The workflow branches based on text extraction results.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Branching paths -->
            <div class="workflow-branches">
                <!-- Success Path -->
                <div class="workflow-branch success-branch">
                    <div class="branch-header">
                        <span class="branch-label success">✓ TEXT EXTRACTED PATH</span>
                    </div>
                    <!-- Step 8a -->
                    <div class="workflow-step" data-step="8a" data-color="green">
                        <div class="step-connector"></div>
                        <div class="step-card">
                            <div class="step-header">
                                <div class="step-number">8a</div>
                                <div class="step-info">
                                    <h3>Gemini AI Classification</h3>
                                    <p>AI classifies documents and determines destination folder.</p>
                                </div>
                               <button class="expand-btn" aria-label="Expand step details"><svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                            </div>
                            <div class="step-details hidden">
                                <p>The extracted text for each document is sent to the Gemini API with a detailed prompt instructing it to classify the document and determine its destination folder (BORROWER vs. DEAL).</p>
                                <div class="resource-links">
                                     <a href="https://docs.google.com/document/d/1oYC27JzERGc03tHxG3udY-zk_oMnadKtk11T3i0HD5k/edit?usp=sharing" target="_blank" class="btn btn--primary">View Full Prompt <svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg></a>
                                     <a href="https://docs.google.com/spreadsheets/d/1_gwHN98yhSFt_9NC5X6v-jw_T9zcH0Hnku2jG8-P_l4/edit?usp=sharing" target="_blank" class="btn btn--secondary">Classification Rules <svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01"></path></svg></a>
                                </div>
                                <div class="code-example">
                                    <div class="code-header">Gemini API Response (Structured JSON):</div>
                                    <pre><code>[
  {
    "document_type": "Purchase Agreement",
    "folder": "Deal",
    "original_name": "Purchase_Agreement.pdf",
    "explanation": "This document outlines the terms of a property sale...",
    "document_url": "https://..."
  },
  {
    "document_type": "Bank statements",
    "folder": "Borrower",
    "original_name": "Bank_Statement_Feb.pdf",
    "explanation": "The document is a Bank of America statement...",
    "document_url": "https://..."
  }
]</code></pre>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- Step 9a -->
                    <div class="workflow-step" data-step="9a" data-color="green">
                        <div class="step-connector"></div>
                        <div class="step-card">
                            <div class="step-header">
                                <div class="step-number">9a</div>
                                <div class="step-info">
                                    <h3>Zapier Agent - Folder Finder</h3>
                                    <p>Finds the correct Google Drive folder automatically.</p>
                                </div>
                                <button class="expand-btn" aria-label="Expand step details"><svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                            </div>
                            <div class="step-details hidden">
                                <p>A Zapier Agent executes a complex set of rules to route documents. For 'Deal' files, it extracts an address from the email subject and searches for a matching folder. For 'Borrower' files, it searches for a folder matching the sender's name. If a folder cannot be found, it notifies a human for manual intervention.</p>
                                <div class="resource-links">
                                     <a href="https://docs.google.com/document/d/1hTqp11Tqtm5BKyaM3PqzfJXm5ue8mbLGcmaStFRtBwA/edit?usp=sharing" target="_blank" rel="noopener noreferrer" class="btn btn--primary">View Agent Prompt <svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg></a>
                                     <a href="https://docs.google.com/document/d/1ZKX_4QiWPOWqzaJ5mRoZUf5Itlaj3s1p8hoytGauGOk/edit?usp=sharing" target="_blank" rel="noopener noreferrer" class="btn btn--secondary">Folder Naming Rules <svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01"></path></svg></a>
                                </div>
                                <div class="code-example">
                                     <div class="code-header">Input/Output Example:</div>
                                    <div class="input-output-grid">
                                        <div class="input-section">
                                            <strong>Input to Agent:</strong>
                                            <pre><code>{
  "from_name": "Yaakov Trout",
  "email_subject": "3654 Malden Avenue...",
  "document_types": "Purchase Agreement,Bank statements",
  ...
}</code></pre>
                                        </div>
                                        <div class="output-section">
                                            <strong>Output from Agent:</strong>
                                            <pre><code>{
  "Deal_Folder_ID": "1u5P-...",
  "Borrower_Folder_ID": "1lLvO-...",
  "Deal_Folder_Document_URLs": "https://...",
  "Borrower_Folder_Document_URLs": "https://..."
}</code></pre>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- Step 10a -->
                    <div class="workflow-step" data-step="10a" data-color="green">
                        <div class="step-card">
                            <div class="step-header">
                                <div class="step-number">10a</div>
                                <div class="step-info">
                                    <h3>Google Drive Upload & Rename</h3>
                                    <p>Automated upload to correct folder with proper naming.</p>
                                </div>
                                <button class="expand-btn" aria-label="Expand step details"><svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                            </div>
                            <div class="step-details hidden">
                                <p>Files are uploaded to their correct Google Drive folders and renamed based on their classification. For example:</p>
                                 <div class="code-example">
                                    <div class="code-header">Final File Operations:</div>
                                    <pre><code>// DEAL DOCUMENT
1. UPLOAD "Purchase_Agreement.pdf" to folder "1u5P-..."
2. RENAME file to "Purchase Agreement.pdf"

// BORROWER DOCUMENT
1. UPLOAD "Bank_Statement_Feb.pdf" to folder "1lLvO-..."
2. RENAME file to "Bank statements.pdf"</code></pre>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Manual Path -->
                <div class="workflow-branch manual-branch">
                    <div class="branch-header">
                        <span class="branch-label manual">✗ MANUAL REVIEW PATH</span>
                    </div>
                    <!-- Step 8b -->
                    <div class="workflow-step" data-step="8b" data-color="red">
                        <div class="step-connector"></div>
                        <div class="step-card">
                            <div class="step-header">
                                <div class="step-number">8b</div>
                                <div class="step-info">
                                    <h3>Human in the Loop</h3>
                                    <p>Human manually reviews and classifies document category.</p>
                                </div>
                                <button class="expand-btn" aria-label="Expand step details"><svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                            </div>
                            <div class="step-details hidden">
                                <p>A notification is sent via Google Hangouts. A human manually reviews the document and determines its category (e.g., "CPL"). The automation then uses this classification to determine the folder routing.</p>
                            </div>
                        </div>
                    </div>
                    <!-- Step 9b -->
                    <div class="workflow-step" data-step="9b" data-color="red">
                        <div class="step-connector"></div>
                        <div class="step-card">
                            <div class="step-header">
                                <div class="step-number">9b</div>
                                <div class="step-info">
                                    <h3>Manual → Agent</h3>
                                    <p>Uses human classification to find correct folder.</p>
                                </div>
                                <button class="expand-btn" aria-label="Expand step details"><svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                            </div>
                            <div class="step-details hidden">
                                <p>The Zapier Agent is triggered with the human-provided classification and follows the same logic as the automated path to find the correct Google Drive folder.</p>
                                <div class="code-example">
                                    <div class="code-header">Input to Agent (from Human Classification):</div>
                                    <pre><code>{
  "from_name": "Yaakov Trout",
  "email_subject": "3654 Malden Avenue...",
  "original_filenames": "scanned_cpl.pdf",
  "document_types": "CPL (Closing Protection Letter)",
  "folders": "Deal",
  "document_urls": "https://.../scanned_cpl.pdf"
}</code></pre>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- Step 10b -->
                    <div class="workflow-step" data-step="10b" data-color="red">
                        <div class="step-card">
                            <div class="step-header">
                                <div class="step-number">10b</div>
                                <div class="step-info">
                                    <h3>Google Drive Upload & Rename</h3>
                                    <p>Upload after manual classification with proper naming.</p>
                                </div>
                               <button class="expand-btn" aria-label="Expand step details"><svg width="16" height="16" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg></button>
                            </div>
                            <div class="step-details hidden">
                                 <p>The process completes by uploading the file to the correct folder and renaming it based on the human's classification.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const expandAllBtn = document.getElementById('expand-all');
            const collapseAllBtn = document.getElementById('collapse-all');
            const summarizeBtn = document.getElementById('summarizeBtn');
            const summaryContainer = document.getElementById('summaryContainer');
            const closeSummaryModal = document.getElementById('closeSummaryModal');

            const allSteps = document.querySelectorAll('.workflow-step');

            // Toggle individual step
            allSteps.forEach(step => {
                const header = step.querySelector('.step-header');
                if (header && !step.classList.contains('decision-point')) {
                    header.addEventListener('click', () => toggleStep(step, true));
                }
            });

            function toggleStep(step, userClicked) {
                const card = step.querySelector('.step-card');
                const details = step.querySelector('.step-details');
                if (card && details) {
                    card.classList.toggle('expanded');
                    details.classList.toggle('expanded');
                    
                    if (userClicked) {
                         setTimeout(() => {
                            step.scrollIntoView({ behavior: 'smooth', block: 'center' });
                        }, 400);
                    }
                }
            }

            // Expand/Collapse All
            expandAllBtn.addEventListener('click', () => allSteps.forEach(step => {
                if (!step.querySelector('.step-card')?.classList.contains('expanded')) {
                    toggleStep(step, false);
                }
            }));
            collapseAllBtn.addEventListener('click', () => allSteps.forEach(step => {
                if (step.querySelector('.step-card')?.classList.contains('expanded')) {
                    toggleStep(step, false);
                }
            }));

            // --- Gemini AI Summary ---
            summarizeBtn.addEventListener('click', () => {
                const summaryContent = document.getElementById('summaryContent');
                const workflowPrompt = `
                    You are an expert technical writer. Your task is to summarize the provided document processing workflow.
                    Generate a response in clean, easy-to-read HTML format.
                    The response should include:
                    1.  A main heading (h3) titled "Workflow Summary".
                    2.  A brief introductory paragraph explaining the purpose of the workflow.
                    3.  A sub-heading (h4) for "Key Technologies".
                    4.  An unordered list detailing the technologies used (Google AppScript, Zapier, Python, Gemini AI).
                    5.  A sub-heading (h4) for "Workflow Paths".
                    6.  An unordered list explaining the two main paths: the automated "Text Extracted Path" and the "Manual Review Path".

                    Workflow Details:
                    - Starts with a user selecting attachments in a Gmail AppScript Plugin.
                    - Zapier automates catching the request, looking up the email, matching files, and sending them to a Python function.
                    - A Python function extracts text.
                    - The flow splits: if text is extracted, Gemini AI classifies it, a Zapier Agent finds the GDrive folder, and the file is uploaded. If not, a human is notified to classify it, then the automation resumes to find the folder and upload.
                `;
                callGeminiApi(workflowPrompt, summarizeBtn, summaryContent);
            });

            async function callGeminiApi(prompt, button, contentElement) {
                const originalButtonHTML = button.innerHTML;
                button.innerHTML = 'Generating... <div class="loader"></div>';
                button.disabled = true;

                const apiKey = ""; // Provided by Canvas environment
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

                try {
                    const payload = { contents: [{ parts: [{ text: prompt }] }] };
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    if (!response.ok) throw new Error(`API error: ${response.status}`);
                    
                    const result = await response.json();
                    const text = result.candidates?.[0]?.content?.parts?.[0]?.text;

                    if (text) {
                        contentElement.innerHTML = text;
                        summaryContainer.classList.remove('hidden');
                    } else {
                        throw new Error("No content in API response.");
                    }
                } catch (error) {
                    console.error("Gemini API Error:", error);
                    contentElement.innerHTML = "<p>Sorry, an error occurred while generating the summary.</p>";
                    summaryContainer.classList.remove('hidden');
                } finally {
                    button.innerHTML = originalButtonHTML;
                    button.disabled = false;
                }
            }

            closeSummaryModal.addEventListener('click', () => {
                summaryContainer.classList.add('hidden');
            });

            // --- Progress Bar ---
            const progressBarFill = document.querySelector('.progress-fill');
            window.addEventListener('scroll', () => {
                const scrollTop = document.documentElement.scrollTop;
                const scrollHeight = document.documentElement.scrollHeight - document.documentElement.clientHeight;
                const scrollPercent = (scrollTop / scrollHeight) * 100;
                progressBarFill.style.width = `${scrollPercent}%`;
            });
        });
    </script>
</body>
</html>

