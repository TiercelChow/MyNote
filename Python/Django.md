[toc]

# 初识Django

### Django适用场景

- 内容管理系统
  - 博客
  - CMS
  - Wiki
- 企业内部系统
  - 会议室预定
  - 招聘管理
  - EPR&CRM
  - 报表系统
- 运维管理系统
  - CMDB
  - 发布管理
  - 作业管理
  - 脚本管理
  - 变更管理
  - 故障管理

### Django的优点和缺点

> 优点

- Python实现，代码干净整洁
- 提供管理后台，能够快速开发
- 复用度高，设计使用上遵循DRY（Don't repeat yourself）原则
- 易于扩展复用的中间件
- 内置的安全框架
- 丰富的第三方类库

> 缺点

- 单体应用，不易并行开发，单点扩展
- 不适合非常小的几行代码项目
- 不适合高并发的toC互联网项目

### Django的MTV框架

> 核心部分

- Model

  与数据库进行交互

- Template

  动态HTML文件中内容不变的部分，定义了每一个网页的整体架构，静态的内容都在模版中定义，动态的内容通过模版的语法在模版中将动态的内容块定义出来，即可通过Model层在数据库访问之后将数据传递到模版中去，最后合成最终的页面在页面展示出来

- View

  用户请求的处理层，处理请求的业务逻辑，通过Model层与数据库进行交互，并获取页面的模版，将动态内容填充至模版中后在用户端的浏览器上展示出来

- Contraller

  网站URL的路由控制器，通过正则表达式将URL的请求分发到不同的视图进行处理

与MVC框架不等同，Contraller层未严格定义

### 创建一个包含用户管理功能的后台

> Django的安装

- 若使用PyCharm创建Django项目可忽略该步骤
- 可以先安装Anaconda，通过conda install django安装
- 也可以通过pip执行pip install django命令安装

> 通过PyCharm新建Django项目





> 初始化

```
migrate
```

> 创建管理员

```
createsuperuser
```

输入管理员用户名及密码即创建成功

> 访问页面

浏览器输入http://127.0.0.1:8000/即可访问页面，http://127.0.0.1:8000/admin即可访问管理员界面，输入上一步设置的用户名和密码即可登录。

### Django项目文件目录

> db.sqlite3

位于根目录下，

> manage.py

位于根目录下

> asgi.py

异步网关接口

> settings.py

整个Django项目的配置文件

```python
# 调试信息
DEBUG = True

# 配置可访问的IP地址
ALLOWED_HOSTS = []

# Django项目中安装的应用
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

# 中间件
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'djangoProjectDemo.urls'

# 使用的模版引擎
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates']
        ,
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'djangoProjectDemo.wsgi.application'

# 数据库
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}


# Password validation
# https://docs.djangoproject.com/en/3.1/ref/settings/#auth-password-validators

AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]


# Internationalization
# https://docs.djangoproject.com/en/3.1/topics/i18n/

LANGUAGE_CODE = 'en-us'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_L10N = True

USE_TZ = True


# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/3.1/howto/static-files/

STATIC_URL = '/static/'

```

> urls.py

路由分发器

> wsgi.py

web server gateway interface

