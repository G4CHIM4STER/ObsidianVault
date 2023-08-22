
# 📚 Полка

```dataview
TABLE WITHOUT ID
	status as Статус,
	rows.file.link as Книга
FROM  #📚Book
WHERE !contains(file.path, "Templates")
GROUP BY status
SORT status
```

## Общий список книг

```dataview
TABLE WITHOUT ID
	status as Статус,
	"![|60](" + cover + ")" as Обложка,
	link(file.link, title) as Название,
	author as Автор,
	category as Жанр
FROM #📚Book 
WHERE !contains(file.path, "Templates")
SORT status DESC, file.ctime ASC
```
