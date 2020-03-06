##Python读写文件操作

包含读写文件（txt、csv、json, pkl）

 - 判断文件是否存在
     + 如果存在就直接加载文件，否则创建一个新文件来保存数据 （CSV格式）

        ```
        import os

        filename = "xxx.xx"
        if not os.path.exists(filename):
            data.to_csv(filename, index=False, header=False, sep='\t')
        else:
            data = pd.read_csv(filename, header=None, sep='\t')
        ```

     + 不存在就新建文件夹（目录）
        ```
        if not os.path.exists(path):
            os.makedirs(path)
        ```

     + 不存在进新建文件
        ```
        if not os.path.exists(filename):
            os.system(r"touch {}".format(filename))  # 调用系统命令行来创建文件
        ```

 - 读写`JSON`文件
     + 逐行读写`JSON`文件

        ```
        # ========== write by lines. ==========
        _list = [{}, {}, ...]
        with open("lines_xxx.json", 'w') as fout:
            for item in _list:  # [{}, {}, ...]
                fout.write(json.dumps(item, ensure_ascii=False) + '\n')
        ```
        参数`ensure_ascii=False`用来正常显示中文，否则文件中中文将以`Unicode`格式来显示，参数`intent=4`用来格式化输出JSON文件，`open()`中的参数`encoding='utf-8'`用来避免中文乱码，输出的文件`lines_xxx.json`中的格式为，
        ```
        line 1: {}
        line 2: {}
        ...
        line n: {}
        ```
        加载上述格式`JSON`文件如下
        ```
        with open("lines_xxx.json", "r") as f:
            lines = f.readlines()
        for line in tqdm(lines):
            sample = json.loads(line)
            ...
            pass
        ```

     + 整体读写保存为{}格式

        ```
        # ========== bert format ==========
        bert_xxx = {}
        with open("xxx.json", 'w') as fout:
            fout.write(json.dumps(bert_xxx, ensure_ascii=False))
        ```
        读取上述格式文件使用，
        ```
        import json

        with open("xxx.json", "r") as f:
            data = json.load(f)
        ```
        加载之后`data`的类型为`dict`

 - 读写CSV格式的文件（以特定分隔符分隔，比如`\t`或者`,`）
     + 读文件
     ```
     import pandas as pd

     train = pd.read_csv('train.txt', header=None, sep='\t')  # DataFrame
     ```
     + 写文件

     加载为DataFrame格式，
     ```
     # type(result_append) is DataFrame
     result_append.to_csv('result.txt', index=False, header=False, sep='\t')
     ```
     `BERT`中的加载方式，
     ```
     def _read_tsv(input_file, quotechar=None):
        """Reads a tab separated value file."""
        with open(input_file, "r") as f:
            reader = csv.reader(f, delimiter="\t", quotechar=quotechar)
            lines = []
            for line in reader:
                lines.append(line)
            return lines
     ```

 - 读写`pickle`文件
     + 读文件
     ```
     import pickle

     fr = open("xxx.pkl", "rb")
     data = pickle.load(fr)
     fr.close()
     ```

     + 写文件
     ```
     fw = open("xxx.pkl", "wb")
     pickle.dump(data, fw)
     fw.close()
     ```
     注意：在操作类似`DataFrame`或者`CSV`格式的文件时，如果某个值内部含有换行符`\r`或者`\n`，直接使用`pd.to_csv()`或者`writeline()`是不行的，需要将其中的换行符移除，更直接的方式就是使用`pickle`的方式进行读写。

## Pycharm
 - 默认python文件注释
    ```
    # -*- coding:utf-8 -*-
    """
    @Time  : ${DATE} ${TIME}
    @Author: Yongbo Wang
    @Email : yongbowin@outlook.com
    @File  : ${NAME}.py
    @Desc  : 
    """
    ```

## python中argparse如何传入列表作为参数

```
import argparse
 
parser = argparse.ArgumentParser()
 
# By default it will fail with multiple arguments.
parser.add_argument('--default')
 
# Telling the type to be a list will also fail for multiple arguments,
# but give incorrect results for a single argument.
parser.add_argument('--list-type', type=list)
 
# This will allow you to provide multiple arguments, but you will get
# a list of lists which is not desired.
parser.add_argument('--list-type-nargs', type=list, nargs='+')
 
# This is the correct way to handle accepting multiple arguments.
# '+' == 1 or more.
# '*' == 0 or more.
# '?' == 0 or 1.
# An int is an explicit number of arguments to accept.
parser.add_argument('--nargs', nargs='+')
 
# To make the input integers
parser.add_argument('--nargs-int-type', nargs='+', type=int)
 
# An alternate way to accept multiple inputs, but you must
# provide the flag once per input. Of course, you can use
# type=int here if you want.
parser.add_argument('--append-action', action='append')
 
# To show the results of the given option to screen.
for _, value in parser.parse_args()._get_kwargs():
    if value is not None:
        print(value)
```

```
$ python arg.py --default 1234 2345 3456 4567
...
arg.py: error: unrecognized arguments: 2345 3456 4567
 
$ python arg.py --list-type 1234 2345 3456 4567
...
arg.py: error: unrecognized arguments: 2345 3456 4567
 
$ # Quotes won't help here... 
$ python arg.py --list-type "1234 2345 3456 4567"
['1', '2', '3', '4', ' ', '2', '3', '4', '5', ' ', '3', '4', '5', '6', ' ', '4', '5', '6', '7']
 
$ python arg.py --list-type-nargs 1234 2345 3456 4567
[['1', '2', '3', '4'], ['2', '3', '4', '5'], ['3', '4', '5', '6'], ['4', '5', '6', '7']]
 
$ python arg.py --nargs 1234 2345 3456 4567
['1234', '2345', '3456', '4567']
 
$ python arg.py --nargs-int-type 1234 2345 3456 4567
[1234, 2345, 3456, 4567]
 
$ # Negative numbers are handled perfectly fine out of the box.
$ python arg.py --nargs-int-type -1234 2345 -3456 4567
[-1234, 2345, -3456, 4567]
 
$ python arg.py --append-action 1234 --append-action 2345 --append-action 3456 --append-action 4567
['1234', '2345', '3456', '4567']
```