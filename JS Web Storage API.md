### localStorage
- allows web applications to store key-value pairs in a web browser persistently across sessions.
```
localStorage.setItem('myKey', 'myValue');
```

```
const retrievedValue = localStorage.getItem('myKey');
```

```
localStorage.removeItem('myKey');
```

```
localStorage.clear();
```

 - localStorage only supports storing strings
	 - store objects in localStorage using the JSON.stringify() method
	 - objects can then be retrieved with the JSON.parse() method

> localStorage should only be used to store non-sensitive data

### ==example== theme toggle (light / dark mode)
```
const applySavedTheme = () => {
	const savedTheme = localStorage.getItem("theme");
	if (savedTheme === "dark") {
		document.body.classList.add("dark-mode");
		toggleButton.textContent = "Enable Light Mode";
	} else {
		document.body.classList.remove("dark-mode");
		toggleButton.textContent = "Enable Dark Mode";
	}
};
```

### ==example== notes app
```
saveButton.addEventListener("click", () => {
	const noteContent = textArea.value.trim();
	createNoteElement(noteContent);
	textArea.value = "";
	const notes = JSON.parse(localStorage.getItem("notes")) ?? [];
	notes.push(noteContent);
	localStorage.setItem("notes", JSON.stringify(notes));
});
```

### keep local storage in sync between tabs
```
window.addEventListener("storage", (e) => {
	if (e.key === "theme") {
		applySavedTheme();
	}
});
```

### sessionStorage
- allows web applications to store key-value pairs in a web browser for a **single session**
```
sessionStorage.setItem('sessionKey', 'sessionValue');
const sessionValue = sessionStorage.getItem('sessionKey');
sessionStorage.removeItem('sessionKey');
sessionStorage.clear();
```

### ==example== storing form data
```
const form = document.querySelector("#checkoutForm");

form.addEventListener("input", (e) => {
	const { name, value } = e.target;
	const formData = JSON.parse(sessionStorage.getItem("formData")) ?? {};
	formData[name] = value;
	sessionStorage.setItem("formData", JSON.stringify(formData));
});

const formData = JSON.parse(sessionStorage.getItem("formData")) ?? {};
for (let field in formData) {
	form.elements[field].value = formData[field];
}
```

## IndexedDB API
- a low-level API for storing structured data, including large datasets;
```
const openRequest = indexedDB.open("myDatabase", 1);

openRequest.onupgradeneeded = (event) => {
	const db = event.target.result;
	const store = db.createObjectStore("myStore");
};

openRequest.onsuccess = (event) => {
	const db = event.target.result;
	const transaction = db.transaction("myStore", "readwrite");
	const store = transaction.objectStore("myStore");
	store.add("value", "key");
};
```

