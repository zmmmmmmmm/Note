## GIT
    git 就是一种👆提到的分布式版本控制系统

<br />

#### GIT如何记录每次提交的变更的

> 直接记录快照，而非比较差异

    git把数据当成是对文件系统的一组快照。每次产生新的提交git都会对当前提交的内容制作一个快照。
    并保存它的索引。

    还有一种做法是：以文件变更列表的方式存储信息。
    将保存的信息看作是一个基本信息 + 随时间累计的差异

<br />

#### GIT如何保证数据完整

    git中的所有数据在提交前都计算“校验和”，然后以校验和来引用。这可以保证所有引用的内容都被git记录过
    git用以计算校验和的机制叫做"SHA-1"散列（hash）。
    这是一个由40个十六进制字符组成的字符串。基于git中的文件内容和文件目录结构计算出的

<br />

#### GIT中的基本交互

```
   working directory(工作目录)     staging area(暂存区)        .git repository(本地git仓库)
            |                             |                              |
            |      add working change     |    commit stage change       |
            |     -------------------->   |  ----------------------->    |
            |                             |                              |
            |                             |                              |
            |                             |                              |
            |              checkout the project(branch ...)              |
            |     <--------------------------------------------------    |
            |                                                            |
```

    git仓库是git管理所有提交过的记录的地方，是远程仓库的镜像。负责和远程仓库交互

    working directory是对git仓库中记录的某个版本牵出的独立内容，供开发者修改。
    开发者将修改后的内容添加到暂存区内

    staging area 内的内容在下一次提交的时候打包提交给本地git仓库

    1.开发者从远程clone一份本地仓库
    2.从本地仓库牵出一份独立内容
    3.修改工作区内容
    4.将修改后的内容提交给staging area(可以分多次)
    5.将staging area的内容计算校验和后提交到本地仓库
    6.本地仓库和远程仓库交互

<br />

#### GIT的配置文件

> git自带一个git config的工具来帮助控制git的外观和行为的配置变量。这些变量分别存储在3个不同的位置

- ###### /etc/gitconfig
    
        系统级别的配置文件: 包含系统上每一个用户及他们的仓库的通用配置。
        如果使用--system(git config --system)。会从此文件中读写配置

- ###### $HOME/.gitconfig

        用户级别的配置文件
        如果使用--global(git config --global)。会从此文件中读写配置

    ```shell
        git config --global user.name 'username'
        git config --global user.email 'email'

        # echo $HOME/.gitconfig
        [user]

            name = 'username'
            email = 'email'
    ```

- ###### $PROJECT/.git/config

        仓库级别的配置,包含了远程仓库地址等信息
        不使用任何参数(git config)

    ```shell
        # echo $PROJECT/.git/config

        [core] # TODO
            repositoryformatversion = 0 # TODO
            filemode = true # TODO
            bare = false # TODO
            logallrefupdates = true # TODO
            ignorecase = true # TODO
            precomposeunicode = true # TODO
        [remote "origin"] # TODO
            url = git@github.com:xxx/xx.git
            fetch = +refs/heads/*:refs/remotes/origin/* # TODO
        [branch "master"] # TODO
            remote = origin # TODO
            merge = refs/heads/master # TODO
    ```

<br />

#### GIT配置的读取

    按照配置维度下层配置覆盖上层配置

```shell
    # /ect/gitconfig
    [user]
        name = 'system'

    # $HOME/.gitconfig
    [user]
        name = 'user'

    # $PROJECT/.git/config
    [user]
        name = 'project'

    # $PROJECT
        user.name = 'project'
```
