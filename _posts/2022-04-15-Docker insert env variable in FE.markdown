---
layout: post
title: Docker 如何在前端项目动态插入并使用变量
date: 2022-04-15 12:07:24.000000000 +09:00
tags: Docker
---

**关键词** Docker

### 前言
根据项目需求，在实现登出功能的时候，需要根据测试环境和生产环境调用不同的登出URL。本文介绍了如何在Docker前端镜像设置变量以及使用变量。

### 解决办法
在生成前端容器的阶段，可以使用同一个镜像，根据不同的环境传入参数形成不同的前端容器。下面会分享一个容器执行阶段动态插入并使用变量的实例。

### 步骤 
#### 1. 在根目录创建`start.sh`文件，文件内容如下:
{% highlight ruby %}
#!/usr/bin/env sh
cat /etc/nginx/nginx.conf
nginx -g "daemon off;"
{% endhighlight %}
> 注: `#!/usr/bin/env sh` 并不是注释的意思，而是选择编译语言的意思。建议使用sh，因为bash可能不是每台服务器都安装的。

> 注： 为什么要加nginx -g "daemon off";因为要让容器能持续运行， 必须要有前台进程，这里要将nginx转为前台进程。

#### 2. 在DockerFile里复制`start.sh`，将其从容器外复制到容器内:
{% highlight ruby %}
...
COPY start.sh /app/start.sh
{% endhighlight %}

#### 3. 在根目录创建`nginx.conf.template`文件，首先从`nginx.conf`复制代码，再在文件的server下添加`ENV_VARS`占位符，代码如下:
{% highlight ruby %}
...
http {
    ...
    server {
        .....
        location /env.json {
            default_type application/json;
            return 200 '${ENV_VARS}';
        }
    }
}
{% endhighlight %}

#### 4. 在项目`server`端创建一个获取变量的方法, 代码如下：
{% highlight ruby %}
type Env = {
  logoutUrl?: string;
};

export async function getEnvironmentVariables() {
  return request<Env>('/env.json', { method: 'GET' });
}
{% endhighlight %}

#### 5. 在项目代码里添加使用变量的方法，代码如下：
{% highlight ruby %}
const logout = () => {
    getEnvironmentVariables()
      .then((data) => {
        const logoutUrl = data?.logoutUrl;
        if (logoutUrl) {
         ...
        }
      })
      .catch((e) => {
        ...
      });
  };
{% endhighlight %}

#### 6. 正常构建镜像之后, 在生成容器时，可通过环境变量传参替换原前端`nginx.conf.template`文件里的`ENV_VARS`字符串
{% highlight ruby %}
docker run -e ENV_VARS='{"logoutUrl": "xxxxxx"}' --name test -p 81:8000 -itd swr.test:v0.0.31

sh start.sh
{% endhighlight %}

> 注： 如果替换的环境变量是JSON格式，需要将变量值用单引号包含，变量值内的属性值使用双引号。如`ENV_VARS='{"logoutUrl": "xxxxxx"}'`

### 后言
这个设计，让我们在前端独立容器化部署时，能通过环境变量解耦登出地址，避免了一次又一次的构建镜像工作量。希望本文会对你有所帮助，如果有什么问题，可在下方留言沟通。