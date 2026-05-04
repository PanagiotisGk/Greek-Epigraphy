# Βιβλιοθήκη Prompts — Ομαδική Εργασία: AI for Epigraphy

| | |
|---|---|
| **Μέλος 1:** [Γκογκάκης Παναγιώτης Χρήστος]  
| **ΑΜ1:** [3230334]  
| **Μέλος 2:** [Παναγόπουλος Δημήτριος]  
| **ΑΜ2:** [3220296]  
| **Μάθημα** | Εφαρμοσμένη Επιστήμη Δεδομένων — Εαρινό Εξάμηνο 2026 |

---

## Ημερολόγιο Αλληλεπιδράσεων με AI

<!--
Ενημερώνετε αυτόν τον πίνακα κάθε φορά που ολοκληρώνετε μια ενότητα.
Καταγράψτε: ημερομηνία, ενότητα, αν χρησιμοποιήθηκε AI, σύντομη περιγραφή.
-->

| Ημερομηνία | Ενότητα | AI; | Περιγραφή |
|---|---|---|---|
| 01/05/2026 | Φόρτωση Δεδομένων | Ναι | debugging python code |
| 02/05/2026 | Α1 — Transliteration | Ναι | Συνάρτηση μετατροπής Λατινικών σε Ελληνικά |
| 02/05/2026 | Α2 — Word Segmentation | Ναι | Segmentaition - dynamic algorithm |
| | Α3 — Λεξιλόγιο & Συχνότητα | | |
| | Α4 — Κύρια Ονόματα | | |
| | Α5 — Clustering & Topics | | |
| 03/05/2026 | Α6 — Χρονολόγηση Aeneas/LLM | Ναι | Εξήγηση του μοντέλου, δημιουργία γραφημάτων για σύγκριση |
| | Β1 — Προεπεξεργασία Εικόνων | | |
| | Β2 — Image Clustering | | |
| | Β3 — OCR Pipeline | | |

---

## Εργαλεία & Μοντέλα που χρησιμοποιήθηκαν

<!--
Συμπληρώστε όλα τα εργαλεία και μοντέλα που χρησιμοποιήσατε.
- Εργαλείο = η εφαρμογή/interface (π.χ. Claude Cowork, ChatGPT, GitHub Copilot, Cursor, Google AI Studio)
- Μοντέλο = το LLM πίσω από το εργαλείο (π.χ. Claude Opus 4.6, GPT-4o, Gemini 2.5 Pro)
-->

| Εργαλείο | Μοντέλο | Σε ποιες ενότητες |
|---|---|---|
| Claude | Claude Opus 4.6, Sonnet 4.6 | *(π.χ. Α1, Α2, Β3)* |
| Chat Gpt | Gpt 5.4, Gpt 5.5 | *(π.χ. Α1, Α2, Β3)* |
| | | |

---

## Φόρτωση Δεδομένων

<!--
Αν χρησιμοποιήθηκε LLM για τη φόρτωση/κατανόηση του dataset, καταγράψτε εδώ.
Αν όχι, αφήστε τη σημείωση «Δεν χρησιμοποιήθηκε LLM».
-->

| | |
|---|---|
| **Εργαλείο** | [Claude] |
| **Μοντέλο** | [Claude Opus 4.6] |
| **Ποιος χρησιμοποίησε** | Μέλος 2 |

**Prompt:**
> [Θα πρέπει να εμφανίσω images σε jupiter notebook. Ποια βιβλιοθήκη να χρησιμοποιήσω]


**Prompt:**
> [In the function I gave you, I want to "pull" the text that is between 'Transcript' and 'Lines'. Something is not working correctly for me and I did not get the correct result. Can you show me where I went wrong and also give me a custom check to do so that I can see that it worked correctly.]

---

## Μέρος Α — Ανάλυση Κειμένων

### Α1. Μετατροπή Λατινικών σε Ελληνικούς Χαρακτήρες

<!--
Αν χρησιμοποιήσατε LLM, συμπληρώστε τον πίνακα και το prompt.
Αν όχι, γράψτε «Δεν χρησιμοποιήθηκε LLM» και διαγράψτε τον πίνακα.
-->

| | |
|---|---|
| **Εργαλείο** | [Claude] |
| **Μοντέλο** | [Claude Opus 4.6] |
| **Ποιος χρησιμοποίησε** | Μέλος 2 |

**Prompt:**
> [I have a dataset of Latin-character transcriptions of ancient Greek inscriptions.
They use a proxy mapping. How should I design a function in Python that:
- maps characters correctly
- handles unknown symbols
- preserves structure?
Give a simple solution with examples.]

**Prompt:**
> [How can I evaluate the quality of a transliteration system, that i did before?
Suggest simple metrics I can implement in Python.]

---

### Α2. Τμηματοποίηση Λέξεων — Word Segmentation

| | |
|---|---|
| **Εργαλείο** | [Claude] |
| **Μοντέλο** | [Claude Opus 4.6] |
| **Ποιος χρησιμοποίησε** | Μέλος 2 |

**Prompt:**

> [I want to do word segmentation in ancient Greek texts that are written in scriptio continua (without spaces). How could I do it? Give me examples, 2-3 methods. I should use the dataset Ericu950/Papyri_1”, split=”train”, streaming=True. ]

**Prompt:**

> [Explain to me about the dynamic programming method. If I were to use this method, how would it be done to ensure proper segmentation, and ultimately evaluation of the results?]

**Prompt:**

> [During segmentation, it breaks up the long words that may exist because it detects small 2-letter words within them. We can see how to fix this problem. ]

**Prompt:**

> [I've built this cost function [....]. How can I improve it so that it doesn't break proper nouns into small pieces? Give examples.]
---

### Α3. Μέγεθος Λεξιλογίου & Συχνότητα Λέξεων

| | |
|---|---|
| **Εργαλείο** | [Claude] |
| **Μοντέλο** | [Claude Opus 4.6] |
| **Ποιος χρησιμοποίησε** | Μέλος 2 |

**Prompt:**

> [Give me Python code for vocabulary analysis of ancient Greek texts: vocabulary size, top-N words, hapax legomena, Zipf plot. Use matplotlib.]

**Prompt:**

> [What does Zipf's law tell us about a corpus of ancient Greek inscriptions? How do we interpret the hapax legomena rate in relation to the nature of the corpus?]

**Prompt:**

> [Give me Python code for vocabulary analysis of ancient Greek texts: vocabulary size, top-N words, hapax legomena, Zipf plot. Use matplotlib.]

**Prompt:**

> [ Suggest simple matplotlib visualizations for word frequency analysis in a university NLP assignment. Include top words, rare words, and a Zipf-like frequency plot. ]
---

### Α4. Κύρια Ονόματα

| | |
|---|---|
| **Εργαλείο** | [Claude] |
| **Μοντέλο** | [Claude Opus 4.6] |
| **Ποιος χρησιμοποίησε** | Μέλος 2 |


**Prompt:**

> [ Suggest simple matplotlib visualizations for word frequency analysis in a university NLP assignment. Include top words, rare words, and a Zipf-like frequency plot. ]

---

### Α5. Ομαδοποίηση Επιγραφών & Θεματική Ανάλυση

| | |
|---|---|
| **Εργαλείο** | [Claude] |
| **Μοντέλο** | [Claude Opus 4.6] |
| **Ποιος χρησιμοποίησε** | Μέλος 2 |


**Prompt:**

> [I want to do clustering on ancient Greek inscriptions. Explain to me the difference between TF-IDF + KMeans and topic modeling (LDA). When do I prefer one over the other? Give an example in Python.]

**Prompt:**

> [I want to cluster ancient Greek inscriptions (~224 texts).
The texts come from word segmentation scriptio continua, so
they may have noise. Which vectorization method is better:
word-level TF-IDF or character n-gram TF-IDF? What are the advantages
and disadvantages of each method? Give an example in Python.]

**Prompt:**

> [My silhouette scores are very low (0.02-0.03). What does this mean? How can I improve the clusters? Would UMAP, DBSCAN, or character-level features help?]

**Prompt:**
> [My silhouette scores are very low (0.02-0.03) in clustering
ancient Greek inscriptions with KMeans + TF-IDF. What does this mean?
Are my clusters useless? How can I improve the results?
Would UMAP, DBSCAN, or character-level features help?]

**Prompt:**
> [ Σου δίνω τα top bigrams και trigrams από τα clusters μου:
Cluster 2: "ΤΩΙ ΔΗΜΩΙ" (33×), "ΤΟΝ ΔΗΜΟΝ" (20×), "ΔΗΜΟΝ ΤΟΝ ΑΘΗΝΑΙΩΝ" (7×)
Cluster 3: "ΑΓΑΘΗΙ ΤΥΧΗΙ" (2×), "ΕΠΙ ΑΡΧΟΝΤΟΣ" (3×)
Cluster 0: "ΕΠΙ ΓΟΝΟΣ" (4×), "ΜΗΝ ΟΔΩΡΟΣ" (3×)
Εξήγησέ μου τι τύπους αρχαίων ελληνικών επιγραφών αντιπροσωπεύουν.
Ποιες είναι γνωστές formulae; Τι θα περίμενε ένας επιγραφολόγος;]

**Prompt:**

> [ Θέλω να κάνω ταυτόχρονα clustering (KMeans) και topic modeling (LDA) σε
αρχαίες ελληνικές επιγραφές. Ποια η διαφορά μεταξύ τους; Πότε τα
αποτελέσματα συμφωνούν και πότε διαφωνούν; Πώς μπορώ να τα συγκρίνω
(confusion matrix, heatmap); Δώσε κώδικα Python.]

**Prompt:**
> [Το LDA μου βρήκε 5 topics σε αρχαίες ελληνικές επιγραφές. Τα top words:
Topic 0: ΤΟΥ, ΤΗΣ, ΕΠΙ, ΠΥΘ, ΦΙΛΟ, ΑΝΤΙ, ΔΗΜ, ΑΛΕΞΑΝΔΡΟΣ
Topic 1: ΚΑΙ, ΤΟΝ, ΤΩΝ, ΤΩΙ, ΤΗΣ, ΤΗΝ, ΕΙΣ, ΕΠΙ, ΚΑΤΑ
Topic 2: ΙΟΣ, ΕΥΣ, ΣΙΟΣ, ΔΙΟΝΥΣΙΟΣ, ΔΙΟΝΥΣΙΟΥ, ΔΙΟ
Topic 3: ΖΩΣΙΜΟΣ, ΜΟΣ, ΕΥΣ, ΕΠΙ, ΣΥΝ
Topic 4: ΟΥΝ, ΤΟΝ, ΥΠΟ, ΔΙΟ, ΜΟΣ, ΣΥΝ, ΔΗΜ
Δώσε μου ερμηνεία για κάθε topic. Τι τύπος επιγραφής αντιπροσωπεύεται;]

**Prompt:**
> [Έχω 4 γραφήματα για clustering αρχαίων επιγραφών: silhouette plot, PCA 2D
scatter, bar μεγεθών, bar μέσου μήκους. Τι γραφήματα θα πρόσθετες; Θέλω
κάτι που δείχνει τη σχέση μεταξύ KMeans clusters και LDA topics.
Δώσε κώδικα σε Python (matplotlib/seaborn) για heatmap και boxplot.]
---

### Α6. Χρονολόγηση με Aeneas & Σύγκριση με LLM

#### Α6a — Prompts βοήθειας κώδικα (Aeneas setup, σύγκριση κ.λπ.)

| | |
|---|---|
| **Εργαλείο** | [Claude] |
| **Μοντέλο** | [Claude Opus 4.6] |
| **Ποιος χρησιμοποίησε** | Μέλος 2 |

**Prompt:**

> [Can you explain to me what the Aeneas model does? I would also like you to show me how I can use it in an inscription analysis I have to do (the inscriptions are in ancient Greek) after first converting them to the appropriate format. Is there an online tool for this conversion that I can use?]

**Prompt:**

> [Can you give me an example so I can make a comparison plot with the data I'm attaching?]

#### Α6b — Prompts χρονολόγησης (LLM ως πείραμα)

| | |
|---|---|
| **Εργαλείο** | [ChatGpt] |
| **Μοντέλο** | [Gpt 5.4] |
| **Ποιος χρησιμοποίησε** | Μέλος 2 |

**Prompt:**

> [Είσαι ειδικός επιγραφολόγος. Σου δίνω μόνο κείμενα αρχαίων ελληνικών επιγραφών. Βάσει γλωσσικών, μορφολογικών και θεματικών χαρακτηριστικών, εκτίμησε τη χρονολόγηση. Εξήγησε τη λογική σου. Κείμενα: [...]  
llm_results = { # ΣΥΜΠΛΗΡΩΣΤΕ ΜΕ ΤΑ ΑΠΟΤΕΛΕΣΜΑΤΑ ΣΟΥ # 'inscription_id': {'estimated_year': ..., 'range': '...', 'reasoning': '...'},}]

---

## Μέρος Β — Ανάλυση Εικόνων

### Β1. Προεπεξεργασία Εικόνων

| | |
|---|---|
| **Εργαλείο** | [εργαλείο] |
| **Μοντέλο** | [μοντέλο] |
| **Ποιος χρησιμοποίησε** | Μέλος 1 / Μέλος 2 / Και οι δύο |

**Prompt:**

> [Αντιγράψτε εδώ το ακριβές prompt που δώσατε]

---

### Β2. Ομαδοποίηση Εικόνων μέσω Embeddings

| | |
|---|---|
| **Εργαλείο** | [εργαλείο] |
| **Μοντέλο** | [μοντέλο] |
| **Ποιος χρησιμοποίησε** | Μέλος 1 / Μέλος 2 / Και οι δύο |

**Prompt:**

> [Αντιγράψτε εδώ το ακριβές prompt που δώσατε]

---

### Β3. Pipeline Αναγνώρισης Κειμένου (OCR)

| | |
|---|---|
| **Εργαλείο** | [εργαλείο] |
| **Μοντέλο** | [μοντέλο] |
| **Ποιος χρησιμοποίησε** | Μέλος 1 / Μέλος 2 / Και οι δύο |

**Prompt:**

> [Αντιγράψτε εδώ το ακριβές prompt που δώσατε]

---

## Σημειώσεις

<!--
Προαιρετικός χώρος για γενικές σημειώσεις σχετικά με τη χρήση AI στην εργασία:
- Ποιες ενότητες ωφελήθηκαν περισσότερο από τη χρήση AI;
- Σε ποια σημεία το AI έκανε λάθη και χρειάστηκε διόρθωση;
- Τι θα κάνατε διαφορετικά;
-->

*(συμπληρώστε εδώ)*
