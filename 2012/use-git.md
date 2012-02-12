---

title: 在Debian上使用git  
date: 2012-02-12  
category: Code

---

**安装git**   
    
    aptitude install git

**生成一对公钥密钥**  

    ssh-keygen -t rsa

**在git中配置个人信息**  

	git config --global user.name Name
	git config --global user.email mail@example.com