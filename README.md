# Fine-Tuned LLM Code Analyzer

**Single fine-tuned LLM for code analysis** - Faster, cheaper, smarter than 6 competing models.

## 📊 Features

✅ **Fast Analysis**: 3-5 seconds per repository  
✅ **Cost-Effective**: 10x cheaper than multi-model approach  
✅ **Accurate Scoring**: Code quality, architecture, documentation, testing, best practices  
✅ **Employability Assessment**: Tier assignment + percentile ranking  
✅ **Supabase Integration**: Automatic result storage  
✅ **GitHub Integration**: Fetch and analyze repos from any GitHub account  

## 🚀 Quick Start

### 1. Clone and Setup

```bash
cd ai_engine_with_fine_tuned_llm
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Configure Environment

Copy `.env.example` to `.env` and fill in:

```bash
cp .env.example .env
```

Edit `.env`:

```ini
# LLM Provider (choose one)
LLM_PROVIDER=together  # or openai, ollama, huggingface

# Together.ai
LLM_API_KEY=your_key_here
LLM_MODEL_NAME=meta-llama/Llama-2-7b-hf-fine-tuned

# Supabase
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_KEY=your_key_here
```

### 3. Create Supabase Table

Run the SQL from `supabase_schema.sql` in your Supabase dashboard SQL editor.

### 4. Start the Engine

```bash
python main.py
```

Or use startup script:

```bash
./start.sh    # Linux/Mac
start.bat     # Windows
```

## 📡 API Endpoints

### Analyze GitHub User

**POST** `/api/analyze/complete`

```json
{
  "github_username": "torvalds",
  "github_token": "ghp_xxxxxxxxxxxx",
  "user_id": "optional-user-id",
  "max_repos": 3
}
```

**Response:**

```json
{
  "github_username": "torvalds",
  "overall_score": 92.5,
  "code_quality_score": 95.0,
  "architecture_score": 90.0,
  "documentation_score": 85.0,
  "testing_score": 92.0,
  "best_practices_score": 91.5,
  "employability_tier": "Excellent",
  "employability_percentile": 92.5,
  "professional_readiness": 87.9,
  "growth_potential": 97.1,
  "recommended_level": "Senior",
  "analysis_duration_seconds": 18.5,
  "status": "completed"
}
```

### Get Latest Results

**GET** `/api/analyze/results/{github_username}`

### Get Analysis History

**GET** `/api/analyze/history/{github_username}?limit=10`

### Health Check

**GET** `/api/analyze/health`

## 🏗️ Project Structure

```
ai_engine_with_fine_tuned_llm/
├── main.py                    # Entry point
├── requirements.txt           # Dependencies
├── .env.example               # Environment template
├── supabase_schema.sql        # Database schema
├── start.bat / start.sh       # Startup scripts
│
└── src/
    ├── api/
    │   └── routes/
    │       └── analysis.py    # API endpoints
    ├── services/
    │   ├── llm_analyzer.py    # Fine-tuned LLM
    │   ├── github_analyzer.py # GitHub API calls
    │   └── supabase_client.py # Database
    ├── models/
    │   └── schemas.py         # Pydantic models
    ├── config/
    │   ├── settings.py        # Configuration
    │   └── prompts.py         # LLM prompts
    └── utils/
        └── logger.py          # Logging
```

## 🔧 Configuration

### LLM Providers

#### Together.ai (Recommended)

1. Sign up: https://www.together.ai/
2. Get API key from dashboard
3. Fine-tune Llama-2 model with your data
4. Set in `.env`:

```ini
LLM_PROVIDER=together
LLM_API_KEY=your_key_here
LLM_MODEL_NAME=your-workspace/your-fine-tuned-model
```

#### OpenAI

```ini
LLM_PROVIDER=openai
LLM_API_KEY=sk-xxxxxxxxxxxx
LLM_MODEL_NAME=gpt-4-fine-tuned-model
```

#### Local Ollama

```ini
LLM_PROVIDER=ollama
LLM_MODEL_NAME=llama2:7b-fine-tuned
```

#### Hugging Face

```ini
LLM_PROVIDER=huggingface
LLM_API_KEY=hf_xxxxxxxxxxxx
LLM_MODEL_NAME=your-org/your-fine-tuned-model
```

## 💾 Database Schema

Automatically creates:

- `github_analysis_results` - Store analysis results
- Indexes for fast queries
- Timestamp tracking

## 📊 Analysis Flow

```
1. Background Search (2-3s)
   - Fetch GitHub user account info
   - Score all repositories by metrics
   - Select top 3 repos

2. Deep Code Analysis (3-5s per repo)
   - Fetch repository code
   - Send to fine-tuned LLM
   - Score: code quality, architecture, documentation, testing, best practices

3. Employability Assessment
   - Calculate weighted scores
   - Assign tier: Excellent/Good/Fair/Beginner
   - Recommend level: Senior/Mid/Junior

4. Store Results
   - Save to Supabase
   - Return to caller
```

## 🎯 Scoring Breakdown

```
Overall Score = 
  Code Quality (30%) + 
  Architecture (25%) + 
  Documentation (15%) + 
  Testing (20%) + 
  Best Practices (10%)

Employability Tier:
  >= 85: Excellent
  >= 70: Good
  >= 50: Fair
  <  50: Beginner

Recommended Level:
  >= 80: Senior
  >= 65: Mid
  <  65: Junior
```

## 🔐 Security Notes

- Never commit `.env` file
- Use GitHub tokens with appropriate scopes
- Supabase key should be server-only (not exposed to frontend)
- Rate limit GitHub API calls: 5,000/hour per token

## 🐛 Troubleshooting

### "Module not found" error

```bash
source venv/bin/activate  # Linux/Mac
pip install -r requirements.txt
```

### LLM API errors

- Check API key in `.env`
- Verify rate limits
- Test with `curl` or Postman

### Supabase connection failed

- Verify `SUPABASE_URL` and `SUPABASE_KEY`
- Check internet connection
- Ensure table exists in Supabase

### GitHub API rate limit

- Use authenticated token (5,000/hour)
- Implement caching
- Reduce repositories analyzed per request

## 📈 Performance Metrics

| Metric | Value |
|--------|-------|
| Background search | 2-3 seconds |
| Per-repo analysis | 3-5 seconds |
| 3 repos total | ~15-20 seconds |
| Database store | <500ms |
| **Total** | **~20-25 seconds** |

**Cost per analysis:**
- 6-LLM approach: $0.50-1.00
- Fine-tuned approach: $0.05-0.10
- **Savings: 90%** 🎉

## 📚 API Documentation

Visit: `http://localhost:8000/docs` for interactive Swagger documentation.

## 🚀 Deployment

### Docker

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "main.py"]
```

### Environment Variables for Production

```ini
API_HOST=0.0.0.0
API_PORT=8000
LOG_LEVEL=INFO
LLM_TIMEOUT=60
```

## 📝 License

Your project license here

## 🤝 Support

For issues or questions, contact your team or check the main project README.

---

**Built with**: FastAPI, Supabase, Fine-Tuned LLM  
**Last Updated**: 2026-06-25
Looking at your actual logs and code, I can see concrete problems — some are in the evaluation criteria like you're asking, but there's a bigger bug hiding underneath that will wreck your accuracy no matter how good the criteria are. Let me go through both.
🚨 Critical bug first (this matters more than criteria refinement)
Look at your logs:
GET .../-Heart-disease-risk-prediction/contents/.venv/Lib/site-packages/_distutils_hack/__init__.py
GET .../-Heart-disease-risk-prediction/contents/.venv/Lib/site-packages/blinker/__init__.py
GET .../CourseWay/contents/backend/node_modules/accepts/index.js
GET .../CourseWay/contents/backend/node_modules/body-parser/index.js
You are analyzing third-party library code, not the student's own code. .venv, site-packages, and node_modules got committed into these repos (common student mistake — no .gitignore), and fetch_repo_code() is grabbing whatever code files it finds in the tree, in whatever order, up to 10 files. It has no concept of "this is dependency code, skip it."
This is why your LLM summary literally says "The repository contains various Python modules related to the blinker..." — it's reviewing the blinker pip package's source, not the student's heart-disease-prediction code. Every score from that analysis is meaningless.
Fix — filter the tree before fetching:
pythonEXCLUDED_DIRS = {
    "node_modules", ".venv", "venv", "env", "site-packages",
    "dist", "build", ".next", "vendor", "__pycache__",
    ".git", "target", "bin", "obj"
}

def _is_code_file(self, path: str) -> bool:
    parts = path.lower().split("/")
    if any(excluded in parts for excluded in EXCLUDED_DIRS):
        return False
    code_extensions = {...}
    return any(path.lower().endswith(ext) for ext in code_extensions)
Also worth adding a .gitignore check itself to your scoring — a repo with node_modules committed is a real signal (negative) about the student's git hygiene, so you could actually score that rather than just filter around it.
Second bug: JSON parsing is silently failing
Your logs show two failed parses:
Failed to parse LLM response: {"code_quality": 75, ..., "summary": The codebase for the web final_backend is well-structured, with c
Notice "summary": The codebase... — no opening quote before the value, and the text just cuts off. The 3B model is not reliably producing valid JSON (small models often break on long free-text fields inside JSON). When parsing fails, you fall back to _default_scores() — all 50s — which silently corrupts your final score (that's part of why the score came out as 59.9 "Fair" — repo 1 and repo 3 both defaulted to 50s while repo 2 succeeded).
Fixes, in order of effort:

Constrain the model — ask for scores only, put summary/strengths/improvements in a separate, simpler call, or drop free text from the JSON schema entirely (numbers parse far more reliably than nested prose).
Use Ollama's format: "json" parameter in the /api/generate call — this forces valid JSON output on modern Ollama versions and would likely fix this outright:

pythonjson={
    "model": self.model_name,
    "prompt": prompt,
    "stream": False,
    "temperature": self.temperature,
    "format": "json"
}

Log which repo/user failed parsing (not just the raw text) so you can audit how often this happens in aggregate — right now you have no way to know what fraction of your 500 users get silently defaulted to 50s.

Now, your actual question — criteria improvements
Given the bugs above are fixed, here's what I'd add/change in ANALYSIS_PROMPT and scoring:
Add missing dimensions:

Complexity/maintainability signal — nesting depth, function length, cyclomatic complexity estimate. Cheap to compute with static analysis (no LLM needed) and adds a dimension the LLM alone is bad at judging consistently.
Consistency across files — right now you sample 10 files somewhat arbitrarily; a repo can look great in 2 files and terrible in the rest. Consider sampling more deliberately (e.g., largest files + entry points + one random) rather than tree order.
Security-relevant patterns — hardcoded secrets/API keys, SQL string concatenation, etc. You mentioned this as "medium priority" earlier but it's cheap to check with regex before even hitting the LLM, and it's a real differentiator for employability.

Fix scoring weight mismatch: your README says Best Practices (10%) but code_quality (30%) + architecture (25%) + documentation (15%) + testing (20%) + best_practices (10%) = 100%, fine — but nowhere in the code I can see is this weighting actually applied; I don't see the aggregation function. Worth double-checking Step 4: Calculating final scores actually uses these weights and not a flat average.
Repo selection: your min_score = 40.0 threshold with no fork/template filter in this actual codebase (unlike the Java pseudocode we discussed) means forks only get a -10 penalty, not exclusion — a heavily-forked tutorial repo could still slip into the top 3 if it's popular. Worth adding an explicit if repo.get("fork"): continue in select_best_repos.
Want me to help rewrite github_analyzer.py's file-fetching logic and the prompt to fix the dependency-folder bug and force stricter JSON output? That's the highest-leverage fix here — everything else is secondary until that's solved.
How "Smarter File Sampling" fixes this:
Instead of taking the first 10 files, we can write a quick sorting algorithm to mathematically score which files are the "most important" to read. We would grade the files themselves before fetching them:

Entry Point Bonus (+50 points): We check the filename. If it's named main.py, server.js, app.ts, index.html, or __init__.py, we guarantee it gets fetched. These files show the LLM how the entire architecture is wired together.
Directory Bonus (+20 points): Files living in src/, lib/, or core/ are usually more important than files sitting in tests/ or the root directory.
Size Scoring: The GitHub Tree API actually returns the size (in bytes) of each file for free! We can prioritize medium-to-large files (e.g., 2KB - 15KB). If a file is 200 bytes, it's probably just a config file and not worth analyzing. If it's 200,000 bytes, it's probably auto-generated (like a package-lock.json) and should be ignored.
Consistency: We can take the top 8 "highest scoring" files, and then randomly select 2 other files just to see if the developer writes good code everywhere, not just in main.py.
Excludes dependency bloat (node_modules, etc.).
Forces strict JSON validation.
Excludes forks completely.
Uses mathematical weighted averages to calculate your employability score.
Uses Smarter File Sampling to intentionally seek out main.py and server.js instead of generic alphabetized files.
Returns data beautifully structured for your backend/frontend to consume.