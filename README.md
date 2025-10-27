# Interview Preparation Roadmap Generator

An intelligent multi-agent system that analyzes job descriptions and generates personalized interview preparation roadmaps using LangChain and Google's Gemini AI.

## 🎯 Overview

This AI agent takes a company name, role, and job description as input and produces a comprehensive preparation roadmap including:
- Interview rounds and structure
- Key topics to study
- Difficulty assessment
- Recommended preparation order
- Estimated preparation time

## 🏗️ Architecture

The system uses a **multi-agent architecture** with three specialized agents:

```
┌─────────────────────────────────────────────────────────┐
│              Interview Prep Agent (Orchestrator)         │
└─────────────────────────────────────────────────────────┘
                            │
                ┌───────────┼───────────┐
                │           │           │
        ┌───────▼──┐  ┌─────▼─────┐  ┌─▼──────────────┐
        │ JD Parser│  │ Interview  │  │ Roadmap Builder│
        │  Agent   │  │  Process   │  │     Agent      │
        └──────────┘  │   Agent    │  └────────────────┘
                      └────────────┘
```

### Agent Responsibilities

#### 1. **JD Parser Agent**
- Extracts technical skills from job descriptions
- Identifies soft skills and responsibilities
- Determines experience level requirements
- Maps keywords to relevant topics (e.g., "REST APIs" → "Backend Development")

**How it works:**
- Uses structured prompts to analyze JD text
- Employs Gemini's NLP capabilities to extract entities
- Outputs structured JSON with categorized information

#### 2. **Interview Process Agent**
- Researches company-specific interview patterns
- Identifies typical interview rounds (MCQ, Coding, System Design, HR)
- Predicts difficulty level based on company and role
- Estimates preparation timeline

**How it works:**
- Leverages Gemini's knowledge of company interview processes
- Matches role requirements with typical interview structures
- Considers experience level for difficulty assessment

#### 3. **Roadmap Builder Agent**
- Creates optimized study sequence
- Groups related topics together
- Prioritizes based on interview rounds
- Considers topic dependencies (fundamentals before advanced)

**How it works:**
- Analyzes outputs from previous agents
- Uses reasoning to determine optimal learning path
- Balances breadth vs. depth based on timeline

## 🚀 Setup & Installation

### Prerequisites
- Python 3.8 or higher
- Google Gemini API key

### Installation Steps

1. **Clone or download the repository**

2. **Install dependencies:**
```bash
pip install langchain langchain-google-genai google-generativeai
```

3. **Set up API key:**

Option A - Environment Variable:
```bash
export GOOGLE_API_KEY='your-api-key-here'
```

Option B - Direct in code:
```python
GEMINI_API_KEY = "your-api-key-here"
```

4. **Run the agent:**
```bash
python interview_prep_agent.py
```

## 💻 Usage

### Basic Usage

```python
from interview_prep_agent import InterviewPrepAgent

# Initialize agent
agent = InterviewPrepAgent()

# Generate roadmap
roadmap = agent.generate_roadmap(
    company="Google",
    role="SDE-1",
    job_description="Your job description here..."
)

# Save to JSON
agent.save_roadmap(roadmap, "my_roadmap.json")
```

### Custom Job Description

```python
custom_jd = """
Your complete job description text here...
Include responsibilities, requirements, skills, etc.
"""

roadmap = agent.generate_roadmap(
    company="Microsoft",
    role="Software Engineer",
    job_description=custom_jd
)

print(json.dumps(roadmap, indent=2))
```

## 📊 Output Format

```json
{
  "company": "Google",
  "role": "SDE-1",
  "rounds": [
    {
      "type": "Online Assessment",
      "topics": ["Arrays", "Strings", "Dynamic Programming"],
      "duration": "90 minutes",
      "description": "Coding challenges on common algorithms"
    },
    {
      "type": "Technical Interview",
      "topics": ["Data Structures", "Algorithms", "Problem Solving"],
      "duration": "45 minutes",
      "description": "Live coding with an engineer"
    }
  ],
  "difficulty": "Hard",
  "recommended_order": [
    "Data Structures & Algorithms",
    "System Design Basics",
    "Object-Oriented Design",
    "Behavioral Questions"
  ],
  "estimated_prep_time": "3-4 months",
  "key_skills": [
    "Python",
    "Java",
    "Algorithms",
    "Data Structures",
    "System Design"
  ],
  "generated_at": "2025-10-27 14:30:00"
}
```

## 🔍 How It Works

### Step-by-Step Process

1. **Input Processing**
   - Receives company name, role, and JD text
   - Validates and prepares data for agents

2. **JD Parsing (Agent 1)**
   - Analyzes job description using NLP
   - Extracts: skills, responsibilities, domains
   - Maps keywords to preparation topics
   - Example mapping:
     - "microservices" → System Design
     - "SQL/NoSQL" → Database Systems
     - "React" → Frontend Development

3. **Interview Analysis (Agent 2)**
   - Uses company knowledge base (from Gemini's training)
   - Identifies typical rounds for the company
   - Determines difficulty based on:
     - Company tier (FAANG vs. startup)
     - Role level (junior vs. senior)
     - Required skills complexity

4. **Roadmap Generation (Agent 3)**
   - Synthesizes information from both agents
   - Creates dependency graph of topics
   - Optimizes learning sequence
   - Considers:
     - Foundation topics first
     - Interview round priorities
     - Time constraints

5. **Output Generation**
   - Compiles structured JSON
   - Saves to file
   - Displays summary

### Reasoning & Skill Extraction

**Skill Extraction Process:**
1. **Keyword Matching**: Identifies explicit skills (e.g., "Python", "Java")
2. **Context Analysis**: Understands implicit requirements (e.g., "scalable systems" implies distributed systems knowledge)
3. **Categorization**: Groups skills into domains (DSA, System Design, etc.)
4. **Prioritization**: Ranks by frequency and importance in JD

**Reasoning Flow:**
```
JD Text → Extract Keywords → Map to Topics → Identify Rounds 
    → Determine Difficulty → Order Topics → Generate Roadmap
```

## 📝 Sample Outputs

The repository includes two example outputs:

1. **`roadmap_google_sde1.json`** - Google SDE-1 position
2. **`roadmap_amazon_sde2.json`** - Amazon SDE-2 position

These demonstrate the agent's capability across different:
- Companies (different interview styles)
- Seniority levels (L3 vs L5+)
- Skill sets (varied tech stacks)

## 🎛️ Configuration

### Adjusting Agent Behavior

**Temperature Setting:**
```python
llm = ChatGoogleGenerativeAI(
    model="gemini-pro",
    temperature=0.3  # Lower = more focused, Higher = more creative
)
```

**Custom Prompts:**
Modify agent prompts in each agent class to adjust behavior:
- `JDParserAgent.prompt` - Change skill extraction logic
- `InterviewProcessAgent.prompt` - Adjust round identification
- `RoadmapBuilderAgent.prompt` - Modify roadmap strategy

## 🔧 Advanced Features

### Multi-Agent Reasoning

The system demonstrates multi-agent collaboration:
- Each agent specializes in one aspect
- Agents pass structured data between stages
- Final output combines all agent insights

### Extensibility

Easily extend with:
- **Web Search Integration**: Add real-time company research
- **Resource Recommendations**: Link to study materials
- **Progress Tracking**: Monitor preparation completion
- **Mock Interview Generator**: Create practice questions

## 🐛 Troubleshooting

**Issue: API Key Error**
```
Solution: Ensure GOOGLE_API_KEY is set correctly
export GOOGLE_API_KEY='your-key'
```

**Issue: JSON Parsing Error**
```
Solution: Agent responses sometimes include markdown. The code includes 
cleaning logic to extract pure JSON.
```

**Issue: Empty Results**
```
Solution: Check if JD text is substantial enough (min 100 words recommended)
```

## 📚 Dependencies

```
langchain >= 0.1.0
langchain-google-genai >= 0.0.6
google-generativeai >= 0.3.0
```

## 🤝 Contributing

To extend this agent:
1. Add new agent classes for additional features
2. Modify prompts for better extraction
3. Integrate external APIs (job boards, company reviews)
4. Add visualization for roadmaps

## 📄 License

MIT License - Feel free to use and modify

## 🎓 Use Cases

- **Job Seekers**: Personalized interview prep plans
- **Career Coaches**: Quick roadmap generation for clients
- **Educational Platforms**: Automated curriculum creation
- **Recruitment Agencies**: Help candidates prepare

## 📞 Support

For issues or questions:
- Check troubleshooting section
- Review example outputs
- Modify prompts for your specific needs

---
