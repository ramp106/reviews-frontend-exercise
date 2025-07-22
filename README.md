# Frontend Take-Home Challenge: HubSpot Marketing Hub Insights Dashboard

## Quick Start

This is a **4-hour frontend challenge** for OMR Reviews. You'll build an interactive dashboard visualizing HubSpot evaluation insights.

**TL;DR**: Vue 3 + Chart.js dashboard with filtering. Incomplete solutions are expected and encouraged!

## What's Included

```
project/
├── data.json          # HubSpot evaluation dataset (~15k conversations)
├── README.md          # This file - setup instructions
└── CHALLENGE.md       # Detailed requirements and evaluation criteria
```

## Setup Instructions

### 1. Create New Nuxt Project

```bash
# Create project
npx nuxi@latest init hubspot-dashboard
cd hubspot-dashboard

# Install dependencies
npm install

# Add required packages
npm install chart.js lodash
npm install -D @types/lodash
```

### 2. Add the Data File

Place the provided `data.json` file in your project root:

```
hubspot-dashboard/
├── data.json          # <- Put the provided file here
├── nuxt.config.ts
└── ...
```

### 3. Configure TypeScript (Optional but Recommended)

The challenge includes TypeScript interfaces - copy them from `CHALLENGE.md` or create your own.

### 4. Development

```bash
# Start development server
npm run dev

# Your app will be at http://localhost:3000
```

## What You're Building

**Core Task**: Interactive dashboard with one Chart.js visualization and basic filtering

**Required Routes**:
- `/` - Dashboard with chart and filters
- `/about` - Your progress documentation (very important!)

**Tech Stack**:
- Vue 3 + Nuxt 3 (Composition API)
- Chart.js for visualizations  
- Tailwind CSS for styling
- TypeScript (recommended)

## Data Overview

The `data.json` contains real conversation data from users evaluating HubSpot Marketing Hub:

- **15,847 total conversations** across 2024 quarters
- **3 questions** with response data:
  - Q1: Current productivity tools being used
  - Q2: Integration requirements 
  - Q3: Evaluation concerns
- **Filterable by**: Company size, industry, quarter

**Pick ONE question** to visualize - you don't need all three!

## Success Criteria

✅ **One working chart** displaying HubSpot data  
✅ **Basic filtering** that updates the chart  
✅ **Detailed About page** documenting your approach  
✅ **Clean code structure** with good component design

**Important**: A working chart + great documentation beats multiple broken features!

## Time Expectations

This is designed for **~4 hours** with AI assistance. Most candidates will not finish everything - that's completely normal and expected.

**Suggested Timeline**:
- Hour 1: Setup + basic chart
- Hour 2: Data processing + aggregation  
- Hour 3: Filtering implementation
- Hour 4: Documentation + polish

## AI Development Tips

Feel free to use Cursor, Claude, GitHub Copilot, etc. Some helpful prompts:

```
"Set up a basic Nuxt 3 project with TypeScript - keep it simple"
"Create a Vue 3 composable that loads and filters HubSpot data"  
"Build a simple Chart.js bar chart component with TypeScript"
"Use Lodash to aggregate numResponses by responseValue for filtered data"
```

## Getting Help

**Stuck on something?** Document it in your About page:
- What you tried
- What didn't work  
- What you'd do next

This kind of problem-solving documentation is valuable for evaluation!

## Evaluation Focus

We care more about:
- **Clear thinking** and problem-solving approach
- **Progress documentation** (even if incomplete)  
- **Code quality** over feature quantity
- **Realistic scope** management

Less about:
- Pixel-perfect design
- Complete feature implementation
- Advanced Chart.js features

## Questions?

If anything is unclear, make reasonable assumptions and document them in your About page.

**Ready?** Read through `CHALLENGE.md` for detailed requirements, then start coding!

---

*Remember: This simulates real work at OMR Reviews. In production, similar dashboards take multiple days. Focus on showing us how you think and solve problems!*