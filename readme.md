# Atishaya Jain

<div align="left">
  <p>
    <strong>Quantitative Researcher | Developer | Software Engineer</strong><br>
    Specializing in Mathematical Optimization, Systematic Trading Systems, and High-Performance Backend Infrastructure.
  </p>
  <p>
    <a href="https://www.linkedin.com/in/atishayj2202/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=flat-square&logo=linkedin&logoColor=white" alt="LinkedIn"></a>
    <a href="https://github.com/atishayj2202"><img src="https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white" alt="GitHub"></a>
    <a href="mailto:atishayj.dev@gmail.com"><img src="https://img.shields.io/badge/Email-D14836?style=flat-square&logo=gmail&logoColor=white" alt="Email"></a>
    <a href="Resumes/Atishay_Jain_Resume%20Sep2025.pdf"><img src="https://img.shields.io/badge/Resume-PDF-red?style=flat-square&logo=adobe-acrobat&logoColor=white" alt="Resume"></a>
  </p>
</div>

---

## 📍 Milestone Corner

* ⚙️ **Current Working**:
  * Deploying ML market-making paper trading environment.
  * Engineering telemetry dashboards for model observability.
  * Actively seeking Systematic Trading & Quant roles.
* 🎯 **Future Aims**:
  * Secure global Quantitative Trading/Research internships.
  * Transition to full-time Quantitative Researcher roles.
* 🏆 **Past Milestones**:
  * First-author portfolio optimization research paper.
  * Four software and data engineering internships.
  * Deployed WhatsApp chatbot (1,000+ daily users).
  * NSUT Delhi B.Tech CS & Data Science.
  * JEE Advanced Rank: 10,730 (Top 1%).

---

## 🔬 Research & Publications

<details>
<summary><b>Adaptive Opposition-Based Learning Symbiotic Organisms Search (AOBL-SOS) for Portfolio Optimization</b> (First Author | Manuscript in Prep, June 2026)</summary>

* **The Challenge**: Classical portfolio models (e.g., Markowitz's mean-variance) exhibit extreme sensitivity to estimation errors and fail in *non-convex, highly constrained* search spaces. Swarm intelligence solvers like Symbiotic Organisms Search (SOS) bypass gradients but suffer from **premature stagnation** and **simplex constraint collapse** under classic Opposition-Based Learning (OBL) operators ($\bar{w} = 1 - w$, which collapses allocations to a uniform $1/N$ vector upon normalization).
* **The Innovation**: 
  1. Developed **Rank-Reversal Simplex Opposition**: A mathematical permutation operator mapping weights directly in rank-space:
     $$w^{op}_{\pi(i)} = w_{\pi(N - i + 1)} \quad \forall i \in \{1, 2, \dots, N\}$$
     This guarantees asset allocations respect position caps ($w_i \le w_{\text{max}}$) and simplex boundaries ($\sum w_i = 1$) *natively*, eliminating destructive normalization passes.
  2. Implemented an **Adaptive Stagnation Trigger** that conditionally injects quasi-opposition vectors when swarm diversity falls.
* **Results (2023-2025 Out-of-Sample Portfolio Optimization on 179 S&P 500 Stocks)**:
  * Annualized Return: ***32.31%*** (vs. SOS: *27.30%* | Equal-Weight: *15.50%*)
  * Sharpe Ratio: ***1.68*** (vs. SOS: *1.46* | Equal-Weight: *0.88*)
  * Sortino Ratio: ***2.48*** (vs. SOS: *2.23* | Equal-Weight: *1.36*)
  * Statistical dominance confirmed via a *Wilcoxon signed-rank test* on daily returns ($Z = 4.82, p < 0.0001$).

</details>

---

## 🚀 Key Projects

<details>
<summary><b>1. AlphaCore: Zero-Gap Academic Replication & Alpha Backtesting Engine</b></summary>

A high-fidelity backtesting framework implementing mathematical models from 8 major academic finance papers and generating a custom champion hybrid strategy.
* **Academic Replication**: Coded models utilizing **GARCH(1,1)** risk-scaling (Daniel & Moskowitz), **nested Hierarchical HMMs** for regime-detection (Oelschläger & Adam), **Bayesian Online Changepoint Detection (BOCPD)** (Adams & MacKay), and **Fractional Integration Filters** for rough volatility modeling ($H = 0.1$, Gatheral et al.).
* **Champion Hybrid Strategy (B-VTMS)**: Fuses cross-sectional momentum, volatility risk-targeting, and real-time Bayesian changepoint halts to flatten exposure before regime-shift drawdowns.
* **Backtesting Rigor**: Enforced a rigorous **walk-forward validation loop** with a 50-day rolling recalibration on a 500-day window, penalizing high turnover with a realistic **5 bps transaction fee**.
* **Out-of-Sample Results (Strictly OOS, Days 1000 - 1460)**:
  
  | Strategy | Universe | WF Sharpe | WF Ann. Return | Verdict |
  | :--- | :---: | :---: | :---: | :---: |
  | **Paper 3: Volatility Momentum** | Top 4 Majors | ***1.46*** | ***+124.75%*** | **RUN** |
  | **Paper 7: Avoiding Crashes** | Top 4 Majors | ***1.64*** | ***+105.08%*** | **RUN** |
  | **Paper 9: B-VTMS Champion Hybrid** | Top 4 Majors | ***2.16*** | ***+40.14%*** | **RUN** |

</details>

<details>
<summary><b>2. Live BTC Options Trading & Arbitrage Systems</b></summary>

Production-ready quantitative trading pipelines for digital asset exchanges.
* **BTC Options System**: Deployed a live covered call options trading system on Binance using options pricing theory (**Black-Scholes**, dynamic delta hedging, and implied volatility surface construction) to extract premium.
* **Execution & Ingest Pipeline**: Engineered a real-time price-feed and signal-processing pipeline over Binance WebSockets, achieving *sub-second execution latency* with automated position execution and Monte Carlo-derived tail risk bounds.
* **Crypto Arbitrage Bots**: Designed and backtested *Triangular Arbitrage*, *Mean Reversion*, and *Skewed Spread* setups on exchange APIs, optimizing socket buffer parameters and memory usage to minimize latency slippage.

</details>

<details>
<summary><b>3. WhatsApp Attendance Bot (Scale & Backend Infrastructure)</b></summary>

An automated high-traffic student chatbot service showcasing robust backend engineering under high concurrency.
* **High-Concurrency Scale**: Serves **1,000+ Daily Average Users** and over 3,000+ unique users, managing concurrent request spikes of up to 500 users.
* **System Architecture**: Built using a Python `FastAPI` server, webhook routing via Meta's WhatsApp Business API, and containerized deployment on auto-scaled **Azure Container Instances**.
* **Bypass Engineering**: Developed an OCR-powered bypass pipeline to handle CAPTCHA verification on the academic portal, enabling automated attendance extraction.

</details>

---

## 💼 Experience

<details>
<summary><b>PriceWaterhouseCoopers (PwC) India | Data Analyst Intern</b> (June 2026 – Present)</summary>

* Architected an end-to-end automated **data discrepancy detection engine** in Python and Streamlit, implementing statistical outlier detection, casing normalization, and unit conversion algorithms across high-volume structured client datasets.
* Leveraged **NLP embeddings** for semantic category grouping to resolve word duplicates (e.g. "SDE" vs "Software Developer"), decreasing manual data sanitization overhead.
* Integrated database synchronization with **SAP Datasphere** for client deployments, consolidating customer accounts recorded across separate warehouse divisions.

</details>

<details>
<summary><b>Inhouse | Backend Engineer</b> (May 2025 – July 2025)</summary>

* Architected an **OCR-driven fallback layer** within the document extraction pipeline, recovering **~15% of failed document-processing requests** caused by non-standard PDF/DOC formats.
* Programmed an automated Markdown-to-Google Docs translation script using Google Workspace APIs, streamlining internal contract generation workflows and **reducing document creation time by ~80%**.
* Developed robust REST API endpoints using `FastAPI`, maintaining comprehensive unit test coverage **above 95%** and resolving high-severity production bugs under strict SLA bounds.

</details>

<details>
<summary><b>Jupiter | Data Engineer Intern</b> (May 2024 – July 2024)</summary>

* Engineered automated ETL pipelines utilizing **GCP Cloud Scheduler** and containerized jobs on **Cloud Run**, eliminating 3 hours/day of manual workflows and achieving a **90%+ reduction in pipeline failure rates**.
* Constructed a real-time Slack alerting system for key business KPIs (customer acquisition, churn rate metrics) to improve corporate stakeholder monitoring.
* Built interactive business intelligence dashboards using `Explo` and `Google Looker Studio` by querying **BigQuery** databases; integrated continuous deployment pipelines via **Cloud Build**.

</details>

<details>
<summary><b>InHouse (InHouse.so) | Back End Developer</b> (December 2023 – January 2024)</summary>

* Developed performant server endpoints and unit tests utilizing Python's `FastAPI` framework, refactoring legacy code and optimizing execution latency.
* Managed SQL database schemas, writing database migrations, and implemented a robust CI/CD pipeline using **Cloud Build** to automate migrations on **CockroachDB** and deployments on **Google Cloud Run**.
* Integrated automated testing suites inside **GitHub Actions** workflows to enforce code style formatting and linting compliance on pull requests.
* Executed the migration of the application's authentication system from Firebase Authentication to **Google Identity Platform** to secure customer data access.
* Programmed API telemetry logging middleware to track endpoint response times, improving production visibility.

</details>

---

## 🎓 Education

* 🏫 **Netaji Subhas Institute of Technology (NSUT), Delhi** (Now Netaji Subhas University of Technology)
  * **Bachelor of Technology (B.Tech) in Computer Science & Data Science (CSDS)** (August 2023 – May 2027)
  * **CGPA**: *7.0/10*
* 🏫 **GD Goenka Public School, East Delhi** (April 2012 – April 2023)
  * **CBSE Class XII (Senior Secondary)**: ***86.2%***
  * **CBSE Class X (Secondary)**: ***88.2%***
* 🏆 **Competitive Entrance Examination Milestones**:
  * **JEE Advanced Rank: 10,730** (Top 1% nationally out of ~200k selected candidates).
  * **JEE Mains Rank: 13,334** (Top 1.2% out of ~1.2M candidates).

---

## 🛠️ Technical Skills

* **Mathematics & Optimization**: Probability Theory, Stochastic Calculus, Swarm Intelligence, Portfolio Optimization, Time Series Analysis, Statistical Inference.
* **Languages**: Python (Expert/Production), C++ (Beginner/Intermediate), JavaScript, SQL.
* **Quant & ML Libraries**: NumPy, pandas, SciPy, statsmodels, scikit-learn, QuantConnect, backtrader, vectorbt, pyfolio.
* **Cloud & Databases**: GCP (Cloud Run, Build, Scheduler), Azure, Docker, PostgreSQL, BigQuery, Redis, CockroachDB, MongoDB.
* **CI/CD & Tools**: Git, GitHub Actions, Flyway, Streamlit.

---
<div align="center">
  <sub>Feel free to explore my repositories. Since several of my production alpha systems are kept in private repositories for intellectual property reasons, please reach out directly or check the enclosed documentation directories for code architectures.</sub>
</div>