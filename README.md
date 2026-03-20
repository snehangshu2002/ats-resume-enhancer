# AI Resume Optimizer & ATS Enhancer

A no-nonsense n8n workflow that helps you get past the bots. Upload your resume, paste a job description, and get back an ATS-optimized version plus a detailed breakdown of where you stand.

---

## What this actually does

Most resumes never reach human eyes. They get filtered out by applicant tracking systems before anyone reads them. This workflow fixes that.

You paste a job description and upload your resume. The workflow analyzes what the job actually wants, compares it to what your resume currently says, and rewrites everything to maximize your chances. You get an optimized PDF back via email, plus a full report on your ATS score and what you need to work on.

---

## Features

**Resume optimization**
- Rewrites your summary to include the right keywords
- Restructures bullet points using action verbs from the job posting
- Keeps everything factual—no made-up experience

**ATS score with breakdown**
- Keyword match (40 points)
- Experience relevance (30 points)
- Seniority alignment (20 points)
- Education match (10 points)

**Practical next steps**
- Specific skills you should learn (with resources)
- Projects that would actually help for this role
- Certifications worth pursuing
- Honest gaps between your profile and the job requirements

**Why scores above 90% are rare**
The scoring is deliberately conservative. A 75% match with a clear path forward is more useful than an inflated 95% that sets you up for rejection later.

---

## How it works

1. **Form submission**: User enters job description, uploads resume PDF, and provides name/email
2. **PDF extraction**: Text is pulled from the resume using n8n's built-in extraction
3. **Job analysis**: Mistral Large parses the JD for keywords, skills, requirements, and red flags
4. **Resume rewrite**: AI generates an ATS-optimized version using the original content
5. **PDF generation**: The optimized resume is converted via html2pdf.app
6. **Delivery**: Gmail sends the PDF with the full ATS report in the email body

---

## What you need to set up

### Required credentials

| Service | Purpose |
|---------|---------|
| **Mistral Cloud API** | Two AI calls: one to analyze the job, one to rewrite the resume |
| **Gmail OAuth2** | Sends the final email with PDF attachment |
| **html2pdf.app** | Converts the optimized HTML resume to PDF (free tier available) |

### Getting your API keys

**Mistral Cloud**
1. Sign up at [console.mistral.ai](https://console.mistral.ai)
2. Create an API key
3. Add to n8n credentials as "Mistral Cloud account"

**html2pdf.app**
1. Register at [html2pdf.app](https://html2pdf.app)
2. Copy your API key from the dashboard
3. Replace the placeholder in the HTTP Request node

**Gmail OAuth2**
1. Use n8n's built-in credential setup
2. Follow the OAuth flow to authorize your Gmail account

---

## Workflow steps in detail

| Step | Node | What happens |
|------|------|--------------|
| 1 | Resume Optimizer Form | User submits job description + PDF resume + contact info |
| 2 | Detect File Type | Identifies the uploaded binary and maps form fields |
| 3 | Extract PDF Text | Pulls text from all pages of the PDF |
| 4 | Normalize PDF Output | Cleans and structures the extracted text |
| 5 | AI: Analyze Job Description | Mistral extracts keywords, skills, requirements, experience level |
| 6 | Parse JD Analysis | Handles JSON parsing with fallbacks for edge cases |
| 7 | AI: Rewrite & Optimize Resume | Full rewrite with ATS scoring and improvement recommendations |
| 8 | Build HTML Resume | Generates HTML for both the resume and the report |
| 9 | Convert HTML to PDF | Sends HTML to html2pdf.app API, returns PDF binary |
| 10 | Send Email with Report | Gmail sends optimized PDF + full ATS analysis |

---

## How the scoring works

The ATS compatibility score isn't a guess. It's calculated across four weighted categories:

- **40% Keywords**: How many required/preferred terms from the JD appear in your resume
- **30% Experience**: How closely your actual experience maps to what they're asking for
- **20% Seniority**: Whether your level matches the role's requirements
- **10% Education**: Degree and certification alignment

The workflow is calibrated to be realistic. An 85% score means you're genuinely competitive, not just that the algorithm is generous.

---

## Important notes

**PDF only**
The workflow only accepts PDF files. If someone uploads a Word doc, it will fail. Ask users to export as PDF first.

**No fabrication**
The AI never invents job titles, companies, dates, or skills. If a required qualification is missing, it's flagged as a gap—not added anyway.

**Score interpretation**
- 75%+: Strong match, competitive candidate
- 60-74%: Moderate match, some gaps to address
- Below 60%: Significant gaps, consider whether this is the right role or if you need more experience

**Customization**
You can adjust the scoring weights, prompt instructions, or email template by editing the relevant nodes. The code nodes have comments explaining what each section does.

---

## Files in this repo

```
├── AI_Resume_Optimizer_Workflow.json    # The actual n8n workflow (import this)
├── README.md                            # This file
└── screenshots/                         # Add workflow screenshots here
```

---

## Importing the workflow

1. Download `AI_Resume_Optimizer_Workflow.json`
2. In n8n, go to Workflows → Import from File
3. Select the JSON file
4. Configure the three credentials (Mistral, Gmail, html2pdf)
5. Activate and test

---

## Troubleshooting

**"No resume file found" error**
Make sure the user is uploading a PDF with the form field labeled "Resume File".

**AI returns invalid JSON**
The parsing nodes have fallback logic, but if Mistral keeps outputting malformed JSON, check your prompt instructions in the AI nodes.

**PDF generation fails**
Verify your html2pdf.app API key is valid and you haven't exceeded rate limits.

**Emails not sending**
Check that your Gmail OAuth2 credentials are properly authorized and haven't expired.

---

## License

MIT. Use it, modify it, sell it as a service—whatever works for you.

---

Built with n8n, Mistral AI, and enough caffeine to make ATS bots tolerable.
