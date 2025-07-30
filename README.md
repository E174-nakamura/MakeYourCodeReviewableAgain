# Promise Flow Analyzer

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![JavaScript](https://img.shields.io/badge/JavaScript-ES2024-yellow.svg)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![TypeScript Ready](https://img.shields.io/badge/TypeScript-Ready-blue.svg)](https://www.typescriptlang.org/)

> **With AI Era, Visualize and analyze JavaScript/TypeScript code to prevent common async/await anti-patterns**

A developer tool that automatically detects Promise-related issues and visualizes asynchronous control flow, specifically designed for the AI-assisted coding era.

## 🎯 Problem Statement

With AI-assisted development, teams can now produce 3x more code with the same number of developers. However, quality assurance mechanisms haven't kept pace, leading to:

- **Ambiguous responsibility boundaries**
- **Inconsistent code granularity**
- **Improper Promise/async-await usage** (especially in web development)

The result: "Everyone uses AI freely, code quality collapses."

## 💡 Solution

**Promise Flow Analyzer** provides automatic system-wide consistency checks while maintaining the convenience of rapid development environments (Web/Python/etc.).

Combines Rust-like strictness with TypeScript/Python-like ease of use, focusing on making code **human-readable and review-friendly**.

## ✨ Key Features

### 🔍 Promise Flow Visualization
- **Visual flow diagrams** showing parallel execution opportunities and error handling scope
- **Mermaid.js integration** for GitHub-compatible diagrams
- **Interactive analysis** with hover details and suggestions

### 🚨 Anti-Pattern Detection
- Sequential `await` statements (parallelization opportunities)
- Missing `await` on `.json()` calls
- Insufficient error handling
- Promise hell (`.then()` chains)
- Mixed `async/await` and `.then()` patterns
- Sequential execution in loops

### 💡 Concrete Improvement Suggestions
- Specific `Promise.all()` usage examples
- Error handling recommendations
- Performance optimization hints

### 🚀 Developer Experience
- **Copy-paste ready**: Just paste your function and click analyze
- **8 test cases included**: Covering typical AI-generated code issues
- **Instant feedback**: Real-time analysis and suggestions

## 🎮 Demo

```javascript
// ❌ BAD: Sequential execution
async function fetchUserData(userId) {
  const user = await fetch(`/api/users/${userId}`);
  const userData = await user.json();
  
  const posts = await fetch(`/api/users/${userId}/posts`);
  const postsData = await posts.json();
  
  return { user: userData, posts: postsData };
}

// ✅ GOOD: Parallel execution
async function fetchUserData(userId) {
  try {
    const [userRes, postsRes] = await Promise.all([
      fetch(`/api/users/${userId}`),
      fetch(`/api/users/${userId}/posts`)
    ]);
    
    const [userData, postsData] = await Promise.all([
      userRes.json(),
      postsRes.json()
    ]);
    
    return { user: userData, posts: postsData };
  } catch (error) {
    console.error('Failed to fetch user data:', error);
    throw new Error('User data fetch failed');
  }
}
```

## 🛠️ Technology Stack

**Current:**
- Vanilla JavaScript (ES2024+)
- Mermaid.js for diagrams
- CSS3 with theme support

**Future plan:**
- TypeScript for type safety
- Node.js for CLI tools
- Language Server Protocol
- WebAssembly for performance
- 
## 🏗️ Architecture(now working)

```
promise-flow-analyzer/
├── src/
│   ├── analyzer.js       # Promise flow analysis engine
│   ├── diagram.js        # Mermaid diagram generation
│   ├── examples.js       # Test cases and examples
│   └── themes.js         # UI theme management
├── styles/
│   └── main.css         # Styling and themes
└── index.html           # Web interface
```


## 🎯 Test Cases

The tool includes 8 comprehensive test cases covering:

1. **Basic**: Simple await usage
2. **Parallel**: Subtle parallelization bugs
3. **Complex**: Proper Promise.all usage
4. **❌ Bad Examples**: Typical AI-generated code issues
5. **✅ Good Examples**: Best practice implementations
6. **❌ Promise Hell**: .then() chain problems
7. **❌ Mixed Patterns**: async/await + .then() mixing

## 🚀 Roadmap

### Phase 1: Core Tool (Current)
- [x] Web-based analyzer
- [x] Mermaid.js visualization
- [x] Basic anti-pattern detection
- [ ] TypeScript AST integration
- [ ] Enhanced error handling

### Phase 2: Developer Integration 
- [ ] **CLI Tool**: CI/CD pipeline integration
- [ ] **VSCode Extension**: Real-time analysis
- [ ] **ESLint Plugin**: Existing workflow integration
- [ ] **npm Package**: Programmatic usage

### Phase 3: Advanced Features 
- [ ] **Language Server Protocol**: IDE integration
- [ ] **AI Integration**: OpenAI/Claude API for suggestions
- [ ] **Team Dashboard**: Code quality metrics
- [ ] **Custom Rules**: Project-specific configurations

### Phase 4: Enterprise & Ecosystem 
- [ ] **SaaS Platform**: Team collaboration features
- [ ] **On-premises**: Enterprise deployment
- [ ] **Plugin System**: Community extensions
- [ ] **Integration APIs**: Third-party tool connections

## 🎯 Target Use Cases

### AI-Assisted Development Teams
- **Code Review**: Visual flow analysis for faster revi
