# Pertemuan 15: Final Project — Integrated Application

## 1. Project Overview

**Objective:** Build a comprehensive Node.js application yang mengintegrasikan:
- Minimal 2 advanced algorithms (D&C, DP, Greedy, Backtracking, MST, Shortest Path)
- Minimal 3 design patterns (creational, structural, behavioral)
- Clean code + SOLID principles
- Full unit test coverage

**Time:** 2 minggu
**Format:** Individual atau pair
**Deliverables:** Code + Documentation + Presentation (15 min)

## 2. Project Ideas

### Option 1: E-Commerce Recommendation Engine
- **Algorithms:** KD-Tree nearest neighbor (D&C variant) + Dynamic Programming (LCS for user preference matching)
- **Patterns:** Factory (recommendation strategies), Observer (price change notifications), Decorator (caching)
- **Deliverables:**
  - User similarity computation
  - Product recommendations
  - Price tracking system

### Option 2: Network Route Optimization
- **Algorithms:** Dijkstra (shortest path) + Kruskal (minimum spanning tree for infrastructure)
- **Patterns:** Strategy (different routing algorithms), Command (route history/undo), Facade (complex routing)
- **Deliverables:**
  - Multi-destination route planning
  - Infrastructure cost optimization
  - Route history & replay

### Option 3: Task Scheduler with Constraints
- **Algorithms:** Backtracking (task scheduling) + Greedy (priority ordering)
- **Patterns:** Builder (task configuration), Strategy (scheduling algorithms), Template Method (execution pipeline)
- **Deliverables:**
  - Constraint-based scheduling
  - Conflict detection
  - Execution report

### Option 4: Text Search & Processing
- **Algorithms:** Dynamic Programming (Edit Distance) + Backtracking (pattern matching)
- **Patterns:** Decorator (text processors), Iterator (traverse results), Command (search history)
- **Deliverables:**
  - Fuzzy search
  - Pattern matching
  - Processing pipeline

## 3. Technical Requirements

### Must Have
- ✅ Node.js + JavaScript ES6+
- ✅ Package.json dengan dependencies
- ✅ Unit tests (Jest / Mocha) dengan >80% coverage
- ✅ README dengan setup instructions
- ✅ Clean code + SOLID principles
- ✅ Documented API / usage examples

### Nice to Have
- 🌟 GitHub repository dengan commit history
- 🌟 CI/CD (GitHub Actions)
- 🌟 Performance benchmarks
- 🌟 Docker support
- 🌟 Web UI (optional)

## 4. Evaluation Criteria

| Aspek | Weight | Notes |
|-------|--------|-------|
| **Algorithm Implementation** | 25% | Correctness, complexity analysis, edge cases |
| **Design Pattern Usage** | 20% | Appropriate selection, correct implementation |
| **Code Quality** | 20% | SOLID, readability, maintainability, tests |
| **Documentation** | 15% | README, comments, API docs, architecture diagram |
| **Presentation** | 20% | Clarity, demo, ability to explain design choices |

## 5. Project Structure

```
my-project/
├── src/
│   ├── algorithms/
│   │   ├── index.js
│   │   ├── dijkstra.js
│   │   └── edit-distance.js
│   ├── patterns/
│   │   ├── factory.js
│   │   ├── observer.js
│   │   └── decorator.js
│   ├── services/
│   │   ├── recommendation.js
│   │   └── scheduler.js
│   └── utils/
│       └── validators.js
├── test/
│   ├── algorithms.test.js
│   ├── patterns.test.js
│   └── services.test.js
├── docs/
│   ├── ARCHITECTURE.md
│   ├── API.md
│   └── DESIGN_PATTERNS.md
├── .github/workflows/
│   └── test.yml
├── jest.config.js
├── package.json
└── README.md
```

## 6. Sample Skeleton (E-Commerce)

```javascript
// src/algorithms/similarity.js
class UserSimilarity {
    constructor(userPreferences) {
        this.users = userPreferences;
    }
    
    // Dynamic Programming: Longest Common Subsequence untuk matching preferences
    findSimilarUsers(userId, k = 5) {
        const target = this.users[userId];
        const similarities = [];
        
        for (const [otherId, preferences] of Object.entries(this.users)) {
            if (otherId === userId) continue;
            const score = this.computeLCS(target, preferences);
            similarities.push({ userId: otherId, score });
        }
        
        return similarities.sort((a, b) => b.score - a.score).slice(0, k);
    }
    
    computeLCS(pref1, pref2) {
        // DP implementation for edit distance / LCS
    }
}

// src/patterns/recommendationFactory.js
class RecommendationStrategyFactory {
    static create(strategyType) {
        const strategies = {
            'collaborative': CollaborativeFiltering,
            'content': ContentBased,
            'hybrid': HybridRecommendation,
        };
        return new strategies[strategyType]();
    }
}

// src/services/recommendationService.js
class RecommendationService {
    constructor(userSimilarity, factory, logger) {
        this.similarity = userSimilarity;
        this.factory = factory;
        this.logger = logger;
    }
    
    recommend(userId, strategy = 'collaborative') {
        const algo = this.factory.create(strategy);
        const similar = this.similarity.findSimilarUsers(userId);
        return algo.recommend(userId, similar);
    }
}

// test/services.test.js
describe('RecommendationService', () => {
    it('should return top 5 recommendations', () => {
        // Mock setup
        const service = new RecommendationService(mockSimilarity, factory, mockLogger);
        const result = service.recommend('USER-1');
        
        expect(result).toHaveLength(5);
        expect(result[0].score).toBeGreaterThan(result[4].score);
    });
});
```

## 7. Presentation Outline

**15 minutes:**
1. Problem statement (2 min)
2. Architecture diagram (1 min)
3. Algorithm explanation (3 min)
4. Design patterns overview (3 min)
5. Live demo (4 min)
6. Q&A (2 min)

**Slides:**
- Title
- Problem & motivation
- Architecture diagram
- Algorithm complexity analysis
- Design patterns used
- Code walkthrough (1-2 key files)
- Demo results
- Challenges & lessons learned

## 8. Common Pitfalls to Avoid

- ❌ Gold plating (over-engineering)
- ❌ Patterns for the sake of patterns
- ❌ No tests
- ❌ Hardcoded dependencies
- ❌ No documentation
- ❌ Copy-paste code from tutorials
- ❌ No complexity analysis
- ❌ Ignoring edge cases

## 9. Resources

- **Algorithm Visualization:** https://visualgo.net
- **Design Patterns:** https://refactoring.guru
- **Testing:** https://jestjs.io (Jest docs)
- **Clean Code:** Robert Martin's "Clean Code" book
- **Performance:** https://nodejs.org/en/docs/guides/simple-profiling/

## 10. Submission Checklist

- [ ] Code committed to GitHub
- [ ] README with setup instructions
- [ ] All tests passing (`npm test`)
- [ ] Code coverage > 80% (`npm run coverage`)
- [ ] No console.log / debugging code
- [ ] SOLID principles followed
- [ ] Architecture diagram included
- [ ] Presentation slides ready
- [ ] Demo script prepared
- [ ] Performance benchmarks run

## 11. Grading Rubric

**Algorithm (25%):**
- Correct implementation: 10 pts
- Complexity analysis: 8 pts
- Edge cases handled: 7 pts

**Design Patterns (20%):**
- Appropriate patterns chosen: 8 pts
- Correct implementation: 7 pts
- No over-engineering: 5 pts

**Code Quality (20%):**
- SOLID followed: 7 pts
- Test coverage: 7 pts
- Readability & documentation: 6 pts

**Documentation (15%):**
- README complete: 5 pts
- API documented: 5 pts
- Architecture clear: 5 pts

**Presentation (20%):**
- Clarity: 7 pts
- Technical depth: 7 pts
- Demo works: 6 pts

**Total: 100 pts**

## 12. Example Timeline (2 weeks)

**Week 1:**
- Days 1-2: Design & architecture
- Days 3-4: Core algorithms
- Days 5: Design patterns & refactor
- Days 6-7: Tests & documentation

**Week 2:**
- Days 1-2: Polish & edge cases
- Days 3-4: Performance optimization
- Days 5: Documentation & demo prep
- Days 6-7: Presentation practice

Good luck! 🚀
