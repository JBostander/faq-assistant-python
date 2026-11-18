# FAQ Assistant (Python + scikit-learn)

A simple customer-service FAQ assistant for **SteadyWheels Driving School**, a fictional business with sample data, created for this demo.

## What it does
A customer types a question in their own words, and the assistant finds the best-matching answer from a set of approved FAQs. If no FAQ matches well enough, it **does not guess**. It tells the customer to contact the business directly.

## How it works
1. **Knowledge base:** 12 FAQs, each with alternative phrasings customers might use.
2. **Retrieval:** questions are converted into numbers using **TF-IDF** and compared with **cosine similarity** to find the closest match. This is the "retrieval" step used in RAG (retrieval-augmented generation) systems.
3. **Safe fallback:** if the best match scores below a threshold (0.3), the assistant hands over to a human instead of risking a wrong or made-up answer.

## Evaluation
Tested on 18 questions: 13 customer questions, 3 off-topic questions that should trigger the fallback, and 2 tricky questions that share keywords with an FAQ but ask about something else.

**Result: 89% accuracy (16/18).** Two failures were questions that share keywords with an FAQ but mean something different.

| Question | Expected | Result |
| --- | --- | --- |
| How long does it take to drive to Cape Town from Kuilsriver? | Fallback ("not sure") | Wrongly matched the lesson-length FAQ (keyword "long") |
| Do you teach on Sundays? | Fallback ("not sure") | Wrongly matched the automatic/manual FAQ (keyword "teach") |

Raising the threshold would not fix this safely: the wrong match for the Cape Town question scored 0.51, while a correct match ("How much are lessons?") scored 0.53, so a higher threshold would also block correct answers.

## Limitations
- It matches words, not meaning, so unusual phrasing or synonyms not in the examples can be missed.
- Questions that share keywords but have a different meaning can be matched wrongly. For example, "How long does it take to drive to Cape Town?" matched the lesson-length FAQ because of the word "long."
- It only knows what is in the FAQ list.
- The test set is small; a real deployment would need far more test questions.

## Next steps
- Use embeddings (meaning-based search) to handle synonyms and keyword confusion better.
- Connect a large language model to phrase answers naturally, using only the retrieved FAQ (a full RAG pipeline).
- Log unanswered and low-confidence questions so the business can add new FAQs and examples.

## Files
- `dnd_faq_assistant.ipynb`: the Google Colab notebook, with outputs and the evaluation report.
- `dnd_faq_assistant.py`: the same code as a plain Python script.

## Tech
Python, scikit-learn (TfidfVectorizer, cosine_similarity), pandas, Google Colab.

Built by Junay Bostander, using AI-assisted development (Claude).
