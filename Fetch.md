```javascript
fetch(URL, {method})
	.then((response) => {})
	.catch(() => {})
```
- `fetch()` არის დღევანდელი შეცვლა `XMLHttpRequest`-ისათვის, მაგრამ `callbacks`-ის მაგივრად, `Fetch` არის აწყობილი `promise`-ზე.
- `method` შეიძლება იყოს `GET`, `POST` ან `HEAD`.
```javascript
async function getData() {
  const url = "https://example.org/products.json";
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const json = await response.json();
    console.log(json);
  } catch (error) {
    console.error(error.message);
  }
}
```
