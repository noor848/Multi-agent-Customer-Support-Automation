# Multi-Agent Customer Support Automation

An intelligent customer support system built with CrewAI that uses two AI agents working sequentially to handle customer inquiries with built-in quality assurance.

## Overview

This system automates customer support responses using a two-stage process:

1. **Support Agent**: Researches and drafts initial responses using documentation
2. **QA Agent**: Reviews and polishes responses to ensure quality

The agents work together with memory enabled, learning from previous interactions to improve response quality over time.

## Architecture
```
Customer Inquiry → Support Agent → QA Review → Final Response
                        ↓
                  (Scrapes Docs)
```

## Prerequisites

- Python 3.7+
- OpenAI API key
- Serper API key

## Installation

Install required packages:
```bash
pip install crewai==0.28.8 crewai_tools==0.1.6 langchain_community==0.0.29
```

## Configuration

Set up API keys using utility functions:
```python
from utils import get_openai_api_key, get_serper_api_key

openai_api_key = get_openai_api_key()
os.environ["OPENAI_MODEL_NAME"] = 'gpt-3.5-turbo'
os.environ["SERPER_API_KEY"] = get_serper_api_key()
```

## Agents

### 1. Senior Support Representative
- **Role**: First-line support responder
- **Goal**: Provide friendly and comprehensive support
- **Tools**: Documentation scraper (CrewAI docs)
- **Key Setting**: `allow_delegation=False` - handles inquiries independently

### 2. Support Quality Assurance Specialist
- **Role**: Quality reviewer and response polisher
- **Goal**: Ensure best-in-class support quality
- **Tools**: None (reviews existing responses)
- **Responsibilities**: 
  - Verify completeness and accuracy
  - Ensure friendly, professional tone
  - Validate all sources and references

## Tasks

### Task 1: Inquiry Resolution
- Analyzes customer inquiry
- Searches relevant documentation
- Drafts detailed response with references
- Ensures no assumptions are made

### Task 2: Quality Assurance Review
- Reviews support agent's draft
- Verifies comprehensive coverage
- Checks tone and professionalism
- Produces final polished response

## Usage

Define customer inquiry details:
```python
inputs = {
    "customer": "DeepLearningAI",
    "person": "Andrew Ng",
    "inquiry": "I need help with setting up a Crew and adding memory?"
}
```

Run the support crew:
```python
result = crew.kickoff(inputs=inputs)
```

Display the response:
```python
from IPython.display import Markdown
Markdown(result)
```

## Key Features

### Memory System (`memory=True`)
The crew maintains three types of memory:

- **Short-term memory**: Context within current execution
- **Long-term memory**: Learns from previous support tickets
- **Entity memory**: Remembers customers, issues, and solutions

This enables the system to:
- Provide consistent responses
- Learn from past interactions
- Improve efficiency over time
- Recall previous customer issues

### Verbose Logging (`verbose=2`)
Detailed logs show:
- Agent reasoning process
- Tool usage and results
- Task progression
- Decision-making steps

### Documentation Integration
The system uses `ScrapeWebsiteTool` to access live CrewAI documentation, ensuring responses are:
- Current and accurate
- Well-referenced
- Based on official sources

## Workflow Example

**Input:**
```python
"How can I add memory to my crew?"
```

**Process:**

1. **Support Agent:**
   - Receives inquiry from Andrew Ng (DeepLearningAI)
   - Scrapes CrewAI documentation
   - Finds memory parameter information
   - Drafts response with code examples and references

2. **QA Agent:**
   - Reviews draft for completeness
   - Verifies accuracy of technical details
   - Ensures friendly, professional tone
   - Polishes final response

**Output:**
- Complete explanation of memory feature
- Code examples showing implementation
- Documentation references
- Friendly, helpful tone

## Advantages Over Manual Support

- **Consistency**: Every response gets quality review
- **Speed**: Automated research and drafting
- **Accuracy**: Always references official documentation
- **Learning**: Improves with each interaction
- **Scalability**: Handle multiple inquiries simultaneously
- **24/7 Availability**: No human limitations

## Configuration Options

### Crew Settings
```python
crew = Crew(
  agents=[support_agent, support_quality_assurance_agent],
  tasks=[inquiry_resolution, quality_assurance_review],
  verbose=2,      # 0=silent, 1=basic, 2=detailed logging
  memory=True     # Enable learning from past interactions
)
```

### Agent Settings
- `allow_delegation`: Set to `False` for independent task handling
- `verbose`: Enable detailed agent activity logs
- `backstory`: Provides context for agent behavior

## Customization

### Add More Tools
```python
from crewai_tools import SerperDevTool, WebsiteSearchTool

search_tool = SerperDevTool()
support_agent.tools.append(search_tool)
```

### Modify Documentation Source
```python
docs_scrape_tool = ScrapeWebsiteTool(
    website_url="https://your-docs-url.com"
)
```

### Adjust Response Style
Modify the `expected_output` in tasks to change:
- Tone (formal/casual)
- Length (brief/detailed)
- Format (bullet points/paragraphs)

## Output

The system returns a markdown-formatted response that includes:
- Complete answer to customer inquiry
- Code examples (if applicable)
- Documentation references
- Friendly, professional language
- No unanswered questions

