# docker install
1、检查内核版本是否是3.10？centos是否是7.0以上？

&emsp;&emsp; a、uname -a

&emsp;&emsp; b、lsb_release -a

2、检查库的配置是否开启？

  &emsp;&emsp;若未开启，通过网址https://wiki.centos.org/AdditionalResources/Repositories实现开启

3、选择安装docker的方法：使用设置repository库 - 安装docker

  &emsp;&emsp;a、安装yum-utils包，提供使用yum-config-manager程序
  
      sudo yum install -y yum-utils
      
  &emsp;&emsp;b、加入docker的库
  
      sudo yum-config-manager —add-repo https://download.docker.com/linux/centos/docker-ce.repo
      
  &emsp;&emsp;c、安装docker引擎
  
    sudo yum install -y docker-ce docker-ce-cli containerd.io
    
  &emsp;&emsp;d、启动docker
  
    sudo systemctl start docker
    
    
&emsp;&emsp;问题：启动不了，并提示Dependency failed for Docker Application Container Engine. 

    
                                           错误的图片
                                           

    分析：从错误的图片中可以看到是containerd启动出错了，那么从containerd中查找问题
    
    抓住关键词：未定义标识seccomp_api_set，百度下好像和库libseccomp有关
    

    抱着试试看的态度，安装了libseccomp库，再启动就ok了(*^▽^*)
    
    用ldd -r /usr/bin/containerd 命令查看了containerd依赖的库，发现也存在libseccomp的软连接
    
        ldd命令是显示可执行模块的dependency
        

4、创建docker组，用于管理docker是非root用户

  &emsp;&emsp;a、sudo groupadd docker
  
  &emsp;&emsp;b、sudo usermod -aG docker $USER # 追加可运行docker的用户
  
  &emsp;&emsp;c、newgrp docker # 更新权限
  
  &emsp;&emsp;d、docker run hello-world：验证没有sudo是否可以执行docker命令
  

  5、实现开机自启docker
  
    sudo systemctl enable docker
    
    或者：sudo chkconfig docker on
































