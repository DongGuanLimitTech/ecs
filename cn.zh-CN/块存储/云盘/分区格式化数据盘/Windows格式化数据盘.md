# Windows格式化数据盘 {#concept_a3f_mg2_wdb .task}

本文介绍如何为Windows实例的全新数据盘创建一个MBR单分区并挂载NTFS文件系统。您也可以根据业务需要，对数据盘进行多分区配置。

格式化数据盘可能存在以下风险：

-   数据盘分区和格式化是高风险行为，请慎重操作。本文介绍如何处理一块全新的数据盘，如果您的数据盘上有数据，请务必为数据盘创建快照，避免数据丢失。具体操作，请参见[创建快照](../../../../cn.zh-CN/快照/使用快照/创建快照.md#)。
-   云服务器ECS仅支持**数据盘**分区操作，不支持**系统盘**分区操作。如果您强行使用第三方工具对系统盘做分区操作，可能引发系统崩溃和数据丢失等未知风险。仅允许在扩容系统盘后做扩展分区或新增分区操作，具体操作，请参见[扩展分区和文件系统\_Windows](../../../../cn.zh-CN/块存储/云盘/扩容云盘/扩展分区和文件系统_Windows.md#)。

## 操作步骤 {#section_4q2_r2q_bvk .section}

本操作步骤仅适用于不大于2TiB的数据盘，大于2TiB的数据盘，请参见[分区格式化大于2 TiB数据盘](../../../../cn.zh-CN/块存储/云盘/分区格式化数据盘/分区格式化大于2 TiB数据盘.md#)。示例步骤以Windows Server 2012 R2 64位操作系统为例，ECS实例挂载的是20GiB的数据盘，分区并格式化了一个MBR单分区。

1.  [远程连接实例](cn.zh-CN/个人版快速入门/连接ECS实例.md#)。
2.  在Windows Server桌面，右键单击**开始**图标，选择**磁盘管理**。 

    ![磁盘管理](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/9605/15664380195089_zh-CN.png)

3.  查找到未格式化分区的数据盘（如磁盘 2），其处于**脱机**状态。 

    ![脱机](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/9605/15664380195090_zh-CN.png)

4.  右键单击磁盘 2周边的空白区，在弹出的菜单中，选择**联机**。 

    ![联机](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/9605/15664380205091_zh-CN.png)

    联机后，磁盘 2的状态显示为**没有初始化**。

5.  右键单击磁盘 2周边的空白区，在弹出菜单中，选择**初始化磁盘**。 

    ![初始化磁盘](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/9605/15664380205092_zh-CN.png)

6.  在初始化磁盘对话框里，选择**磁盘2**，并选择磁盘分区形式： 

    -   MBR目前仍是最常用的分区形式，但是，MBR只支持处理不大于2 TiB的数据盘，而且，只支持分4个主区，如果您要将磁盘分成更多的区，需要将某个主区作为扩展区并在其中创建逻辑分区。
    -   GPT是一种新的分区形式，早期版本的Windows不能识别这种分区形式。GPT能处理的数据盘容量由操作系统和文件系统决定。在Windows操作系统里，GPT最多可以支持128个主分区。
    在本示例中，我们选择MBR分区形式，并单击**确定**。

    ![确定](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/9605/15664380205093_zh-CN.png)

7.  在磁盘管理对话框，右键单击磁盘 2的**未分配**区域，选择**新建简单卷**。 

    ![新建简单卷](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/9605/15664380205094_zh-CN.png)

8.  在新建简单卷向导对话框中，完成以下操作： 
    1.  单击**下一步**。
    2.  指定卷大小：指定简单卷大小。如果您只要创建一个主区，使用默认值。单击**下一步**。 

        ![下一步](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/9605/15664380215099_zh-CN.png)

    3.  分配驱动器号和路径：选择一个驱动器号（即盘符），如本示例中选择F。单击**下一步**。 

        ![下一步](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/9605/15664380215100_zh-CN.png)

    4.  格式化分区：选择格式化设置，包括文件系统、分配单元大小和卷标，确认是否**执行快速格式化**和**启用文件和文件夹压缩**。这里使用默认设置。单击**下一步**。 

        ![启用文件和文件夹压缩](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/9605/15664380215101_zh-CN.png)

    5.  开始创建新简单卷。当向导对话框里出现以下截图中的信息时，说明已经完成新简单卷的创建。单击**完成**关闭新建简单卷向导对话框。 

        ![新建简单卷向导](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/9605/15664380215102_zh-CN.png)


格式化分区完成后，**磁盘管理**中磁盘 2的状态如下图所示。

![磁盘管理](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/9605/15664380215103_zh-CN.png)

您可以在**这台电脑**中查看到新建的驱动器**新加卷 \(F:\)**。至此，您就可以开始使用这个数据盘了。

**相关文档**  


[RebootInstance](../../../../cn.zh-CN/API参考/实例/RebootInstance.md#)

