# Code of Duty — AI-Based Coordinate System for Candidate Grading
*Smart India Hackathon 2024*

"Code of Duty" is an AI-powered recruitment intelligence system that evaluates, filters, and ranks job candidates based on a 3D coordinate model:
- 📘 X-axis: **Education**
- 💼 Y-axis: **Experience**
- 🛠️ Z-axis: **Skills**

This system parses CVs/resumes, scores each coordinate using NLP and custom logic, and visualizes candidates in 3D space. Recruiters can shortlist top candidates instantly while jobseekers gain actionable feedback to upskill.

---

## 🧠 Core Idea

We map every candidate into a 3D coordinate `(x, y, z)`:
- `x` ➝ Education score based on degree, university tier, GPA, etc.
- `y` ➝ Experience score based on years, company tier, role match
- `z` ➝ Skill score based on extracted & matched skills with JD

These coordinates are visualized and ranked for rapid, data-driven hiring.

---

## 🏗️ Technical Architecture

### 📥 Input & Resume Parsing
- **Upload**: PDF/DOCX resumes or direct form entry
- **Libraries**:
  - `pdfminer.six`, `python-docx`: For text extraction
  - `spaCy`: For Named Entity Recognition (NER), lemmatization
  - `re`: To extract durations, years, etc.

### ✨ NLP Pipeline
- Tokenization & Lemmatization
- NER to pull out:
  - 🎓 Universities
  - 🏢 Companies
  - 📚 Degrees & certifications
  - 🛠️ Tools/Tech stack
- Extract years of experience and match job titles to domains

---

## 📊 Coordinate Scoring System

### 🎓 X-axis (Education Score)

The education score is calculated from:

1. **University Tier (out of 40)**
2. **Degree Level (out of 25)**
3. **Branch Relevance (out of 20)**
4. **Certifications & GPA Bonus (out of 15)**

---

#### University Tier (out of 40)
| Tier         | Example Institutions       | Score |
|--------------|----------------------------|--------|
| Tier 1       | IITs, IISC                 | 36–40 |
| Tier 2       | NITs, BITS, Top Private    | 30–35 |
| Tier 3       | State Colleges, Others     | 20–29 |
| Unknown/Unranked | Local Colleges         | <20   |

---

#### 🎓 Degree Level (out of 25)
| Degree        | Score  |
|---------------|--------|
| Ph.D.         | 25     |
| Master’s      | 22     |
| Bachelor’s    | 20     |
| Diploma       | 15     |
| High School   | 10     |

---

#### Branch Relevance (out of 20)
| Branch Type                        | Example Branches                      | Score     |
|-----------------------------------|----------------------------------------|-----------|
| Highly Relevant                   | CSE, DS, IT, AI/ML, Data Science       | 18–20     |
| Moderately Relevant               | ECE, EE, Mech (with coding exposure)   | 14–17     |
| Minimally Relevant                | Civil, Bio, Chem, Others               | 8–13      |
| Not Relevant                      | Non-technical disciplines              | ≤7        |

> *Bonus: Minor specialization in tech field (e.g. CSE minor with Mech major): +2*

---

#### 📜 Certifications & GPA Bonus (out of 15)
- **Certifications**:
  - Industry-relevant (AWS, Azure, Coursera ML, etc.): +5 each (max 10)
- **GPA Bonus**:
  - GPA ≥ 9.0: +5
  - GPA 8.0–8.9: +3
  - GPA 7.0–7.9: +2

---

🧮 **Final Education Score Calculation**:
```python
education_score = university_score + degree_score + branch_score + bonus_score
```
