!pip install spacy
!python -m spacy download en_core_web_sm

import spacy
from heapq import nlargest

# Load spaCy English nlp model
nlp = spacy.load("en_core_web_sm")


def summarize(text, ratio=0.3):

 # Process the Text with spaCy
    doc = nlp(text)
    sentences = list(doc.sents)

# Compute word frequency (ignoring stopwords & punctuation)
    word_freq = {}
    for word in doc:
        if not word.is_stop and not word.is_punct:
            word_freq[word.text.lower()] = word_freq.get(word.text.lower(), 0) + 1

 # Score sentences based on word frequencies
    sentence_scores = {sent: sum(word_freq.get(word.text.lower(), 0) for word in sent) for sent in sentences}

# Select top sentences based on the ratio
    num_sentences = max(1, int(len(sentences) * ratio))
    summary_sentences = nlargest(num_sentences, sentence_scores, key=sentence_scores.get)

    return " ".join(sent.text for sent in summary_sentences)

# Example Usage
article ="""
        The Road to Success: A Journey of Perseverance and Growth
Success is not an overnight achievement but a journey of dedication, hard work, and continuous learning.
It begins with a clear vision and the determination to overcome obstacles. Setting realistic goals and maintaining discipline helps in staying focused.
Challenges and failures are inevitable, but they serve as stepping stones toward growth.
A positive mindset and self-confidence play a crucial role in pushing forward despite difficulties.
Learning from mistakes and adapting to changes improve resilience. Surrounding oneself with like-minded, motivated individuals fosters inspiration and encouragement.
Time management and consistency are key factors in achieving long-term goals. Developing skills, gaining knowledge,and staying updated with industry trends enhance personal and professional growth.
Passion fuels persistence, making hard work enjoyable rather than a burden. Overcoming self-doubt and believing in one’s abilities create a strong foundation for success.
Taking calculated risks opens new opportunities and broadens perspectives. Maintaining a healthy work-life balance ensures sustainable progress.
Helping others and sharing knowledge bring fulfillment and contribute to overall success. Embracing failure as a lesson rather than a setback strengthens determination.
A growth-oriented mindset leads to continuous improvement and innovation. Seeking mentorship from experienced individuals accelerates learning and avoids common pitfalls.
Financial discipline and smart investments secure stability and future success. Accepting constructive criticism and working on self-improvement lead to personal excellence.
Staying humble despite achievements keeps one grounded and open to new learning experiences. Gratitude and appreciation for small victories.
         """
summary = summarize(article)

print("Summary:\n", summary)

output:-
Summary:
 "
        The Road to Success: A Journey of Perseverance and Growth
Success is not an overnight achievement but a journey of dedication, hard work, and continuous learning.
 Developing skills, gaining knowledge,and staying updated with industry trends enhance personal and professional growth.
 A positive mindset and self-confidence play a crucial role in pushing forward despite difficulties.
 Overcoming self-doubt and believing in one’s abilities create a strong foundation for success.
 Setting realistic goals and maintaining discipline helps in staying focused.
 Seeking mentorship from experienced individuals accelerates learning and avoids common pitfalls.

