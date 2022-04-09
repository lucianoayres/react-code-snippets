# useAsync Hook

Run async requests with status, data loading and error handling states.

[Author: Kent C. Dodds](https://github.com/kentcdodds/bookshelf 'Author: Kent C. Dodds')

Related: [api-client](https://github.com/lucianoayres/react-code-snippets/blob/main/snippets/utils/api-client.md 'api-client function') function

## Code Snippet

```javascript
import { useRef, useState, useEffect, useCallback, useReducer } from 'react'

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
function useAsync(initialState) {
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

export { useAsync }
```

### Example Usage

```javascript
/** @jsx jsx */
import { jsx } from '@emotion/core'

import { useState, useEffect } React from 'react'
import Tooltip from '@reach/tooltip'
import { FaSearch } from 'react-icons/fa'
import { Input, BookListUL, Spinner } from './components/lib'
import { BookRow } from './components/book-row'
import { client } from './utils/api-client'
import * as colors from './styles/colors'
import { useAsync } from './utils/hooks'

function DiscoverBooksScreen() {
  const { data, error, run, isLoading, isError, isSuccess } = useAsync()

  const [query, setQuery] = useState('')
  const [queried, setQueried] = useState(false)

  useEffect(() => {
    if (!queried) {
      return
    }

    run(client(`books?query=${encodeURIComponent(query)}`))
  }, [query, queried, run])

  function handleSearchSubmit(event) {
    event.preventDefault()
    setQueried(true)
    setQuery(event.target.elements.search.value)
  }

  return (
    <div
      css={{ maxWidth: 800, margin: 'auto', width: '90vw', padding: '40px 0' }}
    >
      <form onSubmit={handleSearchSubmit}>
        <Input
          placeholder="Search books..."
          id="search"
          css={{ width: '100%' }}
        />
        <Tooltip label="Search Books">
          <label htmlFor="search">
            <button
              type="submit"
              css={{
                border: '0',
                position: 'relative',
                marginLeft: '-35px',
                background: 'transparent'
              }}
            >
              {isLoading ? <Spinner /> : <FaSearch aria-label="search" />}
            </button>
          </label>
        </Tooltip>
      </form>

      {isError ? <p>Oops, something went wrong: {Error}</p> : null}

      {isSuccess ? (
        data?.books?.length ? (
          <BookListUL css={{ marginTop: 20 }}>
            {data.books.map((book) => (
              <li key={book.id} aria-label={book.title}>
                <BookRow key={book.id} book={book} />
              </li>
            ))}
          </BookListUL>
        ) : (
          <p>No books found. Try another search.</p>
        )
      ) : null}
    </div>
  )
}

export { DiscoverBooksScreen }
```
