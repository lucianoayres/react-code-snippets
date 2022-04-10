# api-client Function

Make HTTP requests with custom configurations.

[View it on CodeSandbox](https://codesandbox.io/s/async-river-m2tgbp?file=/src/App.tsx 'TypeScript api-client function code snippet')

[Author: Kent C. Dodds](https://github.com/kentcdodds/bookshelf 'Author: Kent C. Dodds')

## JavaScript

```javascript
function client(endpoint, customConfig = {}) {
  const config = {
    method: 'GET',
    ...customConfig
  }

  return fetch(`${process.env.REACT_APP_API_URL}/${endpoint}`, config).then(
    (response) => response.json()
  )
}

export { client }
```

## TypeScript

```typescript
function client(endpoint: string, customConfig = {}): Promise<[]> {
  console.log(`${process.env.REACT_APP_API_URL}/${endpoint}`)
  const config = {
    method: 'GET',
    ...customConfig
  }

  return fetch(`${process.env.REACT_APP_API_URL}/${endpoint}`, config).then(
    (response) => response.json()
  )
}
```

### Example Usage (JavaScript)

```javascript
  useEffect(() => {
    if (!queried) {
      return
    }
    const data = await client(`books?query=${encodeURIComponent(query)}`)
  }, [])
```

### Example Usage (TypeScript)

```typescript
import { useState, useEffect } from 'react'

import './styles.css'

export default function App() {
  const [apiData, setApiData] = useState([])
  const gitHubUserRepoEndpoint = 'users/lucianoayres/repos'

  function client(endpoint: string, customConfig = {}): Promise<[]> {
    console.log(`${process.env.REACT_APP_API_URL}/${endpoint}`)
    const config = {
      method: 'GET',
      ...customConfig
    }

    return fetch(`${process.env.REACT_APP_API_URL}/${endpoint}`, config).then(
      (response) => response.json()
    )
  }

  useEffect(() => {
    const fetchData = async () => {
      client(gitHubUserRepoEndpoint).then((data: []) => {
        setApiData(data)
      })
    }
    fetchData()
  }, [])

  return (
    <div className="App">
      <h1>GitHub API URL</h1>
      <p>{process.env.REACT_APP_API_URL}</p>
      <h2>Endpoint</h2>
      <p>{gitHubUserRepoEndpoint}</p>
      <ul>
        {apiData.map(({ id, name }) => (
          <li key={id}>{name}</li>
        ))}
      </ul>
    </div>
  )
}
```
