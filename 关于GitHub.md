## 首次SSH密钥设置
在fish中，`eval (ssh-agent)`由于变量的设置方式会报错。为了解决这个问题，请使用csh风格的选项`-c`：
```bash
eval (ssh-agent -c)
```

## 生成SSH密钥对
要创建一套新的SSH密钥对，请在终端中使用下面这条命令：
```bash
ssh-keygen -t rsa -b 4096 -C "your@email.com"
```

## 将你的密钥添加到SSH代理
创建密钥后，需要把它添加到SSH代理中，这样Git才能使用它：
```bash
ssh-add ~/.ssh/id_rsa
```

## 复制你的公钥
在Linux上: 
```bash
cat ~/.ssh/id_rsa.pub
```

## 列出SSH密钥
查看你在SSH代理中加载了哪些密钥：
```bash
ssh-add -l
```

## 将SSH添加到GitHub
现在，你已经生成了SSH密钥，需要把你的**公钥**添加到你的GitHub账户中。

## 测试你的SSH连接
首先，测试对GitHub的SSH连接是否正常：
```shell
ssh -T git@github.com
The authenticity of host 'github.com (140.82.121.3)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,140.82.121.3' (RSA) to the list of known hosts.
Hi your-username! You've successfully authenticated, but GitHub does not provide shell access.
```
如果最后一行包含了你在GitHub上的用户名，那么你就已经成功完成身份验证！