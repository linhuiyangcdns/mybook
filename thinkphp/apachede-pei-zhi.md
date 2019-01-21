```
<VirtualHost *:80>
        DocunmentRoot "D:\Server\Apache\htdocs\api"
        ServerName api.com
        <Directory "D:\Server\Apache\htdocs\api">
                options All
                Allowoverride All
                directoryIndex index.html index.php 
        </Directory>
</VirtualHost>
```

注意apache的host文件中的Vitual host include 



## 2

c:\windows\system32\drivers\etc\hosts 

增加ip对应的域名

