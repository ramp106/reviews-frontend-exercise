# Frontend Challenge: HubSpot Marketing Hub Dashboard

## Executive Summary

**Your Mission**: Build a Vue.js dashboard that visualizes HubSpot evaluation insights through interactive filtering and data visualization.

**Scope**: ~4 hours • Vue 3 + Chart.js • One chart + filtering + documentation

**Success Definition**: Working chart + clear progress documentation (incomplete solutions expected!)

---

## Company Context

You're joining **OMR Reviews** as a Frontend Developer. We build market intelligence tools that help businesses make better software decisions. This challenge simulates real work - creating dashboards that turn vendor evaluation data into actionable insights.

**The Scenario**: Our AI analyzed thousands of customer conversations from users evaluating HubSpot Marketing Hub. Your job: make this data accessible through an interactive dashboard.

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

## Data Structure

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

### Data Overview

**Pick ONE question to visualize** (you don't need all three):

- **Q1 - Current Tools**: "Google Workspace", "Microsoft 365", "Slack", "Notion", "Airtable", "Monday.com"
- **Q2 - Requirements**: "REST API Access", "Zapier Integration", "HubSpot Integration", "Webhooks", "Competitive Pricing"  
- **Q3 - Concerns**: "Too Expensive", "Data Privacy & Security", "Lack of Integrations", "Poor Customer Support"

**Key Points**:
- Data is pre-aggregated (each `DataEntry` represents summed conversation data)
- Filter by dimensions: company size, industry, quarter
- Chart shows `responseValue` vs total `numResponses`

### Quick Implementation Example

```typescript
// Load and process data
import _ from 'lodash';

const processChartData = (questionData: DataEntry[], filters: FilterState) => {
  const filtered = questionData.filter(entry => 
    filters.companySize.includes(entry.companySize) &&
    filters.industry.includes(entry.industry) &&
    filters.quarter.includes(entry.quarter)
  );

  // Aggregate by responseValue
  const aggregated = _(filtered)
    .groupBy('responseValue')
    .mapValues(group => _.sumBy(group, 'numResponses'))
    .entries()
    .sortBy(1)
    .reverse()
    .value();

  return {
    labels: aggregated.map(([label]) => label),
    values: aggregated.map(([, value]) => value)
  };
};

// Chart.js configuration
const chartConfig = {
  type: 'bar',
  data: {
    labels: chartData.labels,
    datasets: [{
      label: 'Mentions',
      data: chartData.values,
      backgroundColor: 'rgba(54, 162, 235, 0.8)'
    }]
  },
  options: {
    indexAxis: 'y', // Horizontal bars
    responsive: true,
    plugins: {
      title: { 
        display: true, 
        text: 'Top Tools - Enterprise Q4 2024' 
      }
    }
  }
};
```

---

## Your Assignment

### Core Requirements (Must Have - 70% of evaluation)

**A. One Working Visualization**
- Simple Chart.js bar chart displaying data from at least ONE question (Q1, Q2, or Q3)
- Show `responseValue` (tool names, requirements, concerns) vs `numResponses`
- Data should update when filters change

**B. Basic Filtering** 
- At least ONE dimension filter working (company size OR industry OR quarter)
- Filters should update chart data in real-time
- Use dropdowns, checkboxes, or buttons - whatever works

**C. About Page Documentation** (Critical!)
- Route: `/about`
- Document your progress, decisions, challenges, and next steps
- This is 50% of your evaluation - be thorough!

### Application Structure Example

```
app/
├── pages/
│   ├── index.vue          # Main dashboard
│   └── about.vue          # Progress documentation  
├── components/
│   ├── ChartComponent.vue # Chart.js wrapper
│   └── FilterPanel.vue    # Filter controls
└── composables/
    └── useHubSpotData.js  # Data loading & processing
```

### Nice to Have (30% of evaluation)

- Multiple filter dimensions working together
- Loading states and error handling
- Responsive design and UI polish  
- Clean component architecture
- Second chart or advanced Chart.js features

---

## About Page Requirements 🎯

**This is critical for evaluation** - your About page should include:

### Required Sections

**1. Progress & Time Breakdown**
```markdown
## What I Completed
✅ Nuxt setup, data loading, basic Chart.js bar chart for Q1 data
⚠️ Company size filtering (works but has rendering bug)  
❌ Industry filtering, responsive design

## Time Spent
- Hour 1: Setup + Chart.js installation
- Hour 2: Chart component + data aggregation (TypeScript issues)
- Hour 3: Company size filter implementation  
- Hour 4: Bug fixing + this documentation
```

**2. Prioritization Decisions**
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
- ✅ Company size dropdown filter (mostly working)
- ✅ Data aggregation logic using Lodash

### Partially Working  
- ⚠️ Chart re-renders with filter changes but has flickering bug
- ⚠️ Filter state persists but doesn't show selected value correctly

### Not Started
- ❌ Industry and quarter filtering  
- ❌ Loading states and error handling
- ❌ Responsive design optimization

## Time Breakdown & Prioritization

**Hour 1**: Nuxt setup + Chart.js integration (45 minutes with AI assistance)
**Hour 2**: Chart component + data processing (longer than expected due to TypeScript config)  
**Hour 3**: Filter implementation (got basic company size working)
**Hour 4**: Bug fixing + documentation

**Why I chose Q1 data**: Current tools seemed most business-relevant and had clear, recognizable values like "Slack" and "Google Workspace" that would make for an intuitive chart.

**Why company size first**: Only 5 options vs 6 industries, seemed simpler to implement and debug.

## Next Steps (Priority Order)

1. **Fix chart flickering bug** - Chart re-creates instead of updating data
2. **Add industry filtering** - Similar dropdown pattern to company size  
3. **Improve filter UI** - Show selected values, clear filters button
4. **Add quarter filtering** - Complete the multi-dimensional filtering
5. **Loading states** - Show spinner while processing data
6. **Responsive design** - Mobile-friendly layout

## Challenges Encountered

**Chart.js TypeScript Configuration**: Spent 30 minutes on import/type issues. AI helped but took multiple iterations to get Chart.js + Vue 3 + TypeScript working together.

**Reactive Data Updates**: Initial approach caused chart to completely re-render on filter changes. Need to research Chart.js `.update()` method for smooth transitions.

**Data Processing Logic**: Took time to understand the pre-aggregated structure. Initially tried to process individual conversations before realizing data was already summed.

## Technical Decisions

**Component Architecture**: 
- Single Chart component with reactive props for reusability
- Filter logic in parent component (would extract to composable for scalability)  
- Used Lodash for data aggregation (cleaner than vanilla JS reduce)

**State Management**: 
- Simple reactive refs in parent component
- Considered Pinia but seemed overkill for this scope
- Would move to composable for production

**Libraries Chosen**:
- Lodash over pandas-js (more familiar, smaller bundle)
- Chart.js over D3 (faster to implement, good enough for requirements)
- Tailwind default classes only (no custom CSS to save time)

## What I'd Do Differently

Given more time, I'd:
- Start with composable architecture from the beginning
- Set up proper Chart.js update patterns before adding filters  
- Create reusable filter components for DRY code
- Add proper TypeScript throughout (some any types used for speed)
```

---

## Evaluation Rubric

### Must Have (70% of score)
- [ ] **One working Chart.js visualization** showing HubSpot data
- [ ] **Basic filtering** that updates chart data  
- [ ] **Comprehensive About page** documenting progress and decisions

### Nice to Have (30% of score)
- [ ] Multiple filter dimensions working together
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
- [ ] Install Chart.js and Lodash  
- [ ] Load data.json and verify structure
- [ ] Basic chart component rendering static data

**Hour 2: Core Functionality**  
- [ ] Data aggregation function (sum numResponses by responseValue)
- [ ] Choose one question (Q1, Q2, or Q3) to visualize
- [ ] Wire up chart with real data
- [ ] Basic styling with Tailwind

**Hour 3: Filtering**
- [ ] Add one filter dimension (company size recommended)
- [ ] Connect filter to chart data updates  
- [ ] Debug any reactivity issues
- [ ] Test different filter combinations

**Hour 4: Documentation & Polish**
- [ ] Create comprehensive About page
- [ ] Code cleanup and comments
- [ ] Test final functionality
- [ ] Document what you'd do next

### If You Get Stuck

**Don't panic!** Document your blockers:

```markdown
## Blockers Encountered

**Chart.js Import Issues (30 minutes)**
- Tried: `import Chart from 'chart.js'` 
- Error: TypeScript import issues
- Solution: Used AI to help with proper Chart.js v4 imports
- Still struggling with: Chart update vs re-create patterns

**Data Processing Logic (20 minutes)**  
- Issue: Confused about pre-aggregated vs raw data
- Tried: Processing as individual conversations first
- Realized: Data already summed, just need to filter and re-aggregate
- Would do next: Add better TypeScript interfaces for clarity
```

This kind of documentation is **valuable** and shows great engineering practices!

---

## Success Examples

### Minimum Viable Solution
```markdown
✅ Nuxt project with Chart.js displaying Q1 data
✅ Company size dropdown filter working  
✅ Clean About page with progress, decisions, next steps
✅ 2-3 reusable Vue components
```

### Strong Solution
```markdown  
✅ All of above plus:
✅ Multiple filters working together (company + industry)
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
✅ Production-ready code quality and testing approach
```

**Remember**: Better to have minimum viable solution working perfectly than strong solution half-broken!

---

## AI Development Strategy

### Recommended AI Prompts

**Setup & Boilerplate**:
```
"Set up basic Nuxt 3 project with TypeScript and Chart.js - keep it simple"
"Create Vue 3 composable for loading JSON data with TypeScript interfaces"  
"Generate Chart.js bar chart component in Vue 3 with reactive props"
```

**Data Processing**:
```
"Use Lodash to filter array by multiple criteria and aggregate by field"
"Create TypeScript interface for HubSpot evaluation data structure"  
"Build reactive filter function that updates Chart.js data"
```

**Component Architecture**:
```
"Create reusable Vue 3 dropdown filter component with TypeScript"
"Build Chart.js wrapper component that handles data updates cleanly"
"Design composable for managing filter state across components"
```

### What to Let AI Handle
- Boilerplate setup and configuration
- Chart.js configuration details  
- Data processing and aggregation patterns
- TypeScript interface generation
- Basic component structure

### What to Focus Your Brain On
- Component architecture decisions
- Data flow and state management  
- User experience and filtering logic
- Documentation and progress tracking
- Prioritization and scope management

---

## Final Reminders

**This Simulates Real Work**: At OMR Reviews, similar dashboards often take multiple days. Don't stress about finishing everything!

**Quality Over Quantity**: One working chart with great documentation beats three broken charts.

**Document Everything**: Your About page is 50% of the evaluation. Be thorough, honest, and specific.

**Use Your Tools**: AI assistance is expected and encouraged. Document what helped vs. what you decided.

**When in Doubt**: Make reasonable assumptions and document them. We want to see how you think through problems.

---

**Ready to build?** Load up your AI assistant, grab some coffee, and show us how you approach real-world frontend challenges!

*Questions? Make assumptions and document them - that's what we do in production too.*