## Nginx的相关命令和配置

##### win下cmd的常用命令

  启动服务：start nginx
  
  停止服务：nginx -s stop
  
  重新加载：nginx -s reload(配置文件被修改后需要执行它)
  
  查看nginx进程：tasklist /fi "imagename eq nginx.exe"
  
  更详细的命令解释，查看官网：http://nginx.org/en/docs/windows.html，在 win10 自带 powerShell 下，命令格式同 gitBash 下一样。
  
##### 如果安装 gitBash，命令格式会有差异
  
  进入到 nginx 目录：
  
  start ./nginx.exe       启动
  
  ./nginx.exe -s stop     停止
  
  ./nginx.exe -s reload   重启
  
  修改 config 配置文件后，最好重新启动一下 nginx 服务，在重启之前，怎么判断 nginx 配置写得对不对呢？
  
  ./nginx.exe -t          测试配置是否正确
  
##### 判断 ua 指向不同的目录  

    listen       80;
    server_name  dfg.test;

    location / {
      root D:/work/dfg/dfg_website/dist;

      if ($http_user_agent ~* "(iPhone|iPod|Opera Mini|Android.*Mobile|NetFront|PSP|BlackBerry|Windows Phone)") {
        root D:/work/dfg/dfg_website_m/dist;
      }

      index  index.html index.htm;
    }

  
## Nginx启动错误排查

  找到logs文件夹里的 error.log 文件，查看报错原因，查询解决方案 -------> 报错原因是有错误占用 443端口
  
  在dos命令窗口输入 netstat -ano 即可查看端口使用情况，如果要查看指定端口是否被占用可以使用命令 netstat -ano|findstr 端口号
  
  例如要查看8080端口号是否已经被占用就使用命令netstat -ano|findstr 8080
    如果结果为空则说明没有被使用；
    如果有值则说明已经被使用，最后一列为使用8080端口号的进程ID

  使用taskkill /PID PID 命令杀掉进程，释放占用的端口即可
  
  ★注意：在安装虚拟机的时候，尤其要注意排查是不是虚拟机占用了端口
