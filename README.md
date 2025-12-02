# 🏥 Dual-Agent AI Medical Diagnosis System

An AI-powered medical diagnosis system built on the DeepSeek API, simulating real-world clinical interactions through multi-agent collaboration, complete with logging and long-term learning mechanisms.

## Inspiration & Project Overview
Inspired by personal healthcare experiences, the project addresses how patients often struggle to accurately describe their symptoms. When physicians cannot easily determine the condition, it leads to repetitive questioning or unnecessary tests, increasing both time and cost. This system enables an AI doctor agent and an AI patient agent to interact under constraints of limited inquiries and patient budget, allowing the AI doctor to continuously learn from consultations and diagnose conditions efficiently.

## ✨ Features
**Dual-Agent System**: Interactive diagnosis between Doctor AI and Patient AI

**Realistic Medical Simulation**: Complete workflow including consultation, testing, and diagnosis

**Intelligent Decision System**: Doctor AI intelligently selects tests based on symptoms

**Long-Term Learning Mechanism**: AI doctor learns from historical interactions and optimizes diagnostic strategies

**Comprehensive Logging System**: Saves detailed interaction records, round logs, and learning memories

**Rich Configuration**: 20+ diseases, 20+ test items, 8 patient personality types

## 🏗️ Project Structure
```
main-dir/
├── readme.md
├── AI_doctor-patient_diagnostic_system-CN/  # Chinese Version
    ├──doctor_memory/       # Doctor's long-term memory directory (auto-generated on first run)
    ├──medical_records/     # Diagnosis records directory (auto-generated on first run)
    ├──round_logs/          # Round logs directory (auto-generated on first run)
    ├──.env                 # Environment file
    ├──main.py              # Main program
    ├──requirements.txt     # Dependencies
├──AI_doctor-patient_diagnostic_system-EN/  # English Version
    ├──doctor_memory/       # Doctor's long-term memory directory (auto-generated on first run)
    ├──medical_records/     # Diagnosis records directory (auto-generated on first run)
    ├──round_logs/          # Round logs directory (auto-generated on first run)
    ├──.env                 # Environment file
    ├──main.py              # Main program
    ├──requirements.txt     # Dependencies
```

## 🚀 Quick Start

### 1. Configure API Key

Create a .env file and add your DeepSeek API key:

```bash
# .env file content
DEEPSEEK_API_KEY=your_api_key_here
```

**Obtain an API Key**:
1. Visit the [DeepSeek Platform](https://platform.deepseek.com/)
2. Register and log in
3. Create a new API key in the "API Keys" page
4. Copy the API key and paste it into your .env file

### 2. Run the System
**Interactive Mode** (Press Enter to proceed each round):
```bash
python main.py
# or
python3 main.py
```

**Auto Mode** (No interaction required):
```bash
python main.py --auto
# or
python3 main.py --auto
```

**Specify Number of Rounds**:
```bash
python main.py --rounds 10  # Run 10 rounds
```

## 🎯 System Mechanics

### Core Mechanics
**Doctor AI:**
Has two types of actions:
1. Ask questions during consultation. Patient responses may be misunderstood and increase suspicion.
2. Request tests, which are more likely to yield accurate results but have a chance of failure. Tests consume the patient's budget.

**Patient AI:** Answers questions based on actual condition but may misunderstand (e.g., considering eating a few hours ago as 'fasting', not considering beer as 'alcohol').

**Trust System:** Consultation increases patient suspicion by 0.1, tests increase it by 0.05. Diagnosis fails if suspicion exceeds 1.

**Budget System:** Each diagnosis starts with an initial budget of 500. The patient has an ideal budget. The doctor only knows the initial budget amount and must control testing costs within this limit. Diagnosis fails if the final cost significantly exceeds the patient's ideal budget. Budget thresholds relate to personality-based ideal cost ranges and spending sensitivity.

**Success Conditions:** Correct diagnosis + Reasonable cost + Patient trust maintained.

### Patient Personality Types (8 Types)
| Personality  | Suspicion Growth | Cost Sensitivity | Ideal Cost Range |
|--------------|------------------|------------------|------------------|
| Cautious     | 0.15             | 0.8              | 80-150 yuan      |
| Casual       | 0.08             | 0.4              | 120-200 yuan     |
| Hypochondriac| 0.25             | 0.3              | 150-250 yuan     |
| Frugal       | 0.12             | 0.9              | 50-100 yuan      |
| Impatient    | 0.20             | 0.5              | 100-180 yuan     |
| Dependent    | 0.05             | 0.6              | 200-300 yuan     |
| Rational     | 0.10             | 0.7              | 150-220 yuan     |
| Skeptical    | 0.30             | 0.4              | 80-120 yuan      |

### Test Items (20+ Types)
Include: Blood Test, Urinalysis, Electrocardiogram (ECG), Chest X-ray, CT Scan, MRI, Ultrasound, Gastroscopy, Liver Function Test, Kidney Function Test, Blood Glucose Test, Lipid Profile, Bone Density Test, Endoscopy, Biopsy, Electroencephalogram (EEG), Pulmonary Function Test, Skin Allergy Test, etc.

### Diseases (20+ Types)
Migraine, Gastritis, Allergic Rhinitis, Common Cold, Hypertension, Diabetes, Asthma, Arthritis, Dermatosis, Insomnia, Pneumonia, Bronchitis, Gastric Ulcer, Kidney Stones, Cholecystitis, Myocarditis, Concussion, Lumbar Disc Herniation, Osteoporosis, Anemia, Hyperthyroidism, Gout, Hepatitis, Irritable Bowel Syndrome (IBS), Depression, Anxiety Disorder, Cataract, Glaucoma, Otitis Media, Sinusitis.

## 🔄 System Workflow
```mermaid
    graph TD
    A[System Start] --> B[Initialize Configuration]
    B --> C{Configuration Validation}
    C -->|Failure| D[Display Error & Exit]
    C -->|Success| E[Create Directory Structure]
    
    E --> F[Initialize System Components]
    F --> G[Load Doctor's Long-Term Memory]
    
    G --> H[Main Interaction Loop Start]
    
    H --> I[Generate Random Case & Patient Personality]
    
    subgraph "Agent Generation"
        I --> J[Create Patient Agent]
        I --> K[Create Doctor Agent]
    end
    
    J --> L[Patient Chief Complaint]
    K --> M[Initialize Doctor State]
    
    L --> N[Create System State]
    M --> N
    
    N --> O{Interaction Round Loop}
    
    O --> P[Doctor Chooses Action]
    
    P -->|Ask Questions| Q[Generate AI Question]
    Q --> R[Patient AI Answer]
    R --> S[Update Suspicion]
    
    P -->|Request Test| T[Select Test Type]
    T --> U[Execute Test / Deduct Cost]
    U --> V[Update Total Cost]
    
    S --> W{Round End Condition? （Success/Failure)}
    V --> W
    
    W -->|No （Budget Exceeded or Trust Lost）| Y
    W -->|Yes| X[Final AI Diagnosis, Evaluate Correctness]
    
    X -->|Diagnosis Correct or Failed| Y[Evaluate Round Result]
    Y --> Z[Doctor Learning Update]
    Z --> AA[Save Round Record]
    
    AA --> BB{Next Round?}
    BB -->|Yes| I
    BB -->|No| CC[Save Complete Interaction Record]
    
    CC --> DD[Display Final Report]
    DD --> EE[System End]
    
    %% Data Flow
    R -.->|Conversation History| Q
    S -.->|Increased Suspicion| P
    V -.->|Deducted Budget, Increased Suspicion| P
    Z -.->|Update Memory| G
    AA -.->|Save Logs| E
```

## 🤖 AI System Details
### 1. Doctor Agent
**Goal:** Correctly diagnose disease, control costs, maintain patient trust
**Capabilities:**
- Intelligent questioning (reasoning based on conversation history)
- Intelligent test selection (based on symptoms and relevance)
- Final diagnosis (comprehensive analysis of all information)
**Learning Mechanism:** Learns from each round's outcome, optimizes questioning and testing strategies

### 2. Patient Agent
**Goal:** Describe condition truthfully, but with possible misunderstandings
**Characteristics:**
- Personality affects suspicion growth and cost sensitivity
- May misunderstand doctor's questions
- True condition hidden within descriptions

### 3. Medical System
**Test Relevance:** Each test has varying diagnostic value for different diseases
**Accuracy Simulation:** Test results may be true positive, true negative, or false negative
**Cost System:** Different tests have different costs, affecting patient trust

## 🧠 Long-Term Learning System
The AI doctor learns from each interaction and continuously optimizes diagnostic strategies:
1. Interaction Review: Automatically analyzes key decision points after each interaction
2. Experience Extraction: Extracts success/failure experiences for future reference
3. Memory Storage: Saves to JSON files in the `doctor_memory/` directory
4. Experience Application: Automatically loads historical experiences at the start of new interactions

Examples of learned insights:
- Balancing the ratio of questions to tests
- Which tests are most valuable for specific symptoms
- Managing patient trust levels
- Optimizing cost-control strategies

## 📊 System Output Example
```
================================================================================
                            🩺 Patient 1 Consultation
================================================================================

【Patient Personality】Skeptical
【Ideal Cost】100 yuan
【True Condition】Diabetes

Patient's Chief Complaint:
Patient: I've been feeling very thirsty lately, drinking lots of water but still feeling dry mouth, and urinating more frequently than before...

💬 Doctor Asks Questions
Doctor: How long have you been experiencing this thirst and frequent urination? Have you had your blood sugar checked?
Patient: About two or three weeks. I haven't checked my blood sugar, thought it was just not drinking enough water...

🔬 Doctor Requests Test
Doctor: Recommend a blood glucose test.
Test Result: Blood glucose test shows significantly elevated levels, consistent with diabetes diagnosis.
Test Cost: 50 yuan

🤔 Doctor Considers Final Diagnosis...
Doctor Diagnosis: Diabetes

✅ Diagnosis Successful!

📈 Learning Progress: Recent Success Rate: 100.0% | Avg Questions: 1.0 | Avg Tests: 1.0

============================================================
                        🩺 Patient 3 Consultation
============================================================

⏱️  API Response Time: 2.84s
【Patient Personality】Rational
【Ideal Cost】401 yuan
【True Condition】Asthma

Patient's Chief Complaint:
⏱️  API Response Time: 2.26s
Patient: Hello doctor, I've been feeling some tightness in my chest lately, and when I breathe it sounds like a whistle. Sometimes I wake up at night feeling short of breath. It's hard to pinpoint when it happens exactly, but it seems to get worse with activity.

Current Round: 3 | Questions: 0 | Tests: 0 | Total Cost: 0 | Remaining Budget: 500 | Patient Suspicion: 0.00 | 📝 Collecting

💬 Doctor Asks Questions
⏱️  API Response Time: 1.62s
Doctor: You mentioned a whistling sound when breathing. Does it occur only during exhalation, or during inhalation as well?
⏱️  API Response Time: 1.65s
Patient: It seems more noticeable when exhaling, not so much when inhaling.

Current Round: 3 | Questions: 1 | Tests: 0 | Total Cost: 0 | Remaining Budget: 500 | Patient Suspicion: 0.10 | 📝 Collecting

... (further interactions) ...

================================================================================
                            🎓 System Final Report
================================================================================

Total Rounds: 5
Success Rate: 80.0%
Average Questions: 3.2
Average Tests: 1.8
Average Cost: 215.4 yuan
Average Cost Ratio: 1.4

Doctor Learning Summary: Recent Success Rate: 80.0% | Avg Questions: 3.2 | Avg Tests: 1.8
```

## ⚙️ Configuration
```python
# API Configuration
DEEPSEEK_API_KEY = os.getenv("DEEPSEEK_API_KEY", "")
DEEPSEEK_BASE_URL = "https://api.deepseek.com"
MODEL_NAME = "deepseek-chat"

# System Basic Configuration
MAX_QUESTIONS_PER_ROUND = 12  # Maximum questions per round
INITIAL_BUDGET = 500          # Initial budget
SUSPICION_THRESHOLD = 0.8     # Suspicion threshold

# Long-Term Memory
ENABLE_LONG_TERM_MEMORY = True  # Enable cross-interaction learning
MAX_HISTORY_GAMES = 10          # Save last 10 interactions

# AI Temperature Parameters
TEMPERATURE_PATIENT_RESPONSE = 0.9    # Patient response - high temperature for diversity
TEMPERATURE_DOCTOR_QUESTION = 0.7     # Doctor question - medium for balance
TEMPERATURE_DOCTOR_DIAGNOSIS = 0.3    # Doctor diagnosis - low for accuracy
```

## 🧪 Testing & Validation
```
# Validate Python syntax
python3 -m py_compile main.py

# View help information
python3 main.py --help

# Run test (3 rounds, auto mode)
python3 main.py --auto --rounds 3
```

## 🔧 Customization & Extension
### Adding New Diseases
1. Add disease name to the `DISEASE_LIBRARY` in the `MedicalGameConfig` class
2. Add relevant test relationships in `TEST_DISEASE_RELEVANCE` in the `MedicalSystem` class

### Adding New Test Items
1. Add new test to `TEST_COSTS` and `TEST_ACCURACY` in the `MedicalGameConfig` class
2. Define test-disease relevance in `TEST_DISEASE_RELEVANCE` in the `MedicalSystem` class
3. Add corresponding result description templates

### Adjusting Patient Personalities
Modify `PERSONALITY_TYPES` in the `MedicalGameConfig` class:

```python
PERSONALITY_TYPES = {
    "New Personality": {
        "suspicion_gain": 0.15,     # Suspicion growth rate
        "cost_sensitivity": 0.8,    # Cost sensitivity
        "ideal_cost_range": (80, 150)  # Ideal cost range
    },
    # ... existing personalities
}
```

## 🤝 Contributing
Issues and Pull Requests are welcome!

## 📄 License
MIT License

## 🙏 Acknowledgments
- [DeepSeek](https://www.deepseek.com/) - For the powerful AI API
- [OpenAI Python SDK](https://github.com/openai/openai-python) - Excellent API client
- [Colorama](https://github.com/tartley/colorama) - Cross-platform colored terminal output