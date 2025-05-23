# Code of Duty - AI-Based Coordinate System for Candidate Grading
*Smart India Hackathon 2024*

"Code of Duty" is an AI-powered recruitment intelligence system that evaluates, filters, and ranks job candidates based on a 3D coordinate model:
- X-axis: **Education**
- Y-axis: **Experience**
- Z-axis: **Skills**

This system parses CVs/resumes, scores each coordinate using NLP and custom logic, and visualizes candidates in 3D space. Recruiters can shortlist top candidates instantly while jobseekers gain actionable feedback to upskill.

---

## Core Idea

We map every candidate into a 3D coordinate `(x, y, z)`:
- `x` ➝ Education score based on degree, university tier, GPA, etc.
- `y` ➝ Experience score based on years, company tier, role match
- `z` ➝ Skill score based on extracted & matched skills with JD

These coordinates are visualized and ranked for rapid, data-driven hiring.

---

## Technical Architecture

### Input & Resume Parsing
- **Upload**: PDF/DOCX resumes or direct form entry
- **Libraries**:
  - `pdfminer.six`, `python-docx`: For text extraction
  - `spaCy`: For Named Entity Recognition (NER), lemmatization
  - `re`: To extract durations, years, etc.

### NLP Pipeline
- Tokenization & Lemmatization
- NER to pull out:
  - Universities
  - Companies
  - Degrees & certifications
  - Tools/Tech stack
- Extract years of experience and match job titles to domains

---

## Coordinate Scoring System

### X-axis (Education Score)

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

#### Degree Level (out of 25)
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

#### Certifications & GPA Bonus (out of 15)
- **Certifications**:
  - Industry-relevant (AWS, Azure, Coursera ML, etc.): +5 each (max 10)
- **GPA Bonus**:
  - GPA ≥ 9.0: +5
  - GPA 8.0–8.9: +3
  - GPA 7.0–7.9: +2

---

**Final Education Score Calculation**:
```python
education_score = university_score + degree_score + branch_score + bonus_score
```

### Y-axis (Experience Score)

The experience score evaluates the depth, relevance, and quality of a candidate’s industry exposure. It is composed of:

1. **Company Tier & Reputation (out of 30)**
2. **Total Work Experience (out of 25)**
3. **Domain Relevance (out of 25)**
4. **Seniority & Impact (out of 10)**
5. **Continuity / Gap Penalty & Recency Bonus (out of 10)**

---

#### Company Tier & Reputation (out of 30)

| Tier        | Examples                                  | Score     |
|-------------|-------------------------------------------|-----------|
| Tier 1      | FAANG, Big Tech (Google, Amazon, Meta), MNCs (McKinsey, Goldman Sachs, etc.) | 25–30     |
| Tier 2      | Funded startups, mid-size tech firms, national giants (Infosys, TCS, etc.) | 18–24     |
| Tier 3      | Small startups, local orgs, academic projects | 10–17     |
| Freelance/Open Source | Depends on impact, team size, visibility | Variable (5–25) |

- Scores may be averaged or weighted if multiple companies are involved.

---

#### Total Work Experience (out of 25)

| Duration         | Score     |
|------------------|-----------|
| >6 years         | 22–25     |
| 3–6 years        | 18–21     |
| 1–3 years        | 13–17     |
| <1 year          | 8–12      |
| Internships only | 5–7       |

- Part-time roles and freelance stints are prorated.
- Teaching Assistant (TA) or research assistant roles may count for partial credit.

---

#### Domain Relevance (out of 25)

How aligned is the candidate's past work with the target job's technical/domain requirements?

| Relevance Level     | Description                                            | Score     |
|---------------------|--------------------------------------------------------|-----------|
| Highly Relevant     | Direct match (e.g., ML Engineer → ML role)             | 22–25     |
| Moderately Relevant | Some overlap, transferable skills                      | 15–21     |
| Minimally Relevant  | Little overlap, general exposure                       | 8–14      |
| Irrelevant          | Unrelated domains                                      | ≤7        |

- Keywords matched via NLP + semantic similarity
- Role-based weight adjustments

---

#### Seniority & Impact (out of 10)

| Impact Level          | Examples                                              | Score     |
|------------------------|-------------------------------------------------------|-----------|
| Leadership Roles       | Tech Lead, Product Owner, Manager                     | 8–10      |
| IC with Impact         | Delivered major modules, owned features               | 5–7       |
| Contributor            | Assisted teams, worked under supervision              | 2–4       |
| Intern/Junior Level    | Little autonomy, learning stage                       | 0–2       |

- Bonus points for patents, open-source leadership, published papers, or awards

---

#### Continuity, Gaps, and Recency (out of 10)

| Attribute                 | Scoring Notes                                     | Score     |
|--------------------------|---------------------------------------------------|-----------|
| No gaps, recent work     | Continuous employment, past year active           | 8–10      |
| Small gaps               | <6 months, well justified                         | 5–7       |
| Gaps 6–12 months         | Partial penalty                                   | 3–4       |
| >1 year gap, no projects | Heavy penalty unless exceptional reason given     | 0–2       |

- Recency bonus: +1–2 for recent, relevant job within last 12 months
- Side projects or academic activity may compensate partially

---

### Final Experience Score Calculation:

```python
experience_score = company_score + duration_score + domain_score + seniority_score + continuity_score
```
