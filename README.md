# Escape the Chapter

**SmartWorld** turns a student's supplied textbook passage into a three-stage escape-room quiz. The questions, hints, clues, and final word are grounded in the student's pasted text or uploaded material.

## View the app

[Open Escape the Chapter](https://extract-the-lirzcqwfh5xdkghxskbwse.streamlit.app/)

## How to play

1. Try the Biology or History sample, paste a passage, upload files, or combine pasted text with uploads.
2. Answer the Easy, Medium, and Hard questions in order. Correct answers reveal evidence and unlock the next room.
3. Collect all three clue fragments, assemble the passage-related final word, and escape.
4. Track points, three lives, hints, saved bonus points, checkpoints, and time in the live game and final report.

Players begin with **60 points** and **3 lives**. Each hint costs **20 points**. Completing all three questions without a hint earns **20 saved bonus points**.

## Supported uploads

Upload up to **5 mixed files** per adventure. Each file can be up to **10 MB**, and the combined limit is **25 MB**. Paste text and upload files together if useful.

- **Images:** PNG, JPG/JPEG/JPE/JFIF, WEBP, BMP, GIF, TIF/TIFF, HEIC, HEIF. Animated or multi-page image files use their first frame/page.
- **Documents:** PDF (up to 12 pages per PDF), TXT, Markdown, CSV, JSON, HTML/HTM, XML, RTF, DOCX, PPTX, XLSX, ODT, ODP, and ODS.
- Text-based PDFs and text in supported office documents are extracted in the app. Scanned PDFs and uploaded images are sent to Gemini for reading. Text is combined from all selected files and the pasted passage. For pictures embedded inside an Office file, upload the picture separately as an image too.
- Questions are created only from readable supplied material. The app rejects questions if their evidence cannot be checked against the source.

The app supports common school image and document formats; it cannot read every possible file type. Password-protected PDFs, legacy `.doc`/`.ppt`/`.xls` files, and unsupported formats should be saved as PDF, DOCX, PPTX, XLSX, or a supported image/text format before upload.

## AI and speed

Custom quizzes use **Gemini 3.5 Flash-Lite** by default, with minimal thinking for faster question generation. Set `GEMINI_MODEL` in the hosting environment to choose another model that your API key can access. The two built-in sample adventures work without an API key.

The app reads the Google API key from Streamlit secrets. It never asks students to enter the key. Add this in the deployed app's **Settings → Secrets**:

```toml
GEMINI_API_KEY = "your-google-ai-studio-key"
```

Do not commit a real key to GitHub or place it in `app.py`. Uploaded image and scanned-PDF contents are sent to Google's Gemini API when a custom quiz is generated. Review Google's current API terms and data handling before using student data.

## Update the existing Streamlit app

You can update the repository already connected to the live app; you do not need to create another repository.

1. Replace the connected repository's `app.py` with the updated `app.py` from this project folder.
2. Replace `requirements.txt` with the one provided here so the document readers are installed.
3. Commit and push both files to the branch used by your existing Streamlit app.
4. Streamlit Community Cloud will rebuild the same app. Keep `GEMINI_API_KEY` in the app's **Settings → Secrets**.
5. When the rebuild finishes, refresh the live app link above. If it still shows the old version, use **Manage app → Reboot** and refresh again.

## Project files

- `app.py` — Streamlit application
- `Escape_the_Chapter_Final.py` — matching standalone copy of the current application file
- `requirements.txt` — Python dependencies for the hosted application
- `.streamlit/secrets.toml.example` — safe example showing the required secret name; it contains no real key
- `.gitignore` — excludes private secrets and local environments from Git

## Notes

- Upload size is limited to keep the hosted app responsive. Large documents may take longer to read and generate questions from.
- Saved bonus points and game progress are stored in the active Streamlit session and are not a permanent student account or database.
- Public users can consume the owner's Gemini API quota. Monitor usage and keep the API key private.
