
--
layout: blog
title: '使用Shell推送Git仓库-简单版'
date: 2017-04-28 12:11:34
categories: blog
tags: code
image: ''
--

## 使用Shell上传Git
先上代码
```
#!/bin/sh
echo -n "push当前项目到Git:(y/n)"
read is_current
push_folder=""
if [[ $is_current = "n" ]]; then
	echo -n "请输入项目路径："
	read $push_folder
elif [ $is_current = "y" ]
	push_folder=$(pwd)
	echo -n $push_folder
else
	push_folder=$(pwd)
fi

echo -n "push路径#" $push_folder
dir=$(ls -al $push_folder | awk '/^d/ {print $NF}')
is_git=0
for file in ${dir}/*; do
    temp_file=`basename $file`
    if [[ $temp_file = ".git" ]]; then
	echo -n "当前文件夹已初始化仓库"
	is_git=1
	# 退出循环
	break 
    fi
    echo "文件名 = " $temp_file
done   

echo -n "输入commit内容:"
read $commit_content

if [ $is_git = 1 ]; then
	echo -n "当前文件夹已初始化仓库"
	# 添加，push当前项目
	git add .
	git commit -m "$commit_content"
	git push origin master
fi

#for i in $dir
#do
#    echo $i
#done 
```

代码比较简单，获取一个文件夹，列出文件夹是否有.git 来判断当前文件夹是否已经初始化仓库，如果初始化过了，使用git命令进行提交
分支固定是origin的master，所以要确保在使用之前已经有远程仓库关联了

```
遇到的坑主要是shell中进行if判断的时候左右的[ ]和内容要分开，比较的 = 号两边也记得要分开
```



