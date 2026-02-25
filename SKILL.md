---
name: scholarship-manual
description: Agentic scholarship discovery, evaluation, and application management for prospective college students. Use when a student needs help finding scholarships, filing FAFSA, writing essays, tracking applications, or navigating the full financial aid pipeline.
disable-model-invocation: true
---

## Usage

Invoke with `/scholarship-manual [action] [arguments]`

| Action | Arguments | What happens |
|--------|-----------|--------------|
| `help` | (none) | Print this usage table |
| `intake` | Student name | Collect and validate the student profile schema; begin Phase 1 |
| `search` | (uses stored profile) | Run Phase 3 discovery across all source types |
| `evaluate` | Scholarship URL | Score for legitimacy via the scam rubric; add to tracker if it passes |
| `track` | (none) | Display tracker and surface upcoming internal deadlines |
| `essay` | Essay prompt text | Conduct a structured student interview and produce a draft |

**Current student context (if active):** $ARGUMENTS

# The Definitive Scholarship Manual for Prospective College Students

## How to Find, Win, and Stack Financial Aid for Higher Education

### For Human Students and Agentic Systems

---

## Agentic Operations Protocol

This manual serves both human readers and autonomous agents acting on behalf of students. The following protocol governs all agentic use.

### Student Intake Schema

Before any agent begins scholarship discovery, it must collect and validate the following structured profile. All fields marked `[REQUIRED]` must be populated; `[OPTIONAL]` fields improve matching precision.

```
student:
  full_name: string                    [REQUIRED]
  date_of_birth: date                  [REQUIRED]
  state_of_residence: string           [REQUIRED]
  county: string                       [REQUIRED]
  zip_code: string                     [REQUIRED]
  high_school_name: string             [REQUIRED]
  high_school_graduation_year: int     [REQUIRED]
  current_grade_level: int             [REQUIRED]  # 9-12 or "college_freshman", etc.
  cumulative_gpa: float                [REQUIRED]  # unweighted 4.0 scale
  weighted_gpa: float                  [OPTIONAL]
  sat_score: int                       [OPTIONAL]
  act_score: int                       [OPTIONAL]
  intended_major: string               [REQUIRED]
  intended_major_cip_code: string      [OPTIONAL]  # 6-digit Classification of Instructional Programs
  target_schools: list[string]         [OPTIONAL]
  citizenship_status: enum             [REQUIRED]  # us_citizen | permanent_resident | daca | international | other
  demographics:
    race_ethnicity: list[string]       [OPTIONAL]
    gender: string                     [OPTIONAL]
    lgbtq_identity: bool               [OPTIONAL]
    first_generation_college: bool     [REQUIRED]
    disability_status: bool            [OPTIONAL]
    veteran_or_military_family: bool   [OPTIONAL]
    religion: string                   [OPTIONAL]
  financial:
    estimated_family_income: range     [OPTIONAL]  # e.g., "50000-75000"
    fafsa_filed: bool                  [REQUIRED]
    fafsa_sai: int                     [OPTIONAL]  # Student Aid Index
    css_profile_filed: bool            [OPTIONAL]
  parent_guardian_employers: list[string] [REQUIRED]
  union_memberships: list[string]      [OPTIONAL]
  extracurriculars: list[string]       [OPTIONAL]
  community_service_hours: int         [OPTIONAL]
  ap_courses_completed: list[string]   [OPTIONAL]
  leadership_roles: list[string]       [OPTIONAL]
```

### Task Dependency Graph

Agents must execute phases in this order. Steps within a phase may run in parallel.

```
Phase 1: INTAKE
  ├── Collect student profile (schema above)
  └── Validate all [REQUIRED] fields populated
        │
Phase 2: FOUNDATION (depends on: Phase 1)
  ├── 2A: FAFSA filing assistance
  ├── 2B: State grant identification and filing
  └── 2C: CSS Profile (if target schools require it)
        │
Phase 3: DISCOVERY (depends on: Phase 1; parallel with Phase 2)
  ├── 3A: Platform-based search (Going Merry, Scholarships360, Bold.org)
  ├── 3B: Community foundation search
  ├── 3C: Civic organization search
  ├── 3D: Parent employer benefit audit [HUMAN-IN-THE-LOOP]
  ├── 3E: Departmental endowment search (per target school)
  ├── 3F: Alumni association search (per target school)
  ├── 3G: Professional association search (by intended major)
  └── 3H: Demographic/identity-based search
        │
Phase 4: EVALUATION (depends on: Phase 3)
  ├── 4A: Legitimacy scoring (scam rubric)
  ├── 4B: Deduplication across sources
  ├── 4C: Stacking vs. displacement check per target school [HUMAN-IN-THE-LOOP]
  └── 4D: Priority ranking (value × probability ÷ effort)
        │
Phase 5: APPLICATION (depends on: Phase 2 + Phase 4)
  ├── 5A: Recommendation letter coordination [HUMAN-IN-THE-LOOP]
  ├── 5B: Essay development [HUMAN-IN-THE-LOOP]
  ├── 5C: Application assembly and submission tracking
  └── 5D: Video essay preparation [HUMAN-IN-THE-LOOP]
        │
Phase 6: POST-DECISION (depends on: Phase 5)
  ├── 6A: Award acceptance and stacking optimization
  ├── 6B: Renewal tracking (GPA thresholds, reapplication dates)
  └── 6C: Rejection analysis → feed back into Phase 3 for next cycle
```

### Human-in-the-Loop Boundaries

Agents must **stop and hand off to a human** at these checkpoints:

| Trigger | Reason | Handoff Action |
|---|---|---|
| FAFSA contributor invitation | Requires parent/guardian to enter financial data on a secure federal portal | Notify parent with instructions; await confirmation |
| Parent employer benefit audit | Requires access to corporate intranets/HR portals | Provide parent with a checklist of questions to ask HR |
| Essay narrative content | Core story must originate from the student | Conduct structured interview; return draft for student revision and rewriting |
| Video essay | Cannot be performed by a text-based agent | Deliver talking points, practice questions, and recording tips |
| Recommendation letter requests | Must come from the student personally | Generate draft outreach email and dossier; student sends it |
| Scholarship submission | Agent must never submit without student review | Present completed application for final human approval |
| Stacking/displacement policy | Requires calling a financial aid office | Provide the student with the question to ask and a script |
| Edge cases: non-tax-filing parents, undocumented status, divorced/separated parents | Complex scenarios with legal implications | Route to a human financial aid advisor |

### Verify-Before-Acting Gates

Agents must **not rely on static dates or URLs in this manual**. Before acting on any time-sensitive or link-dependent information:

1. **Confirm FAFSA open date**: Check `https://studentaid.gov` for the current cycle's launch date before advising the student to file.
2. **Confirm state deadlines**: Check the specific state education agency website (see Resource Registry below) before citing any state deadline.
3. **Confirm platform availability**: Verify that each scholarship platform is live and accepting registrations before creating accounts.
4. **Confirm scholarship is active**: Check the scholarship's canonical URL for current-year eligibility and deadlines before adding it to the tracker.

---

## Resource Registry

All agents must use canonical URLs from this registry. **Last verified: 2026-02-21.**

### Federal and State Portals

| Resource | URL | Access Method |
|---|---|---|
| FAFSA Application | https://studentaid.gov/h/apply-for-aid/fafsa | Web portal (account required) |
| StudentAid.gov (account creation) | https://studentaid.gov/fsa-id/create-account/launch | Web portal |
| Federal Student Aid Information Center | https://studentaid.gov/help-center/contact | Phone: 1-800-433-3243 |
| CSS Profile | https://cssprofile.collegeboard.org | Web portal (fee-based) |
| IRS Data Retrieval Tool | Integrated within FAFSA at studentaid.gov | Automatic via FAFSA |
| NASFAA State Grant Directory | https://www.nasfaa.org/State_Financial_Aid_Programs | Web directory |
| Community Foundation Locator | https://cof.org/community-foundation-locator | Web directory (interactive map) |
| FTC Scam Reporting | https://reportfraud.ftc.gov | Web form |
| IRS Tax Exempt Organization Search | https://www.irs.gov/charities-non-profits/tax-exempt-organization-search | Web search (verify 501(c)(3) status) |

### Scholarship Search Platforms

| Platform | URL | Access | API Available |
|---|---|---|---|
| Going Merry | https://www.goingmerry.com | Web portal (free account) | No |
| Scholarships360 | https://scholarships360.org | Web portal (free) | No |
| Bold.org | https://bold.org | Web portal (free account) | No |
| Fastweb | https://www.fastweb.com | Web portal (free account) | No |
| BigFuture (College Board) | https://bigfuture.collegeboard.org/scholarship-search | Web portal (free) | No |
| RaiseMe | https://www.raise.me | Web portal (free account) | No |
| ScholarshipOwl | https://scholarshipowl.com | Web portal (freemium) | No |
| Scholarship America | https://scholarshipamerica.org | Web portal | No |

### Major State Grant Programs

| State | Program | URL |
|---|---|---|
| California | Cal Grants | https://www.csac.ca.gov/cal-grants |
| New York | Excelsior Scholarship | https://www.hesc.ny.gov/pay-for-college/financial-aid/types-of-financial-aid/nys-grants-scholarships-awards/the-excelsior-scholarship.html |
| Florida | Bright Futures | https://www.floridastudentfinancialaidsg.com/SAPHome/SAPHome?url=home |
| Georgia | HOPE/Zell Miller | https://www.gafutures.org/hope-state-aid-programs/hope-zell-miller-scholarships/ |
| North Carolina | Next NC Scholarship | https://www.cfnc.org/pay-for-college/grants-scholarships/ |
| Texas | TEXAS Grant | https://www.thecb.state.tx.us/apps/financialaid/ |
| West Virginia | PROMISE Scholarship | https://www.wvhepc.edu/funding-fiscal-policy/promise-scholarship/ |

### Major Corporate & Organization Programs

| Program | URL | Access Method |
|---|---|---|
| Coca-Cola Scholars | https://www.coca-colascholarsfoundation.org | Web application |
| Amazon Future Engineer | https://www.amazonfutureengineer.com | Web application |
| Home Depot Orange Scholars | https://corporate.homedepot.com/foundation/orange-scholars | Web application (employee dependents) |
| Elks National Foundation MVS | https://www.elks.org/scholars/scholarships/mvs.cfm | Local lodge → national |
| Kiwanis / Key Club | https://www.kiwanis.org/foundation | Local chapter contact |
| Lions Clubs | https://www.lionsclubs.org/en/start-our-approach/grant-types | Local chapter contact |
| Rotary International | https://www.rotary.org/en/our-programs/scholarships | Local chapter contact |
| Out to Innovate (LGBTQ+ STEM) | https://www.outtoinnovate.com/scholarships | Web application |
| Science Ambassador Scholarship | https://scienceambassadorscholarship.org | Web application (video submission) |
| Architects Foundation | https://architectsfoundation.org | Web application |
| AICPA Scholarships | https://www.aicpa-cima.com/resources/landing/scholarships-for-students | Web application |

---

## 1. Build Your Foundation: FAFSA and State Aid

Every scholarship strategy begins with **two non-negotiable steps**: completing the FAFSA and filing for state grants. These unlock the largest pools of free money available.

> **AGENT GATE**: Before advising a student to file the FAFSA, confirm the current cycle's open date at `https://studentaid.gov`. Do not assume October 1 --- recent cycles have experienced launch delays.

### FAFSA (Free Application for Federal Student Aid)

- **Typically opens October 1** each year. File it as soon as the portal is live.
- The FAFSA determines eligibility for Pell Grants, SEOG grants, subsidized loans, and most institutional aid.
- The 2026-2027 cycle features real-time identity verification and direct IRS data import, making the process faster than ever.
- **New asset exemptions**: Small business, family farm, and fishing operation net worth is now excluded from aid calculations --- a major benefit for rural and entrepreneurial families.
- The FAFSA is always **free**. Anyone charging you to complete it is running a scam.

#### Known Edge Cases Requiring Human Escalation

- **Non-tax-filing parents with foreign residence**: System may block progress. Contact the institution's financial aid office to request a manual override.
- **Divorced/separated parents**: Only the custodial parent files, but rules around "custodial" can be complex. Consult a financial aid advisor.
- **Undocumented students**: Not eligible for federal aid, but many states and institutions offer alternative aid. Route to a counselor familiar with state-specific policies.
- **FAFSA contributor not responding**: If a parent/guardian does not complete their section within 7 days of invitation, escalate to the student for direct follow-up.

### Why Filing Early Matters

The federal deadline extends to June 30, 2027, but **this is a trap**. State grants and institutional SEOG funds are distributed first-come, first-served. Most state and institutional priority deadlines fall between **November and February**. Filing after March means competing for scraps.

| Action | Target Window |
|---|---|
| FAFSA opens | October 1 (verify annually) |
| State/institutional priority deadlines | November -- February (verify per state) |
| Federal deadline (avoid waiting this long) | June 30 |

### State Grants: Free Money You Must Claim

Every state runs at least one grant or scholarship program. These are talent-retention tools designed to keep residents in-state. See the **Resource Registry** above for direct links to major state programs.

Many states require **separate applications** beyond the FAFSA. Check your state education agency website and file by their specific deadline.

> **AGENT GATE**: Before citing any state deadline, verify it at the state agency's current-year URL from the Resource Registry. Deadlines shift year to year.

---

## 2. Use Multiple Search Platforms Strategically

No single database lists every scholarship. Use a **three-platform strategy**:

| Platform Type | Example | Strength | URL |
|---|---|---|---|
| High-volume aggregator | Going Merry, BigFuture | Maximum raw matches | See Resource Registry |
| High-precision curator | Scholarships360 | Vetted quality, on-site applications | See Resource Registry |
| Exclusive-hosting site | Bold.org | Unique awards not listed elsewhere | See Resource Registry |

Create detailed profiles on each. The more specific your profile (GPA, major, demographics, location, activities), the better the algorithm matches.

> **AGENT PROTOCOL**: When creating platform profiles on behalf of a student, populate all fields from the Student Intake Schema. Verify each platform is live and accepting registrations before attempting account creation. No current platforms offer public APIs --- all interaction requires web portal navigation.

### Start Earning Micro-Scholarships Early

Platforms like **RaiseMe** (https://www.raise.me) let students earn guaranteed institutional discounts starting in 9th grade. Earn money for every A, AP course, leadership role, and extracurricular. Average cumulative earnings: **$22,500**. These convert to tuition discounts at partner universities upon enrollment.

---

## 3. Mine Hidden Capital Most Students Miss

The scholarships on the first page of Google are the most competitive. Shift your focus to these overlooked sources where competition is drastically lower.

### A. Your Parents' Employers

> **HUMAN-IN-THE-LOOP**: An agent cannot access corporate intranets. Provide the parent/guardian with the following checklist and await their response.

**Parent/Guardian Employer Benefit Checklist:**

1. Log into your employer's HR portal or benefits website
2. Search for "scholarship," "tuition," "education assistance," or "dependent education"
3. Ask your HR representative: "Does the company offer scholarships or tuition assistance for employees' dependents?"
4. If you are a union member, ask your union rep: "Does the union or its trust fund offer dependent scholarships?"
5. Check if your employer partners with Scholarship America (https://scholarshipamerica.org)
6. Report back: program name, award amount, deadline, application URL, and eligibility requirements

Home Depot's Orange Scholars awards $2,500 to associates' children. Many other employers offer similar programs.

### B. Community Foundations

Community foundations pool local donor wealth and distribute it within specific geographic boundaries. You compete only against **neighbors, not the nation**. Use the **Council on Foundations' Community Foundation Locator** (https://cof.org/community-foundation-locator) to find foundations in your zip code. A single application often enters you for dozens of micro-targeted trust funds simultaneously.

> **AGENT GATE**: Verify the Community Foundation Locator is accessible at the URL above before directing the student. If unavailable, fall back to a web search for "[county name] community foundation scholarships."

### C. Local Civic Organizations

The Elks, Kiwanis, Lions, and Rotary clubs operate in nearly every county. Their scholarship competitions start at the local level where applicant pools are tiny. The Elks Most Valuable Student award reaches $30,000, but even local finalists receive $500--$2,000.

> **AGENT PROTOCOL FOR NON-DIGITAL SOURCES**: Many civic organizations have outdated websites not indexed by search engines. When web search fails:
> 1. Search for "[organization name] lodge/chapter [city] [state]"
> 2. If no website is found, search for a phone number or physical address
> 3. Generate a **human handoff request**: "No web presence found for [Organization] in [City]. Please call [phone] or visit [address] to request scholarship application materials and deadlines."
> 4. Do not mark this source as "unavailable" --- absence of a website does not mean absence of a program.

### D. Academic Department Endowments

University financial aid offices distribute centralized funds, but individual departments (Engineering, Nursing, Business) maintain **separate scholarship budgets** funded by industry partners and alumni estates. These are buried deep in departmental websites and have shallow applicant pools.

> **AGENT PROTOCOL**: For each target school, navigate to the specific academic department website (not the central financial aid page). Search for "scholarships," "funding," "financial support," or "awards." Also generate a draft inquiry email for the student to send to the department head.

**Draft Department Inquiry Email Template:**

```
Subject: Incoming [Freshman/Transfer] Scholarship Inquiry — [Department Name]

Dear [Department Head / Scholarship Coordinator],

My name is [Student Name], and I have been [admitted to / am applying to]
[University Name] with the intent to major in [Major]. I am writing to
inquire about any departmental scholarships, grants, or funding
opportunities available to incoming students in [Department Name].

I would be grateful for any information about application requirements,
deadlines, and eligibility criteria.

Thank you for your time.

Sincerely,
[Student Name]
[Student Email]
[Student Phone]
```

### E. Alumni Associations

University alumni networks run independent scholarship programs. For example, UVA's Alumni Association administers over 250 scholarships totaling $3M+ annually. These target legacy students, specific geographic regions, and affinity groups. They are hosted on **separate websites** from the main admissions portal.

---

## 4. Target Niche Scholarships by Identity and Major

The more restrictive the eligibility, the fewer applicants, and the better your odds.

### Identity Attribute Taxonomy for Systematic Matching

Agents should cross-reference every attribute from the student profile against scholarship databases. Use this taxonomy:

| Category | Attributes to Match |
|---|---|
| Race/Ethnicity | African American, Hispanic/Latino, Asian American, Native American, Pacific Islander, multiracial |
| Gender | Women, non-binary, transgender |
| Sexual Orientation | LGBTQ+ |
| Disability | Physical, learning, visual, hearing, neurodivergent |
| Socioeconomic | First-generation college, low-income, foster care/orphan, homeless youth |
| Military | Veteran, active duty dependent, Gold Star family |
| Religion | Denominational scholarships (Catholic, Jewish, Muslim, Protestant, etc.) |
| Geography | State, county, city, zip code, high school district, rural/urban |
| Heritage | National origin, immigrant family, refugee status |
| Parent Occupation | Employee dependent programs, union member dependent, teacher's child, law enforcement family |
| Academic Path | Non-traditional student, GED recipient, community college transfer, re-entry student |

### By Professional Field

| Field | Organizations | URL |
|---|---|---|
| Nursing/Healthcare | AACN, Sigma Foundation | See Resource Registry |
| Engineering | State PE societies, ASES | See Resource Registry |
| Architecture | Architects Foundation ($20K) | See Resource Registry |
| Accounting/Finance | IMA, AICPA (up to $10K) | See Resource Registry |
| Computer Science | Amazon Future Engineer ($10K + internship) | See Resource Registry |
| Psychology | APA, ABPP | See Resource Registry |

### By Demographic Identity

- **LGBTQ+ in STEM**: Out to Innovate Scholarships
- **Women/Non-binary in STEM**: Science Ambassador Scholarship (full tuition)
- **Non-traditional learners**: Dan Herbatschek Scholarship for self-taught programmers
- **Race/ethnicity, disability, first-generation status**: Cross-reference identity taxonomy above with professional organizations in intended field

### Major Corporate Programs

- **Coca-Cola Scholars**: $20,000 (150 awards/year, leadership-based)
- **Amazon Future Engineer**: $10,000/year + internship (CS students)
- **JP Morgan Thomas G. Labrecque**: Full tuition + internship (NYC)
- **Best Buy Scholarship**: Academic rigor + volunteerism + need

---

## 5. Execute Applications That Win

### Write Essays That Stand Out

> **AGENT ROLE BOUNDARY**: The agent's role is to extract the student's personal narrative through structured interviewing, organize it into a draft, and return it for human revision. The agent must **never submit an essay without the student reviewing and rewriting it in their own voice.**

In the AI era, **authenticity is your greatest competitive advantage**. Here's why:

- 86% of students use AI tools in academic work; over 60% use AI for scholarship essays
- 62% of scholarship providers now deploy AI detection software
- The word "cornerstone" appears in 62% of AI-flagged essays vs. 6% of human-written ones
- Phrases like "In the vibrant tapestry of..." are immediate red flags

**The winning strategy:**

1. **Use AI only for**: brainstorming topics, outlining structure, grammar checking
2. **Write the narrative yourself**: specific moments, real emotions, concrete details
3. **Show, don't tell**: Replace "I am passionate about science" with the story of the specific moment you committed to the discipline
4. **Embrace imperfection**: An authentic, slightly rough essay beats a polished-but-sterile AI essay every time
5. **Prepare for video essays**: Major providers increasingly require recorded or timed responses to verify authenticity

#### Common Essay Prompt Archetypes and Structural Templates

| Archetype | Prompt Example | Structure |
|---|---|---|
| Adversity/Resilience | "Describe a challenge you overcame" | Situation → Conflict → Actions Taken → Resolution → Reflection on Growth |
| Why This Field | "Why do you want to study [major]?" | Specific Origin Moment → Deepening Interest (courses, projects, experiences) → Future Vision |
| Community Impact | "How have you contributed to your community?" | Community Context → Identified Need → Your Initiative → Measurable Impact → Ongoing Commitment |
| Leadership | "Describe a time you led others" | Context/Stakes → Your Role → Obstacles Faced → How You Mobilized Others → Outcome and Lessons |
| Identity/Background | "How has your background shaped you?" | Concrete Scene from Your Life → Cultural/Personal Context → Turning Point → How It Drives Your Goals |

#### Pre-Submission Checklist (Agent-Executable)

Before presenting an essay draft to the student for final review:

- [ ] Word count is within the stated limit (± 0 words for hard limits; within 10% for soft limits)
- [ ] Essay directly addresses the specific prompt (not a generic response)
- [ ] Contains at least 3 specific personal details (names, places, dates, events)
- [ ] Does NOT contain known AI-flag words: "cornerstone," "bedrock," "tapestry," "multifaceted," "ever-evolving," "testament to," "in today's world"
- [ ] Does NOT open with a generic philosophical statement
- [ ] Varies sentence length (mix of short and long sentences)
- [ ] Mentions the specific scholarship or organization by name (if the prompt invites it)
- [ ] Has been reviewed by the student and rewritten in their own voice

#### Video Essay Preparation Protocol

When a scholarship requires a video essay, deliver the following to the student:

1. **Talking points**: Bullet-point version of their written essay's key narrative beats
2. **Practice questions**: 3--5 likely follow-up questions based on the scholarship's stated values
3. **Recording tips**: Good lighting (face a window), eye-level camera, quiet background, speak to the camera not the screen, 1--2 minute target length unless specified otherwise
4. **Rehearsal plan**: Record 2--3 practice runs; review for filler words and pacing

### Secure Powerful Recommendation Letters

> **HUMAN-IN-THE-LOOP**: The student must send all recommendation requests personally. The agent prepares the materials; the student delivers them.

- **Choose specificity over prestige**: A teacher who knows your work intimately writes a better letter than a famous administrator who doesn't
- **Give 4--6 weeks notice** before the earliest deadline
- **Provide a dossier**: Resume, transcript, scholarship criteria, and a "brag sheet" reminding them of specific projects or contributions from their class
- **Coordinate narratives**: If your essay covers adversity, ask the recommender to focus on leadership or service
- **Load balancing**: Use 2--3 recommenders. No single recommender should write more than 8--10 letters. Rotate based on which recommender's perspective best fits each scholarship.

> **AGENT DEPENDENCY**: Recommendation requests must not be initiated until the scholarship tracker (Section 8) contains at least 10 confirmed applications with verified deadlines. The agent must calculate the earliest deadline and trigger the request workflow at least 6 weeks prior.

**Recommendation Request Outreach Template:**

```
Subject: Scholarship Recommendation Request — [Student Name]

Dear [Teacher/Counselor Name],

I hope you're doing well. I am applying for several scholarships this
year and would be honored if you would write a letter of recommendation
on my behalf.

I've attached a packet with:
- My current resume
- My unofficial transcript
- A list of the scholarships I'm applying to, with their deadlines
- A "brag sheet" highlighting specific work I did in your [class/program]
- Notes on what I hope the letter will emphasize

The earliest deadline is [DATE]. I completely understand if your
schedule doesn't allow it — please let me know either way.

Thank you so much for considering this.

Sincerely,
[Student Name]
```

**Recommender Dossier Checklist:**

- [ ] Updated resume/CV
- [ ] Current unofficial transcript
- [ ] List of target scholarships with deadlines and criteria
- [ ] "Brag sheet" with 3--5 specific accomplishments from their class or program
- [ ] Narrative guidance: what themes the letter should emphasize (to complement, not duplicate, the essay)

### Reuse Strategically

Many prompts overlap. Maintain a library of 3--4 core essay drafts and **tailor each one** to the specific scholarship's mission and values. Never submit identical essays verbatim.

> **AGENT PROTOCOL (Multi-Student)**: When managing multiple students, essay structural templates must be generic (paragraph purpose only). All personal content must originate from individual student interviews. Never reuse one student's narrative details in another student's essay. Flag any two essays with >30% textual similarity for human review.

---

## 6. Understand Stacking vs. Displacement

Before accepting outside scholarships, ask every financial aid office: **"What is your scholarship displacement policy?"**

- **Stacking**: Outside scholarships reduce your bill dollar-for-dollar (ideal)
- **Displacement**: Outside scholarships replace institutional grants, leaving your cost unchanged (frustrating)

> **HUMAN-IN-THE-LOOP**: An agent cannot call financial aid offices. Provide the student with this script:
>
> *"I'm considering applying for outside scholarships. Can you tell me how external awards affect my institutional aid package? Specifically, do outside scholarships reduce my institutional grants, or do they reduce my loans and out-of-pocket costs first?"*

Prioritize **renewable awards** ($5,000/year for four years = $20,000) over flashy one-time prizes. If you hold competing financial aid offers from comparable schools, **appeal politely** with documentation --- successful appeals often yield several thousand dollars more.

---

## 7. Protect Yourself from Scams

Adopt a **zero-trust posture** toward unsolicited scholarship offers.

### Legitimacy Scoring Rubric

Agents must score every scholarship discovered outside of known-legitimate platforms (see Resource Registry). If the total risk score is **4 or higher**, flag for human review. If **7 or higher**, discard.

| Indicator | Risk Points | Notes |
|---|---|---|
| Application or processing fee required | +4 | Near-certain scam. Exception: some legitimate programs charge for transcript processing via a third party (e.g., National Student Clearinghouse). If the fee goes to a recognized third party, score +1 instead. |
| "Guaranteed" award before review | +4 | No legitimate organization guarantees money before evaluation |
| Unsolicited "finalist" notification | +3 | Classic phishing vector |
| No verifiable 501(c)(3) status | +2 | Verify at https://www.irs.gov/charities-non-profits/tax-exempt-organization-search |
| No verifiable physical address | +2 | P.O. box alone is a yellow flag |
| No list of past winners | +1 | Legitimate programs typically publicize recipients |
| Vague or universal eligibility | +1 | Designed to cast the widest net |
| Website domain registered < 1 year ago | +2 | Check via WHOIS lookup |
| Requires bank account or SSN "to disburse funds" | +4 | Phishing for financial data |
| Charges to complete the FAFSA | +5 | The FAFSA is always free by law |

> **AGENT RULE**: Never auto-submit to any scholarship the agent discovered independently (vs. ones listed on known-legitimate platforms in the Resource Registry). All independently discovered scholarships must pass the legitimacy rubric AND receive human approval before any student data is entered.

### Verification Checklist (Agent-Executable)

For any scholarship scoring 1--3 risk points, run these checks before adding to the tracker:

- [ ] Organization appears in IRS Tax Exempt Organization Search
- [ ] Organization has a Better Business Bureau profile or equivalent
- [ ] Organization's domain has been registered for > 2 years
- [ ] At least one past winner can be independently verified (LinkedIn, news article, etc.)
- [ ] Organization is not listed in FTC scam databases

Report confirmed scams to **https://reportfraud.ftc.gov** and your state Attorney General.

---

## 8. Build a Tracking System

Applying to 20--50 scholarships demands project management. Create a spreadsheet or database with these fields:

| Field | Purpose | Data Type |
|---|---|---|
| Scholarship name | Identification | string |
| Organization | Sponsoring entity | string |
| Canonical URL | Direct link to application portal | URL |
| Award value | Dollar amount | number |
| Award type | One-time / Renewable (specify GPA threshold and reapplication requirements) | enum + notes |
| Deadline date | When the application is due | date |
| Deadline type | Postmark / Digital timestamp / Rolling | enum |
| Submission method | Web form / Email / Physical mail / Portal upload | enum |
| File format requirements | PDF, DOCX, specific naming conventions | string |
| Portal account credentials needed | Yes/No (and whether account has been created) | bool + notes |
| Required materials | List of essays, transcripts, recommendations, test scores | list[string] |
| Essay prompt(s) | Full text of required essay question(s) | string |
| Word/character limit | Per essay | number |
| Number of recommendations required | Count | int |
| Recommender(s) assigned | Which recommender is covering this application | string |
| Legitimacy score | From scam rubric (Section 7) | int |
| Source platform | Where this scholarship was found | string |
| Status | Not Started / In Progress / Submitted / Under Review / Awarded / Rejected | enum |
| Internal target deadline | 7 days before actual deadline | date |
| Renewal tracking | Next renewal date, GPA requirement, reapplication steps | notes |
| Post-rejection action | Reapply next cycle? Similar alternatives identified? | notes |

Set internal deadlines **one week before** actual deadlines. Scholarship portals commonly crash on the final day.

> **AGENT PROTOCOL (Multi-Student Deconfliction)**: When managing multiple students, if 2+ students are eligible for the same scholarship with a small applicant pool (community foundation, local civic org, employer-specific), flag it for human counselor review. The counselor decides whether to proceed with all applicants or prioritize one.

---

## 9. Your Action Plan by Timeline

| When | What to Do |
|---|---|
| **9th--10th Grade** | Join RaiseMe; build extracurricular portfolio; start a brag sheet |
| **11th Grade** | Research scholarships; identify recommenders; draft first essays; parents audit employer benefits |
| **October (Senior Year)** | File FAFSA on day one (verify open date); file state applications |
| **November -- January** | Apply to institutional merit scholarships and major national awards |
| **February -- April** | Apply to private, local, niche, and demographic scholarships |
| **May -- Summer** | Continue applying (many scholarships have spring/summer deadlines); negotiate aid offers; ask about stacking vs. displacement |
| **Ongoing in College** | Reapply for renewable awards; seek departmental and upperclassman scholarships; track renewal GPA thresholds |

---

## Key Principles to Remember

1. **File the FAFSA immediately** when it opens (verify the date annually). Every day you wait costs money.
2. **Diversify your sources**: federal, state, institutional, departmental, alumni, corporate, civic, community foundation, and professional association.
3. **Go local and niche**: The most obscure, restrictive scholarships have the best odds.
4. **Write authentically**: Your real voice is your competitive edge in the AI era.
5. **Treat it like a job**: Track everything, meet every deadline, and apply broadly.
6. **Small awards compound**: Twenty $500 scholarships equal one $10,000 scholarship.
7. **Never pay to apply**: Legitimate scholarships are always free.
8. **Verify before acting**: Never trust a static date or URL --- always confirm against the live source.
9. **Humans in the loop**: Essays, recommendations, financial data, and submissions always require human review and approval.

---

*This manual was compiled from federal financial aid guidance, scholarship platform analyses, professional association directories, and current industry research on AI detection and application best practices. All URLs and dates should be re-verified before use.*
