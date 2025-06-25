# ✅ Modern Todo App

A sleek, feature-rich task management application built with React featuring glassmorphism design and advanced productivity features.

## ✨ Features

- **Smart Task Management** - Add, edit, delete, and organize tasks with intuitive controls
- **Advanced Filtering** - View all tasks, active only, or completed tasks
- **Bulk Operations** - Mark all complete/incomplete or clear completed tasks at once
- **Real-time Statistics** - Live counters for total, completed, and remaining tasks
- **Full Keyboard Support** - Complete keyboard navigation for power users
- **Responsive Design** - Perfect experience on desktop, tablet, and mobile
- **Smooth Animations** - Delightful micro-interactions and transitions
- **Accessibility First** - ARIA labels, focus indicators, and screen reader support

## 🛠️ Tech Stack

- **React 18+** - Modern React with hooks and functional components
- **CSS3** - Advanced styling with gradients, glassmorphism, and animations
- **JavaScript ES6+** - Modern JavaScript patterns and best practices
- **Local State Management** - Efficient React state with performance optimizations

## ⌨️ Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Enter` | Add new task or start editing |
| `Space` | Toggle task completion |
| `Escape` | Cancel editing or clear input |
| `Double Click` | Edit task text |
| `Tab` | Navigate between elements |

## 🎨 Design Highlights

- **Glassmorphism UI** - Modern frosted glass effect with backdrop blur
- **Gradient Backgrounds** - Beautiful purple-blue gradients for visual depth
- **Smooth Transitions** - 60fps animations throughout the interface
- **Color Psychology** - Green for completed, red for delete, blue for actions
- **Typography** - Clean Inter font for optimal readability
- **Micro-interactions** - Hover effects, button animations, and state feedback


## 💻 Installation & Usage

1. **Clone the repository**
   ```bash
   git clone https://github.com/fzpzmz/react-todo-app.git
   cd modern-todo-app
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start development server**
   ```bash
   npm start
   ```

4. **Build for production**
   ```bash
   npm run build
   ```

## 🔧 Core Functionality

The app implements a complete task management system with:

- **State Management** - Efficient React hooks with useCallback and useMemo
- **Performance Optimization** - Memoized components and minimized re-renders
- **Data Persistence** - Task state maintained during session
- **Input Validation** - 200 character limit and trim whitespace
- **Sort Logic** - Incomplete tasks first, then by creation date
- **Filter System** - Dynamic filtering without performance impact

## 🌟 Code Highlights

```javascript
// Performance optimized task filtering
const filteredTodos = useMemo(() => {
  let filtered = todos
  if (filter === FILTERS.ACTIVE) {
    filtered = todos.filter(todo => !todo.isDone)
  } else if (filter === FILTERS.COMPLETED) {
    filtered = todos.filter(todo => todo.isDone)
  }
  return filtered.sort((a, b) => {
    if (a.isDone === b.isDone) {
      return new Date(b.createdAt) - new Date(a.createdAt)
    }
    return a.isDone - b.isDone
  })
}, [todos, filter])

// Memoized callbacks for optimal performance
const handleAddTodo = useCallback(() => {
  const trimmedValue = value.trim()
  if (!trimmedValue || trimmedValue.length > 200) return
  // ... task creation logic
}, [value])
```

## 📊 Browser Support

- ✅ Chrome 60+
- ✅ Firefox 55+
- ✅ Safari 12+
- ✅ Edge 79+

## 📧 Contact

**Timofei Starovoitov** - fezpezmez@gmail.com

Project Link: [https://github.com/fzpzmz/react-todo-app](https://github.com/fzpzmz/react-todo-app)

---