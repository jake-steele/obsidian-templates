<%"---"%>
tags: 📥/📹
created: <% tp.file.creation_date("YYYY-MM-DD") %>
modified: <% tp.file.last_modified_date("YYYY-MM-DD") %>
<%"---"%>
<%*
const url = await tp.system.clipboard()
const response = await fetch(`https://youtube.com/oembed?url=${url}&format=json`)
const data = await response.json()
console.log(data)

// Create a safe title... there's probably a better way to do this
const title = data.title.replaceAll("", "").replaceAll('"', '').replaceAll("\\", "").replaceAll("/", "").replaceAll("<", "").replaceAll(">", "").replaceAll(":", "").replaceAll("|", "").replaceAll("?", "")
const author = data.author_name
const author_url = data.author_url
const html = data.html

// Change target path to whatever, has to exist (I think; read the Templater docs)
const newPath = "3-Permanent/36-Misc-Learning/Youtube/" + title
await tp.file.move(newPath)
const regex = /(v=|youtu.be\/)([^?]*)/gm
const m = regex.exec(url)

-%>

> [!meta]+ Metadaten
> Source:: [<% title %>](<% url %>) </br>
> Channel: [<% author %>](<% author_url %>) </br>
> Published:  </br>
> Watched: <% tp.date.now() %>


---

## YouTube Video
![](<% url %>)


## Notes
- <% tp.file.cursor(0) %>
