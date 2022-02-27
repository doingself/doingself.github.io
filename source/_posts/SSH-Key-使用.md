
# SSH Key

Secure Shell (SSH) 是一个允许两台电脑之间通过安全的连接进行数据交换的网络协议。 通过加密保证了数据的保密性和完整性。

对称加密只需要一个密钥，非对称加密需要两个密钥成对使用，分为公钥（public key）和私钥（private key）
如果使用私钥加密（这个过程一般称为“签名”），只有使用对应的公钥解密。

SSH 密钥登录采用的是非对称加密，每个用户通过自己的密钥登录


## SSH服务端和客户端程序

OpenSSH (OpenBSD Secure Shell) 是一套使用ssh协议，通过计算机网络，提供加密通讯会话的计算机程序。

如果需要作为ssh的服务端，则需要安装openssh。

如果仅是作为ssh客户端，直接使用ssh命令即可。

## 生成密钥
 
 默认生成在  `/c/Users/Administrator/.ssh/id_dsa`, `id_dsa` 是私钥, `id_dsa.pub` 是公钥

 `ssh-keygen -t rsa -C "这里换成你的邮箱@163.com"`
 `-t` 参数用来指定密钥的加密算法，一般会选择 DSA 算法或 RSA 算法。 如果省略该参数，默认使用 RSA 算法。
 `-C` 参数可以为密钥文件指定新的注释，格式为username@host。
 `-b` 参数指定密钥的二进制位数。这个参数值越大，密钥就越不容易破解，但是加密解密的计算开销也会加大。 一般来说，-b至少应该是1024，更安全一些可以设为2048或者更高。


```
Administrator@SKY-20211128AGK MINGW64 ~
$ cd ~

Administrator@SKY-20211128AGK MINGW64 ~
$ pwd
/c/Users/Administrator

Administrator@SKY-20211128AGK MINGW64 ~
$ ssh-keygen -t rsa -C doingself@163.com
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/Administrator/.ssh/id_rsa
Your public key has been saved in /c/Users/Administrator/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:K5/DNboqsok0mSEHteatAmgDot+6DXkz5uQJlUJw+kc doingself@163.com
The key's randomart image is:
+---[RSA 3072]----+
|. o              |
| = .             |
|= + E            |
|=* o .           |
|=+= =   S        |
|+o=B     .o      |
|.=* B ...o .     |
|.oo%.+ o+.       |
|. =+=...+o       |
+----[SHA256]-----+

Administrator@SKY-20211128AGK MINGW64 ~
$
```

查看电脑的所有公钥

```
Administrator@SKY-20211128AGK MINGW64 ~
$ ls -l ~/.ssh/id_*.pub
-rw-r--r-- 1 Administrator 197121 607 Feb 27 20:33 /c/Users/Administrator/.ssh/id_dsa.pub
```

## 使用私钥

ssh-agent 命令让用户在整个 Bash 对话（session）之中，只在第一次使用 SSH 命令时输入密码，然后将私钥保存在内存中，后面都不需要再输入私钥的密码了。

1. eval \`ssh-agent\`: 当前对话启用ssh-agent 
2. ssh-agent: 查看环境
3. ssh-add id_rsa: 添加私钥
4. ssh-add -l: 查看所有已经添加的私钥
5. ssh-add -d name-of-key-file: 从内存中删除指定的私钥

```
Administrator@SKY-20211128AGK MINGW64 ~
$ eval `ssh-agent`
Agent pid 2848

Administrator@SKY-20211128AGK MINGW64 ~
$ ssh-agent
SSH_AUTH_SOCK=/tmp/ssh-TgBvWGD1C8rS/agent.2852; export SSH_AUTH_SOCK;
SSH_AGENT_PID=2853; export SSH_AGENT_PID;
echo Agent pid 2853;

Administrator@SKY-20211128AGK MINGW64 ~
$ ssh-add ~/.ssh/id_rsa
Enter passphrase for /c/Users/Administrator/.ssh/id_rsa:
Identity added: /c/Users/Administrator/.ssh/id_rsa (doingself@163.com)

Administrator@SKY-20211128AGK MINGW64 ~
$ ssh-add -l
3072 SHA256:K5/DNboqsok0mSEHteatAmgDot+6DXkz5uQJlUJw+kc doingself@163.com (RSA)

Administrator@SKY-20211128AGK MINGW64 ~
$
```

## 使用公钥

1. 打开 [Github SSH and GPG Keys](https://github.com/settings/ssh)
2. `Title` 自定义
3. 完整复制公钥内容 粘贴到 `Key` 中


## 测试

`ssh -T git@github.com`

```
Administrator@SKY-20211128AGK MINGW64 ~
$ ssh -T git@github.com
The authenticity of host 'github.com (20.205.243.166)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? y
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Hi doingself! You've successfully authenticated, but GitHub does not provide shell access.

Administrator@SKY-20211128AGK MINGW64 ~
$
```

# 鸣谢

- https://wangdoc.com/ssh/key.html
