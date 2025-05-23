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
### Z-axis (Skills Score)

The Skills Score represents how well a candidate's technical and soft skill set matches the target role. This includes not just keyword matches, but also contextual relevance, confidence of usage, and recency. The scoring is divided into:

1. **Core Skill Match (out of 30)**
2. **Bonus/Optional Skill Match (out of 20)**
3. **Recency & Confidence (out of 20)**
4. **Tooling & Tech Stack Familiarity (out of 15)**
5. **Certifications & Assessments (out of 15)**

---

#### Core Skill Match (out of 30)

| Match Quality        | Description                                                     | Score     |
|----------------------|------------------------------------------------------------------|-----------|
| Perfect Match         | All required skills matched with high usage confidence          | 25–30     |
| Good Match            | Most required skills matched; confident in usage                | 18–24     |
| Partial Match         | Few key skills missing, some unfamiliar tools                   | 10–17     |
| Poor Match            | Major required skills missing or no evidence of usage           | ≤9        |

- Extracted via NLP keyword + semantic matching
- Weightage based on skill criticality to role

---

#### Bonus/Optional Skill Match (out of 20)

These are not mandatory, but help distinguish strong candidates from others.

| Bonus Match Level     | Examples                              | Score     |
|------------------------|---------------------------------------|-----------|
| Extensive Extras       | 3–4 strong bonus skills                | 16–20     |
| Moderate Extras        | 1–2 bonus skills                      | 10–15     |
| Minimal Extras         | Only basic extras                     | 5–9       |
| None                   | No extras                             | 0–4       |

- Includes soft skills (e.g., leadership, agile methodologies) and non-critical tech

---

#### Recency & Confidence (out of 20)

| Confidence/Recency Level | Interpretation                                     | Score     |
|---------------------------|----------------------------------------------------|-----------|
| High & Recent             | Used in last 1 year + in multiple projects         | 17–20     |
| Moderate                  | Used 1–2 years ago, or only in one context         | 11–16     |
| Low                       | Mentioned in resume, but unclear experience level  | 5–10      |
| Outdated or Unverified    | Skill listed but never actively used               | ≤4        |

- Assessed via context parsing, frequency, and chronology from experience descriptions

---

#### Tooling & Tech Stack Familiarity (out of 15)

| Familiarity Level     | Description                                      | Score     |
|------------------------|--------------------------------------------------|-----------|
| Deeply Familiar        | Tools/frameworks used end-to-end (e.g., Git, Docker, CI/CD) | 12–15     |
| Moderate Familiarity   | Tools mentioned and used in minor capacity       | 7–11      |
| Basic Mention Only     | Tools listed, but usage unclear or outdated      | 3–6       |
| None                   | No tooling evidence                              | 0–2       |

- Special attention to domain-relevant tools (e.g., TensorFlow for ML, Figma for UI/UX)

---

#### Certifications & Assessments (out of 15)

| Certification Value    | Description                                        | Score     |
|-------------------------|----------------------------------------------------|-----------|
| Top-tier + Verified     | Google, Microsoft, Coursera (capstone), Kaggle   | 12–15     |
| Recognized Platforms    | Udemy, LinkedIn, Codeforces, internal MOOC        | 8–11      |
| Entry-level/Incomplete  | Basic or generic quizzes                          | 4–7       |
| No Certifications       | None provided                                     | 0–3       |

- Additional weight if directly related to the job description
- Public portfolios and GitHub profiles may supplement

---

### Final Skills Score Calculation:

```python
skills_score = core_match + bonus_match + recency_confidence + tools_familiarity + certification_score
```
### Score Normalization & Visualization

After calculating the raw scores for each coordinate (X, Y, Z), it is essential to normalize these values to a uniform scale for fair comparison and visualization.

---

#### 1. **Normalization Process**

Normalize each coordinate score to a scale of **0 to 100** using min-max scaling:
  
```python
def normalize_score(raw_score, min_score, max_score):
    if max_score == min_score:
        return 0  # Avoid division by zero if all scores are equal
    return ((raw_score - min_score) / (max_score - min_score)) * 100
```
#### 2. **3D Coordinate Tuple**
Each candidate is represented as a 3D point:
```python
candidate_coords = (x_normalized, y_normalized, z_normalized)
```

#### 3. **Visualization Techniques**
Use `matplotlib` for interactive 3D scatter plots.
```python
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

# candidates_data: list of tuples (x, y, z)
x_vals = [c[0] for c in candidates_data]
y_vals = [c[1] for c in candidates_data]
z_vals = [c[2] for c in candidates_data]

ax.scatter(x_vals, y_vals, z_vals, c='blue', marker='o')

ax.set_xlabel('Education Score (X)')
ax.set_ylabel('Experience Score (Y)')
ax.set_zlabel('Skills Score (Z)')

plt.title('Candidate 3D Coordinate Visualization')
plt.show()
```
#### 4. **Use Cases of Visualization**
- Recruiters can quickly identify clusters of top candidates.
- Candidates can visualize their relative strengths and weaknesses.
- Hiring managers can adjust filtering thresholds dynamically.
- Automated dashboards can highlight outliers and potential matches.

### Candidate Dashboard

The Candidate Dashboard provides personalized insights into a candidate's profile based on their 3D coordinate scores (Education, Experience, Skills). It enables candidates to track their performance and receive actionable feedback for improvement.

---

#### Features:

- **View Personal 3D Coordinates and Scores**
  - Displays normalized X, Y, Z scores visually.
  - Interactive 3D graph showing candidate’s relative position among peers.

- **Axis-wise Improvement Feedback**
  - Highlights the weakest dimension(s) in the candidate’s profile.
  - Provides detailed textual suggestions on how to improve each axis.

- **AI-Generated Personalized Tips**
  - Uses an LLM or rule-based system to generate actionable advice.
  - Example:
    > “To increase your Skills score (Z-axis), learn Django and deploy a project demonstrating your abilities.”

---

#### Example UI Components (React + Plotly)

```jsx
import React from 'react';
import Plot from 'react-plotly.js';

const CandidateDashboard = ({ candidateData }) => {
  const { xScore, yScore, zScore, candidateId } = candidateData;

  const feedback = () => {
    if (zScore < 50) {
      return "To increase your Skills score (Z-axis), learn Django and deploy a project demonstrating your abilities.";
    }
    // Add more conditions as needed
    return "Keep up the great work!";
  };

  return (
    <div>
      <h2>Candidate Dashboard - {candidateId}</h2>
      <Plot
        data={[
          {
            x: [xScore],
            y: [yScore],
            z: [zScore],
            type: 'scatter3d',
            mode: 'markers',
            marker: { size: 8, color: 'red' },
          },
        ]}
        layout={{
          width: 600,
          height: 400,
          title: 'Your 3D Coordinate Profile',
          scene: {
            xaxis: { title: 'Education (X)' },
            yaxis: { title: 'Experience (Y)' },
            zaxis: { title: 'Skills (Z)' },
          },
        }}
      />
      <div>
        <h3>Feedback</h3>
        <p>{feedback()}</p>
      </div>
    </div>
  );
};

export default CandidateDashboard;
```
This dashboard can be extended to include:

- **Historical score tracking**  
  Track candidate progress over time with graphs and timelines.

- **Skill-building resource links**  
  Provide curated courses, tutorials, and projects tailored to improvement areas.

- **Integration with the AI feedback engine for dynamic tips**  
  Generate personalized, context-aware advice using AI/LLM models to guide candidate growth.

  ---

## Tech Stack Summary

| Category            | Tools                                     | Purpose                                   |
|---------------------|---------------------------------------------------------|--------------------------------------------------|
| Programming Language | Python 3.10                                             | Backend logic, AI/ML, API development             |
| Backend Frameworks   | Flask / FastAPI                                         | REST API development, lightweight, performant     |
| NLP & AI Libraries   | spaCy, nltk, gensim, Transformers / LLM APIs (optional) | Text processing, NER, semantic similarity, AI feedback |
| Data Handling       | pandas, numpy                                           | Data manipulation, numerical computations          |
| Machine Learning    | scikit-learn                                           | Clustering (K-Means), ranking algorithms           |
| Database            | PostgreSQL + pgvector extension                         | Storage of candidate data, vector similarity search|
| Cloud Storage       | AWS S3                                                 | Resume file uploads                                |
| Frontend (Optional) | React.js, Plotly.js / matplotlib                        | Dashboard UI, 3D visualization                      |
| DevOps & Infra      | Docker, CI/CD pipelines, AWS / GCP / Azure              | Containerization, deployment, scalable hosting     |
| Miscellaneous       | Regular Expressions, Git/GitHub                          | Resume parsing, source control                      |

 
