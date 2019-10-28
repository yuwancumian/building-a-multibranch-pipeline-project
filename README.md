

安装好后第一次访问

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-27-161148.jpg)

安装推荐的插件

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-27-161148.jpg)

创建登录账户

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-27-image-20191018095752340.png)

创建任务

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-24-025509.jpg)



需要安装这么多？

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-24-031513.jpg)



创建pipeline

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-24-090633.jpg)

为jenkins 访问 github 生成一个 token

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-27-image-20191024170932236.png)

选择从 jenkin 官方提供的start repo

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-27-image-20191024171040802.png)

提示遇到docker: not found

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-24-095739.jpg)

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-25-021617.jpg)

看一下repo 里的 jenkinfile，原来是要在 jenkins 容器里运行docker，

```json
pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
    }
}
```

pipeline 的语法稍后研究，这里先看看怎么能方便快捷的用jenkins 编译前端任务。其实只需要在 jenkins 容器里安装一个node 插件，

系统管理--->管理插件--->下载NodeJS插件

系统管理--->Global Tool Configuration--->选择需要安装的nodejs版本

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-25-024848.jpg)

配置nodejs 环境变量

需要在 构建环境勾选 Provide Node & npm bin/folder to PATH

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-28-005459.jpg)

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-25-025146.jpg)



然后新建一个freestyle project 的jenkins 任务，仍然选择jenkins 官方提供的repo，在execute shell 里配置简单的node任务

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-28-005654.jpg)

进入刚才建立的jenkins 任务，在左侧菜单里点击 build with parameters ，

终于显示构建成功

![](http://tchuang.oss-cn-chengdu.aliyuncs.com/2019-10-27-image-20191025105754597.png)