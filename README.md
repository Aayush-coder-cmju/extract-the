# Escape the Chapter - SmartWorld

An adventure-style Streamlit quiz for revising a textbook passage. Students paste text or upload a supported page, then solve Easy, Medium, and Hard questions to collect pieces of a secret word.

## What is included

- `app.py` - the complete Streamlit application
- `requirements.txt` - Python packages needed to run it
- `.streamlit/secrets.toml.example` - a safe placeholder for the app owner's Google API key
- `.gitignore` - keeps local secrets and Python environments out of Git
- The project presentation PDF is available separately; it is not required to run or deploy the app.

## Requirements

- Windows 10/11 or another computer with Python 3.10 or newer; on Windows, make sure `python` is available from PowerShell
- Internet access for custom passage/image quiz generation
- A Google AI Studio API key for custom material; sample quizzes work without a key

The app uses the Google Gen AI Python SDK with **Gemini 3.5 Flash-Lite** (`gemini-3.5-flash-lite`) for custom quizzes. Google lists this model as multimodal, including image and PDF inputs, and optimized for low-latency, lower-cost document processing. See the [official model guide](https://ai.google.dev/gemini-api/docs/models/gemini-3.5-flash-lite).

## Run locally on Windows

1. Download and extract the ZIP. Open the extracted `Escape the Chapter Final` folder. Confirm that `app.py` and `requirements.txt` are in this folder.
2. In File Explorer, open this folder, click the address bar, type `powershell`, and press Enter.
3. Create a private Python environment and install the packages:

   ```powershell
   python -m venv .venv
   .\.venv\Scripts\python.exe -m pip install --upgrade pip
   .\.venv\Scripts\python.exe -m pip install -r requirements.txt
   ```

4. To use your own passages or textbook images, create a key in [Google AI Studio](https://aistudio.google.com/app/apikey). Copy the example secrets file and open the private copy:

   ```powershell
   Copy-Item ".streamlit\secrets.toml.example" ".streamlit\secrets.toml"
   notepad ".streamlit\secrets.toml"
   ```

   Replace the placeholder with your key and save:

   ```toml
   GEMINI_API_KEY = "paste-your-real-key-here"
   ```

   Keep `.streamlit\secrets.toml` private. It is excluded from Git. Never put the real key in `app.py`, in the ZIP, or in a student-facing form.

5. Start the app:

   ```powershell
   .\.venv\Scripts\python.exe -m streamlit run app.py
   ```

   Streamlit opens a browser page, normally `http://localhost:8501`. To stop the local server, return to PowerShell and press Ctrl+C.

## How to play

1. Try the Biology or History sample, or enter a topic and paste at least 20 words from a passage. You can instead upload TXT, Markdown, CSV, PDF, PNG, JPG/JPEG, or WEBP. Scanned PDFs and textbook photos are read by Gemini; text-based PDFs and text files are extracted by the app.
2. Press **Start the adventure**. The app creates exactly three passage-based questions: Easy, Medium, then Hard. Each correct answer reveals evidence from the passage, unlocks the next phase, and awards a secret-word fragment.
3. Arrange the three shuffled fragments, type the completed word, and press **Enter the word** to finish.
4. The player starts with 60 points and 3 lives. A wrong answer costs one life. A hint costs 20 points; up to two hints are available per question. Correct answers unlock rooms but do not add score points. Completing all three questions without a hint earns 20 points in the saved bonus wallet.
5. Saved bonus can be kept or selected during play to pay for hints before regular score points. The wallet lasts for the current Streamlit browser session and carries into new adventures in that session.
6. When all three lives are used, the app starts a fresh round on the same topic and supplied passage. It resets lives, carries the score, saved bonus, and total timer forward, and avoids earlier questions when possible.
7. The report shows the score, bonus balance, answers, hints, lives lost, round history, per-question times, total time, and revision suggestions. Share it only when you choose WhatsApp or email; the original passage is not included in the share text.

## Upload notes

- Text-based PDFs are extracted on the app server. Scanned PDFs are sent to Gemini for text reading, so a separate photo conversion is not required.
- PNG, JPG/JPEG and WEBP uploads are validated, rotated using their photo metadata, resized for legibility, and sent to Gemini as an image. If a page is blurry or the content is too short, the app asks you to try again.
- Uploads are limited to 10 MB; PDFs are limited to 12 pages. This keeps image and document requests within a practical size for the hosted app.
- Every generated question must include an evidence quote found in the source text. Invalid or unsupported quiz data is rejected.
- Pasted text can be combined with a text file, image, or scanned PDF. The quiz must preserve the pasted passage and add only readable text from the upload.

## Key privacy and public-use notes

Students never enter the API key. The app reads it from local Streamlit secrets or the host's secret settings. Pasted text, textbook photos, and scanned PDFs may be sent to Google's API when a custom quiz is generated. The app has no login or database; game data and saved bonus stay in the active Streamlit session.

Anyone using a public app can trigger requests with the owner's API key and use its quota. Keep the key private and monitor the API project's usage limits. Do not include `.streamlit/secrets.toml` in GitHub.

## Update the existing Streamlit app

You can keep using the GitHub repository that already powers your Streamlit app; a new repository is not required.

1. Extract this ZIP. In the existing GitHub repository, replace `app.py` with this package's `app.py`. Keep it at the same path configured as the Streamlit app file. If your current app uses the repository root, place it beside `requirements.txt`.
2. Add or update `requirements.txt` from this package in the same repository location as the app. You may also update `README.md` and `.gitignore`. Do not upload `.streamlit/secrets.toml` or a real API key; `.streamlit/secrets.toml.example` is safe to commit.
3. Commit the changes to the same branch connected to Streamlit Community Cloud. Streamlit Cloud should rebuild the existing app from that commit.
4. Keep the key in the hosted app's **Settings -> Secrets**. If it is already configured, leave it there. Otherwise add:

   ```toml
   GEMINI_API_KEY = "paste-your-real-key-here"
   ```

   Save the settings. Never put the real key in GitHub or in `app.py`.

5. Wait for the rebuild to finish, then refresh the app. If it still shows the old version, open **Manage app -> Reboot** and refresh again.
6. To let anyone use the app, set its sharing access to public in Streamlit app settings and copy the app's `.streamlit.app` link.

Official guides: [update and deploy an app](https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/deploy), [manage secrets](https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/secrets-management), and [share an app](https://docs.streamlit.io/deploy/streamlit-community-cloud/share-your-app).

## Verification checklist

- Biology and History sample games can be opened without an API key.
- Sample answers, evidence quotes, hints, lives, theme changes, clue collection, and the secret-word gate use the same game rules as custom quizzes.
- Custom passage, photo and scanned-PDF question generation requires `GEMINI_API_KEY`; the key is not requested in the student interface.
- Streamlit Community Cloud deployment requires the owner to add the key in the app's Secrets settings and make the deployed app public.

TO view the App : https://extract-the-lirzcqwfh5xdkghxskbwse.streamlit.app/
