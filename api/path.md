# path

path模块提供了处理文件和目录路径的工具。
```javascript
  const path=require('path')
```

## Windows vs. POSIX

path模块默认运行模式取决于node运行的操作系统。具体而言，当`node`在`Windows`操作系统上运行时，path模块应使用Windows的路径样式。

我们以 `path.basename()` 方法为例，使用windows路径样式 **`C:\temp\myfile.html`**。

在`POSIX`系统上：

```javascript
path.basename('C:\\temp\\myfile.html');
// Returns: 'C:\\temp\\myfile.html'
```

在`windows`系统上：

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

注意：在windows上，node遵循每个驱动器工作目录的概念，就是说有反斜杠和没有反斜杠存在区别。比如：`path.resolve('c:')`和`path.resolve('c:\\')`。

## path.basename(path[, ext])

- `path` <string>

- `ext` <string> 文件扩展名

- `retrun`  <string>

  ​

`path.basename`返回文件路径的最后一部分，文件扩展名可被忽略。

例如：

```javascript
path.basename('/foo/bar/baz/asdf/quux.html');
// Returns: 'quux.html'

path.basename('/foo/bar/baz/asdf/quux.html', '.html');
// Returns: 'quux'
```

## path.delimiter

- <string>

提供操作系统特定的路径分隔符：

- `;`  Windows
- `:`  POSIX

例如，在POSIX上：

```javascript
console.log(process.env.PATH);
// Prints: '/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin'

process.env.PATH.split(path.delimiter);
// Returns: ['/usr/bin', '/bin', '/usr/sbin', '/sbin', '/usr/local/bin']
```

例如，在windows上：

```javascript
console.log(process.env.PATH);
// Prints: 'C:\Windows\system32;C:\Windows;C:\Program Files\node\'

process.env.PATH.split(path.delimiter);
// Returns ['C:\\Windows\\system32', 'C:\\Windows', 'C:\\Program Files\\node\\']
```

## path.dirname(path)

- `path` <string>
- Returns: <string>

`path.dirname()`返回一个路径的目录名称，最后一级目录的分割符被忽略；

例如:

```javascript
path.dirname('/foo/bar/baz/asdf/quux');
// Returns: '/foo/bar/baz/asdf'
```

## path.extname(path)

- `path` <string>
- Returns: <string>

`path.extname()`返回文件的扩展名称，取包括最后一个`.`之后的字符（以`.`开头的不算）；

例如:·

```javascript
path.extname('index.html');
// Returns: '.html'

path.extname('index.coffee.md');
// Returns: '.md'

path.extname('index.');
// Returns: '.'

path.extname('index');
// Returns: ''

path.extname('.index');
// Returns: ''
```

## path.format(pathObject)

- `pathObject`<Object>
  - `dir`<string>
  - `root`<string>
  - `base` <string>
  - `name`<string>
  - `ext` <string>
- Returns:<string>

`path.format（）`方法返回一个路径字符串。 与`path.parse（）`相反。

例如, 在 POSIX上:

```javascript
// If `dir`, `root` and `base` are provided,
// `${dir}${path.sep}${base}`
// will be returned. `root` is ignored.
path.format({
  root: '/ignored',
  dir: '/home/user/dir',
  base: 'file.txt'
});
// Returns: '/home/user/dir/file.txt'

// `root` will be used if `dir` is not specified.
// If only `root` is provided or `dir` is equal to `root` then the
// platform separator will not be included. `ext` will be ignored.
path.format({
  root: '/',
  base: 'file.txt',
  ext: 'ignored'
});
// Returns: '/file.txt'

// `name` + `ext` will be used if `base` is not specified.
path.format({
  root: '/',
  name: 'file',
  ext: '.txt'
});
// Returns: '/file.txt'

```

在Windows上:

```javascript
path.format({
  dir: 'C:\\path\\dir',
  base: 'file.txt'
});
// Returns: 'C:\\path\\dir\\file.txt'
```

## path.isAbsolute(path)

- `path`<string>
- Returns:<boolean>

`path.isAbsolute()`判断是不是绝对路径；

例如， 在 POSIX:

```javascript
path.isAbsolute('/foo/bar'); // true
path.isAbsolute('/baz/..');  // true
path.isAbsolute('qux/');     // false
path.isAbsolute('.');        // false

```

在 Windows:

```javascript
path.isAbsolute('//server');    // true
path.isAbsolute('\\\\server');  // true
path.isAbsolute('C:/foo/..');   // true
path.isAbsolute('C:\\foo\\..'); // true
path.isAbsolute('bar\\baz');    // false
path.isAbsolute('bar/baz');     // false
path.isAbsolute('.');           // false
```

## path.join([...paths])

- `...paths`<string>
- Returns: <string>

`path.join（）`使用平台特定的分隔符作为分隔符将所有给定的路径字段连接在一起，规范化结果路径。

例如:

```javascript
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..');
// Returns: '/foo/bar/baz/asdf'

path.join('foo', {}, 'bar');
// throws 'TypeError: Path must be a string. Received {}'
```

