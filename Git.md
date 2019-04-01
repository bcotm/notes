# Git
## Two simple ideas. Git Tracks contents not files. 
- **Object database**. Git是一个对象数据库。
- **Index file**. Index file是工作目录与数据库之间的缓存。

## Git对象:
**对象名称**是其哈希值(SHA-1)，可通过7/40位字母来索引。

### **不可变对象**
- **Blob**  
内容为二进制数据。两个相同内容的文件，只会生成一个Blob对象。
- **Tree**  
包含指向**Tree**或者**Blob**对象的指针，以及对应的目录或文件名称。一个Tree对象对应工作区的一个目录。如果其内容递归相同，只有一个Tree对象。
- **Commit**  
包含指向**Tree**对象的指针，指向父节点的指针，以及相关描述，author(修改人)，committer(创建提交的人)，timestamp等。
- **Tag**  
对象名、对象类型，tag name， tagger等。

## ref
ref是一个名字，包含了一个commit。比如tag, branch_name
指代tree中某个文件：tree-ish:path/to/file

### **可变对象**
- **Index**
全局单例对象，维护当前对应的文件列表及状态。

## Git工作与对象
### git add
创建blob对象。
### git commit
1. 创建tree对象，从修改文件目录一直到根目录，没变动则为之前，变动则创建。
2. 创建commit对象，指向工作区根目录的tree对象，并记录committer，author等。
3. 将当前分支(`.git/HEAD`:`ref: refs/heads/master`)指向刚创建的commit对象。比如master分支的文件`.git/refs/heads/master`写入`74ac3ad..`

Git中**ref**是一段提交历史的入口，用符号链接来操作历史，如`HEAD`、`MERGE_HEAD`、`FETCH_HEAD`、`fix-for-bug-376`。

### git checkout commit_hash
1. 找到对应的commit对象。
2. 将其指向的tree对象中的文件写到工作区。
3. 将其指向的tree对象中的文件写到index。
4. 将commit对象的哈希值写入HEAD，HEAD处于 `detached` 状态，`.git/HEAD`文件中只有commit哈希值。

### git branch branch_name
1. 创建文件`.git/refs/heads/branch_name`。
2. 将HEAD指向的commit对象的哈希值写入该文件。

### git merge merged_commit_hash_or_ref
当前提交为合并的目的提交，`merged_commit`为源提交。

1. 源提交是目的提交的祖先，git无任何操作。
2. 目的提交时原提交的祖先，git使用**fast-forward**合并。
    - 将源提交tree对象对应的文件写入工作区和index。
    - 使用`fast-forward`将目的提交指向源提交。 
    - `HEAD`指向的ref更新为源提交。
3. 源提交和目的提交在不同的提交线。创建合并提交。
    - 源提交哈希值写入`.git/MERGE_HEAD`，合并时文件才存在。
    - 查找源提交与目的提交最近的公共父提交。
    - 给源提交、目的提交、基提交创建索引。
    - 创建源提交和目的提交相对于基提交的差异列表，文件路径+文件状态(添加、移除、修改、冲突)，如果与基提交对比只有一个修改，则会自动合并。
    - 将差异中的项更新到工作区、index。如果有冲突,
        + 工作区文件，
        > <<<<<<< destination_commit   
        > modified value   
        > \=======   
        > modified value 2   
        > \>>>>>>> source_commit
        + index内容，
        > stage filename commit_hash   
        > 1 data/number.txt xxx   
        > 2 data/number.txt xxx   
        > 3 data/number.txt xxx   
        > 1,2,3分别是基提交、目的、源的哈希。
    - 提交更新后的index，并将目的提交指向新创建的提交。如果有冲突，add+commit创建合并提交。

### git remote add remote_repo address
1. 在当前仓库`.git/config`中添加
> [remote "remote_rep"]  
>   url = address

### git fetch remote_repo branch_name
1. 获取remote_repo的branch_name的哈希值commit_hash。
2. 创建包含commit_hash提交依赖的所有对象的列表，包括提交对象以及祖先提交以及树图内的所有对象。并将这些更新到`.git/objects`中。
3. 将`.git/refs/remotes/remote_repo/branch_name`的内容更新为commit_hash。
4. `./git/FETCH_HEAD`内容设置为：
> commit_hash branch 'branch_name' of remote_repo

### git pull remote_repo branch_name
git fetch + git merge

### git clone source_repo dest_repo
1. 创建目录并初始化仓库。
2. 添加source_repo为远程仓库`origin`，并pull。

### git push remote_repo branch_name
不能是远程仓库当前的已检出分支。

### 裸仓库
### git clone source_repo dest_repo --bare



  
### Comment
```java
/*
First line is treated as title
Blank line
Detail descriptions.
*/
```

# Commands

- add
- commit
- log
- status
- diff
- branch
- merge
- rebase
- stash
- cherry-pick
    + 结合楼下可以恢复detach的commit，从一个分支挑选一个commit到当前分支
- reflog
    + git commit xxx 1-6个
    + git reset --hard 1
    + git commit xxx 7
    + git reflog //找到6的commitid
    + git cherry-pick 6 //awesome!
- checkout
- pull  fetch + merge
- push
- tag
- show
- reset
- ls-tree
- cat-file -t SHA-1

https://github.com/pysnow530/git-from-the-inside-out  
https://blog.csdn.net/yuzaipiaofei/article/details/6639866  
http://ndpsoftware.com/git-cheatsheet.html  
https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting  

# Differences between **revert**, **reset**, **checkout**
用`reset`、`checkout`做private的改变，`revert`做public的改变。
## tree-ish commit-ish
git可以在需要tree对象的时候给它一个commit或者tag对象，这时就使用commit或者tag所指向的tree对象。
可以从tree-ish参数(commit, tag, tree)得到一个tree对象。
可以从commit-ish参数(tag, commit)得到一个commit对象。
commit-ish^n:commit的第n个直接父节点，比如合并时可以有2个父节点。
commit-ish~n: == commit-ish^^^^..

git log ^HEAD~2 HEAD
从HEAD能达到的commit集合中排出从HEAD2能达到的。
比如查看从master分出来的test分支修改的内容
git log ^master test

## revert
This command creates a new **commit** that undoes the changes from a previous **commit**. This command adds new history to the project (it doesn't modify existing history).

## reset
两个作用，修改index中path，将当前分支的HEAD指向commit，对应修改index和working directory。
Reset current `HEAD` to the specified state.可能会导致detached状态，
原来的`HEAD`在`ORIG_HEAD`里。
### 参数为commit对象。
如果是commit结尾或无结尾(默认`HEAD`)，把当前`HEAD`指向commit，然后可选的修改index和working directory。

|working|index|HEAD|target|argument|working|index|HEAD|
|:--:|:--:|:--:|:--:|:--|:--:|:--:|:--:|
|A|B|C|D|\--soft|A|B|D|
|||||\--mixed|A|D|D|
|||||\--hard|D|D|D|
|||||\--merge|disallowed|||
|||||\--keep|disallowed|||

|working|index|HEAD|target|argument|working|index|HEAD|
|:--:|:--:|:--:|:--:|:--|:--:|:--:|:--:|
|A|B|C|C|\--soft|A|B|C|
|||||\--mixed|A|C|C|
|||||\--hard|C|C|C|
|||||\--merge|disallowed|||
|||||\--keep|A|C|C|

|working|index|HEAD|target|argument|working|index|HEAD|
|:--:|:--:|:--:|:--:|:--|:--:|:--:|:--:|
|B|B|C|D|\--soft|B|B|D|
|||||\--mixed|B|D|D|
|||||\--hard|D|D|D|
|||||\--merge|D|D|D|
|||||\--keep|disallowed|||

|working|index|HEAD|target|argument|working|index|HEAD|
|:--:|:--:|:--:|:--:|:--|:--:|:--:|:--:|
|B|B|C|C|\--soft|B|B|C|
|||||\--mixed|B|C|C|
|||||\--hard|C|C|C|
|||||\--merge|C|C|C|
|||||\--keep|B|C|C|

|working|index|HEAD|target|argument|working|index|HEAD|
|:--:|:--:|:--:|:--:|:--|:--:|:--:|:--:|
|B|C|C|D|\--soft|B|C|D|
|||||\--mixed|B|D|D|
|||||\--hard|D|D|D|
|||||\--merge|disallowed|||
|||||\--keep|disallowed|||

|working|index|HEAD|target|argument|working|index|HEAD|
|:--:|:--:|:--:|:--:|:--|:--:|:--:|:--:|
|B|C|C|C|\--soft|B|C|C|
|||||\--mixed|B|C|C|
|||||\--hard|C|C|C|
|||||\--merge|B|C|C|
|||||\--keep|B|C|C|

### 参数为目录文件。(Unstage a file)
`git reset path`与`git add path`对立。
如果是tree-ish+paths结尾，**是将tree-ish拷贝到index**。不会修改`HEAD`。
**文件撤销add**：相当于unstage，撤销add。
git reset file.txt
git reset --mixed HEAD file.txt
**仓库文件恢复到某个版本**
git reset eb342fe file.txt + git commit 

### 例子
#### 撤销add
```
edit a.c
git add a.c
// 准备pull其他文件。
git reset
git pull
// 此时wd中a.c仍为修改过的版本。
```
#### 撤销commit并重做
```
git commit ...
git reset --soft HEAD^
edit
git commit -a -c ORIG_HEAD
```
#### 撤销前几个commit新建一个branch
```
// 现在当前HEAD新建分支
git branch newbranch
// 把HEAD以及master分支移动到前3个commit
git reset --hard HEAD~3
// 切换到新分支
git checkout newbranch
```
#### 撤销pull或merge
```
git pull
// 清空index 和 wd
git reset --hard HEAD
// =======================
// 拉取topic分支做了ff合并，但是不想要这个topic
git pull . topic/branch
// 用合并时的原始分支修改index wd
git reset --hard ORIG_HEAD
```

## checkout
**修改分支、恢复工作树文件。**(Discard changes in wd.)
Switch **branches** or restore working tree files
checkout 不修改分支ref，对历史没有任何修改。  
没有`paths`就切换分支(移动`HEAD`)
### detached HEAD
`HEAD`指向特定的commit，而不是一个命名的branch。当HEAD离开时，就回不来了。
### branch
更新工作目录与index或tree匹配，修改HEAD指向当前branch。