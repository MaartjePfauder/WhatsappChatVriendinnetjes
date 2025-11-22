# WhatsApp Friends Chat Corpus (2020–2025)


## 1. Corpus description

This repository contains a small text corpus based on a private WhatsApp group chat between 11 friends.  
The messages span the period from 2020 to 2025 and are written in Dutch, with occasional English phrases and emojis.

The corpus is derived from a single exported WhatsApp `.txt` file:

- Original file: `data/ChatVriendinnetjes.txt`
- Number of participants: 11
- Language: Mainly Dutch
- Genre: Informal, conversational, private messaging
- Time span: 2020–2025
- number of messages: 33722
- number of tokens: 275490

## 2. Target audience and intended use

The corpus is intended as a small, illustrative dataset for:

- practicing corpus creation and preprocessing in Python
- basic NLP experiments (tokenization, lemmatization, part-of-speech tagging)
- exploring informal, conversational Dutch

Because this is a private chat between identifiable (although anonimized) individuals, the corpus is **not** meant for public distribution or large-scale research.  
The repository is for course work only. 

## 3. Text selection criteria

The source of the corpus is a single WhatsApp group chat.  
Selection criteria were:

- Only messages from one specific group of 11 friends.
- Time span: all messages in the exported file, from 2020 to 2025.
- Message types:
  - Text messages are included.
  - Media was not downloaded
  - WhatsApp placeholders such as "<Media weggelaten>" (media omitted) were excluded from the final `Document` column.
  - Emoticons are included
- System messages (e.g. "Messages are end-to-end encrypted", group icon changed, people joined/left) were removed.

## 4. Data collection process

1. The WhatsApp chat was exported from the mobile app as a `.txt` file (without media).
2. This export was saved as `ChatVriendinnetjes.txt` and placed in the `data/` folder.
3. A Jupyter Notebook (`notebooks/ChatAnalysis.ipynb`) was used to:
   - read the `.txt` file,
   - parse each message (timestamp, sender, message text) with regex,
   - preprocess and annotate the texts with spaCy,
   - save the annotated corpus as a single CSV file (`corpus.csv`).

## 5. Cleaning and preprocessing

The following preprocessing steps were performed in the notebook:

- **Parsing the WhatsApp format**:  
  Each line following the pattern  
  `DD-MM-YYYY HH:MM - Sender: Message`  
  was split using regular expressions into:
  - date
  - time
  - sender
  - original message text

- **Removing unwanted messages**:
  - Messages equal to `<Media weggelaten>` were removed (no textual content).
  - Messages written by temporary member 'Camille Werk' where removed.
  - Emotions were preserved

## 6. Annotations and tools

All NLP preprocessing was done in Python using:
- `pandas` for data handling
- `spaCy` with the `nl_core_news_sm` model for Dutch

For each message, spaCy was used to produce:
- **Tokens**: space-separated list of token strings for the message.
- **Lemmas**: space-separated list of lemmas.
- **Parts-of-speech**: space-separated list of universal POS tags (e.g., `NOUN`, `VERB`, etc.).

Additionally, a separate CSV file `average_pos_data.csv` contains aggregate statistics about POS distributions in the chat (relative frequencies of POS by speaker).

## 7. File formats and column descriptions

### 7.1. Files

- `data/ChatVriendinnetjes.txt`  
  Original WhatsApp export in plain text format.

- `corpus.csv`  
  Final annotated corpus, one row per message.

- `notebooks/ChatAnalysis.ipynb`  
  Jupyter Notebook with all preprocessing and annotation code.

- `average_pos_data.csv` 
  Aggregated POS statistics derived from `corpus.csv`.

### 7.2. Columns in `corpus.csv`

Each row represents one WhatsApp message.

- **MessageID**  
  ID of the original text file (`ChatVriendinnetjes.txt`).

- **Person**  
  The person that sent the message.

- **Date**  
  The date on which the message was sent.

- **Time**  
  The time on which the message was sent.

- **Message** 
  The content of the message.

- **Doc** 
  The NLP processed version of the message.

- **Tokens**  
  Tokenized version of the message as a space-separated string.

- **Lemmas**  
  Corresponding lemmas for the tokens, space-separated.

- **Parts-of-speech**  
  POS tags for each token, space-separated, aligned to `Tokens`.

## 8. Quality checks

The following checks were done to ensure data quality:

- Verified that the total number of parsed messages matches the expected number of lines with the WhatsApp date-time pattern.
- Checked for missing values in key columns (`Document`, `Tokens`, `Lemmas`, `Parts-of-speech`).
- Manually inspected a small random sample of rows to confirm:
  - correct parsing of date, time, and sender,
  - correct handling of emojis (utf-8-sig),
  - reasonable tokenization, lemmatization, and POS tags.
