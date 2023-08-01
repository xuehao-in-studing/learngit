<!--
 * @Author: Jeason 19938943480@163.com
 * @Date: 2023-08-01 12:28:20
 * @LastEditors: Jeason 19938943480@163.com
 * @LastEditTime: 2023-08-01 14:30:12
 * @FilePath: \undefinede:\learngit\git.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
# git的使用  
## 一、创建版本库
步骤(从新建文件夹开始)：
1. 打开git bash应用
2. 使用cd命令进入想要创建仓库的文件夹
3. 执行以下命令：
4. `mkdir dirname`
5. `cd dirname`
6. `git init` (如果从本机的项目文件创建库直接5、6两部就OK了)

通过以上步骤创建了版本库，可以通过pwd(present work directory)进入当前目录，查看是否在工作目录下。  

另外，在当前目录下会多出来一个.git文件(隐藏的)，表明已经初始化完毕。

## 二、添加文件到版本库
### 2.1 添加文件
命令：
1. `git add filename`
2. `git commit -m “description for this commit”`  

提醒： -m后面跟的是本次修改说明，以方便历史记录查看。  
一次性提交全部修改：`git add -A`

### 2.2 暂存区与工作区
工作区是指本机的文件夹以及文件内容，暂存区是暂时存放工作区的区域，它并不是内存，是从工作区到版本文件的缓冲。  

add：**将当前文件的修改添加到了暂存区(可以理解为中间内存，缓存等)**  
commit：**把暂存区的文件提交到了当前的git分支(可以理解为从缓存提交到了存储区)。**

可以通过`git status`查看工作区与暂存区的状态。  

### 2.3 查看暂存区中文件内容
`cat filename`  

## 三、版本控制
### 3.1 版本回退
在多次add和commit之后，可以通过版本回退回退到之前的版本，不至于几个月的工作全部丢失。  

**查看修改历史**：`git log`  
**回退命令**：`git reset --hard HEAD^`  

上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。  

### 3.2 回到未来
如果回到了过去，但是想要重回未来怎么办？  
**如果没有关闭命令行**  
1. 如果没有关闭当前命令行，向上找到版本id
2. 命令：`git reset --hard 版本id(可以只写前几位)`

<div align="center">
    <img src="https://github.com/xuehao-in-studing/git_learn/assets/102791379/fd4cacf5-8695-4b63-9a9d-934489595863" alt="版本回退">
</div>  

穿梭到了未来，看来我的程序还有补救的机会(┭┮﹏┭┮)。

**如果关闭了命令行怎么办?**  
1. `git reflog` 查看历史命令
2. 在命令中找版本id，使用 `git reset --hard 版本id` 回到未来

<div align="center">
    <img src="https://github.com/xuehao-in-studing/git_learn/assets/102791379/0bae8859-e0fd-4f96-a8a9-950cd5dd8ca8#pic_center" alt="版本回退">
</div>  
yes，又穿梭到了未来((●'◡'●))!


### 3.3 管理修改
git 如果没有add，那么文件的修改就不会加入到暂存区，也就无法通过commit把修改提交到工作区。  
在没有add之前，提示信息是红色的，add之后，提示信息是绿色的。    
<div align="center">
    <img src="https://github.com/xuehao-in-studing/git_learn/assets/102791379/5f790bf3-0def-488c-831c-820bc2b4a54b" alt="修改信息">
</div>  

在第一张图片中发生了一次修改，但是没有add到暂存区，那么也就无法commit修改。  
**提交全部修改：** `git add -A`

### 3.4 撤销修改
`git checkout -- readme.txt`来撤销readme.txt的修改，**-- 很重要，不能省去，否则变成了分支管理命令**。  
撤销分为几种情况，以上所提到的撤销丢弃的是工作区的修改。  
1. 文件修改后还没有放到暂存区(没有add)，那么撤销之后就和版本库一模一样的状态。
2. 文件修改后放到了暂存区，并且又修改了，那么撤销修改就回到放到暂存区之后的状态。  

那么如果已经提交到了暂存区，又想回到提交到暂存区之前的状态：
1. 版本回退
2. 撤销修改
但是要谨慎，会把之前的工作全部撤销，并且此时你还没有提交到分支，分支里没有提交记录，不能返回了。



## 四、远程仓库
### 4.1 添加远程仓库
远程仓库一般指的是把自己的git仓库托管到GitHub平台上。  
首先设置SSH密匙，详见[这篇博客](https://blog.csdn.net/nofaliure/article/details/132018287?spm=1001.2014.3001.5501)。  
添加步骤：
1. 在GitHub新建repo，**注意不要添加readme文件**，否则相当于在GitHub创建了一个仓库，在之后的提交会报错。
2. **创建远程仓库**：`git remote add origin ***` ,其中*** 是从github的这里复制来的连接，可以采用HTTPS，也可以SSH协议；origin是指远程库的名字，可以任意设置，但一般是origin。
3. **本地库内容推送到远程库**:`git push -u origin master`，-u 把master本地分支和远程的master分支关联了起来(本地和远程分支名必须相同)，推送时就可以简化命令。
4. 把**本地的分支更新到GitHub**:`git push origin master`

### 4.2 解绑远程仓库
解绑仓库首先查看仓库的内容：
`git remote -v`  
其次，根据名字删除，比如删除origin:`git remote rm origin`  

注意这仅仅是把本地的git库和远程的git库断开，而不是删除了远程库，删除远程库需要在GitHub进行删除。

### 4.3克隆远程库
1. `cd file path`
2. `git clone git@github.com:michaelliao/gitskills.git`，其中clone后的地址是GitHub的地址，将项目克隆到了你当前的文件夹(pwd)下面。

## 五、分支管理
为什么需要分支？如果你和你的同事同时在完成一个项目，但是需要进度同步，那么你们就必须同时开发，并且进度和功能不能够互相影响。这时就可以各自创建各自的分支进行开发。
### 5.1 创建与合并分支
**创建并切换分支**：`git checkout -b dev`或者`git switch -c dev`   
这个命令相当于创建并切换，切换到dev分支。  
等于：`git branch dev`+`git checkout/switch dev`  

创建的新分支都是空的：  
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/3b044ba1-27eb-4778-b22a-67eb4898de75" alt="创建的分支">
</div>  

**查看分支**：`git branch`  
这个命令会列出所有分支，当前分支会标一个*。 
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/07e32ff6-bb37-45c1-b11f-7e205a3984bc" alt="查看分支">
</div>   

分支互不干扰，在master分支下无法查看new分支下的内容。newbranch.txt仅仅在new分支下存在，而在master分支下不存在。
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/d1f981db-6d74-4cad-93c7-e54aa36119ed" alt="分支互不干扰">
</div>  
因此，可以通过的分支合并来合并两个分支的工作内容。  

**合并分支**：`git merge dev`  
合并指定分支到当前分支上。  
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/71e556ab-22d6-4ceb-a59b-03bde2a4034e" alt="分支合并">
</div>
 

**删除分支**：`git branch -d <name>`  
在合并分支之后就可以放心删除分支了。

### 5.2 解决冲突

**多个分支在不同的文件上进行更改并提交(add and commit)**
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/b6907819-56a4-4071-9b38-8a86ed21849b" alt="分支一">
</div>
上图是分支一发生修改后的情况。  
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/9deb7351-f64d-4330-8ca5-b76e9c74bfae" alt="分支二">
</div>  
上图是分支二修改后的情况，可以看到两个的新文件不同。   

每个分支在不同的文件上进行修改，然后进行合并是可以的：  
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/7f076786-3389-48e9-b2f7-6d1d513e7f0b" alt="分支合并">
</div>  

---  
**多个分支在同一个文件更改并提交**  

如果几个分支(branch1 and branch2)都同时在一个文件(newbranch.txt)上进行了修改并保存分支(add and commit)，那么在合并时会发生冲突，如下图所示：  
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/5ebf1330-938b-4955-8ef5-7240c08565a8" alt="分支合并冲突">
</div>  
这时候git会显示不同分支的更改，并需要做出选择，或者把这些全部删除重新更新，相当于在branch2上又做出了更改，接下来就可以合并。  

<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/b27d1276-bdaa-4ab9-926e-e67b78cfbf46" alt="分支合并冲突">
</div>  
合并过程如下，从蓝色的状态变为红色的状态：
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/06805367-3a06-4213-a85d-3489b3cd9bd9" alt="合并指针">
</div>  

接下来就合并成功了。  
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/5b5340c5-270f-4a55-8d6c-3312b5734728" alt="分支合并冲突">  
</div>  

**总结：将git合并失败的文件手动编辑为我们想要的内容，再提交。**  

### 5.3 分支管理策略
在团队开发过程中，master分支是最稳定的分支(发行版本)，一般不用来直接开发，而是在新的分支上进行开发，然后合并到master分支。  

合并过程当中要常用`--no-ff`,意思是不采用 fast forward模式，在合并过程中留下历史记录。  

### 5.4 bug分支
软件开发过程中怎么修复bug？

## 遇到了问题：
在将本地仓库同步到远程仓库的时候，发现进行remote add连接之后，push命令不能直接使用，错误信息显示必需先pull一下，把GitHub上的文件拉下来，pull仍然会报错，这是因为在创建repo的时候添加了readme文件，所以不是一个空的分支，必须把两个不相关的库进行合并才可以进行下一步操作。  
命令：`git merge origin/master --allow-unrelated-histories`  
或者干脆不创建readme，之后再添加更省事。
