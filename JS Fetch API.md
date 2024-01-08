### Working with fetch

```
async function fetchData() {
	try {
		const response = await fetch('https://api.example.com/data');
		const data = await response.json();
		console.log(data);
	} catch (error) {
		console.error(Error:', error);
	}
}
```

> fetch and response.json returns a promise

> fetch returns a ReadableStream through the body property of a response object.

### Error handling with fetch
```
async function fetchData() {
	try {
		const response = await fetch('https://api.example.com/data');
		if (!response.ok) {
			throw new Error (`HTTP error! Status: ${response.status}`);
		}
		const data = await response.json();
	} catch (error) {
		console.error(Error:', error);
	}
}
```

> fetch will not throw an error if the status code is in the 4XX or 5XX

> we can handle manual errors using the **ok** or **status** property on the response object.

### Sending request headers with fetch

```
fetch('https://api.example.com/data', {
	headers: {
		"Content-Type": "application/json"
	}
})
```

```
const headers = new Headers({
	"Content-Type": "application/json",
	"Authorization": `Bearer ${token}`
})

fetch('https://api.example.com/data', { headers })
```

> utilizing the Headers constructor function

### POST requests with fetch

> by default the method is set to 'GET'

```
const payload = {
	title: "The Impact of Artificial Intelligence on Healthcare",
	content: "In recent years, artificial intelligence (AI) has ...",
	author: "John Doe"	
}

fetch('https://api.example.com/data', {
	method: 'POST',
	headers: {
		"Content-Type": "application/json"
	},
	body: JSON.stringify(payload)
}
```

### Uploading files with fetch

```
<input type="file" name="logo" id="myFile" />
```

```
const fileInput = document.getElementById("myFile");
fileInput.addEventListener('change', (event) => {
	const file = event.target.files[0];
	uploadFile(file);
})
```

```
async function uploadFile(file) {
	const formData = new FormData();
	formData.append("logo", file);
	
	const response = await fetch('https://api.example.com/upload', {
		method: 'POST',
		body: formData
	});
}
```

