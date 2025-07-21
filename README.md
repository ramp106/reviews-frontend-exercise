# Frontend Take-Home Challenge: Insights Dashboard

## Company Context
You're joining **OMR Reviews** as a **Frontend Developer**. We're building next-generation market intelligence tools that help businesses make better software decisions. This challenge simulates real work you'd do - creating data visualization dashboards that turn complex conversation data into actionable insights.

**The Scenario:** Our AI systems have analyzed 15,847 customer conversations about productivity tools, extracting structured insights about user preferences, requirements, and concerns. Your job is to make this data accessible and actionable through an interactive dashboard.

---

## The Challenge

**Your Mission:** Build a Vue.js dashboard that enables users to explore productivity tool insights through interactive filtering and data visualization.

**Core Requirements:**
- Process and visualize complex multi-dimensional data
- Implement dynamic filtering across multiple chart types  
- Create professional data visualizations
- Demonstrate clean component architecture and modern Vue.js practices

---

## Technical Constraints

| Constraint | Requirement |
|------------|-------------|
| **Framework** | Vue 3 + Nuxt 3 (Composition API) |
| **Styling** | Tailwind CSS |
| **Charts** | Chart.js for visualizations |
| **Expected Effort** | ~4 hours |
| **Browser Support** | Chrome desktop |
| **Additional Libraries** | Use any appropriate tools and libraries that help you succeed |

---

## Dataset Overview

You'll work with **real conversation data** from the provided `data.json` file.

**Data Structure:**
- **3 Questions** about productivity tools, requirements, and concerns
- **Multi-dimensional data** filterable by:
  - **Company Size:** Startup (1-10), Small (11-50), Medium (51-200), Large (201-1000), Enterprise (1000+)
  - **Industry:** E-Commerce, Media & Marketing, Finance & Banking, SaaS & Technology, Education, Consulting  
  - **Quarter:** 2024-Q1, Q2, Q3, Q4

**Question 1 - Tools Used:** 
```json
{
  "companySize": "Startup (1-10)",
  "industry": "E-Commerce", 
  "quarter": "2024-Q1",
  "responseValue": "Google Workspace",
  "numResponses": 12
}
```
**Question 2 - Requirements:** Simple strings like `"REST API Access"`, `"Zapier Integration"`

**Question 3 - Concerns:** Simple strings like `"Too Expensive"`, `"Data Privacy & Security"`

---

## Your Assignment

### **A. Core Visualizations**
**Must Build (Required):**
1. **Tools Chart** - Bar chart from Question 1 data showing productivity tools by usage frequency
2. **Requirements Chart** - Bar chart from Question 2 data showing requirement types by frequency


### **B. Data Processing & Filtering**
Build a filtering system that works across all visualizations by the **dimensional attributes**:
- **Company Size:** Startup, Small, Medium, Large, Enterprise
- **Industry:** E-Commerce, Media & Marketing, Finance & Banking, SaaS & Technology, Education, Consulting  
- **Quarter:** 2024-Q1, Q2, Q3, Q4

**Important:** Filters should segment the data by these dimensions (e.g., "Show only Enterprise companies in Finance sector for Q3"), not filter the response values themselves (tools, requirements, concerns).

### **C. Application Structure**
- **Dashboard Route** (`/`) - Main interface with filters + charts
- **About Route** (`/about`) - Technical decisions and assumptions
- **Component Architecture** - Clean, reusable components

### **D. Polish & Documentation**
- Code cleanup and optimization
- About page explaining your approach
- Optional: Animations, tooltips, advanced features

---

## What to Submit

**Required Deliverables:**
1. **Working Vue 3 + Nuxt 3 application** with the dashboard functionality
2. **Source code** organized however you prefer (we don't dictate structure)
3. **README with setup instructions** - how to install and run your app
4. **About page** (`/about` route) explaining your technical approach and any assumptions

**What We'll Test:**
- Navigate to your dashboard (`/` route)
- Use the filter elements to see data update across charts
- View your about page to understand your approach
- Review your code organization and implementation

---

## What We're Evaluating

**Must-Have (Core Assessment):**
- Vue 3 Composition API proficiency
- Data processing and state management
- Chart.js implementation skills  
- Project architecture and structure

**Bonus Points:**
- Creative data parsing solutions
- Smooth user experience (animations, loading states)
- Smart default filtering (e.g., latest quarter selected)
- Advanced Chart.js features (tooltips, interactions)
- Clean code organization and naming

**Not Required:**
- Perfect visual design (focus on functionality)
- Comprehensive error handling
- Unit tests (explain strategy in About page if time allows)

---

## Getting Started

**Time Management Strategy:**
1. **Setup** (30 min) - Project initialization, basic routing
2. **Core Charts** (90 min) - Build charts with data aggregation and Chart.js implementation
3. **Data & Filtering** (60 min) - Add interactive filters that update charts
4. **Layout & UI** (30 min) - Layout and styling
5. **Polish** (30 min) - About page, final touches

**Data Processing:** All three questions use simple responseValue strings that can be used directly for chart categories. The main challenge is filtering and aggregating the data based on the dimensional attributes (company size, industry, quarter).

**Library Freedom:** While we've specified core technologies (Vue 3, Nuxt 3, Tailwind, Chart.js), feel free to use any additional tools and libraries that make you more productive. This could include data processing libraries (**Danfo.js**, **Arquero**, **Data-Forge**, **pandas-js**), state management solutions (**Pinia**, **Vuex**), utility libraries (**Lodash**, **Ramda**), or any other tools that help you build a better solution.

---

## Success Context

This challenge reflects real work at OMR Reviews. We regularly build dashboards that help users understand complex market data. We value:

- **Practical thinking** over perfect architecture
- **Clean, maintainable code** that teammates can understand
- **Realistic scope management** given time constraints

**Remember:** A working dashboard with 2 solid charts is better than an incomplete one with 3 ambitious features.

---

*Ready to dive in? We're excited to see your approach to this data visualization challenge!*

**Questions?** Focus on functionality first, then enhance if time allows. Good luck!
