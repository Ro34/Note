## json & dict转换
使用json库
```python
import json
```
提供了两种方法：
- `loads()`：将json数据转化成dict数据
- `dumps()`：将dict数据转化成json数据
文件操作
- `load()`：读取json文件数据，转成dict数据
- `dump()`：将dict数据转化成json数据后写入json文件


#### dict转json
```python
print(dict)  # 输出：{'name': 'many', 'age': 10, 'sex': 'male'}
j = json.dumps(dict)
print(j)  # 输出：{"name": "many", "age": 10,"sex": "male"}
```

#### 对象转json
```python
stu = Student('007', '007', 28, 'male', '13000000000', '123@qq.com')
print(type(stu))  # <class 'json_test.student.Student'>
stu = stu.__dict__  # 将对象转成dict字典
print(type(stu))  # <class 'dict'>
print(stu)  # {'id': '007', 'name': '007', 'age': 28, 'sex': 'male', 'phone': '13000000000', 'email': '123@qq.com'}

j = json.dumps(obj=stu)
print(j)  # {"id": "007", "name": "007", "age": 28, "sex": "male", "phone": "13000000000", "email": "123@qq.com"}
```

#### json转dict
```python
j = '{"id": "007", "name": "007", "age": 28, "sex": "male", "phone": "13000000000", "email": "123@qq.com"}'
dict = json.loads(s=j)
print(dict)  # {'id': '007', 'name': '007', 'age': 28, 'sex': 'male', 'phone': '13000000000', 'email': '123@qq.com'}
```
#### json数据转成对象

```python
j = '{"id": "007", "name": "007", "age": 28, "sex": "male", "phone": "13000000000", "email": "123@qq.com"}'
dict = json.loads(s=j)
stu = Student()
stu.__dict__ = dict
print('id: ' + stu.id + ' name: ' + stu.name + ' age: ' + str(stu.age) + ' sex: ' + str(
    stu.sex) + ' phone: ' + stu.phone + ' email: ' + stu.email)  # id: 007 name: 007 age: 28 sex: male phone: 13000000000 email: 123@qq.com
```

#### dump()
```python
dict = {}
dict['name'] = 'many'
dict['age'] = 10
dict['sex'] = 'male'
print(dict)  # {'name': 'many', 'age': 10, 'sex': 'male'}
with open('1.json', 'w') as f:
    json.dump(dict, f)  # 会在目录下生成一个1.json的文件，文件内容是dict数据转成的json数据
```

#### load()
```python
with open('1.json', 'r') as f:
    dict = json.load(fp=f)
    print(dict)  # {'name': 'many', 'age': 10, 'sex': 'male'}
```