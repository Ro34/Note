## 带变量的查询

```python
serverContext = '123'
c.execute("SELECT PLATFORMCONTEXT FROM mission_list WHERE SERVERCONTEXT=?",(serverContext,))
```