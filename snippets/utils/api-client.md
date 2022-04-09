# api-client Function

Make HTTP requests with custom configuration.

[Author: Kent C. Dodds](https://github.com/kentcdodds/bookshelf 'Author: Kent C. Dodds')

## Code Snippet

```javascript
function client(endpoint, customConfig = {}) {
  const config = {
    method: 'GET',
    ...customConfig
  }

  return window
    .fetch(`${process.env.REACT_APP_API_URL}/${endpoint}`, config)
    .then((response) => response.json())
}

export { client }
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
