# NJ Title I Public School Enrollment Analysis

**Project Summary**

---

## 1. How Was the Data Extracted?

- I completed the data extraction process using the **Pandas** library in Python. Most of the work was conducted in VS Code using the Jupyter Notebook interface. I also created a virtual environment to manage a kernel with the necessary libraries required for this project.

- The most challenging aspect was extracting data from the Title I Grants to Local Educational Agencies PDF. Due to its original format, I first had to convert the PDF into a CSV file before successfully loading it as a Pandas DataFrame for analysis.

- **enrollment_2425_1_.xlsx** — Loaded using `pd.read_excel()` targeting the `School` sheet with `skiprows=2`, after installing the `openpyxl` dependency required by pandas to read Excel files.

---

## 2. How Was the Data Merged?

- The enrollment and Title I datasets were merged using **District Code** as the primary joining key via `pd.merge()` with a left join, preserving all school-level enrollment records.

- The LEA ID in the Title I dataset (e.g. `3400660`) was initially unclear — after a brief investigation, it was determined to be a composite of **State code (34) + County code + District code**. The last 4 digits were extracted using string slicing to isolate the District Code, which matched the enrollment dataset's District Code column, enabling a successful join. The final merged dataframe contained **2,528 rows and 68 columns**.

---

## 3. Language & Libraries Used

- **Language:** Python (Jupyter Notebook in VS Code)
- **Data manipulation:** Pandas — used for loading, cleaning, filtering, grouping, and merging all datasets
- **Visualization:** Matplotlib — horizontal bar chart for county-level analysis; Seaborn — ranked bar chart with hue encoding for school-level analysis

---

## 4. Challenges Encountered & How They Were Resolved

- **ParserError on CSV load** — The NJPubSchool.csv had 3 description rows before the header, mismatched quote characters, and encoding issues. Resolved by adding `skiprows=3`, `quoting=3`, `encoding='latin1'`, and `on_bad_lines='skip'`.

- **Missing openpyxl module** — With one of the files being in Excel format, the `openpyxl` module had to be installed inside the active conda environment to successfully read the file.

- **Trailing space in column name** — The Title I dataset had `'LEA ID '` (with a trailing space), causing a KeyError. Fixed by stripping whitespace from all column names using `df.columns.str.strip()`.

- **LEA ID join key** — The Title I dataset had no District Code column, only a composite LEA ID. After analyzing the structure (34 + county + district), the last 4 digits were extracted and converted to float to match the enrollment dataset's District Code format, enabling the merge.

- **Footnote row in Title I data** — The `PART D SUBPART 2` row at the bottom of the Title I dataset was not a real district. It was identified via `df.tail()` and removed with a boolean filter before merging.

---

## 5. Questions Answered & Key Observations

- **Q1 — NJ counties by grades 5–8 students in Title I schools:** Middlesex County led with 24,018 students, followed by Bergen (12,913) and Hudson (12,772). The horizontal bar chart revealed a significant gap between the top 3 counties and the rest, with Salem (358), Warren (747), and Sussex (915) at the bottom. A `Charters` category appeared in the county grouping, indicating charter schools are tracked separately from geographic counties.

- **Q2 — Top 10 Title I schools by grades 5–8 enrollment:** After filtering out 0-enrollment schools (elementary schools not serving those grades), 755 schools remained. The Seaborn chart with district-level color coding showed clear clustering of high-enrollment schools in urban districts. Many schools with 0 students in grades 5–8 confirmed the presence of K–4 only elementary schools within Title I districts.

- **Additional Observation 1 — Districts with $0 Title I allocation:** A large number of districts in the merged dataset had $0 in Title I funding, meaning they are not federally designated Title I schools despite being in the NJ directory. Filtering for allocation > 0 was critical to isolating truly Title I funded schools.

- **Additional Observation 2 — Enrollment concentration in urban districts:** The top schools by grades 5–8 enrollment were overwhelmingly located in high-density urban districts such as those in Middlesex, Hudson, and Bergen counties, suggesting that Title I funding is heavily concentrated in urbanized, high-need areas of New Jersey.

---

## Project Files

| Folder | Contents |
|---|---|
| `data/` | NJPubSchool.csv, enrollment_2425_1_.xlsx, newjerseypdf-40553.csv |
| `notebooks/` | Jupyter notebook with full analysis |
| `dashboard/` | Exported visualizations (PNG charts) |
