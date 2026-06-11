# 🎭 Multimodal Emotion Analyzer

A multimodal emotion detection system that analyzes emotions from both facial images and text using computer vision and NLP, then summarizes the overall emotional state.

## Files

| File | Description |
|---|---|
| `emotion_analysis.ipynb` | Main Jupyter notebook |
| `test_nlp.xlsx` | Excel dataset with a `text` column for sentiment analysis |
| `Sentiment Analysis/` | Folder containing facial images for emotion detection |

## Requirements

```bash
pip install opencv-python fer nltk pandas matplotlib openpyxl
```

Then in Python:

```python
import nltk
nltk.download('vader_lexicon')
```

> **Note:** This project was built and run on **Google Colab**. Files are loaded from Google Drive. Update the paths if running locally.

## Google Drive Setup

The notebook expects this folder structure in your Drive:

```
My Drive/
└── Projects/
    └── NLP/
        ├── test_nlp.xlsx
        └── Images/
            ├── Happy1.jpeg
            ├── Happy2.jpg
            ├── Angry1.jpeg
            ├── Angry2.jpeg
            └── Frustrated.jpeg
```

If running locally, update these variables in the notebook:

```python
image_folder = '/content/drive/My Drive/Projects/NLP/Images'
text_file = '/content/drive/My Drive/Projects/NLP/test_nlp.xlsx'
```

## Dataset

### Images (`Images/`)
JPEG or PNG facial images. The FER library detects faces and classifies emotions as `happy`, `angry`, `sad`, or `frustrated`.

### Excel (`test_nlp.xlsx`)
Must contain a single column:

| Column | Description |
|---|---|
| `text` | A sentence or phrase to analyze for emotion |

Example rows:

| text |
|---|
| Today has been amazing! I feel so grateful for everything. |
| I keep trying, but nothing is working |
| Everything feels so heavy, and I just want to rest |

## How It Works

1. **Image Analysis** — uses the `FER` (Facial Expression Recognition) library to detect faces and identify the dominant emotion per image, with confidence thresholds per emotion class
2. **Text Analysis** — uses NLTK's VADER sentiment analyzer combined with keyword matching to classify each text entry as `happy`, `angry`, `frustrated`, or `sad`
3. **Summary** — combines all image and text emotions, finds the most frequent one, and returns a wellness message
4. **Live Input** — prompts the user to type how they're feeling and detects the emotion in real time

## Emotions Detected

| Emotion | Trigger |
|---|---|
| `happy` | High positive sentiment or VADER compound ≥ 0.4 |
| `angry` | Dominant FER angry score > 0.8, or keywords like `furious`, `enraged` |
| `frustrated` | FER frustrated score > 0.6, or keywords like `annoyed`, `frustrated` |
| `sad` | FER sad score > 0.4, or high negative sentiment compound < -0.5 |

## Sample Output

```
Image Emotions:
{'Happy1.jpeg': 'happy', 'Angry1.jpeg': 'angry', 'Angry2.jpeg': 'angry', 'Frustrated.jpeg': 'frustrated', 'Happy2.jpg': 'happy'}

Text Emotions:
                                                text     emotion
0  Today has been amazing! I feel so grateful for...       happy
1                I got a promotion, this is amazing!       happy
2              I keep trying, but nothing is working       angry
3  Everything feels so heavy, and I just want to ...         sad
4         I can't stop smiling, this is the best day       happy
5       That is unfair, I'm beyond furious right now       angry
6               This is so hard, it's pissing me out  frustrated
7          I feel so empty and the world seems blank         sad

Most common emotion: happy (5 times). Keep smiling!

What are you feeling right now? DGM sir appreciated me today
Emotion detected: happy
```

## Wellness Messages

| Emotion | Message |
|---|---|
| 😊 `happy` | Keep smiling! |
| 😠 `angry` | Anger is bad for health. Stay calm! |
| 😢 `sad` | It's okay to feel sad. Take care of yourself! |
| 😤 `frustrated` | Take a deep breath, it's okay! |

## Notes

- The notebook must be run on Google Colab or paths updated for local use
- Excel file must have a column named exactly `text` (lowercase)
- Images must be `.png`, `.jpg`, or `.jpeg`
- FER requires a face to be detectable in the image; images with no face return `"No face detected"`
