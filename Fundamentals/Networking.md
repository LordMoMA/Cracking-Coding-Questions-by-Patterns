## Useful code snippets for networking applications

Templating

Make, parse and validate a template:

```go
var strTempl = template.Must(template.New("TName").Parse(strTemplateHTML))
```

Use the html filter to escape HTML special characters, when used in a web context:

```go
{{html .}} or with a field FieldName {{ .FieldName |html }}
```

Use template-caching.

The difference between executing a template on the server (like err := templates.ExecuteTemplate(w, "template.html", data)) and rendering on the frontend using a library like React lies in where the rendering logic is executed and how the data is sent to the client.

Server-side rendering (like Go's ExecuteTemplate): The HTML is fully rendered on the server before it's sent to the client. The server takes the template and the data, combines them to generate the full HTML, and then sends this HTML to the client. The client then displays the received HTML. This approach is good for SEO, as search engines can easily crawl the fully rendered page. However, it might require more server resources, as the server has to generate the full HTML for each request.

Client-side rendering (like React): The server sends the raw data (usually as JSON) and the template (as JavaScript code) to the client. The client's browser then executes the JavaScript to combine the data and the template and generate the HTML. This approach offloads the rendering work to the client, reducing the load on the server. It also allows for more interactive user interfaces, as the page can be updated without needing to contact the server. However, it might be less friendly to SEO, as search engines might not execute the JavaScript and see the fully rendered page.