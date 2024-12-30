---
title: "file"
linkTitle: "file"
type: "docs"
description: 文件系统操作
weight: 100
---

## read

`read(filepath: str) -> str`

读取文件 `filepath` 中的内容，并返回一个字符串实例。

```python
import file

a = file.read("test.txt")
```

## glob

`glob(pattern: str) -> str`

返回一个包含所有匹配 `pattern` 的文件名的列表。

```python
import file

json_files = file.glob("./*.json")
```

## modpath

`modpath() -> str`

返回当前模块的根路径（kcl.mod 文件路径或单个 \*.k 文件路径）。

```python
import file

modpath = file.modpath()
```

## workdir

`workdir() -> str`

返回当前工作目录的路径。

```python
import file

workdir = file.workdir()
```

## exists

`exists(filepath: str) -> bool`

判断文件路径是否存在。如果路径指向一个存在的实体，则返回 True。此函数会遍历符号链接以查询目标文件的信息。

```python
import file

file_exists = file.exists("test.txt")
```

## abs

`abs(filepath: str) -> str`

返回路径的规范化绝对形式，其中所有中间路径均已规范化并解析为符号链接。

```python
import file

abs_file_path = file.abs("test.txt")
```

## mkdir

`mkdir(directory: str, exists: bool=False)`

如果指定路径上不存在目录，则创建一个新目录。

```python
import file

file.mkdir("path")
```

## delete

`delete(directory: str)`

在指定路径上删除一个文件或空目录。

```python
import file

file.delete("test.txt")
```

## cp

`cp(src: str, dest: str)`

将文件或目录从源路径复制到目标路径。

```python
import file

file.cp("src", "dest")
```

## mv

`mv(src: str, dest: str)`

将文件或目录从源路径移动到目标路径。

```python
import file

file.mv("src", "dest")
```

## size

`size(filepath: str) -> int`

获取指定路径上文件的大小。

```python
import file

size = file.size("test.txt")
```

## write

`write(filepath: str, content: str)`

将内容写入指定路径的文件。如果文件不存在，将会被创建。如果文件存在，其内容将被替换。

```python
import file

file.size("test.txt", "content")
```

## read_env

`read_env(key: str) -> str`

从当前进程中读取环境变量 `key` 的值。

```python
import file

value = file.read_env("ENV_VAR")
```

## current

`current() -> str`

读取正在执行的当前 KCL 代码文件或模块的路径。

```python
import file

value = file.current()
```
