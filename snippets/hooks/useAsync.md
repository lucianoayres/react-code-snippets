# useAsync Hook

Run async requests with status, data loading and error handling states.

[Author: Kent C. Dodds](https://github.com/kentcdodds/bookshelf 'Author: Kent C. Dodds')

Related: [api-client](https://github.com/lucianoayres/react-code-snippets/blob/main/snippets/utils/api-client.md 'api-client function') function

## JavaScript

```javascript
import { useRef, useLayoutEffect, useCallback, useReducer } from 'react'

function useSafeDispatch(dispatch) {
  const mounted = useRef(false)
  useLayoutEffect(() => {
    mounted.current = true
    return () => (mounted.current = false)
  }, [])
  return useCallback(
    (...args) => (mounted.current ? dispatch(...args) : void 0),
    [dispatch]
  )
}

const defaultInitialState = { status: 'idle', data: null, error: null }

export function useAsync(initialState) {
  const initialStateRef = useRef({
    ...defaultInitialState,
    ...initialState
  })
  const [{ status, data, error }, setState] = useReducer(
    (s, a) => ({ ...s, ...a }),
    initialStateRef.current
  )

  const safeSetState = useSafeDispatch(setState)

  const setData = useCallback(
    (data) => safeSetState({ data, status: 'resolved' }),
    [safeSetState]
  )
  const setError = useCallback(
    (error) => safeSetState({ error, status: 'rejected' }),
    [safeSetState]
  )
  const reset = useCallback(
    () => safeSetState(initialStateRef.current),
    [safeSetState]
  )

  const run = useCallback(
    (promise) => {
      if (!promise || !promise.then) {
        throw new Error(
          `The argument passed to useAsync().run must be a promise. Maybe a function that's passed isn't returning anything?`
        )
      }
      safeSetState({ status: 'pending' })
      return promise.then(
        (data) => {
          setData(data)
          return data
        },
        (error) => {
          setError(error)
          return Promise.reject(error)
        }
      )
    },
    [safeSetState, setData, setError]
  )

  return {
    isIdle: status === 'idle',
    isLoading: status === 'pending',
    isError: status === 'rejected',
    isSuccess: status === 'resolved',

    setData,
    setError,
    error,
    status,
    data,
    run,
    reset
  }
}
```

### Example Usage

[View it on CodeSandbox]('JavaScript useAsync code snippet')

```javascript
import { useEffect } from 'react'
import { useAsync } from './utils/useAsync'
import { client } from './utils/api-client'

import './styles.css'

export default function App() {
  const { data, error, run, isLoading, isError, isSuccess } = useAsync()
  const gitHubUserRepoEndpoint = 'users/lucianoayres/repos'

  useEffect(() => {
    run(client(gitHubUserRepoEndpoint))
  }, [run])

  return (
    <div className="App">
      <h1>GitHub API URL</h1>
      <p>{process.env.REACT_APP_API_URL}</p>
      <h2>Endpoint</h2>
      <p>{gitHubUserRepoEndpoint}</p>

      {isLoading ? (
        <p>Loading...</p>
      ) : isError ? (
        <p>Error: {error.message}</p>
      ) : null}

      {isSuccess ? (
        data?.length ? (
          <ul>
            {data.map(({ id, name }) => (
              <li key={id} aria-label={name}>
                {name}
              </li>
            ))}
          </ul>
        ) : (
          <p>No repositories found. Try another search.</p>
        )
      ) : null}
    </div>
  )
}
```

## TypeScript

```typescript
import { useRef, useLayoutEffect, useCallback, useReducer } from 'react'

function useSafeDispatch(dispatch: any) {
  const mounted = useRef(false)
  useLayoutEffect(() => {
    mounted.current = true
    return () => {
      mounted.current = false
    }
  }, [])
  return useCallback(
    (...args: any) => (mounted.current ? dispatch(...args) : void 0),
    [dispatch]
  )
}

const defaultInitialState = { status: 'idle', data: null, error: null }

export function useAsync(initialState = {}) {
  const initialStateRef = useRef({
    ...defaultInitialState,
    ...initialState
  })
  const [{ status, data, error }, setState] = useReducer(
    (s: any, a: any) => ({ ...s, ...a }),
    initialStateRef.current
  )

  const safeSetState = useSafeDispatch(setState)

  const setData = useCallback(
    (data: any) => safeSetState({ data, status: 'resolved' }),
    [safeSetState]
  )
  const setError = useCallback(
    (error: any) => safeSetState({ error, status: 'rejected' }),
    [safeSetState]
  )
  const reset = useCallback(
    () => safeSetState(initialStateRef.current),
    [safeSetState]
  )

  const run = useCallback(
    (promise: Promise<any>) => {
      if (!promise || !promise.then) {
        throw new Error(
          `The argument passed to useAsync().run must be a promise. Maybe a function that's passed isn't returning anything?`
        )
      }
      safeSetState({ status: 'pending' })
      return promise.then(
        (data) => {
          setData(data)
          return data
        },
        (error) => {
          setError(error)
          return Promise.reject(error)
        }
      )
    },
    [safeSetState, setData, setError]
  )

  return {
    isIdle: status === 'idle',
    isLoading: status === 'pending',
    isError: status === 'rejected',
    isSuccess: status === 'resolved',

    setData,
    setError,
    error,
    status,
    data,
    run,
    reset
  }
}
```

### Example Usage

[View it on CodeSandbox](https://codesandbox.io/s/useasync-typescript-um5qqw?file=/src/App.tsx 'Typescript useAsync code snippet')

```typescript
import { useEffect } from 'react'
import { useAsync } from './utils/useAsync'
import { client } from './utils/api-client'

import './styles.css'

interface RepositoryData {
  id: string
  name: string
}

export default function App() {
  const { data, error, run, isLoading, isError, isSuccess } = useAsync()
  const gitHubUserRepoEndpoint = 'users/lucianoayres/repos'

  useEffect(() => {
    run(client(gitHubUserRepoEndpoint))
  }, [run])

  return (
    <div className="App">
      <h1>GitHub API URL</h1>
      <p>{process.env.REACT_APP_API_URL}</p>
      <h2>Endpoint</h2>
      <p>{gitHubUserRepoEndpoint}</p>

      {isLoading ? (
        <p>Loading...</p>
      ) : isError ? (
        <p>Error: {error.message}</p>
      ) : null}

      {isSuccess ? (
        data?.length ? (
          <ul>
            {data.map(({ id, name }: RepositoryData) => (
              <li key={id} aria-label={name}>
                {name}
              </li>
            ))}
          </ul>
        ) : (
          <p>No repositories found. Try another search.</p>
        )
      ) : null}
    </div>
  )
}
```
