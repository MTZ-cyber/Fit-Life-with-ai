# FitLife AI

FitLife AI is a small web app built with **Python**, **Streamlit**, and the **Grok API (xAI)**.
It asks the user three questions:

1. How much time do you have for physical activity?
2. Do you have any illness, disability, injury, or health problem?
3. What is your fitness level (Beginner, Intermediate, or Active)?

Grok AI then suggests suitable **exercises**, **sports or games**, and **healthy food options**, plus some safety tips.

> **Medical disclaimer:** FitLife AI gives general wellness ideas only. It does **not** diagnose illnesses and does **not** replace advice from a doctor or other qualified health professional. The app and its AI instructions are written to keep suggestions gentle and safe, but AI can make mistakes. Always check with a doctor before starting a new exercise or diet plan.

## Project files

| File | Purpose |
|------|---------|
| `application.py` | The complete Streamlit app (screen + Grok API call) |
| `requirements.txt` | Python libraries needed (`streamlit`, `requests`) |
| `readme_help.md` | This guide |

## How it works

1. The user fills in the three fields and clicks **Get my recommendations**.
2. The app reads the API key from Streamlit secrets (it is never written in the code).
3. The app sends the answers to `https://api.x.ai/v1/chat/completions` together with safety rules (a "system prompt") telling Grok not to diagnose, not to suggest medicine, and to choose gentle options when a health problem is mentioned.
4. The answer is shown on the page with a reminder that it is not medical advice.

---

## 1. Get a Grok (xAI) API key

1. Go to <https://console.x.ai> and sign in or create an account.
2. Open the **API Keys** page and create a new key. It starts with `xai-`.
3. Copy it and keep it private. You may need to add credits to your xAI account for the API to work.

**Never** paste your key into `application.py`, GitHub, a chat, or a screenshot.

---

## 2. Run the app on your computer

### Step 1 - Install Python
Install Python 3.9 or newer from <https://www.python.org/downloads/>. On Windows, tick **"Add Python to PATH"** during installation.

### Step 2 - Make a project folder
Create a folder (for example `fitlife-ai`) and put the three files in it.

### Step 3 - Open a terminal in that folder
Windows: open the folder, click the address bar, type `cmd`, press Enter.
Mac/Linux: open Terminal and use `cd` to enter the folder.

### Step 4 - (Recommended) Create a virtual environment
```bash
python -m venv venv
```
Activate it:
- Windows: `venv\Scripts\activate`
- Mac/Linux: `source venv/bin/activate`

### Step 5 - Install the libraries
```bash
pip install -r requirements.txt
```

### Step 6 - Add your API key locally
Create a folder named `.streamlit` inside your project folder, and inside it create a file named `secrets.toml` with this content:

```toml
XAI_API_KEY = "xai-your-real-key-here"
```

Your folder should look like this:
```
fitlife-ai/
├── application.py
├── requirements.txt
├── readme_help.md
└── .streamlit/
    └── secrets.toml     <- local only, do NOT upload to GitHub
```

### Step 7 - Start the app
```bash
streamlit run application.py
```
Your browser opens at <http://localhost:8501>. Press `Ctrl + C` in the terminal to stop it.

---

## 3. Upload the three files to GitHub (website only)

1. Go to <https://github.com> and sign in (or create a free account).
2. Click the **+** icon (top right) → **New repository**.
3. Enter a name such as `fitlife-ai`. Choose **Public** (the free Streamlit Cloud plan works with public repositories). Click **Create repository**.
4. On the new empty repository page, click **uploading an existing file**. (If you do not see it, click **Add file → Upload files**.)
5. Drag and drop these **three files** (or use "choose your files"):
   - `application.py`
   - `requirements.txt`
   - `readme_help.md`
6. Scroll down and click **Commit changes**.

> **Do not upload** the `.streamlit` folder or `secrets.toml`. Your API key must stay out of GitHub. If you ever upload a key by mistake, delete that key in the xAI console and create a new one.

Check that the three files appear in the repository file list.

---

## 4. Deploy on Streamlit Community Cloud

1. Go to <https://share.streamlit.io> and sign in with your **GitHub** account. Allow access to your repositories if asked.
2. Click **Create app** (or **New app**), then choose **Deploy a public app from GitHub**.
3. Fill in:
   - **Repository:** `your-username/fitlife-ai`
   - **Branch:** `main`
   - **Main file path:** `application.py`
   - **App URL:** choose a name (optional)
4. Click **Advanced settings** and paste your secret into the **Secrets** box (see the next section).
5. Click **Deploy**. The first start takes a few minutes while the libraries install. You then get a public link such as `https://your-app-name.streamlit.app`.

### Entering the Grok API key in Streamlit Cloud Secrets

In the **Secrets** box, type exactly this (use your own key, keep the quotes):

```toml
XAI_API_KEY = "xai-your-real-key-here"
```

Click **Save**. The app reads it with `st.secrets["XAI_API_KEY"]`.

**To add or change the key after deploying:**
1. Open your app list at <https://share.streamlit.io>.
2. Click the **⋮** menu next to your app → **Settings** → **Secrets**.
3. Edit the text, click **Save**, and wait a few seconds. If the app does not pick it up, use **Reboot app**.

(Button names on the Streamlit website change occasionally, but the steps stay the same.)

---

## Troubleshooting

| Problem | Likely fix |
|---------|-----------|
| "API key not found" | The secret is missing or misspelled. The name must be exactly `XAI_API_KEY`. |
| "The API key was rejected" | The key is wrong, expired, or was deleted. Create a new one. |
| "Access denied" or "no credits" message | Your xAI account may need credits or permission to use the API. |
| "Model not found" | xAI may have retired the model. Open the xAI docs (<https://docs.x.ai>), find a current model name, and change the `MODEL = "grok-4.3"` line near the top of `application.py`. |
| App is slow | Grok may take 10-60 seconds to answer. Wait for the spinner to finish. |
| `streamlit` command not found | Activate your virtual environment and run `pip install -r requirements.txt` again. |

## Notes for the lab report

- The API key is stored in Streamlit secrets, not in the code, so it is not exposed on GitHub.
- The safety rules are in `SYSTEM_PROMPT` in `application.py`. They tell the AI not to diagnose, not to recommend medicine or extreme diets, and to ask the user to see a doctor when warning signs are mentioned.
- Limits of the project: it does not save any user data, and the AI's output cannot be fully guaranteed to be safe, which is why the disclaimers are shown on the page.
