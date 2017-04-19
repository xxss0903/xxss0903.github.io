
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

# 进行git的函数，处理从add到push
function gitpush()
{
	error_str="fatal"
	origin_add=""

	# 仓库add 
	var=$(git add . 2>&1)
	echo $var

	# 下面通过while循环判断两个git的命令是否成功执行，没有成功执行继续输入

	# commit
	while [ "1" = "1"  ]
	do
		echo -n "输入commit内容:"
		read commit_msg
		echo $commit_msg
		# 判断是否commit成功
		var=$(git commit -m "$commit_msg" 2>&1)
		echo $var
		if [[ "$var" =~ $error_str ]]; then
			echo "***提交错误***"
		else
			echo "***提交成功***"
			break
		fi	
	done


	# push
	while [ "1" = "1" ]
	do
		echo "***开始push本地仓库***"
		var=$(git push -u origin master 2>&1)
		if [[ $var =~ $error_str ]]; then 
			echo -n "推送失败，未添加远程仓库，是否添加远程仓库(y/n): "
			read add_origin_repo
			if [[ $add_origin_repo = "y" ]]; then
				echo -n "输入远程仓库地址:"
				read origin_add
				var=$(git remote add origin $origin_add 2>&1)
				echo $var
			else
				echo "***退出***"
				break
			fi
			var=$(git push -u origin master 2>&1)
		elif [[ $var =~ "git pull" ]]; then 
			echo "***pull远程仓库***"
			var=$(git pull 2>&1)
			echo $var
		else
			echo $var
			echo "***push完成***"
			break
		fi
	done
}


echo -n "push当前项目到Git:(y/n)"
read is_current
push_folder=""
echo -n "输入(y/n):" $is_current
echo 
if [[ $is_current = "y" ]]; then
	push_folder=$(pwd)
elif [[ $is_current = "n" ]]; then
	echo -n "请输入项目路径："
	read push_folder
else
	push_folder=$(pwd)
fi

echo "项目地址:" $push_folder
dir=$(ls -al $push_folder | awk '/^d/ {print $NF}')
is_git=0
for file in ${dir}/.; do
	temp_file=`basename $file`
	# echo "$temp_file"
	if [[ $temp_file =~ ".git" ]]; then
		is_git=1
		break 
	fi
done   


if [[ $is_git = 1 ]]; then
	# 仓库已经初始化，直接push
	echo  "***已初始化仓库***"
	gitpush
else
	# 当前仓库没有初始化，需要新初始化，然后push
	echo "***仓库未初始化，开始初始化仓库***"
	var=$(git init 2>&1)
	echo $var

	# 调用提交函数
	gitpush
fi
```

代码比较简单，获取一个文件夹，列出文件夹是否有.git 来判断当前文件夹是否已经初始化仓库，如果初始化过了，使用git命令进行提交
分支固定是origin的master，所以要确保在使用之前已经有远程仓库关联了

* 添加了上传的函数，通过重定向获取git的输出信息，判断是否成功进行git，非成功之后继续进行

```
遇到的坑主要是shell中进行if判断的时候左右的[ ]和内容要分开，比较的 = 号两边也记得要分开
```



