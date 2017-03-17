```获取goroutine的pid
```
[原文地址](http://colobu.com/2016/04/01/how-to-get-goroutine-id/)

	func GoID() int {
		var buf [64]byte
		n := runtime.Stack(buf[:], false)
		idField := strings.Fields(strings.TrimPrefix(string(buf[:n]), "goroutine "))[0]
		id, err := strconv.Atoi(idField)
		if err != nil {
			return 0
		}
		return id
	}