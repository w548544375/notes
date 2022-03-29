# **Windows Docker Desktop数据迁移指南**
---
> docker desktop 在安装之后会在wsl系统中生成2个子系统,如下所示
```powershell
❯ wsl --list --all
适用于 Linux 的 Windows 子系统分发版:
docker-desktop
docker-desktop-data
```
---

## 迁移步骤
1. 关闭docker,在运行的docker图标上右键，选择 _`Quit Docker Desktop`_
2. 关闭正在运行的wsl子系统
```powershell
wsl --shutdown
```
3. 导出`docker-desktop-data`的虚拟硬盘
> docker-desktop-data的数据会存放在 `C:\Users\当前用户\AppData\Local\Docker\wsl\data\ext4.vhdx`
```powershell
wsl --export docker-desktop-data D:\WslDatas\whatever.tar #  D:\WslDatas\whatever改为导出文件的存储文件名
```
> 注意一定要写文件后缀名
4. 注销 `docker-desktop-data` 子系统
```powershell
wsl --unregister docker-desktop-data
```
5. 导入刚才导出的tar文件到wsl系统中
```powershell
wsl --import docker-desktop-data D:\datadir D:\WslDatas\whatever.tar --version 2 #D:\datadir 表示创建的新的子系统的虚拟硬盘的存放位置  D:\WslDatas\whatever.tar 为上面我们导出的tar的文件的位置 --version 2 代表使用wsl2 创建子系统
```
6. 启动Docker Desktop

### 至此数据迁移完成