# simple-web-server
- ref :[Build A Web Server With Golang In 20 Mins (2021) - Beginner Friendly!](https://www.youtube.com/watch?v=ASBUp7stqjo&ab_channel=AkhilSharma)
---
![](https://i.imgur.com/rJvh96O.png)

## Server code
```python
package main

import (
	"fmt"
	"log"
	"net/http"
)

func formHandler(w http.ResponseWriter, r *http.Request) {
	if err := r.ParseForm(); err != nil {
		fmt.Fprintf(w, "ParseForm() err: %v", err)
		return
	}
	fmt.Fprintf(w, "POST request successful\n")
	name := r.FormValue("name")
	address := r.FormValue("address")
	fmt.Fprintf(w, "Name = %s\n", name)
	fmt.Fprintf(w, "Address = %s\n", address)
}

func helloHandler(w http.ResponseWriter, r *http.Request) {
	if r.URL.Path != "/hello" {
		http.Error(w, "404 not found", http.StatusNotFound)
		return
	}
	if r.Method != "GET" {
		http.Error(w, "method is not support", http.StatusNotFound)
		return
	}
	fmt.Fprintf(w, "hello!")
}

func main() {
	FileServer := http.FileServer(http.Dir("./static"))
	http.Handle("/", FileServer)
	http.HandleFunc("/form", formHandler)
	http.HandleFunc("/hello", helloHandler)

	fmt.Println("Starting server at port 8080")
	if err := http.ListenAndServe(":8080", nil); err != nil {
		log.Fatal(err)
	}
}

```

## Index.html
```html
<html>

<head>
    <title>Static Website</title>
</head>

<body>
    <h2>Static Website</h2>
</body>

</html>
```

## Form.html

```html
<!DOCTYPE html>

<html>

<head>
    <meta charset="UTF-8" />

</head>

<body>
    <form method="POST" action="/form">
        <label>Name</label><input name="name" type="text" value="">
        <label>Address</label><input name="address" type="text" value="">

        <input type="submit" value="submit" />
    </form>
</body>

</html>
```

---
# Test
|Start the server (execute the code)|
|-----------|
|![](https://i.imgur.com/Un1spz9.png)|

|Open a browser and type `localhost:8080`|
|--|
|![](https://i.imgur.com/c1rzCXn.png)|
|we can see the `index.html`|

|Enter `localhost:8080/hello`|
|--|
|![](https://i.imgur.com/zCvXP7j.png)|
|This is `helloHander()`|


|Enter `localhost:8080/form.html`|
|--|
|![](https://i.imgur.com/q5UO5tr.png)|
|Fill the blank|

|After you submit|
|--|
|![](https://i.imgur.com/mKVGVGO.png)|