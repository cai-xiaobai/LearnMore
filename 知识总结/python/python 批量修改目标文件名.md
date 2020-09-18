## python 批量修改目标文件名

### 1、os模块

​		在程序中，我们经常需要对大量文件和路径进行操作，比如：查询某一路径下的同种类型文件，批量修改文件名字等。这些操作就依赖于os模块。os模块提供了非常丰富的方法用来处理文件和目录。

os模块参考文档：https://docs.python.org/zh-cn/3.7/library/os.html

```python
import os
print(f'当前工作路径:{os.getcwd()}')
```

结果

```shell
当前工作路径:C:\Users
```

### 2、re模块

​		正则表达式是本身是一种小型的、高度专业化的编程语言，是用来匹配处理字符串的。而在python中，通过内嵌集成re模块，程序可以直接调用来实现正则匹配。

re模块参考文档:https://docs.python.org/zh-cn/3/library/re.html

### 3、os模块中的path

​		**os.path**模块主要用于文件的属性获取，例如：去掉目录路径、单独返回文件名、判断指定路径（目录或文件）是否存在等等。以下是该模块的几种常用方法：

| 函数名                    | 使用方法                                                     |
| ------------------------- | ------------------------------------------------------------ |
| isdir(path)               | 判断指定路径是否存在且是一个目录                             |
| isfile(path)              | 判断指定路径是否存在且是一个文件                             |
| splitext(path)            | 分离文件名与扩展名，返回(f_name,f-extension)元组             |
| join(path1[,path2[,...]]) | 将path1,path2各部分组合成一个路径名                          |
| exists(path)              | 判断指定路径（目录或文件）是否存在                           |
| getsize(file)             | 返回指定文件的尺寸，单位是字节                               |
| getctime(file)            | 返回指定文件的创建时间（浮点型秒数，可用time模块的gmttime()或localtime()函数换算） |

```python
import os
# 返回绝对路径
result = os.path.abspath('./new文件夹')
print(result)

# 判断是否是文件夹
result2 = os.path.isdir('./new文件夹')
print(result2)

# 判断是否是文件
result3 = os.path.isfile('./os.ipynb')
print(result3)

# 分离文件名与扩展名
result4 = os.path.splitext('os.ipynb')
print(result4)

# 判断指定路径下的文件是否存在
result5 = os.path.exists('C:/Users/os.ipynb')
print(result5)

# 返回指定文件的创建时间
result6 = os.path.getctime('C:/Users/new文件夹')
print(result6)

# 拓展 时间换算
import time
timearr = time.localtime(result6)
mytime = time.strftime('%y-%m-%d %H:%M:%S',timearr)
print(mytime)
```

结果：

```shell
C:\Users\new文件夹
True
True
('os', '.ipynb')
True
1599098851.2891724
20-09-03 10:07:31
```

### 4、os模块中的listdir 和 rename

`os.listdir()`方法用于返回指定的文件夹包含的文件或文件夹的名字的列表

`os.rename()`方法用于重命名文件或目录。

`os.rename(oldpath,newpath)` 

- -oldpath    要修改的目录名或文件名
- -newpath  修改后的目录名或文件名

```python
import os
# 获取到文件夹下的所有文件名字
result = os.listdir('C:/Users')
# print(result)

oldpath = os.path.join('C:/Users','phone-v.png')
newpath = os.path.join('C:/Users','phone.png')
print(oldpath)
# 修改名字的方法
os.rename(oldpath,newpath)
```

结果：

```shell
C:/Users/phone-v.png
```



### 5、自动化修改文件名

在了解以上知识，我们就能够动手开始自动化修改文件名了，功能包括，在头部添加、在尾部添加、替换、指定类型文件

- target_dir 文件路径
- addstr       添加/修改的字段
- position    
  - end   在尾部添加
  - head 在头部添加
  - replace 替换
- oldstr     需要替换的文本
- newstr   替换后的新文本

```python
# 批量修改文件名字
import re
def filename_modify(target_dir,addstr='',position = 'end',oldstr='',newstr='',filetype=''):
    # 1. 判断路径是否存在
    if os.path.exists(target_dir)==False:
        raise Exception('路径下不存在该文件')
        
    # 2. 遍历文件夹中的文件名
    for file in os.listdir(target_dir):
        # 分割文件名和扩展名 ('文件名','')
        filename = os.path.splitext(file)[0]
        fileExpand = os.path.splitext(file)[1]
        
        # 不修改文件夹的名字 
        if fileExpand == '':
            continue
        # 过滤文件的类型，从而达到修改指定类型
        if filetype != None and filetype not in fileExpand:
            continue
        
        # 判断添加的位置
        if(position == 'end'):
            # 将所有文件的后面添加 -new
            newname = filename + addstr + fileExpand
        elif position == 'head':
            newname = addstr + filename + fileExpand
        elif position == 'replace':
            pattern = re.findall(oldstr,filename)
            for value in pattern:   
                filename = filename.replace(value,newstr)
                
            newname = filename + fileExpand
        
        # 修改名字
        oldpath = os.path.join(target_dir,file)
        newpath = os.path.join(target_dir,newname)
        os.rename(oldpath,newpath)
#         print(newname)
    pass
filename_modify('C:/Users/Untitled Folder/new文件夹',position='head',addstr='ABC-',filetype='xlsx')
```

