## 📅 Daily Questions
### 🌜 Last night, after work, I...
- 

### 🙌 One thing I'm excited about right now is...
- 

### 🚀 One+ thing I plan to accomplish today is...
- [ ] 

### 👎 One thing I'm struggling with today is...
- 

---

## 📝 Scratch Space
- <% tp.file.cursor() %>

---
## To-Do:
### New-Dos:
- [ ] 

### Due Today: (📅 <% moment(tp.file.title).format("YYYY-MM-DD") %>)

```tasks
not done
due before <% moment(tp.file.title).add(1,'days').format("YYYY-MM-DD") %>
```

### Done Today: (✅ <% moment(tp.file.title).format("YYYY-MM-DD") %>)

```tasks
done <% moment(tp.file.title).format("YYYY-MM-DD") %>
```

---
## See Also...
### Notes created today
```dataview
List FROM "" WHERE file.cday = date("<%tp.date.now("YYYY-MM-DD")%>") SORT file.ctime asc
```

### Notes last touched today
```dataview
List FROM "" WHERE file.mday = date("<%tp.date.now("YYYY-MM-DD")%>") SORT file.mtime asc
```
