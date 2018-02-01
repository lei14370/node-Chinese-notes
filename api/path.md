##path
path模块提供了处理文件和目录路径的工具。
```javascript
  const path=require('path')
```

## Windows vs. POSIX

path模块默认运行模式取决于node运行的操作系统。具体而言，当node在Windows操作系统上运行时，path模块应使用Windows的路径样式。

我们以 path.basename() 方法为例，使用windows路径样式 **C:\temp\myfile.html**。

在POSIX系统上：

```javascript
path.basename('C:\\temp\\myfile.html');
// Returns: 'C:\\temp\\myfile.html'
```

在windows系统上：

```javascript
path.basename('C:\\temp\\myfile.html');
// Returns: 'myfile.html'
```

为了实现持续一致的结果，在任何操作系统使用Windows文件路径时，使用path.win32：

```javascript
path.win32.basename('C:\\temp\\myfile.html');
// Returns: 'myfile.html'
```

在任何操作系统使用POSIX文件路径时，使用path.posix：

```javascript
path.posix.basename('/tmp/myfile.html');
// Returns: 'myfile.html'
```

