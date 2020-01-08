# Git 的一些使用场景

## 1. fork 的仓库，想要同步一些 fork 的主仓库的修改到当前的仓库

**第一步**
```bash
git remote -v
```
如果是第一次操作的话，理论上只会看到自己仓库的2条记录。 或者多条记录的话, 里面包含主仓库的地址的话，请忽略下一条命令的操作。

**第二步**
```bash
git remote add upstream(此处的名字随意) 主仓库的地址
```

**第三步**
```bash
git remote -v
```
这时理论上可以看到 4条或者4条以上的记录： 2条当前仓库的地址(fetch/push), 2条主仓库的地址(fetch/push)

**第四步**
```bash
git fetch upstream(第二步时定义的名字)
```
从主仓库拉取修改到本地

**第五步**
```bash
git merge upstream/master (合并主仓库的 master 分支到当前分支)
```

**第六步**
```bash
git push
```
提交修改到自己的仓库