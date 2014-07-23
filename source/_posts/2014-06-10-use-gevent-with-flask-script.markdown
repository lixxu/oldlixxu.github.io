---
layout: post
title: "Flask-script 和 gevent 结合使用"
date: 2014-06-10 15:09:51 +0800
comments: true
categories: Python
tags: Python gevent flask-script
---
之前一直没用过 `flask-script`, 可是看了很多其他人的例子都在用这么个玩意. 碰巧手上有个现在做的一个应用, 所以打算用一下.

用了之后出现问题了: `python manage.py runserver` 使用的是默认的 `wsgi 服务器`, 而我在 windows 下一直使用 `gevent` 调试的. 
看了下文档, 差不多就是这样:
{% codeblock manage.py lang:python mark:40-53 %}
#!/usr/bin/env python
#-*- coding: utf-8 -*-

from __future__ import unicode_literals
import os
import os.path

from app import create_app, db
from app.models import User
from flask.ext.script import Manager, Shell
from flask.ext.migrate import Migrate, MigrateCommand

if os.path.isfile('app.env'):
    # print('importing environment from app.env...')
    with open('app.env') as f:
        for line in f:
            if not line.lstrip().startswith('#') and '=' in line:
                items = line.strip().split('=', 1)
                os.environ[str(items[0].strip())] = str(items[1].strip())

app = create_app(os.getenv('APP_ENV') or 'default')
manager = Manager(app)
migrate = Migrate(app, db)


def make_shell_context():
    return dict(app=app, db=db, User=User)

manager.add_command('shell', Shell(make_context=make_shell_context))
manager.add_command('db', MigrateCommand)


@manager.command
def deploy():
    from flask.ext.migrate import upgrade

    upgrade()


@manager.command
def rungevent():
    import werkzeug.serving
    from werkzeug.debug import DebuggedApplication
    from gevent.wsgi import WSGIServer

    @werkzeug.serving.run_with_reloader
    def run():
        app.debug = True
        dapp = DebuggedApplication(app, evalex=True)
        ws = WSGIServer(('', 5000), dapp)
        ws.serve_forever()

    run()

if __name__ == '__main__':
    manager.run()

{% endcodeblock %}

然后使用下面的命令就可以了:
```bash
python manage.py rungevent
```

可是在 `rungevent` 里, 无论怎么传参数控制是否处在 `debug` 状态, 始终是 `autoreload`, 这可能是 `app.env` 里我设定了开发模式导致的.

不过, 算了, 生产环境我是用 `uwsgi` 的, 用不到这个玩意.
