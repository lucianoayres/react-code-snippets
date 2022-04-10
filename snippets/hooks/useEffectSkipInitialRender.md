# useAsync Hook

Run useEffect with a dependency array and skip initial render.

[View it on CondeSandbox](https://codesandbox.io/s/useeffect-skip-initial-render-wooguc?file=/src/App.js 'JavaScript useEffectSkipInitialRender code snippet')

## JavaScript

```javascript
import { useEffect, useRef, useState } from 'react'

import './styles.css'

export default function App() {
  const [count, setCount] = useState(0)
  const initialRender = useRef(true)

  useEffect(() => {
    if (initialRender.current) {
      console.log('Execute only on first render')
      initialRender.current = false
    } else {
      console.log(
        count > 1
          ? `Executed ${count} times after the first render`
          : `Executed ${count} time after the first render`
      )
    }
  }, [count])

  return (
    <div className="App">
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Add Count</button>
    </div>
  )
}
```
