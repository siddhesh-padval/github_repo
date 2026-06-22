# Quiz App — Question Bank Repository

This repo holds the question papers consumed by the Quiz Android app.
The app fetches `index.json` at launch to discover available papers, then
downloads + caches each paper's JSON file as needed.

## Structure

```
index.json              <- list of all available papers + metadata
papers/
  gate_cs_2024.json      <- one file per paper
  csir_net_dec2023.json
  ...
```

## Adding a new paper

1. Write/collect your paper as a `.tex` file using this format per question:

   ```latex
   \question{Section Name}{Marks}
   <question text, can include $...$ or \[...\] math>
   \begin{enumerate}
   \opt <option 1>
   \opt <option 2>
   \opt <option 3>
   \opt <option 4>
   \end{enumerate}
   \ans{N}    % 1-4, the correct option number
   ```

2. Run the converter script (from the `parser/` folder in the main project):

   ```bash
   python3 tex_to_json.py your_paper.tex papers/your_paper.json \
       --exam GATE --subject CS --year 2025
   ```

3. Copy the resulting JSON into `papers/`.

4. Add an entry to `index.json`:

   ```json
   {
     "paper_id": "gate_cs_2025",
     "exam": "GATE",
     "subject": "CS",
     "year": "2025",
     "title": "GATE CS 2025",
     "file": "papers/gate_cs_2025.json",
     "total_questions": 65,
     "total_marks": 100.0
   }
   ```

5. Commit and push. The app will pick up the new paper next time it syncs
   (no app update required).

## Notes on negative marking

The converter applies a default negative-marking rule (1/3 for 1-mark
questions, 2/3 for 2-mark questions, 0 otherwise) which matches common
GATE convention. If your exam's rule differs, hand-edit the
`negative_marks` field in the generated JSON before committing.

## Raw file URLs

GitHub raw URLs follow this pattern (replace USERNAME/REPO/BRANCH):

```
https://raw.githubusercontent.com/USERNAME/REPO/BRANCH/index.json
https://raw.githubusercontent.com/USERNAME/REPO/BRANCH/papers/gate_cs_2024.json
```

The app's `BASE_URL` constant should point at this raw path.
