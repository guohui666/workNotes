### 使用flask-migrate管理数据库

- #### `安装flask-migrate`
```pip install flask-migrate```

安装完成后在项目中会生成 `migrations` 文件夹，可以对flask-migrate做相应的配置

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
from flask_script import Manager
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__, static_url_path='')
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://root:123456@localhost:3306/python'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False  # 跟踪对象的修改，在本例中用不到调高运行效率，所以设置为False
db = SQLAlchemy(app)
db.init_app(app)
register_blueprints(app)

manager = Manager(app=app)
if __name__ == '__main__':
    app.run()
```
