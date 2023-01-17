# gomodule

## gomodule的相关命令

### 查看某一个module所依赖的其他module
```shell
$ go list -m -json all
```
会输出类似以下的内容：
```json
{
    "Path": "hello",
    "Main": true,
    "Dir": "chapter10/sources/go-module/hello",
    "GoMod": "chapters/chapter10/sources/go-module/hello/go.mod",
    "GoVersion": "1.14"
}
{
    "Path": "bitbucket.org/bigwhite/c",
    "Version": "v0.0.0-20180714063616-861b08fcd24b",
    "Time": "2018-07-14T06:36:16Z",
    "Dir": "/Users/tonybai/Go/pkg/mod/bitbucket.org/bigwhite/c@v0.0.0-20180714063616-861b08fcd24b"
    "GoMod": "/Users/tonybai/Go/pkg/mod/cache/download/bitbucket.org/bigwhite/c/@v/vv0.0.0-20180714063616-861b08fcd24b.mod"
}
{
    "Path": "bitbucket.org/bigwhite/d",
    "Version": "v0.0.0-20180714005150-3e3f9af80a02",
    "Time": "2018-07-14T00:51:50Z",
    "Indirect": true,
    "Dir": "/Users/tonybai/Go/pkg/mod/bitbucket.org/bigwhite/d@v0.0.0-20180714005150-3e3f9af80a02",
    "GoMod": "/Users/tonybai/Go/pkg/mod/cache/download/bitbucket.org/bigwhite/d/@v/v0.0.0-20180714005150-3e3f9af80a02.mod"
}
```

用下面命令输出简要信息
```shell
$ go list -m all
hello
bitbucket.org/bigwhite/c v0.0.0-20180714063616-861b08fcd24b
bitbucket.org/bigwhite/d v0.0.0-20180714005150-3e3f9af80a02
```

