import { useState, useCallback, useMemo } from 'react'
import './styles.css'

const generateId = (() => {
  let id = 0
  return () => String(++id)
})()

const FILTERS = {
  ALL: 'all',
  ACTIVE: 'active',
  COMPLETED: 'completed'
}

function AddToDoForm({ onChange, onSubmit, value }) {
  const handleSubmit = (event) => {
    event.preventDefault()
    if (value.trim()) {
      onSubmit()
    }
  }

  const handleKeyDown = (event) => {
    if (event.key === 'Escape') {
      onChange('')
    }
  }

  return (
    <form className="input-container" onSubmit={handleSubmit}>
      <input
        value={value}
        type="text"
        className="task-input"
        placeholder="Add new task..."
        maxLength="200"
        autoComplete="off"
        onChange={(event) => onChange(event.target.value)}
        onKeyDown={handleKeyDown}
      />
      <button className="add-button" type="submit" disabled={!value.trim()} aria-label="Add task">
      </button>
    </form>
  )
}

function TaskEdit({ onEdit, value: defaultValue, onClose }) {
  const [value, setValue] = useState(defaultValue)

  const handleSubmit = (event) => {
    event.preventDefault()
    const trimmedValue = value.trim()
    if (trimmedValue) {
      onEdit(trimmedValue)
      onClose()
    }
  }

  const handleKeyDown = (event) => {
    if (event.key === 'Escape') {
      onClose()
    }
  }

  return (
    <form onSubmit={handleSubmit} className="edit-form">
      <input
        className="input__edit"
        value={value}
        type="text"
        onChange={(event) => setValue(event.target.value)}
        onKeyDown={handleKeyDown}
        placeholder={defaultValue}
        autoFocus
      />
      <button type="submit" className="task__edit__btn" disabled={!value.trim()}>
        ✓
      </button>
      <button className="task__cancel__btn" type="button" onClick={onClose} aria-label="Cancel edit">
        ✕
      </button>
    </form>
  )
}

function Task({ value, onDelete, id, onComplete, isDone, onEdit }) {
  const [isEditing, setIsEditing] = useState(false)

  const handleEdit = useCallback((newValue) => {
    onEdit(id, newValue)
  }, [id, onEdit])

  const handleDelete = useCallback(() => {
    onDelete(id)
  }, [id, onDelete])

  const handleComplete = useCallback((event) => {
    onComplete(id, event.target.checked)
  }, [id, onComplete])

  const handleKeyDown = (event) => {
    if (event.key === 'Enter' && !isEditing) {
      setIsEditing(true)
    }
    if (event.key === ' ' && !isEditing) {
      event.preventDefault()
      onComplete(id, !isDone)
    }
  }

  if (isEditing) {
    return (
      <li className="task-item editing">
        <TaskEdit
          onEdit={handleEdit}
          onClose={() => setIsEditing(false)}
          value={value}
        />
      </li>
    )
  }

  return (
    <li className={`task-item ${isDone ? 'completed' : ''}`}>
      <div
        className={`task-checkbox ${isDone ? 'checked' : ''}`}
        onClick={() => onComplete(id, !isDone)}
        role="checkbox"
        aria-checked={isDone}
        tabIndex={0}
        onKeyDown={(e) => e.key === 'Enter' && onComplete(id, !isDone)}
      ></div>
      <span
        className="task-text"
        onDoubleClick={() => setIsEditing(true)}
        onKeyDown={handleKeyDown}
        tabIndex={0}
        role="button"
        aria-label={`Edit task: ${value}`}
      >
        {value}
      </span>
      <button
        className="delete-button"
        type="button"
        onClick={handleDelete}
        aria-label="Delete task"
      >
      </button>
    </li>
  )
}

function FilterButtons({ currentFilter, onFilterChange }) {
  return (
    <div className="filter-buttons">
      {Object.entries(FILTERS).map(([key, value]) => (
        <button
          key={value}
          className={`filter-btn ${currentFilter === value ? 'active' : ''}`}
          onClick={() => onFilterChange(value)}
        >
          {key.charAt(0) + key.slice(1).toLowerCase()}
        </button>
      ))}
    </div>
  )
}

function TaskList({ todos, onDelete, onComplete, onEdit, filter }) {
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

  if (todos.length === 0) {
    return (
      <div className="empty-state">
        <div className="empty-state-icon">📝</div>
        <div className="empty-state-text">No tasks yet</div>
        <div className="empty-state-subtext">Add your first task above</div>
      </div>
    )
  }

  if (filteredTodos.length === 0) {
    return (
      <div className="empty-state">
        <div className="empty-state-icon">🎯</div>
        <div className="empty-state-text">No {filter} tasks</div>
        <div className="empty-state-subtext">Try a different filter</div>
      </div>
    )
  }

  return (
    <ul className="tasks-list">
      {filteredTodos.map((todo) => (
        <Task
          key={todo.id}
          value={todo.value}
          id={todo.id}
          isDone={todo.isDone}
          onComplete={onComplete}
          onDelete={onDelete}
          onEdit={onEdit}
        />
      ))}
    </ul>
  )
}

function BulkActions({ todos, onToggleAll, onClearCompleted }) {
  const completedCount = todos.filter(todo => todo.isDone).length
  const allCompleted = todos.length > 0 && completedCount === todos.length

  if (todos.length === 0) return null

  return (
    <div className="bulk-actions">
      <button
        className={`bulk-btn ${allCompleted ? 'active' : ''}`}
        onClick={() => onToggleAll(!allCompleted)}
        aria-label={allCompleted ? 'Mark all as incomplete' : 'Mark all as complete'}
      >
        {allCompleted ? '↺' : '✓'} {allCompleted ? 'Undo All' : 'All Done'}
      </button>
      {completedCount > 0 && (
        <button
          className="bulk-btn clear-btn"
          onClick={onClearCompleted}
          aria-label={`Clear ${completedCount} completed tasks`}
        >
          🗑 Clear Done ({completedCount})
        </button>
      )}
    </div>
  )
}

export default function App() {
  const [value, setValue] = useState('')
  const [todos, setTodos] = useState([])
  const [filter, setFilter] = useState(FILTERS.ALL)

  const handleAddTodo = useCallback(() => {
    const trimmedValue = value.trim()
    if (!trimmedValue || trimmedValue.length > 200) return

    const newTodo = {
      value: trimmedValue,
      id: generateId(),
      isDone: false,
      createdAt: new Date().toISOString()
    }

    setTodos(prevTodos => [...prevTodos, newTodo])
    setValue('')
  }, [value])

  const handleDeleteTodo = useCallback((id) => {
    setTodos(prevTodos => prevTodos.filter(todo => todo.id !== id))
  }, [])

  const handleCompleteTodo = useCallback((id, isDone) => {
    setTodos(prevTodos =>
      prevTodos.map(todo =>
        todo.id === id ? { ...todo, isDone } : todo
      )
    )
  }, [])

  const handleEditTodo = useCallback((id, newValue) => {
    setTodos(prevTodos =>
      prevTodos.map(todo =>
        todo.id === id ? { ...todo, value: newValue } : todo
      )
    )
  }, [])

  const handleToggleAll = useCallback((shouldComplete) => {
    setTodos(prevTodos =>
      prevTodos.map(todo => ({ ...todo, isDone: shouldComplete }))
    )
  }, [])

  const handleClearCompleted = useCallback(() => {
    setTodos(prevTodos => prevTodos.filter(todo => !todo.isDone))
  }, [])

  const stats = useMemo(() => {
    const completedCount = todos.filter(todo => todo.isDone).length
    const totalCount = todos.length
    const remainingCount = totalCount - completedCount
    return { completedCount, totalCount, remainingCount }
  }, [todos])

  return (
    <div className="container">
      <h1 className="title">My Tasks</h1>
      
      <div className="input-section">
        <AddToDoForm
          value={value}
          onChange={setValue}
          onSubmit={handleAddTodo}
        />
      </div>

      {todos.length > 0 && (
        <>
          <FilterButtons
            currentFilter={filter}
            onFilterChange={setFilter}
          />
          <BulkActions
            todos={todos}
            onToggleAll={handleToggleAll}
            onClearCompleted={handleClearCompleted}
          />
        </>
      )}
      
      <div className="tasks-container">
        <TaskList
          todos={todos}
          onDelete={handleDeleteTodo}
          onComplete={handleCompleteTodo}
          onEdit={handleEditTodo}
          filter={filter}
        />
      </div>
      
      {stats.totalCount > 0 && (
        <div className="stats">
          <div className="stats-item">
            <span className="stats-number">{stats.totalCount}</span>
            <span>Total</span>
          </div>
          <div className="stats-item">
            <span className="stats-number">{stats.completedCount}</span>
            <span>Done</span>
          </div>
          <div className="stats-item">
            <span className="stats-number">{stats.remainingCount}</span>
            <span>Left</span>
          </div>
        </div>
      )}
    </div>
  )
}
