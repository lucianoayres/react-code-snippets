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

## TypeScript

```typescript
function client(endpoint: string, customConfig = {}): Promise<[]> {
  const config = {
    method: 'GET',
    ...customConfig
  }

  return fetch(`${process.env.REACT_APP_API_URL}/${endpoint}`, config).then(
    (response) => response.json()
  )
}
```

### Example Usage

```javascript
  useEffect(() => {
    if (!queried) {
      return
    }
    const data = await client(`books?query=${encodeURIComponent(query)}`)
  }, [])
```
