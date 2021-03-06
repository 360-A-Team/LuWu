# 陆吾

```
888               888       888          
888               888   o   888          
888               888  d8b  888          
888      888  888 888 d888b 888 888  888 
888      888  888 888d88888b888 888  888 
888      888  888 88888P Y88888 888  888 
888      Y88b 888 8888P   Y8888 Y88b 888 
88888888  "Y88888 888P     Y888  "Y88888 
```

**陆吾**即**肩吾**，中国古代神话传说中的昆仑山神明，人面虎身虎爪而九尾。

在红队工作中，基础设施的部署和监控是非常重要的一部分，一个灵活快速的自动化部署工具能够使前期繁杂的工作变的高效简单。本项目按照模块化设计，以BS为架构，使用浏览器web界面控制基础设施部署，分别包括域名模块、vps模块、功能模块、配置模块。

## 安装

这里主要说明下通过docker应该如何进行进行构建：

### 系统依赖

#### docker

docker 安装请参考[官方文档](https://docs.docker.com/engine/install/)

#### 系统组件

- redis
- postgresql
- frontend
- backend

## 配置

### 系统配置

系统的主要配置都在conf目录下, 安装、使用的时候请注意配置是否正确

```
conf
├── backend
│   └── env.default
├── frontend
│   └── nginx.conf
├── postgresql
│   └── postgresql.conf
├── redis
│   └── redis.conf
├── supervisor
│   ├── supervisor.api.conf
│   └── supervisor.task.conf
└── terraform
    └── terraform.rc
```

#### 环境变量配置

请注意**conf/backend**的**env.default**是示例文件，使用的时候请放在src/backend目录下， 比如：

```bash
cp conf/backend/env.default src/backend/.env
```

根据具体情况，可能需要对配置`src/backend/.env`进行更改

同理，完成对前端项目的环境配置

```bash
cp conf/frontend/env.default  src/frontend/.env
```

### 初始化系统

请参考[alembic官方文档](https://alembic.sqlalchemy.org/en/latest/tutorial.html)

```bash
# 升级到最新记录 && 生成目前的model记录 && 升级到最新记录
alembic upgrade head && alembic revision --autogenerate && alembic upgrade head

# 写入用户数据(在src/backend目录下运行)
python initial_data.py
```

### 使用配置

使用系统时请先在**配置管理**页面完成ISP、SSH等相关配置

- 域名ISP， 目前只支持**NameSilo**
- VPS ISP， 目前支持**Vultr**和**DigitalOcean**
- C2 Profile， 即CS的 profile
- SSH KEY， 这里的SSH KEY会被写入到被创建的VPS，所以也可以通过页面提供的私钥直接连接目标VPS
- 网站模板， 目前是NGINX反代静态网站的模式，所以需要上传**zip**压缩打包后的静态网站文件

## 引用的开源项目

* https://github.com/mail-in-a-box/mailinabox
* https://github.com/bluscreenofjeff/Malleable-C2-Randomizer
* https://github.com/bluscreenofjeff/Red-Team-Infrastructure-Wiki
* https://github.com/threatexpress/cs2modrewrite
