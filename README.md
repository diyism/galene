    grok 对galene的强烈推荐:
    特点：极致轻量高性能 SFU，资源占用极低（0.25 核心可支撑约 100 人讲座模式）。
    优势：单二进制文件，部署极简单；延迟低、CPU 占用小；原生支持音视频、屏幕共享、文字聊天。

    放到arm64的路由器或树莓派或rock64之类的单板机上作为服务器运行非常合适:
    在一台有 Go 的 x86_64 电脑（或云服务器）上交叉编译:
    $ git clone https://github.com/jech/galene.git
    $ cd galene
    $ git tag | tail -10
    $ git checkout v1.1
    $ go clean -cache
    $ sed -i -E '/^func value\(/!{s/([^a-zA-Z0-9_])value\(([^)]*)\)/\1(value)(\2)/g}' diskwriter/diskwriter.go
    $ CGO_ENABLED=0 GOOS=linux GOARCH=arm64 go build -ldflags='-s -w' -o galene-arm64
    $ scp galene-arm64 user@arm64_ip:/home/user/

    在arm64单板机内:
    $ sudo install galene-arm64 /usr/bin/galene
    $ mkdir groups
    $ echo '{"public": true, "users": {"user1":{"password":"user1", "permissions":"op"}, "user2":{"password":"user2", "permissions":"op"}}}' > groups/night-watch.json
    $ galene &
    Now point your browser at <https:/localhost:8443/group/night-watch/>, ignore the unknown certificate warning, and log in with username gal and password gal. 


# The Galene videoconferencing system

Galene is a fully-featured videoconferencing system that is easy to deploy
and requires very moderate server resources.  It is described at
<https://galene.org>.

## Quick start

```sh
git clone https://github.com/jech/galene
cd galene
CGO_ENABLED=0 go build -ldflags='-s -w'
mkdir groups
echo '{"users": {"vimes": {"password":"sybil", "permissions":"op"}}}' > groups/night-watch.json
./galene &
```

Point your browser at <https://localhost:8443/group/night-watch/>, ignore
the unknown certificate warning, and log in with username *vimes* and
password *sybil*.

For full installation instructions, please see the file [galene-install.md][1]
in this directory.

## Documentation

  * [galene-install.md][1]: full installation instructions
  * [galene.md][2]: usage and administration;
  * [galene-client.md][3]: writing clients;
  * [galene-protocol.md][4]: the client protocol;
  * [galene-api.md][5]: Galene's administrative API.
  
## Contributing

In order to contribute to Galene, you may:

  * send patches to [the Galene mailing list][6] by sending mail to
    <galene@lists.galene.org>;
  * submit pull requests on GitHub.

For general discussion, please use the [Galene mailing list][6] (feel free
to send mail without subscribing).  Please do not use Github for general
discussion.

## Further information

Galène's web page is at <https://galene.org>.

Answers to common questions and issues are at <https://galene.org/faq.html>.


-- Juliusz Chroboczek <https://www.irif.fr/~jch/>

[1]: <galene-install.md>
[2]: <galene.md>
[3]: <galene-client.md>
[4]: <galene-protocol.md>
[5]: <galene-api.md>
[6]: <https://lists.galene.org/>
