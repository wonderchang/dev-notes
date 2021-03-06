---
title: Python / File I/O
---
# [Python](python.md) / File I/O

## File Object ??

  - [file object - Glossary — Python 3\.6\.5rc1 documentation](https://docs.python.org/3/glossary.html#term-file-object)
      - File object 對外提供 file-oriented API (例如 `read()`、`write()` 等)，居間協調對底層 resource 的存取 -- 可能是 on-disk file、standard I/O、in-memory buffer、socket 等。
      - 事實上有 3 種 file object -- raw/buffered binary file??、text file，這些 interface 定義在 `io` module，而建立 file object 最標準的方法是透過 `open()`。
  - [file(name[, mode[, buffering]]) - 2\. Built\-in Functions — Python 2\.7\.14 documentation](https://docs.python.org/2.7/library/functions.html#file) `file()` 做為 `file` type 的 constructor function，接受的參數跟 `open()` 一樣。不過開啟檔案建議用 `open()`，而 `file` 可以用在 type testing，例如 `isinstance(f, file)`。
  - [File Objects - 5\. Built\-in Types — Python 2\.7\.14 documentation](https://docs.python.org/2/library/stdtypes.html#file-objects) #ril
  - [15\.2\. io — Core tools for working with streams — Python 2\.7\.14 documentation](https://docs.python.org/2.7/library/io.html) 提供 stream handling 的 interface，在 Python 2 可以跟 `file` 替換，但在 Python 3 就反過來變成 default interface #ril

## File-like Object ??

  - [file object - Glossary — Python 3\.6\.5rc1 documentation](https://docs.python.org/3/glossary.html#term-file-object) File object 也稱做 file-like object 或 stream，就是個同義字 (synonym)；雖然這麼說，但 `isinstance(StringIO.StringIO(), file)` 結果卻是 false。
  - [7\.5\. StringIO — Read and write strings as files — Python 2\.7\.14 documentation](https://docs.python.org/2/library/stringio.html) 也實作了 file-like #ril
  - [Check if object is file\-like in Python \- Stack Overflow](https://stackoverflow.com/questions/1661262/) 提到 "behave like a real file"，所以 `isinstance(fp, file)` 這樣的檢查不好，但要怎麼檢查 file-like 呢? #ril
  - [15.1.4. Files and Directories - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os-file-dir) #ril

## open(), close() ??

  - `open().read()` 後沒 close 會怎樣?? 放 context manager 裡最安全
  - [open(name[, mode[, buffering]]) - 2\. Built\-in Functions — Python 2\.7\.14 documentation](https://docs.python.org/2.7/library/functions.html#open) #ril
  - [python \- Does a File Object Automatically Close when its Reference Count Hits Zero? \- Stack Overflow](https://stackoverflow.com/questions/1834556/) #ril

## Mode ??

  - [7.2. Reading and Writing Files - 7\. Input and Output — Python 2\.7\.15 documentation](https://docs.python.org/2/tutorial/inputoutput.html#reading-and-writing-files) #ril
      - `open(filename, mode)` 最常搭配 2 個參數，第 2 個參數 `mode` -- string containing a few characters describing the way in which the file will be used (依用途不同)，預設是 `r`。
      - 上述的 character 可以是 `r` (reading)、`w` (writing) (覆寫現有的檔案)、`a` (appending) (接在檔尾)。另外 `r+` 用在 reading & writing? 如何控制讀寫的位置??
      - 在 Windows 上可以額外加上 `b` 要求以 binary mode 開啟檔案 (例如 `rb`、`wb` 或 `r+b`)，因為 Python 會在讀寫 text data 時改變 EOL character，所以不適用 binary data，不過在 Unix-like 上加上 `b` 也不會有問題，反而可以讓 code 達到 platform-independent。
  - [7.2. Reading and Writing Files - 7\. Input and Output — Python 3\.7\.1 documentation](https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files) Python 2 加不加 `b` 沒差，但 Python 就有差了 #ril
  - [text file, binary file - Glossary — Python 3\.7\.1 documentation](https://docs.python.org/3/glossary.html#term-text-file) #ril
      - Text file - A file object able to read and write `str` objects. Often, a text file actually accesses a byte-oriented datastream and handles the text encoding AUTOMATICALLY. 但 `open()` 時也沒給 encoding，如何自動做?? Examples of text files are files opened in text mode (`'r'` or `'w'`), `sys.stdin`, `sys.stdout`, and instances of `io.StringIO`. 有 system encoding 這種東西嗎??
      - Binary file - A file object able to read and write bytes-like objects. Examples of binary files are files opened in binary mode (`'rb'`, `'wb'` or `'rb+'`), `sys.stdin.buffer`, `sys.stdout.buffer`, and instances of `io.BytesIO` and `gzip.GzipFile`.
      - 顯然 `open()` 時有沒有加 `b`，決定了讀寫時可接受的型態 `str` 或 bytes-like；會有下面的狀況：

            >>> with open('data', 'w') as f: f.write(b'data')
            ...
            Traceback (most recent call last):
              File "<stdin>", line 1, in <module>
            TypeError: write() argument must be str, not bytes
            >>> with open('data', 'wb') as f: f.write('data')
            ...
            Traceback (most recent call last):
              File "<stdin>", line 1, in <module>
            TypeError: a bytes-like object is required, not 'str'

## CWD (Current Working Directory)

  - [os.getcwd() - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.getcwd) `os.getcwd()` 與 `os.getcwdu()` 都會傳回 current working directory，只是型態不同分別是 `str` 與 `unicode`。
  - [os.chdir(path) - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.chdir) `os.chdir(path)` 跟 `os.fchrid(fd)` 都可以切換 CWD，只是前者接受 pathname，後者接受 file descriptor；注意不是 `chcwd()`。

## Directory Listing

要知道某個目錄底下有哪些檔案或目錄，最簡單的方式就是用 `os.listdir()`。

```
$ pwd; tree -aNF
/tmp/dir
.
|-- file.txt
|-- .hidden.txt
|-- subdir/
|   |-- file.png
|   |-- file.txt
|   |-- .hidden.txt
|   |-- subdir2-1/
|   |   |-- file.png
|   |   |-- file.txt
|   |   `-- .hidden.txt
|   |-- subdir2-2/
|   `-- subdir2-3/
`-- 子目錄/
    |-- a.png
    |-- b.png
    |-- c.png
    `-- 檔案.txt
```

```
>>> import os
>>> files = os.listdir('/tmp/dir') # <1>
>>> print files
['.hidden.txt', 'subdir', '\xe5\xad\x90\xe7\x9b\xae\xe9\x8c\x84', 'file.txt']
>>> for file in files: print file,
...
.hidden.txt subdir 子目錄 file.txt
>>>
>>> print os.getcwd(), os.listdir('subdir') # <2>
/tmp/dir ['subdir2-2', 'subdir2-1', '.hidden.txt', 'file.png', 'subdir2-3', 'file.txt']
>>>
>>> files = os.listdir('/tmp/dir/\xe5\xad\x90\xe7\x9b\xae\xe9\x8c\x84'.decode('utf-8')) # <3>
>>> print files
[u'a.png', u'c.png', u'\u6a94\u6848.txt', u'b.png']
>>> for file in files: print file,
...
a.png c.png 檔案.txt b.png
```

 1. `listdir(path)` 可以列出 `path` 底下的檔案跟子目錄（包括隱藏檔，但不包含 `.` 跟 `..` 這兩個項目）。
 2. `listdir(path)` 的 `path` 也可以是相對於 CWD 的路徑。
 3. `listdir()` 回傳的結果並沒有排序過，而且回傳結果的字串型態（`str` 或 `unicode`）會跟著 `path` 走。

如果要過濾路徑名稱符合某些條件的檔案，改用 `glob.glob()` 可以省掉一些工。

```
>>> import glob
>>> glob.glob('/tmp/dir/*') # <1>
['/tmp/dir/subdir', '/tmp/dir/\xe5\xad\x90\xe7\x9b\xae\xe9\x8c\x84', '/tmp/dir/file.txt']
>>> print os.getcwd(), glob.glob(u'*')
/tmp/dir [u'subdir', u'\u5b50\u76ee\u9304', u'file.txt']
>>>
>>> for entry in glob.iglob(u'*'): print repr(entry), # <2>
... 
u'subdir' u'\u5b50\u76ee\u9304' u'file.txt'
>>>
>>> glob.glob('.*.txt') # <3>
['.hidden.txt']
>>>
>>> glob.glob(u'*/*.txt') # <4>
[u'subdir/file.txt', u'\u5b50\u76ee\u9304/\u6a94\u6848.txt']
>>> glob.glob(u'**/*.txt')
[u'subdir/file.txt', u'\u5b50\u76ee\u9304/\u6a94\u6848.txt']
>>>
```

 1. `glob(pathname)` 會列出符合 `pathname` 這個 pattern 的檔案或目錄（但不包含隱藏檔）。

    由於 `glob()` 背後也是呼叫 `listdir()`，回傳結果的字串型態（`str` 或 `unicode`）會跟著 `pathname` 走。

    `pathname` 也可以是相對於 CWD 的路徑，但注意結果也變成相對路徑了。也就是說結果是用絕對路徑或相對路徑來表示，跟 `pathname` 的表示法有關。另外 `pathname` 支援 shell-style wildcards 的用法，包括 `*`、`?` 以及 `[]`，更多的細節可以參考 `fnmatch` 模組。

 2. `iglob()` 跟 `glob()` 一樣，只是回傳 iterator 而已，適用目錄下的檔案數很多時。
 3. 在 pattern 前面明確加上 `.` 可以列出隱藏檔，但這麼一來非隱藏檔就都被濾除了。
 4. `glob()` 支援 `*/*` 的用法，但也僅限於該層目錄，並不會自動往下鑽（改成 `**/*` 的寫法也沒用）。

如果搜尋的範圍是整個樹狀結構（directory tree）而非單個目錄，`os.walk()` 會更為適用。

    walk(top, topdown=True, onerror=None, followlinks=False)

```
>>> for dirpath, dirnames, filenames in os.walk('/tmp/dir/subdir'): # <1>
...     print dirpath
...     print '    sub directories:', dirnames
...     print '    files:', filenames
...
/tmp/dir/subdir
    sub directories: ['subdir2-2', 'subdir2-1', 'subdir2-3']
    files: ['.hidden.txt', 'file.png', 'file.txt']
/tmp/dir/subdir/subdir2-2
    sub directories: []
    files: []
/tmp/dir/subdir/subdir2-1
    sub directories: []
    files: ['.hidden.txt', 'file.png', 'file.txt']
/tmp/dir/subdir/subdir2-3
    sub directories: []
    files: []
>>>
>>> for dirpath, dirnames, filenames in os.walk(u'subdir', topdown=False): # <2>
...     print dirpath
...     print '    sub directories:', dirnames
...     print '    files:', filenames
...
subdir/subdir2-2
    sub directories: []
    files: []
subdir/subdir2-1
    sub directories: []
    files: [u'.hidden.txt', u'file.png', u'file.txt']
subdir/subdir2-3
    sub directories: []
    files: []
subdir
    sub directories: [u'subdir2-2', u'subdir2-1', u'subdir2-3']
    files: [u'.hidden.txt', u'file.png', u'file.txt']
```

 1. `walk()` 會鑽進樹狀結構，傳回每一層目錄的內容 (3-tuple `(dirpath, dirnames, filenames)`)。其中 `dirpath` 是某一層目錄的路徑名稱，至於是用絕對路徑或相對路徑來表示，則跟一開始傳入的 `top` 有關。`dirnames` 是 `dirpath` 底下零或多個子目錄的清單，至於檔案的清單則在 `filenames`。
 2. 改用 `topdown=False` 後，目錄處理的順序變成由下而上（bottom-up; `subdir/subdir2-*` --> `subdir`），跟預設 `topdown=True` 的由上而下（top-down; `subdir` --> `subdir/subdir2-*`）不同。

使用 `walk()` 時，最關鍵的就是瞭解 top-down 跟 bottom-up 的差異，因為這將關係到能否在過程中對 `dirnames` 動手腳，進而影響 `walk()` 接下來處理子目錄的順序，甚至選擇性地只處理某些目錄。

觀察上面的例子，處理到 `/tmp/dir/subdir` 時，子目錄的處理順序是 `subdir2-2` --> `subdir2-1` --> `subdir2-3`。如果想讓處理的順序變成 `subdir2-1` --> `subdir2-2` --> `subdir2-3`，就必須在處理到 `/tmp/dir/subdir` 時，對 `dirnames` 動點手腳：

```
>>> for dirpath, dirnames, filenames in os.walk('/tmp/dir/subdir'):
...     dirnames.sort() # <1>
...     print dirpath
...     print '    sub directories:', dirnames
...     print '    files:', filenames
...
/tmp/dir/subdir
    sub directories: ['subdir2-1', 'subdir2-2', 'subdir2-3']
    files: ['.hidden.txt', 'file.png', 'file.txt']
/tmp/dir/subdir/subdir2-1
    sub directories: []
    files: ['.hidden.txt', 'file.png', 'file.txt']
/tmp/dir/subdir/subdir2-2
    sub directories: []
    files: []
/tmp/dir/subdir/subdir2-3
    sub directories: []
    files: []
>>>
>>> for dirpath, dirnames, filenames in os.walk('/tmp/dir/subdir', topdown=False): # <2>
...     dirnames.sort()
...     print dirpath
...     print '    sub directories:', dirnames
...     print '    files:', filenames
...
/tmp/dir/subdir/subdir2-2
    sub directories: []
    files: []
/tmp/dir/subdir/subdir2-1
    sub directories: []
    files: ['.hidden.txt', 'file.png', 'file.txt']
/tmp/dir/subdir/subdir2-3
    sub directories: []
    files: []
/tmp/dir/subdir
    sub directories: ['subdir2-1', 'subdir2-2', 'subdir2-3']
    files: ['.hidden.txt', 'file.png', 'file.txt']
```

 1. 對 `dirnames` 做 in-place 的排序，就可以影響 "接下來" 處理子目錄的順序。
 2. 同樣的動作在 bottom-up 時起不了作用，因為處理到 `/tmp/dir/subdir` 之前，`walk()` 早已走過底下的子目錄。

---

參考資料：

  - [`os.listdir()` - os — Miscellaneous operating system interfaces — Python 3\.7\.2 documentation](https://docs.python.org/3/library/os.html#os.listdir) #ril
  - [`os.walk()` - os — Miscellaneous operating system interfaces — Python 3\.7\.2 documentation](https://docs.python.org/3/library/os.html#os.walk) #ril
  - [glob — Unix style pathname pattern expansion — Python 3\.7\.2 documentation](https://docs.python.org/3/library/glob.html) #ril

## File Descriptor (FD) ??

  - [`os.fdopen(fd[, mode[, bufsize]])` - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.16 documentation](https://docs.python.org/2/library/os.html#os.fdopen)

      - Return an open file object connected to the file descriptor `fd`. The `mode` and `bufsize` arguments have the same meaning as the corresponding arguments to the built-in `open()` function. If `fdopen()` raises an exception, it leaves `fd` UNTOUCHED (UNCLOSED).
      - Availability: Unix, Windows.
      - Changed in version 2.3: When specified, the `mode` argument must now start with one of the letters `'r'`, `'w'`, or `'a'`, otherwise a `ValueError` is raised.
      - Changed in version 2.5: On Unix, when the `mode` argument starts with `'a'`, the `O_APPEND` flag is set on the file descriptor (which the `fdopen()` implementation already does on most platforms).

    這裡沒提到 `mode` 參數要跟當初 file descriptor 開啟的一樣，否則會丟 `OSError: [Errno 22] Invalid argument`：

        >>> f = open('/tmp/file', 'wb')
        >>> f.fileno(), os.fstat(f.fileno())
        (3, posix.stat_result(st_mode=33188, st_ino=8614118475, st_dev=16777220, st_nlink=1, st_uid=502, st_gid=0, st_size=0, st_atime=1557967095, st_mtime=1557967122, st_ctime=1557967122))
        >>> f2 = os.fdopen(f.fileno())
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
        OSError: [Errno 22] Invalid argument
        >>> f2 = os.fdopen(f.fileno(), 'rb')
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
        OSError: [Errno 22] Invalid argument
        >>> f2 = os.fdopen(f.fileno(), 'wb') # mode 要一樣才不會丟錯
        >>> assert f.fileno() == f2.fileno()
        >>> f2.fileno(), os.fstat(f2.fileno())
        (3, posix.stat_result(st_mode=33188, st_ino=8614118475, st_dev=16777220, st_nlink=1, st_uid=502, st_gid=0, st_size=0, st_atime=1557967095, st_mtime=1557967122, st_ctime=1557967122))
        >>> del f
        >>> os.fstat(f2.fileno())
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
        OSError: [Errno 9] Bad file descriptor

  - [File Descriptor Operations - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.16 documentation](https://docs.python.org/2/library/os.html#file-descriptor-operations) #ril

      - These functions operate on I/O streams referenced using file descriptors.

      - File descriptors are small integers corresponding to a file that HAS BEEN OPENED BY THE CURRENT PROCESS.

        For example, standard input is usually file descriptor 0, standard output is 1, and standard error is 2. Further files opened by a process will then be assigned 3, 4, 5, and so forth. The name “file descriptor” is slightly DECEPTIVE; on Unix platforms, sockets and pipes are also referenced by file descriptors.

        例如 Bash redirection 常用的 `2>&1` 就是 file descriptor，因為是針對目前的 process，所以數字都從 0, 1, 2 起算；不過它不只用來表示 file，也可以是 socket 或 pipe。

      - The `fileno()` method can be used to obtain the file descriptor associated with a file object when required. Note that using the file descriptor directly will BYPASS THE FILE OBJECT methods, ignoring aspects such as INTERNAL BUFFERING of data.

        從 file object 可以取得 file descriptor/no：

            >>> f = open('/tmp/file', 'wb')
            >>> f.fileno()
            3

        要將 file descriptor 包裝成 file object，則可以用 `os.fdopen()`，例如從 `tempfile.mkstemp()` 拿到 file descriptor 時。

      - `os.close(fd)`

        Close file descriptor fd. Availability: Unix, Windows.

        Note This function is intended for low-level I/O and must be applied to a file descriptor as returned by `os.open()` or `pipe()`. To close a “file object” returned by the built-in function `open()` or by `popen()` or `fdopen()`, use its `close()` method.

      - `os.open(file, flags[, mode])`

        Open the file file and set various flags according to `flags` and possibly its mode according to `mode`. The default mode is `0777` (octal), and the current UMASK VALUE ?? is first masked out. Return the file descriptor for the newly opened file.

        For a description of the flag and mode values, see the C run-time documentation; flag constants (like `O_RDONLY` and `O_WRONLY`) are defined in this module too (see `open()` flag constants). In particular, on Windows adding `O_BINARY` is needed to open files in binary mode.

        Availability: Unix, Windows.

        Note This function is intended for low-level I/O. For normal usage, use the built-in function `open()`, which returns a “file object” with `read()` and `write()` methods (and many more). To wrap a file descriptor in a “file object”, use `fdopen()`.

## Pathname ??

  - [10\.1\. os\.path — Common pathname manipulations — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.path.html) #ril

## File, Directory ??

  - [os.stat(path) - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.stat) #ril

## Directory Listing ??

  - [os.listdir() - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.listdir) #ril
  - [os.walk() - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.walk) #ril
      - `os.walk(top, topdown=True, onerror=None, followlinks=False)` 是個 generator，會遶行 `top` directory 底下所有的目錄 (分為 top-down 跟 bottom-up 兩個方向)，每一層目錄 (包括 `top` 自己)，都會產出 (yield) 一個 3-tuple `(dirpath, dirnames, filenames)`。
      - 其中 `dirpath` 是目前要處理的目錄 (可能是相對或絕對路徑)，底下的子目錄 (不含 `.` 與 `..`) 跟檔案則分別在 `dirnames` 與 `filenames` 裡。
      - `topdown=True` 時，3-tuple 裡的 `dirnames` 還沒遶進去，所以有機會調整它 (in-place)，進而影響 `os.walk()` 接下來會依序遶進哪些 subdirectory。而當 `topdown=False` 時，由於拿到 3-tuple 時 `dirnames` 裡的 subdirectory 都已經走過，所以修改它並沒有效果。
      - 背後是用 `listdir()` 來取得每一層目錄的內容，過程中若遇到錯誤，預設會忽略，除非有指定 `onerror` 一個 function，發生錯誤時會被呼叫並傳入 `OSError`，可以從 `filename` attribute 拿到出狀況的 directory。
      - `followlinks=False` 表示預設不會遶進 symbolic link 型態的目錄。

## File/Directory Operation - Copy, Move/Rename, Delete ??

  - [os.mkdir(path[, mode]) - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.mkdir)
  - [os.makedirs(path[, mode]) - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.makedirs)
  - [os.rmdir(path) - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.rmdir) #ril
  - [os.remove(path) - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.remove) #ril
  - [os.removedirs(path) - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.removedirs) #ril
  - [os.rename(src, dst) - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.rename) #ril
  - [os.renames(old, new) - 15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.renames) #ril
  - [10\.10\. shutil — High\-level file operations — Python 2\.7\.14 documentation](https://docs.python.org/2/library/shutil.html) #ril

## Temporary File/Dir ??

單純要取得系統的暫存目錄，呼叫 `tempfile.gettempdir()` 即可。

```
>>> import tempfile # Linux
>>> tempfile.gettempdir() # 傳回值的型態是字串。
'/tmp'
>>>

>>> import tempfile # Windows
>>> print tempfile.gettempdir()
c:\docume~1\user\locals~1\temp
>>>
```

但有時候為了保有原來的檔名，也為了避開相同檔名的衝突，把檔案放在某個暫存目錄下是個不錯的做法。

    tempfile.mkdtemp([sufﬁx='' [, preﬁx='tmp' [, dir=None]]])

```
>>> d = tempfile.mkdtemp()
>>> type(d), d
(<type 'str'>, '/tmp/tmplpylEJ') # 傳回暫存目錄的絕對路徑。跟 `NamedTemporaryFile` 一樣，是 'prefix' 加上 6 個亂數字元，再串上 'suffix'。
```

參考資料：

  - [10\.6\. tempfile — Generate temporary files and directories — Python 2\.7\.16 documentation](https://docs.python.org/2/library/tempfile.html) #ril

      - This module generates temporary files and directories. It works on all supported platforms.

      - In version 2.3 of Python, this module was overhauled for ENHANCED SECURITY. It now provides three new functions, `NamedTemporaryFile()`, `mkstemp()`, and `mkdtemp()`, which should eliminate all remaining need to use the INSECURE `mktemp()` function.

        Temporary file names created by this module NO LONGER CONTAIN THE PROCESS ID; instead a string of SIX RANDOM CHARACTERS is used.

      - Also, all the user-callable functions now take additional arguments which allow direct control over the location and name of temporary files. It is no longer necessary to use the global `tempdir` and `template` variables. To maintain backward compatibility, the argument order is somewhat odd; it is recommended to USE KEYWORD ARGUMENTS FOR CLARITY.

  - [tempfile — Generate temporary files and directories — Python 3\.7\.3 documentation](https://docs.python.org/3/library/tempfile.html) #ril

      - `TemporaryFile`, `NamedTemporaryFile`, `TemporaryDirectory`, and `SpooledTemporaryFile` are high-level interfaces which provide AUTOMATIC CLEANUP and can be used as CONTEXT MANAGERS. `mkstemp()` and `mkdtemp()` are lower-level functions which require MANUAL CLEANUP.

        這裡 context manager 跟 automatic cleanup 都是 Python 3 才有的。

      - Files names used by this module include a string of random characters which allows those files to be SECURELY CREATED IN SHARED TEMPORARY DIRECTORIES.

        為什麼檔名加進了一些亂數就變安全了? --> 用 `mkstemp()` 產生的檔案只有執行身份的 user ID 可以讀寫。

    `tempfile.mkstemp(suffix=None, prefix=None, dir=None, text=False)`

      - Creates a temporary file in the MOST SECURE manner possible. There are no race conditions in the file’s creation, assuming that the platform properly implements the `os.O_EXCL` flag for `os.open()`.

        The file is READABLE AND WRITABLE ONLY BY THE CREATING USER ID. If the platform uses permission bits to indicate whether a file is executable, the file is executable by no one. The file descriptor is not inherited by child processes.

        用起來像這樣：

            $ python
            >>> import tempfile
            >>> tempfile.mkstemp()
            (3, '/var/folders/1c/_3jhpxmn0cz8mbsf068f116h0000gp/T/tmpALF3BQ')

            $ ls -l /var/folders/1c/_3jhpxmn0cz8mbsf068f116h0000gp/T/tmpALF3BQ
            -rw-------  1 jeremykao  staff  0 May 16 07:27 /var/folders/1c/_3jhpxmn0cz8mbsf068f116h0000gp/T/tmpALF3BQ

      - Unlike `TemporaryFile()`, the user of `mkstemp()` is responsible for deleting the temporary file when done with it.
      - If `suffix` is not `None`, the file name will end with that suffix, otherwise there will be no suffix. `mkstemp()` does NOT PUT A DOT between the file name and the suffix; if you need one, put it at the beginning of `suffix`.
      - If `prefix` is not `None`, the file name will begin with that prefix; otherwise, a default prefix is used. The default is the return value of `gettempprefix()` or `gettempprefixb()`, as appropriate.

      - If `dir` is not `None`, the file will be created in that directory; otherwise, a default directory is used. The default directory is chosen from a platform-dependent list, but the user of the application can control the directory location by setting the `TMPDIR`, `TEMP` or `TMP` environment variables.

        There is thus no guarantee that the generated filename will have any nice properties, such as not requiring quoting when passed to external commands via `os.popen()`.

      - If any of `suffix`, `prefix`, and `dir` are not `None`, they must be the SAME TYPE. If they are bytes, the returned name will be `bytes` instead of `str`. If you want to force a `bytes` return value with otherwise default behavior, pass `suffix=b''`.

        Changed in version 3.5: `suffix`, `prefix`, and `dir` may now be supplied in `bytes` in order to obtain a `bytes` return value. Prior to this, only `str` was allowed. `suffix` and `prefix` now accept and default to `None` to cause an appropriate default value to be used.

      - If `text` is specified, it indicates whether to open the file in BINARY MODE (THE DEFAULT) or text mode. On some platforms, this makes no difference.

        `mkstemp()` returns a TUPLE containing an OS-LEVEL HANDLE to an open file (as would be returned by `os.open()`) and the absolute pathname of that file, in that order.

            $ python
            >>> import tempfile
            >>> tempfile.mkstemp()
            (3, '/var/folders/1c/_3jhpxmn0cz8mbsf068f116h0000gp/T/tmpALF3BQ')

        只是 file descriptor 要怎麼用? 可以直接用 `os.read()` 跟 `os.write()` 讀寫，要轉成 file object 的話則用 `os.fdopen()` 搭配 mode `wb` 或 `wt`，依 `text=False` (default) 或 `text=True` 而定。

## 如何操作檔案?

  - 5.9. File Objects - 5. Built-in Types — Python 2.7.14 documentation https://docs.python.org/2/library/stdtypes.html#bltin-file-objects #ril

## open() 的 `r+` mode 是什麼?

```
with open('file', 'w') as f:
    f.write('content')

with open('file', 'r') as f:
    assert f.read() == 'content'

with open('file', 'r+') as f:
    assert f.read(3) == 'con'
    f.write('-modified')
    assert f.read() == '' # 因為 write() 的關係，file pointer 已移到檔尾

# 雖然只讀了 3 個字元，但寫入還是從檔尾
with open('file', 'r') as f:
    assert f.read() == 'content-modified'
```

參考資料：

  - 7. Input and Output — Python 2.7.14 documentation https://docs.python.org/2/tutorial/inputoutput.html#reading-and-writing-files 提到 `r+` 是 "opens the file for both reading and writing"。
  - 2. Built-in Functions — Python 2.7.14 documentation https://docs.python.org/2/library/functions.html#open 不只有 `r+`，還有 `w+` 跟 `a+`，都是 "for updating"，但好像只有 `r+` 比較有意義? 因為 `w+` 一開始就會把檔案清空，而 `a+` 一開始讀取的位置就指向檔尾 ...
  - Python Files I/O https://www.tutorialspoint.com/python/python_files_io.htm 提到 file pointer 的概念，而 `r+` 一開始的 file pointer 在檔頭。

## 如何刪除檔案或目錄?

  - python - How to delete a file or folder? - Stack Overflow https://stackoverflow.com/questions/6996603/ 分別用 `os.remove()`、`os.rmdir()` (資料夾要是空的)，而 `shutil.rmtree()` 可以刪除有內容的資料夾。
  - `/Users/jeremykao/dropbox/Public/site/note//files/python/listing.asciidoc`

## 如何判斷檔案是不是資料夾??

  - [how to check if a file is a directory or regular file in python? \- Stack Overflow](https://stackoverflow.com/questions/3204782/) 用 `os.path.isdir(path)` 與 `os.path.isdir(path)` 檢查 #ril

## 如何建立資料夾??

  - [15\.1\. os — Miscellaneous operating system interfaces — Python 2\.7\.14 documentation](https://docs.python.org/2/library/os.html#os.mkdir) `os.mkdir(path)` 與 `os.makedirs(path)`，後者會自動建立中間的資料夾 #ril

## 如何找出檔名符合某個 pattern 的檔案?

  - 10.7. glob — Unix style pathname pattern expansion — Python 2.7.14rc1 documentation https://docs.python.org/2/library/glob.html #ril

## 如何檢查檔案是否存在?

  - 用 `os.path.exists(path)` 檢查 `path` 所表示的檔案/目錄是否存在，例如 `os.path.exists('filename')`
  - 若 `path` 指向一個 symblic，要目標檔案/目錄也存在才會傳回 `True`。

參考資料：

  - 10.1. os.path — Common pathname manipulations — Python 2.7.14 documentation https://docs.python.org/2/library/os.path.html#os.path.exists `os.path.exists(path)` 當 "refers to an existing path" 時傳回 `True`，若是 "broken symbolic link" 則傳回 `False`；測試當 `path` 指向一個 symbolic link 時，要目標檔案/目錄也存在才會回傳 `True`。

## 如何一次讀寫檔案內容?

```
with open('filename', 'r') as f:
    content = f.read()
```

  - [Reading and Writing Files - 7\. Input and Output — Python 2\.7\.14 documentation](https://docs.python.org/2/tutorial/inputoutput.html#reading-and-writing-files) `f.read([size])` 沒有提供 `size` 時，會把檔案的內容整個讀出來；自己要注意檔案可能太大的問題。
