# api-client Function

Make HTTP requests with custom configurations.

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

### Example Usage (JavaScript)

[View it on CodeSandbox](https://codesandbox.io/s/api-client-javascript-h3nh7h?file=/src/App.js 'Javascript api-client function code snippet')

```javascript
import { useState, useEffect } from 'react'
import './styles.css'

export default function App() {
  const [apiData, setApiData] = useState([])
  const gitHubUserRepoEndpoint = 'users/lucianoayres/repos'

  function client(endpoint, customConfig = {}) {
    const config = {
      method: 'GET',
      ...customConfig
    }

    return fetch(`${process.env.REACT_APP_API_URL}/${endpoint}`, config).then(
      (response) => response.json()
    )
  }

  useEffect(() => {
    client(gitHubUserRepoEndpoint).then((data) => {
      setApiData(data)
    })
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

### Example Usage (TypeScript)

[View it on CodeSandbox](https://codesandbox.io/s/async-river-m2tgbp?file=/src/App.tsx 'TypeScript api-client function code snippet')

```typescript
import { useState, useEffect } from 'react'
import './styles.css'

export default function App() {
  const [apiData, setApiData] = useState([])
  const gitHubUserRepoEndpoint = 'users/lucianoayres/repos'

  function client(endpoint: string, customConfig = {}): Promise<[]> {
    const config = {
      method: 'GET',
      ...customConfig
    }

    return fetch(`${process.env.REACT_APP_API_URL}/${endpoint}`, config).then(
      (response) => response.json()
    )
  }

  useEffect(() => {
    client(gitHubUserRepoEndpoint).then((data: []) => {
      setApiData(data)
    })
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
