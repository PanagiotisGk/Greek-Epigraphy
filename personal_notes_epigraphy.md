# Προσωπικές Σημειώσεις — AI for Epigraphy

> Αυτό το αρχείο είναι για **προσωπική χρήση και επανάληψη**.
> Σκοπός: να καταγράφουμε τι κάνει κάθε κομμάτι κώδικα, γιατί το κάνει, και τι μάθαμε.
> Δομή: αντιστοιχεί 1-1 στα κελιά του `team_notebook_epigraphy.ipynb`.

---

## Cell 0 — Εισαγωγή (Markdown)

Τίτλος εργασίας, στοιχεία μελών, πίνακας συνεισφοράς, οδηγίες, άδειες χρήσης.

**Τι πρέπει να θυμάμαι:**
- Πριν υποβάλω: Kernel → Restart & Run All
- Αρχείο ονομασίας: `<ΑΜ1>_<ΑΜ2>.ipynb`
- Η αναφορά Howe et al. (2025) είναι υποχρεωτική

---

## Cell 1 — Επιπλέον Βιβλιοθήκες (Markdown)

Αν εγκαταστήσουμε κάτι επιπλέον (π.χ. `cltk`, `pyLDAvis`, `evaluate`), το τεκμηριώνουμε εδώ.

---

## Cell 2 — Imports (Code)

```python
import warnings, pathlib, collections, matplotlib, numpy, pandas, seaborn, PIL, tqdm
import re, nltk                           # NLP
import sklearn (KMeans, PCA, TfidfVectorizer)  # ML
import torch, transformers                 # Deep Learning
import cv2                                 # Image Processing
```

**Τι κάνει κάθε βιβλιοθήκη:**

| Βιβλιοθήκη | Χρήση στην εργασία |
|---|---|
| `pandas` | Χειρισμός δεδομένων — DataFrame με τις μεταγραφές |
| `numpy` | Αριθμητικές πράξεις, πίνακες embeddings |
| `matplotlib` / `seaborn` | Γραφήματα (bar charts, scatter plots, heatmaps) |
| `PIL (Pillow)` | Φόρτωση και εμφάνιση εικόνων |
| `cv2 (OpenCV)` | Προεπεξεργασία εικόνων (blur, threshold, morphology) |
| `re` | Regular expressions — χρήσιμο στο transliteration/segmentation |
| `nltk` | NLP εργαλεία — tokenization, frequency distributions |
| `sklearn` | Clustering (KMeans), PCA, TF-IDF vectorization |
| `torch` | PyTorch — τρέχει τα neural network μοντέλα (ViT, TrOCR) |
| `transformers` | HuggingFace — φόρτωση pre-trained μοντέλων (ViT, TrOCR, κ.λπ.) |
| `tqdm` | Progress bars — χρήσιμο σε loops με πολλές εικόνες |

**Τι σημαίνει το `DEVICE`:**
- `torch.device('cuda')` = χρήση GPU (γρήγορο, αν υπάρχει)
- `torch.device('cpu')` = χρήση CPU (πιο αργό, δουλεύει παντού)

**Σημειώσεις:**
- *(συμπληρώστε εδώ κατά τη διάρκεια της εργασίας)*

---

## Cell 3 — Φόρτωση Δεδομένων (Markdown)

Εξηγεί τη δομή του dataset: Annotations (txt), Images (png), Proxies (PDF).

**Τι πρέπει να θυμάμαι:**
- Τα annotations είναι σε λατινικούς χαρακτήρες (ΟΧΙ ελληνικούς)
- Scriptio continua = χωρίς κενά μεταξύ λέξεων
- Κάθε εικόνα αντιστοιχεί σε annotation μέσω `inscription_id`

---

## Cell 4 — Φόρτωση Δεδομένων (Code)

```python
DATA_DIR = Path('data')
ANNOTATIONS_DIR = DATA_DIR / 'Annotations'
IMAGES_DIR = DATA_DIR / 'Images'
```

**Τι κάνει βήμα-βήμα:**
1. Ορίζει τα paths προς τους φακέλους δεδομένων
2. Διαβάζει κάθε `.txt` αρχείο → αποθηκεύει σε dictionary `{id: text}`
3. Βρίσκει όλα τα `.png` αρχεία εικόνων
4. Φτιάχνει DataFrame με στήλες: `inscription_id`, `text_latin`, `text_length`, `has_image`

**Νέες έννοιες:**
- `Path.glob('*.txt')` — βρίσκει όλα τα αρχεία με κατάληξη `.txt` σε φάκελο
- `txt_file.stem` — το όνομα αρχείου χωρίς κατάληξη (π.χ. `IG_I2_1` από `IG_I2_1.txt`)

**Σημειώσεις:**
- *(συμπληρώστε μετά την εκτέλεση — πόσες μεταγραφές, πόσες εικόνες, αντιστοιχίες)*

---

## Cell 5 — Προεπισκόπηση Δείγματος (Code)

Εμφανίζει μια δειγματική εικόνα squeeze μαζί με τη μεταγραφή σε λατινικούς.

**Νέες έννοιες:**
- `Image.open()` — φόρτωση εικόνας με Pillow
- `ax.imshow(img, cmap='gray')` — εμφάνιση grayscale εικόνας σε matplotlib

**Σημειώσεις:**
- *(συμπληρώστε — τι δείχνει η εικόνα, πόσο ευανάγνωστη είναι)*

---

## Cell 6 — Τίτλος Μέρους Α (Markdown)

Αρχή Μέρους Α — Ανάλυση Κειμένων (50 μονάδες).

---

## Cell 7 — Τίτλος Α1 (Markdown)

**Ερώτημα Α1 (6 μονάδες):** Μετατροπή λατινικών χαρακτήρων σε κεφαλαία ελληνικά.

**Τι ζητάει:**
- Φτιάξε πίνακα αντιστοίχισης (π.χ. `a → Α`, `b → Β`)
- Χρησιμοποίησε το Proxies PDF
- Δείξε παραδείγματα πριν/μετά

---

## Cell 8 — Α1: Κώδικας Transliteration (Code)

**Τι πρέπει να υλοποιήσω:**
1. `LATIN_TO_GREEK` dictionary — πίνακας αντιστοίχισης
2. `transliterate()` function — εφαρμόζει τον πίνακα σε κάθε χαρακτήρα
3. Νέα στήλη `df['text_greek']` — μεταγραφή σε ελληνικά

**Βασικές εντολές που θα χρειαστώ:**
- `str.translate()` ή `str.replace()` — αντικατάσταση χαρακτήρων
- `str.upper()` — κεφαλαιοποίηση

**Προσοχή:**
- Μπορεί να υπάρχουν δίψηφοι χαρακτήρες (π.χ. `th → Θ`, `ph → Φ`)
- Πρέπει να ελέγξω τους δίψηφους ΠΡΙΝ τους μονοψήφους

**Σημειώσεις:**
- *(συμπληρώστε — ποιος πίνακας χρησιμοποιήθηκε, ειδικές περιπτώσεις)*

---

## Cell 9 — Σχόλιο Α1 (Markdown)

Εδώ γράφουμε: περιγραφή του transliteration system, αμφισημίες, παραδείγματα.

---

## Cell 10 — Τίτλος Α2 (Markdown)

**Ερώτημα Α2 (10 μονάδες):** Word segmentation σε scriptio continua.

**Hint του καθηγητή:** Papyri dataset για λεξιλόγιο:
```python
from datasets import load_dataset
ds = load_dataset('Ericu950/Papyri_1', split='train', streaming=True)
```

---

## Cell 11 — Α2: Κώδικας Word Segmentation (Code)

**Τι πρέπει να υλοποιήσω:**
1. Κατασκευή λεξιλογίου (vocabulary) αρχαίων ελληνικών
2. Αλγόριθμος τμηματοποίησης (π.χ. greedy, dynamic programming)
3. Εφαρμογή σε όλες τις μεταγραφές

**Πιθανές προσεγγίσεις:**
- **Greedy (max match):** Ξεκινά από αριστερά, βρίσκει τη μεγαλύτερη λέξη που ταιριάζει → πάει στο υπόλοιπο
- **Dynamic Programming:** Βρίσκει τη βέλτιστη τμηματοποίηση (ελάχιστες λέξεις / μέγιστο coverage)
- **BPE/WordPiece:** Subword tokenization (πιο σύνθετο)

**Νέες έννοιες:**
- `load_dataset(..., streaming=True)` — φορτώνει data χωρίς να τα κατεβάσει ολόκληρα στη RAM
- Dynamic programming — *(συμπληρώστε αν χρησιμοποιηθεί)*

**Σημειώσεις:**
- *(συμπληρώστε — ποια μέθοδος δούλεψε, accuracy, παραδείγματα)*

---

## Cell 12 — Σχόλιο Α2 (Markdown)

Εδώ γράφουμε: μέθοδος, προκλήσεις, αξιολόγηση ποιότητας, παραδείγματα.

---

## Cell 13 — Τίτλος Α3 (Markdown)

**Ερώτημα Α3 (6 μονάδες):** Μέγεθος λεξιλογίου, συχνές λέξεις, σπάνιες λέξεις, γραφήματα.

---

## Cell 14 — Α3: Κώδικας Λεξιλογίου (Code)

**Τι πρέπει να υλοποιήσω:**
1. Μοναδικές λέξεις (vocabulary size)
2. Top-20 πιο συχνές λέξεις
3. Σπάνιες λέξεις (1–2 εμφανίσεις)
4. Γραφήματα: bar chart + frequency distribution

**Βασικές εντολές:**
- `Counter()` — μέτρηση εμφανίσεων
- `Counter.most_common(20)` — οι 20 πιο συχνές
- `value_counts()` — αν δουλεύουμε με Series

**Γράφημα Zipf:**
- Νόμος Zipf: η n-οστή πιο συχνή λέξη εμφανίζεται ~1/n φορές σε σχέση με την πρώτη
- Log-log plot: αν ακολουθεί Zipf, θα φαίνεται ευθεία γραμμή

**Σημειώσεις:**
- *(συμπληρώστε — vocab size, κυρίαρχες λέξεις, % σπάνιων)*

---

## Cell 15 — Σχόλιο Α3 (Markdown)

Εδώ γράφουμε: vocab size, Zipf analysis, τι αντικατοπτρίζουν οι συχνές λέξεις.

---

## Cell 16 — Τίτλος Α4 (Markdown)

**Ερώτημα Α4 (6 μονάδες):** Κύρια ονόματα, κατανομή ανά φύλο.

---

## Cell 17 — Α4: Κώδικας Κύριων Ονομάτων (Code)

**Τι πρέπει να υλοποιήσω:**
1. Εντοπισμός κύριων ονομάτων (NER ή dictionary-based)
2. Ταξινόμηση ανά φύλο
3. Γραφήματα: top-15 ονόματα + κατανομή φύλου

**Πιθανές πηγές ονομάτων:**
- LGPN (Lexicon of Greek Personal Names)
- CLTK NER module
- Custom λίστα ονομάτων

**Πώς ξεχωρίζω ανδρικά/γυναικεία:**
- Ανδρικά: συνήθως λήγουν σε -ΟΣ, -ΗΣ, -ΩΝ
- Γυναικεία: συνήθως λήγουν σε -Α, -Η, -Ω

**Σημειώσεις:**
- *(συμπληρώστε — πηγή, μέθοδος, κυρίαρχα ονόματα)*

---

## Cell 18 — Σχόλιο Α4 (Markdown)

Εδώ γράφουμε: μέθοδος εντοπισμού, αναλογία φύλων, ερμηνεία.

---

## Cell 19 — Τίτλος Α5 (Markdown)

**Ερώτημα Α5 (10 μονάδες):** Clustering + topic modeling.

---

## Cell 20 — Α5: Clustering Κώδικας (Code)

**Τι πρέπει να υλοποιήσω:**
1. Αναπαράσταση κειμένων (TF-IDF ή embeddings)
2. Clustering (KMeans, DBSCAN κ.λπ.)
3. Απεικόνιση σε 2D (PCA/UMAP)
4. Ανάλυση κυρίαρχων λέξεων ανά cluster

**Νέες έννοιες:**
- `TfidfVectorizer` — μετατρέπει κείμενα σε αριθμητικά vectors βάσει σπανιότητας λέξεων
- `KMeans(n_clusters=k)` — χωρίζει σε k ομάδες βάσει απόστασης
- **Elbow method:** Τρέχω KMeans για k=2,3,...,10 → γράφημα inertia → βρίσκω το "αγκώνα"
- **Silhouette score:** Μετρική ποιότητας clustering (πιο κοντά στο 1 = καλύτερο)
- `PCA(n_components=2)` — μείωση διαστάσεων σε 2D για απεικόνιση
- `UMAP` — εναλλακτική μείωση διαστάσεων (καλύτερη σε μη-γραμμικά δεδομένα)

**Σημειώσεις:**
- *(συμπληρώστε — αριθμός clusters, elbow plot, ερμηνεία)*

---

## Cell 21 — Α5: Topic Modeling (Code)

**Τι πρέπει να υλοποιήσω:**
1. Topic modeling (LDA, NMF, ή BERTopic)
2. Top-10 keywords ανά topic
3. Απεικόνιση (word clouds, bar charts)

**Νέες έννοιες:**
- **LDA (Latent Dirichlet Allocation):** Θεωρεί κάθε κείμενο ως μείγμα topics, κάθε topic ως μείγμα λέξεων
- **NMF (Non-negative Matrix Factorization):** Παρόμοιο αλλά ντετερμινιστικό
- **BERTopic:** Σύγχρονη μέθοδος — χρησιμοποιεί embeddings + clustering + c-TF-IDF

**Σημειώσεις:**
- *(συμπληρώστε — ποια μέθοδος, πόσα topics, ερμηνεία)*

---

## Cell 22 — Σχόλιο Α5 (Markdown)

Εδώ γράφουμε: αν τα clusters αντιστοιχούν σε δομή, αν έχουν κοινές φράσεις, θέματα.

---

## Cell 23 — Τίτλος Α6 (Markdown)

**Ερώτημα Α6 (12 μονάδες):** Χρονολόγηση 10 επιγραφών — Aeneas vs LLM.

---

## Cell 24 — Α6: Aeneas Χρονολόγηση (Code)

**Τι πρέπει να υλοποιήσω:**
1. Εγκατάσταση Aeneas (predictingthepast)
2. Επιλογή 10 επιγραφών
3. Εκτέλεση χρονολόγησης

**Νέες έννοιες:**
- **Aeneas:** Pre-trained μοντέλο DeepMind εκπαιδευμένο σε αρχαίες ελληνικές επιγραφές
- Εκτιμά χρονολογία (π.χ. "3ος αι. π.Χ.") βάσει γλωσσικών χαρακτηριστικών

**Σημειώσεις:**
- *(συμπληρώστε — installation steps, τυχόν προβλήματα)*

---

## Cell 25 — Α6: LLM Χρονολόγηση & Σύγκριση (Code)

**Τι πρέπει να υλοποιήσω:**
1. Στείλω τις ίδιες 10 μεταγραφές σε LLM (π.χ. GPT-4, Claude)
2. Συγκριτικός πίνακας: Aeneas vs LLM
3. Γράφημα σύγκρισης

**Σημειώσεις:**
- *(συμπληρώστε — ποιο LLM, αποτελέσματα, σύγκριση)*

---

## Cell 26 — Σχόλιο Α6 (Markdown)

Εδώ γράφουμε: σύγκριση, domain-specific vs general-purpose, common failure modes.

---

## Cell 27 — Τίτλος Μέρους Β (Markdown)

Αρχή Μέρους Β — Ανάλυση Εικόνων (50 μονάδες).

---

## Cell 28 — Τίτλος Β1 (Markdown)

**Ερώτημα Β1 (12 μονάδες):** Προεπεξεργασία εικόνων — δείξτε αρχικές → βήματα → καθαρισμένες.

---

## Cell 29 — Β1: Εμφάνιση Αρχικών Εικόνων (Code)

**Τι πρέπει να υλοποιήσω:**
- Επιλογή 3–5 δειγμάτων
- Εμφάνιση σε grid (subplot)

**Σημειώσεις:**
- *(συμπληρώστε — ποια δείγματα, ποιότητα αρχικών εικόνων)*

---

## Cell 30 — Β1: Pipeline Προεπεξεργασίας (Code)

**Τι πρέπει να υλοποιήσω:**
1. `preprocess_image()` function
2. Εμφάνιση ενδιάμεσων βημάτων

**Βήματα προεπεξεργασίας (OpenCV):**

| Βήμα | Εντολή OpenCV | Τι κάνει |
|---|---|---|
| Grayscale | `cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)` | Μετατροπή σε ασπρόμαυρο |
| Denoise | `cv2.GaussianBlur(img, (5,5), 0)` | Μείωση θορύβου |
| CLAHE | `cv2.createCLAHE(clipLimit=2.0)` | Βελτίωση τοπικής αντίθεσης |
| Otsu | `cv2.threshold(img, 0, 255, cv2.THRESH_OTSU)` | Αυτόματο binarization |
| Adaptive | `cv2.adaptiveThreshold(...)` | Binarization ανά περιοχή |
| Morphology | `cv2.morphologyEx(img, cv2.MORPH_CLOSE, kernel)` | Κλείσιμο μικρών κενών |

**Σημειώσεις:**
- *(συμπληρώστε — ποια βήματα δούλεψαν καλύτερα, σειρά εφαρμογής)*

---

## Cell 31 — Β1: Σύγκριση Πριν/Μετά (Code)

Side-by-side εμφάνιση αρχικής vs καθαρισμένης εικόνας.

**Σημειώσεις:**
- *(συμπληρώστε)*

---

## Cell 32 — Σχόλιο Β1 (Markdown)

Εδώ γράφουμε: ποια βήματα, ποια βελτίωσαν αναγνωσιμότητα, δύσκολες εικόνες.

---

## Cell 33 — Τίτλος Β2 (Markdown)

**Ερώτημα Β2 (16 μονάδες):** Image clustering μέσω ViT embeddings + PCA/UMAP.

---

## Cell 34 — Β2: Εξαγωγή Embeddings (Code)

**Τι πρέπει να υλοποιήσω:**
1. Φόρτωση pre-trained ViT (π.χ. `google/vit-base-patch16-224`)
2. Εξαγωγή embeddings (feature vectors) για κάθε εικόνα

**Νέες έννοιες:**
- **ViT (Vision Transformer):** Χωρίζει εικόνα σε patches (16x16 pixels), τα κάνει embed, τα περνά από transformer
- **Embedding:** Ένα vector (π.χ. 768 αριθμοί) που αναπαριστά την εικόνα — εικόνες "παρόμοιες" έχουν κοντινά vectors
- **Batching:** Στέλνω πολλές εικόνες μαζί στο μοντέλο (αντί μία-μία) → πιο γρήγορα

**Σημειώσεις:**
- *(συμπληρώστε — χρόνος εκτέλεσης, μέγεθος embeddings)*

---

## Cell 35 — Β2: PCA + UMAP + Clustering (Code)

**Τι πρέπει να υλοποιήσω:**
1. PCA → μείωση σε 50 διαστάσεις (ενδιάμεσο βήμα)
2. UMAP → μείωση σε 2D για απεικόνιση
3. KMeans → ομαδοποίηση
4. Scatter plot με χρωματισμό ανά cluster

**Νέες έννοιες:**
- **PCA (Principal Component Analysis):** Βρίσκει τις διευθύνσεις μέγιστης διακύμανσης → μειώνει dimensions
- **UMAP:** Μη-γραμμική μείωση — διατηρεί καλύτερα τη δομή γειτονιών από PCA
- **Pipeline:** PCA πρώτα (για ταχύτητα) → UMAP μετά (για ποιότητα)

**Σημειώσεις:**
- *(συμπληρώστε — αριθμός clusters, πώς φαίνεται το scatter plot)*

---

## Cell 36 — Β2: Κεντροειδή & Ονοματοδοσία (Code)

**Τι πρέπει να υλοποιήσω:**
1. 3 εικόνες κοντά στο κεντροειδές κάθε cluster
2. Ονοματοδοσία θεμάτων (manual + LLM)
3. Χρήση filenames για context (εποχή;)

**Νέες έννοιες:**
- **Κεντροειδές:** Το "μέσο σημείο" ενός cluster — η πιο αντιπροσωπευτική εικόνα
- Βρίσκω κεντροειδές → μετρώ απόσταση κάθε σημείου → παίρνω τα 3 πιο κοντινά

**Σημειώσεις:**
- *(συμπληρώστε — ονόματα clusters, ερμηνεία, ζεύγη εικόνων)*

---

## Cell 37 — Σχόλιο Β2 (Markdown)

Εδώ γράφουμε: μοντέλο embeddings, κριτήριο αριθμού clusters, ονόματα topics, ζεύγη, ίδιος τόπος.

---

## Cell 38 — Τίτλος Β3 (Markdown)

**Ερώτημα Β3 (22 μονάδες):** OCR pipeline — baseline (TrOCR) + βελτιωμένη λύση + CER αξιολόγηση.

---

## Cell 39 — Β3: Baseline TrOCR (Code)

**Τι πρέπει να υλοποιήσω:**
1. Φόρτωση TrOCR (`microsoft/trocr-base-printed`)
2. OCR σε όλες τις εικόνες
3. Αποθήκευση predictions

**Νέες έννοιες:**
- **TrOCR:** Transformer OCR — encoder (ViT) "βλέπει" εικόνα, decoder (GPT-2 style) παράγει κείμενο
- **VisionEncoderDecoderModel:** Αρχιτεκτονική HuggingFace που συνδυάζει vision encoder + text decoder

**Σημειώσεις:**
- *(συμπληρώστε — χρόνος εκτέλεσης, πρώτες εντυπώσεις)*

---

## Cell 40 — Β3: CER Αξιολόγηση (Code)

**Τι πρέπει να υλοποιήσω:**
1. `compute_cer()` function
2. CER ανά εικόνα
3. Μέσο CER + 5 καλύτερα/χειρότερα

**Νέες έννοιες:**
- **CER (Character Error Rate):** Πόσο % των χαρακτήρων είναι λάθος
- Υπολογίζεται με edit distance (Levenshtein): `CER = (S + D + I) / N`
  - S = substitutions, D = deletions, I = insertions, N = χαρακτήρες ground truth
- CER = 0 → τέλειο, CER = 1 → εντελώς λάθος

**Σημειώσεις:**
- *(συμπληρώστε — baseline CER, ποιες εικόνες πάνε καλά/χάλια)*

---

## Cell 41 — Β3: Βελτιωμένη Λύση (Code)

**Τι πρέπει να υλοποιήσω:**
- Βελτίωση σε σχέση με baseline (fine-tuning, augmentation, ensemble, VLM, post-processing)

**Ιδέες:**
- Fine-tuning TrOCR στα squeeze images
- Data augmentation (rotation, noise, contrast)
- Post-processing με language model (διόρθωση εξόδου)
- VLM (π.χ. Qwen-VL, LLaVA) — vision + language model

**Σημειώσεις:**
- *(συμπληρώστε — τι δοκιμάσαμε, τι δούλεψε)*

---

## Cell 42 — Β3: Συγκριτικός Πίνακας (Code)

**Τι πρέπει να υλοποιήσω:**
- Πίνακας: Μοντέλο | Μέσο CER | Median CER | Best | Worst
- Box plot ή bar chart σύγκρισης

**Σημειώσεις:**
- *(συμπληρώστε — τελικά αποτελέσματα, βελτίωση %)*

---

## Cell 43 — Σχόλιο Β3 (Markdown)

Εδώ γράφουμε: μοντέλα, CER, βελτιώσεις, αδυναμίες, ICDAR submission.

---

## Cell 44 — Συμπεράσματα & Αναφορές (Markdown)

**Τι πρέπει να γράψουμε:**
- 5+ σημεία-κλειδιά (transliteration, segmentation, clustering, χρονολόγηση, OCR)
- Πηγές δεδομένων & αναφορές (ήδη προσυμπληρωμένες)

---

## Γενικές Σημειώσεις Εργασίας

*(Χώρος για σημειώσεις που δεν αντιστοιχούν σε συγκεκριμένο κελί)*

- ...
