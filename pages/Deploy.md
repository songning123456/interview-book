#### Linux常用命令
| 命令 | 解释 | 
| :----- | :----- | 
| man rm(rm --help) | 查看帮助 | 
| cd | 进入目录 | 
| ps -ef&#124;grep java | 查看进程 | 
| pstree&#124;grep java | 查看进程树 | 
| kill somePid | 杀掉某进程 | 
| kill -9 $(ps -ef&#124;grep udpserver &#124; grep java&#124;awk '{print $2}' ) | 删除 udpserver 进程 | 
| rpm -aq&#124;grep php | 查看安装介质 | 
| pwd | 查看当前目录 | 
| ls -l -t | -l 显示详情，-t 按时间排序 | 
| ll | 相当于 ls -l | 
| find / -name libNativeMethod.so | 等同 ll &#124; grep someFile | 
| grep someText * | 在当前目录所有文本中查找 | 
| ifconfig | IP地址配置，可以使用setup命令启动字符界面来配置 | 
| chmod a+x someFile | 所有用户都可以执行 | 
| chmod u+x someFile | 当前用户可以执行 | 
| env | 环境配置，相当window下set | 
| env &#124;grep PATH | 查看环境变量 | 
| export | 相当于set classpath | 
| echo | 输出变量名 | 
| netstat -npl | 查看端口 | 
| lsof -i :22 | 查看端口进程 | 
| cp from to | 拷贝文件 | 
| cp -fr ./j2sdk1.4.2_04 /usr/java | 拷贝目录 | 
| mkdir | 创建目录 | 
| mv | 剪切或者重命名 | 
| rm -r | 递归删除， -f表示force | 
| >someFile | 清空文件内容 | 
| which java | 查看java进程对应的目录 | 
| who | 显示当前用户 | 
| users | 显示当前会话 | 
| zip -r filename.zip filesDir | 某个文件夹打zip包 | 
| unzip someFile.zip | 解压zip文档到当前目录 | 
| gunzip someFile.cpio.gz | 解压 .gz | 
| cpio -idmv < someFile.cpio | CPIO操作 | 
| ps auxwww&#124;sort -n -r -k 5&#124;head -5 | 按资源占用情况来排序，第一个5表示第几列，第二个5表示前几位 | 
| hostname -i | 显示本机机器名，添加i，显示etc/hosts对应ip地址 | 
| rpm -ivh some.rpm | 安装软件 | 
| rpm -Uvh some.rpm | 更新软件 | 
| rpm -qa &#124; grep someSoftName | 是否已安装某软件 | 
| tar -xvzf some.tar.gz | 解压缩包 | 
| tar –cvzf some.tar.gz fileDir | 打压缩包 | 
| shutdown -i6 -y 0 | 立即重启服务器 | 
| reboot | 立即重启服务器，相当于shutdow -r now | 
| halt | 立即关机，shutdown -h | 
| shutdown -r 23:30<br> shutdown -r +15<br> shutdown -r +30 | 定时重启 | 
| gdmsetup | 启动系统配置管理界面，需要在图形界面执行 | 
| setup | 启动文字配置管理界面 | 
| vi /etc/sysconfig/network | 修改机器名，然后要重启机器或者service network restart | 
| locale | 显示系统语言 | 
| export LANG=zh_CN.GBK | 设定系统语言，解决console中文乱码 | 
| ln -s src_full_file the_link_name | 创建软链接 | 
| last | 倒序查看已登陆用户历史 | 
| history | 查看历史命令 | 
| tail -10 someFile | 查看文件后10行内容 | 
| head -10 someFile | 查看文件前10行内容 | 
| tail -f someFile | 实时查看文件内容，用于调试 | 
| date -s 10/09/2009 | 修改日期 | 
| date -s 13:24:00 | 修改时间，直接date显示时间 | 
| df -k | 查看文件磁盘空间 | 
| df -v | 查看文件空间 | 
| du | 查看磁盘空间使用情况 | 
| free | 查看内存使用情况 | 
| top | 查看当前系统资源使用情况 | 
| vmstat 5 10 | 每5秒刷新一次，刷新10次；time、timex、uptime、iostat、sar | 
| cat /proc/cpuinfo&#124;grep processor&#124;wc –l | 获取CPU个数 | 
| service mysql start | 启动mysql服务 | 
| service mysql stop | 停止mysql服务 | 
| service mysql status | 显示mysql服务状态 | 
| service --status-all | 查看已有服务 | 


#### 如何查看内存使用情况？
![vmstat](/images/Deploy/vmstat.PNG)


<table>
    <tr>
        <td rowspan="2">procs</td>
        <td>r</td>
        <td>列表示运行和等待cpu时间片的进程数，如果长期大于1，说明cpu不足，需要增加cpu</td>
    </tr>
    <tr>
        <td>b</td>
        <td>列表示在等待资源的进程数，比如正在等待I/O或者内存交换等</td>
    </tr>
    <tr>
        <td rowspan="4">memory</td>
        <td>swpd</td>
        <td>切换到内存交换区的内存数量(k表示)。如果swpd的值不为0，或者比较大，比如超过了100m，只要si、so的值长期为0，系统性能还是正常</td>
    </tr>
    <tr>
        <td>free</td>
        <td>当前的空闲页面列表中内存数量(k表示)</td>
    </tr>
    <tr>
        <td>buff</td>
        <td>作为buffer cache的内存数量，一般对块设备的读写才需要缓冲</td>
    </tr>
    <tr>
        <td>cache</td>
        <td>作为page cache的内存数量，一般作为文件系统的cache，如果cache较大，说明用到cache的文件较多，如果此时IO中bi比较小，说明文件系统效率比较好</td>
    </tr>
    <tr>
        <td rowspan="2">swap</td>
        <td>si</td>
        <td>由内存进入内存交换区数量</td>
    </tr>
    <tr>      
        <td>so</td>
        <td>由内存交换区进入内存数量</td>
    </tr>
    <tr>
        <td rowspan="2">io</td>
        <td>bi</td>
        <td>从块设备读入数据的总量(读磁盘)(每秒kb)</td>
    </tr>
    <tr>      
        <td>bo</td>
        <td>块设备写入数据的总量(写磁盘)(每秒kb)</td>
    </tr>
    <tr>
        <td rowspan="2">system</td>
        <td>in</td>
        <td>列表示在某一时间间隔中观测到的每秒设备中断数</td>
    </tr>
    <tr>      
        <td>cs</td>
        <td>列表示每秒产生的上下文切换次数，如当cs比磁盘I/O和网络信息包速率高得多，都应进行进一步调查</td>
    </tr>    
    <tr>
        <td rowspan="4">cpu</td>
        <td>us</td>
        <td>列显示了用户方式下所花费 CPU 时间的百分比。us的值比较高时，说明用户进程消耗的cpu时间多，但是如果长期大于50%，需要考虑优化用户的程序</td>
    </tr>
    <tr>      
        <td>sy</td>
        <td>列显示了内核进程所花费的cpu时间的百分比。这里us+sy的参考值为80%，如果us+sy大于80%说明可能存在CPU不足</td>
    </tr>
    <tr>      
        <td>id</td>
        <td>列显示了cpu处在空闲状态的时间百分比</td>
    </tr>
    <tr>      
        <td>wa</td>
        <td>列显示了IO等待所占用的CPU时间的百分比。这里wa的参考值为30%，如果wa超过30%，说明IO等待严重，这可能是磁盘大量随机访问造成的，也可能磁盘或者磁盘访问控制器的带宽瓶颈造成的(主要是块操作)</td>
    </tr>
</table>


