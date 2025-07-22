# OMR Reviews – Frontend Challenge: HubSpot Marketing Hub Dashboard

## Executive Summary

**Your Mission**: Build a Vue.js dashboard that visualizes HubSpot evaluation insights through interactive multi-select filtering and data visualization.

**Scope**: ~4 hours • Vue 3 + Chart.js • One chart + multi-select filtering + documentation

**Success Definition**: Working chart + clear progress documentation (incomplete solutions expected!)

---

## Company Context

You're joining **OMR Reviews** as a Frontend Developer. We build market intelligence tools that help businesses make better software decisions. This challenge simulates real work - creating dashboards that turn vendor evaluation data into actionable insights.

**The Scenario**: Our AI analyzed 15,847 customer conversations from users evaluating HubSpot Marketing Hub. Your job: make this data accessible through an interactive dashboard.

---

## Technical Requirements

| Constraint | Requirement | Notes |
|------------|-------------|-------|
| **Framework** | Vue 3 + Nuxt 3 (Composition API) | Use what you know best |
| **Styling** | Tailwind CSS | Basic styling is fine |
| **Charts** | Chart.js | One simple chart |
| **Time** | ~4 hours | With AI assistance |
| **Scope** | One feature done well > multiple broken features | Quality over quantity |

---

## Data Structure & Sample

### Data Sample
```json
{
  "metadata": {
    "title": "HubSpot Evaluation Data",
    "total_conversations": 15847,
    "date_range": "2024-Q1 to 2024-Q4"
  },
  "uniqueValues": {
    "companySize": ["Startup (1-10)", "Small (11-50)", "Medium (51-200)", "Large (201-1000)", "Enterprise (1000+)"],
    "industry": ["E-Commerce", "Media & Marketing", "Finance & Banking", "SaaS & Technology", "Education", "Consulting"],
    "quarter": ["2024-Q1", "2024-Q2", "2024-Q3", "2024-Q4"]
  },
  "questions": [
    {
      "questionId": "q1",
      "questionNumber": 1,
      "question": "What productivity tools are you currently using?",
      "questionType": "multi_response",
      "description": "Popular productivity software currently in use",
      "data": [
        {
          "companySize": "Startup (1-10)",
          "industry": "SaaS & Technology", 
          "quarter": "2024-Q4",
          "responseValue": "Notion",
          "numResponses": 43
        },
        {
          "companySize": "Enterprise (1000+)",
          "industry": "Finance & Banking",
          "quarter": "2024-Q3", 
          "responseValue": "Google Workspace",
          "numResponses": 127
        }
      ]
    },
    {
      "questionId": "q2", 
      "questionNumber": 2,
      "question": "What integration requirements do you have?",
      "questionType": "multi_response",
      "description": "Technical integration needs and API requirements",
      "data": [
        {
          "companySize": "Medium (51-200)",
          "industry": "E-Commerce",
          "quarter": "2024-Q2",
          "responseValue": "REST API Access", 
          "numResponses": 89
        }
      ]
    },
    {
      "questionId": "q3",
      "questionNumber": 3, 
      "question": "What are your main concerns about HubSpot?",
      "questionType": "multi_response",
      "description": "Primary evaluation concerns and blockers",
      "data": [
        {
          "companySize": "Large (201-1000)",
          "industry": "Media & Marketing",
          "quarter": "2024-Q1",
          "responseValue": "Too Expensive",
          "numResponses": 156
        }
      ]
    }
  ]
}
```

### TypeScript Interfaces

```typescript
// Individual conversation data point
interface DataEntry {
  companySize: 'Startup (1-10)' | 'Small (11-50)' | 'Medium (51-200)' | 'Large (201-1000)' | 'Enterprise (1000+)';
  industry: 'E-Commerce' | 'Media & Marketing' | 'Finance & Banking' | 'SaaS & Technology' | 'Education' | 'Consulting';
  quarter: '2024-Q1' | '2024-Q2' | '2024-Q3' | '2024-Q4';
  responseValue: string; // e.g., "Google Workspace", "REST API Access", "Too Expensive"
  numResponses: number;  // How many times this was mentioned
}

// Question structure  
interface Question {
  questionId: 'q1' | 'q2' | 'q3';
  questionNumber: number;
  question: string;
  questionType: 'multi_response';
  description: string;
  data: DataEntry[];
}

// Root data structure
interface HubSpotData {
  metadata: {
    title: string;
    total_conversations: number;
    date_range: string;
  };
  uniqueValues: {
    companySize: string[];
    industry: string[];  
    quarter: string[];
  };
  questions: Question[];
}
```

### Data Loading Options

**Option 1 - Static Import** (Recommended for simplicity):
```typescript
// composables/useHubSpotData.js
import hubspotData from '~/data.json'

export const useHubSpotData = () => {
  return {
    data: hubspotData,
    loading: false
  }
}
```

**Option 2 - Fetch Approach** (More realistic):
```typescript  
// composables/useHubSpotData.js
export const useHubSpotData = () => {
  const { data, pending } = useFetch('/data.json')
  return {
    data: data.value,
    loading: pending.value
  }
}
```

### Data Overview

**Pick ONE question to visualize** (you don't need all three):

- **Q1 - Current Tools**: "Google Workspace", "Microsoft 365", "Slack", "Notion", "Airtable", "Monday.com"
- **Q2 - Requirements**: "REST API Access", "Zapier Integration", "HubSpot Integration", "Webhooks", "Competitive Pricing"  
- **Q3 - Concerns**: "Too Expensive", "Data Privacy & Security", "Lack of Integrations", "Poor Customer Support"

**Key Points**:
- Data is pre-aggregated (each `DataEntry` represents summed conversation data)
- Use **multi-select filtering** by dimensions: company size, industry, quarter
- Chart shows `responseValue` vs total `numResponses`

### Simple Data Processing Example

```typescript
import * as pd from 'pandas-js';

// Filter and aggregate data for Chart.js
const processChartData = (question: Question, filters: FilterState) => {
  // Filter data - empty arrays mean "show all"
  const filtered = question.data.filter(entry => 
    (!filters.companySize.length || filters.companySize.includes(entry.companySize)) &&
    (!filters.industry.length || filters.industry.includes(entry.industry)) &&
    (!filters.quarter.length || filters.quarter.includes(entry.quarter))
  );

  // Aggregate using pandas-js
  const df = new pd.DataFrame(filtered);
  const aggregated = df
    .groupBy('responseValue')
    .sum('numResponses')
    .sortValues('numResponses', { ascending: false })
    .toJSON();

  // Generate filter summary for subtitle
  const filterParts = [];
  if (filters.companySize.length) filterParts.push(`${filters.companySize.join(', ')}`);
  if (filters.industry.length) filterParts.push(`${filters.industry.join(', ')}`);
  if (filters.quarter.length) filterParts.push(`${filters.quarter.join(', ')}`);
  
  const subtitle = filterParts.length 
    ? `Filtered: ${filterParts.join(' | ')}` 
    : 'All Data';

  return {
    labels: Object.keys(aggregated.numResponses),
    values: Object.values(aggregated.numResponses),
    title: question.question, // Use the actual question text
    subtitle: subtitle
  };
};

// Example: Chart.js configuration with dynamic titles
const createChartConfig = (chartDataObject) => {
  return {
    type: 'bar',
    data: {
      labels: chartDataObject.labels,
      datasets: [{
        label: 'Mentions',
        data: chartDataObject.values,
        backgroundColor: 'rgba(54, 162, 235, 0.8)'
      }]
    },
    options: {
      indexAxis: 'y', // Horizontal bars
      responsive: true,
      plugins: {
        title: { 
          display: true, 
          text: chartDataObject.title // Dynamic from question data
        },
        subtitle: {
          display: true,
          text: chartDataObject.subtitle // Shows filter state
        }
      }
    }
  };
};

// Usage example:
// const question = hubspotData.questions[0]; // Q1: "What productivity tools are you currently using?"
// const chartDataObject = processChartData(question, filterState);
// const chartConfig = createChartConfig(chartDataObject);
```

---

## Your Assignment

### Core Requirements (Must Have - 70% of evaluation)

**A. One Working Visualization**
- Simple Chart.js bar chart displaying data from ONE question (Q1, Q2, or Q3)
- Show `responseValue` (tool names, requirements, concerns) vs `numResponses`
- Data should update when filters change

**B. Multi-Select Filtering** 
- At least ONE dimension with multi-select capability (company size OR industry OR quarter)
- Users can select multiple options (e.g., "Startup + Small" or "Q3 + Q4")
- Filters should update chart data in real-time
- Use checkboxes, multi-select dropdowns, or toggle buttons

**C. About Page Documentation** (Critical!)
- Route: `/about`
- Document your progress, decisions, challenges, and next steps
- This is 50% of your evaluation - be thorough!

### Application Structure

```
app/
├── pages/
│   ├── index.vue          # Main dashboard
│   └── about.vue          # Progress documentation  
├── components/
│   ├── ChartComponent.vue # Chart.js wrapper
│   └── FilterPanel.vue    # Multi-select filter controls
├── composables/
│   └── useHubSpotData.js  # Data loading & processing
└── public/
    └── data.json          # HubSpot dataset (if using fetch approach)
```

### Nice to Have (30% of evaluation)

- Multiple filter dimensions working together (e.g., company size + industry)
- Loading states and error handling
- Responsive design and UI polish  
- Clean component architecture with reusable filters
- Filter state persistence or "clear all" functionality
- Second chart or advanced Chart.js features

---

## About Page Requirements 🎯

**This is critical for evaluation** - your About page should include:

### Required Sections

**1. Progress & Time Breakdown**
```markdown
## What I Completed
✅ Nuxt setup, data loading, basic Chart.js bar chart for Q1 data
✅ Company size multi-select filter (functional, minor rendering issues)  
⚠️ Industry filtering partially implemented
❌ Quarter filtering, responsive design

## Time Spent
- Hour 1: Setup + Chart.js installation (30 min with AI)
- Hour 2: Chart component + data aggregation (TypeScript issues)
- Hour 3: Multi-select filter implementation  
- Hour 4: Bug fixing + this documentation
```

**2. Prioritization Decisions**
- Which question data you chose and why
- What you focused on first and why
- Trade-offs you made given time constraints

**3. Next Steps**  
- Concrete next features you'd implement
- How you'd improve what you built
- What you'd refactor

**4. Challenges & Solutions**
- Technical blockers you encountered
- How you solved them (or didn't)
- What you learned

**5. Technical Decisions**
- Component architecture choices
- State management approach  
- Libraries used and alternatives considered

### Example About Content

```markdown
## Progress Made

### Completed Features
- ✅ Basic Nuxt 3 setup with TypeScript
- ✅ Chart.js bar chart displaying Q1 data (Current Tools)  
- ✅ Company size multi-select filter with checkboxes
- ✅ Data aggregation logic using pandas-js

### Partially Working  
- ⚠️ Chart re-renders with filter changes but has flickering bug
- ⚠️ Multi-select shows selected count but not individual selections

### Not Started
- ❌ Industry and quarter multi-select filtering  
- ❌ Loading states and error handling
- ❌ Responsive design optimization

## Time Breakdown & Prioritization

**Hour 1**: Nuxt setup + Chart.js integration (45 minutes with AI assistance)
**Hour 2**: Chart component + data processing (longer than expected due to TypeScript config)  
**Hour 3**: Multi-select filter implementation (got basic company size working)
**Hour 4**: Bug fixing + documentation

**Why I chose Q1 data**: Current tools seemed most business-relevant and had clear, recognizable values like "Slack" and "Google Workspace" that would make for an intuitive chart.

**Why company size first**: Only 5 options vs 6 industries, seemed simpler to implement multi-select logic.

## Next Steps (Priority Order)

1. **Fix chart flickering bug** - Chart re-creates instead of updating data
2. **Add industry multi-select filtering** - Similar checkbox pattern to company size  
3. **Improve filter UI** - Show selected items clearly, add clear filters button
4. **Add quarter multi-select filtering** - Complete the multi-dimensional filtering
5. **Loading states** - Show spinner while processing data
6. **Responsive design** - Mobile-friendly layout

## Challenges Encountered

**Multi-Select State Management**: Initially used simple boolean flags, realized I needed arrays to track multiple selections. Refactored to use reactive arrays with proper includes() logic.

**Chart.js TypeScript Configuration**: Spent 30 minutes on import/type issues. AI helped but took multiple iterations to get Chart.js + Vue 3 + TypeScript working together.

**Reactive Data Updates**: Initial approach caused chart to completely re-render on filter changes. Need to research Chart.js `.update()` method for smooth transitions.

## Technical Decisions

**Multi-Select Approach**: 
- Used checkbox arrays rather than fancy multi-select dropdowns for simplicity
- Filter state stored as arrays: `{ companySize: ['Startup (1-10)', 'Small (11-50)'] }`
- Empty arrays mean "no filter applied" rather than "filter everything out"

**Component Architecture**: 
- Single Chart component with reactive props for reusability
- Filter logic in parent component (would extract to composable for scalability)  
- Used pandas-js for data aggregation (SQL-like operations, great for analytics)

**State Management**: 
- Simple reactive refs in parent component
- Considered Pinia but seemed overkill for this scope
- Would move to composable for production

## What I'd Do Differently

Given more time, I'd:
- Start with composable architecture from the beginning
- Set up proper Chart.js update patterns before adding filters  
- Create reusable multi-select components for DRY code
- Add proper TypeScript throughout (some any types used for speed)
- Implement filter state URL persistence for better UX
```

---

## Evaluation Rubric

### Must Have (70% of score)
- [ ] **One working Chart.js visualization** showing HubSpot data
- [ ] **Multi-select filtering** for at least one dimension that updates chart data  
- [ ] **Comprehensive About page** documenting progress and decisions

### Nice to Have (30% of score)
- [ ] Multiple filter dimensions working together with multi-select
- [ ] Clean component architecture and code quality
- [ ] UI polish, responsive design, error handling
- [ ] Advanced Chart.js features or multiple charts

### What Gets You Hired
✅ **Detailed progress documentation** with honest assessment  
✅ **Clear prioritization reasoning** and trade-off decisions  
✅ **One polished feature** over multiple broken attempts  
✅ **Concrete next steps** showing you understand the bigger picture  
✅ **Problem-solving transparency** - what you tried, what didn't work

### What We Don't Expect
❌ Complete implementation of all features  
❌ Perfect visual design or pixel-perfect styling  
❌ Comprehensive error handling or edge cases  
❌ Production-ready code quality

---

## Time Management Strategy

### Hour-by-Hour Guide

**Hour 1: Foundation**
- [ ] Nuxt 3 setup with TypeScript
- [ ] Install Chart.js and pandas-js: `npm install chart.js pandas-js`
- [ ] Load data.json (static import or useFetch) and verify structure
- [ ] Basic chart component rendering static data

**Hour 2: Core Functionality**  
- [ ] Data aggregation function (sum numResponses by responseValue)
- [ ] Choose one question (Q1, Q2, or Q3) to visualize
- [ ] Wire up chart with real data
- [ ] Basic styling with Tailwind

**Hour 3: Multi-Select Filtering**
- [ ] Add one multi-select filter dimension (company size recommended)
- [ ] Connect multi-select state to chart data updates  
- [ ] Debug any reactivity issues
- [ ] Test multiple selections working together

**Hour 4: Documentation & Polish**
- [ ] Create comprehensive About page
- [ ] Code cleanup and comments
- [ ] Test final functionality
- [ ] Document what you'd do next

### If You Get Stuck

**Don't panic!** Document your blockers:

```markdown
## Blockers Encountered

**Multi-Select State Logic (45 minutes)**
- Tried: Simple boolean flags for each option
- Issue: Couldn't track multiple selections properly
- Solution: Refactored to use reactive arrays with .includes() logic
- Still struggling with: UI showing selected count vs individual items

**Chart.js Import Issues (30 minutes)**
- Tried: `import Chart from 'chart.js'` 
- Error: TypeScript import issues
- Solution: Used AI to help with proper Chart.js v4 imports
- Would do next: Research Chart update patterns for smooth transitions
```

This kind of documentation is **valuable** and shows great engineering practices!

---

## Success Examples

### Minimum Viable Solution
```markdown
✅ Nuxt project with Chart.js displaying Q1 data
✅ Company size multi-select filter working with checkboxes
✅ Clean About page with progress, decisions, next steps
✅ 2-3 reusable Vue components
```

### Strong Solution
```markdown  
✅ All of above plus:
✅ Multiple multi-select filters working together (company + industry)
✅ Smooth chart transitions and loading states
✅ Excellent component architecture with composables
✅ Detailed technical decision documentation
```

### Outstanding Solution
```markdown
✅ All of above plus:  
✅ Multiple question visualizations or chart types
✅ Advanced Chart.js features (tooltips, animations)
✅ Responsive design and error handling
✅ Production-ready code quality and filter state persistence
```

**Remember**: Better to have minimum viable solution working perfectly than strong solution half-broken!

---

## Final Reminders

**This Simulates Real Work**: At OMR Reviews, similar dashboards often take multiple days. Don't stress about finishing everything!

**Quality Over Quantity**: One working chart with great multi-select filtering and documentation beats three broken charts.

**Document Everything**: Your About page is 50% of the evaluation. Be thorough, honest, and specific.

**Use Your Tools**: AI assistance is expected and encouraged. Document what helped vs. what you decided.

**When in Doubt**: Make reasonable assumptions and document them. We want to see how you think through problems.

---

---

## 🚨 Before You Start: READ FIRST!

**STOP** - Don't jump into coding yet! 

**Take 10-15 minutes to fully read and understand this challenge**:

1. **Read the entire CHALLENGE.md** - Don't skim, actually read it
2. **Understand the data structure** - Look at the sample JSON, understand what you're building
3. **Choose your question** - Pick Q1, Q2, or Q3 early and stick with it
4. **Plan your approach** - Which filter dimension will you tackle first? Multi-select checkboxes or dropdowns?
5. **Set up your environment** - Then start coding

**Why this matters**: Senior developers who rush into coding without understanding requirements often waste hours rebuilding. The candidates who succeed read carefully, plan their approach, and execute methodically.

**Pro tip**: Many candidates miss that the About page is 50% of the evaluation. Don't treat documentation as an afterthought!

---

**Ready to build?** Load up your AI assistant, grab some coffee, and show us how you approach real-world frontend challenges!

*Questions? Make assumptions and document them - that's what we do in production too.*