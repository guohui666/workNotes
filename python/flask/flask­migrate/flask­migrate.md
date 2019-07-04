### 使用flask-migrate管理数据库

- #### `安装flask-migrate`
```pip install flask-migrate```

- #### `将数据库模型交给flask-migrate管理`

`manager.py`

```python
from flask_script import Manager
from app import app
from flask_migrate import Migrate, MigrateCommand
from models import * # 引入需要管理的数据模型

manager = Manager(app)

migrate = Migrate(app, db)

manager.add_command('db', MigrateCommand)

if __name__ == '__main__':
    manager.run()

```
`app.py`

```python
from flask import Flask
from blueprint import register_blueprints
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__, static_url_path='')
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://root:123456@localhost:3306/python'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False  # 跟踪对象的修改，在本例中用不到调高运行效率，所以设置为False
db = SQLAlchemy(app)
db.init_app(app)
register_blueprints(app)

if __name__ == '__main__':
    app.run()
```
- #### `初始化flask-migrate`
1. 执行`python manager.py db init`初始化flask-migrate，项目中会生成 `migrations` 文件夹，可以对flask-migrate做相应的配置，此命令只需要运行一次
2. 执行`python manager.py db migrate`生成迁移文件，可以在`migrations`中的`versions`文件夹下看到此次生成的迁移文件，打开后可查看此次模型的修改情况，每当模型有更改的时候就需要执行一次本命令
3. 执行`python manager.py db upgrade`将迁移文件所描述的更改同步到数据库中，每次需要同步时都需要执行一次此命令，在此之前需要执行`python manager.py db migrate`

- #### `配置检测字段更改`

在`migrations`下的env.py中添加以下配置

```python
compare_type=True,  # 检查字段类型
compare_server_default=True  # 比较默认值

#最终变为
context.configure(
            connection=connection,
            target_metadata=target_metadata,
            process_revision_directives=process_revision_directives,
            **current_app.extensions['migrate'].configure_args,
            compare_type=True,  # 检查字段类型
            compare_server_default=True  # 比较默认值
)
```

- #### `一些注意点`

1. 设置默认值：若不设置默认值的话，生成字段的时候将会为`null`,在数据模型中使用`server_default`设置默认值
```python

class Student(db.Model):  # 继承SQLAlchemy.Model对象，一个对象代表了一张表
    s_id = db.Column(db.Integer, primary_key=True, autoincrement=True, unique=True)  # id 整型，主键，自增，唯一
    s_name = db.Column(db.String(20))  # 名字 字符串长度为20
    s_age = db.Column(db.Integer, default=20)  # 年龄 整型，默认为20
    s_tt = db.Column(db.String(10), default="哈哈", server_default="呵呵") # 新增字段，使用server_default设置默认值
    t_id = db.Column(db.Integer, db.ForeignKey('teacher.t_id'))  # 外键
    __tablename__ = 'student'  # 该参数可选，不设置会默认的设置表名，如果设置会覆盖默认的表名

    def __init__(self, name, age, t_id, s_tt):  # 初始化方法，可以对对象进行创建
        self.s_name = name
        self.s_age = age
        self.t_id = t_id
        self.s_tt = s_tt

    def __repr__(self):  # 输出方法，与__str__类似，但是能够重现它所代表的对象
        return '<Student %r, %r, %r>' % (self.s_id, self.s_name, self.s_age)

```

