## PyModflow-demo 简介

用modflow模拟地下水，溶质运移，溯源。利用GMS预处理不规则边界。

1. `modflow_ipynbook文件夹`中的`modflow最终功能实现-2D转化-溯源-add河流-chd插值-正确.ipynb`这个文件是最完整的文件。
2. `modflow文件夹`内是`modflow的可执行文件`，包括modflow、modpath、mt3dms等。用到的为
   - modflow\mp7
   - modflow\mf6
3. 用到 python 的包为 FloPy 和 h5py，flowpy可以支持如下的[modflow模型](https://github.com/modflowpy/flopy/blob/58cb48e7a6240d2a01039b5a5ab3c67c5111662c/docs/supported_packages.md#flopy-supported-packages)，


当模型遇到无法解决的问题，可以查找源码中的注释，如下为[flopy的源码](https://github.com/modflowpy/flopy/tree/cb8c21907ef35172c792419d59e163a86e6f0939/flopy/mf6/modflow)、[modpath的源码](https://github.com/modflowpy/flopy/tree/a13f8b940c9057d18732d8d49ec25d2b9b2f362e/flopy/modpath)、[modflow 6的官方手册](https://flopy.readthedocs.io/en/latest/_notebooks/tutorial03_mf6_data.html)

如下为[modflow的官方教程](https://github.com/modflowpy/flopy/blob/78e42643ebc8eb5ca3109de5b335e18c3c6e8159/docs/notebook_examples.md)，需用jupyter notebook打开，也可以直接进入这个[目录](https://github.com/modflowpy/flopy/tree/2749f16765b70d842940f4be79c169aa0c03ee74/examples/Notebooks)


1. 如果想要了解非均匀网格，可以参考[这个链接](https://github.com/modflowpy/flopy/blob/78e42643ebc8eb5ca3109de5b335e18c3c6e8159/examples/Notebooks/flopy3_gridgen.ipynb)

2. 如下是modpath比较好的[教程1结构化网格](https://github.com/modflowpy/flopy/blob/78e42643ebc8eb5ca3109de5b335e18c3c6e8159/examples/Notebooks/flopy3_modpath7_structured_example.ipynb)和[教程2非结构网格](https://github.com/modflowpy/flopy/blob/78e42643ebc8eb5ca3109de5b335e18c3c6e8159/examples/Notebooks/flopy3_modpath7_unstructured_example.ipynb)：

3. 如下是[mt3dms垂向的模型](https://github.com/modflowpy/flopy/blob/78e42643ebc8eb5ca3109de5b335e18c3c6e8159/examples/Notebooks/flopy3_MT3DMS_examples.ipynb)教程

4. [将模型结果导出成shp或者tif](https://github.com/modflowpy/flopy/blob/78e42643ebc8eb5ca3109de5b335e18c3c6e8159/examples/Notebooks/flopy3_Modflow_postprocessing_example.ipynb)

## 关于常州土壤和地下水预警系统，可以参考如下项目

[提取站点污染物浓度或者站点排放](https://github.com/AlfreadChen/city_coap)

[万睿智慧园区解决方案](https://market.cloud.tencent.com/products/36456)

[RAMEV C4 智能数据采集通讯终端](https://market.cloud.tencent.com/products/22792)

[物联网卡贴片卡工业物联网卡流量卡](https://market.cloud.tencent.com/products/29414)

[4G网关 WIFI网关 网关模组 家庭网关 智能家居 智能硬件 mesh模组 ZigBee模组](https://market.cloud.tencent.com/products/18251)

[智慧水务管理平台智能水表水质监测系统平台设备安装施工运维落地定制软件开发源码](https://market.cloud.tencent.com/products/24303)

[农业物联网大数据平台](https://market.cloud.tencent.com/products/33844)

[智慧城市数字孪生可视化管理平台](https://market.cloud.tencent.com/products/34588)

[小程序定制开发 -IoT、智能家居、物联网、车联网、智慧城市、医疗健康](https://market.cloud.tencent.com/products/14107)

## 小白零基础入门

需要安装：

1. VScode - 用于编程
   - 需要安装一些插件比如：python相关的各种插件
2. 某VPN - 用于翻墙
3. win10系统 - 在软件商店安装 Debian 虚拟机
   - 也可安装 Ubuntu 或者 CentOS
4. 一个Github账户

## python基础

```py
主要的数据结构包括：

- list 用 [] 表示
- dictionary 用 {} 表示

需要学习一些 numpy、matplotlib 的基础
- np.ones() 表示创建都是1的矩阵
- np.zeros() 表示创建都是0的矩阵
- 等等

import 导入包
def 创建函数
print 打印


"",'' 都表示字符
for 表示循环

range(5) 表示创建一个[0,1,2,3,4]的list

```

## 进行一些基础配置

Debian虚拟机打开：

```sh
sudo apt-get update
sudo apt-get upgrade
```

安装python相关的包

```sh
sudo install python3 pip
pip install flopy 
pip install numpy
pip install h5py
pip install matplotlib
```

赋予权限

```sh
sudo chmod -R 777 /当前目录
```

## 进入jupyterNotebook尝试跑一跑

### 破解h5

```py
import h5py 
import numpy as np
```

输入h5文件所在位置

```py
sim_name_ini = "2022.10.20-FJMFQF"
sim_ws_ini = "2022.10.20-FJMFQF_MODFLOW"
```

读取h5里面的内容，确认一下路径对不对，如果不对，路径需要修改

```py
f1 = h5py.File("/home/nandahgy/yqc_model/" + sim_ws_ini + "/" + sim_name_ini + ".h5",'r')
f2 = h5py.File("/home/nandahgy/yqc_model/" + sim_ws_ini + "/" + sim_name_ini + ".hed.h5",'r')
```

#### 读取f1

读取模型的离散层数

```py
NROW = 213
NCOL = 283
NLAY = 4
```

读取f1里面的idomain的值，代表模型的域，是一个3维的矩阵

```py
nums = [0] * (NROW * NCOL)
idomain = np.ones((NLAY, NROW, NCOL), dtype = int) # 参与模拟的区域，1 为参与
for layi in range(NLAY):
  iboundname = 'ibound' + str(layi + 1)
  nums = list(f1['Arrays'][iboundname])
  tmp = []
  for rowi in range(NROW):
    tmp.append(nums[rowi * NCOL:(1 + rowi) * NCOL])
  idomain[layi,:,:] = np.array(tmp)
```

设置sconc初始浓度，并从f1里面读取strt初始水头

```py
# 初始条件: # Initial conditions
# Starting concentrations 初始浓度
sconc = 0.0
# Starting Heads 初始水头
nums = [0] * (NROW * NCOL)
strt = np.zeros((NLAY, NROW, NCOL), dtype=float)

for layi in range(NLAY):
  StartHeadname = 'StartHead' + str(layi + 1)
  nums = list(f1['Arrays'][StartHeadname])
  tmp = []
  for rowi in range(NROW):
    tmp.append(nums[rowi * NCOL:(1 + rowi) * NCOL])
  strt[layi,:,:] = np.array(tmp)
```

从f1读取top和bot的值，就是顶和底

```py
nums = list(f1['Arrays']['top1'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
top1 = np.array(tmp)

nums = list(f1['Arrays']['bot1'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
bot1 = np.array(tmp)

nums = list(f1['Arrays']['bot2'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
bot2 = np.array(tmp)

nums = list(f1['Arrays']['bot3'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
bot3 = np.array(tmp)

nums = list(f1['Arrays']['bot4'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
bot4 = np.array(tmp)

bottoms = np.zeros((NLAY, NROW, NCOL), dtype=float)
bottoms[0,:,:] = bot1
bottoms[1,:,:] = bot2
bottoms[2,:,:] = bot3
bottoms[3,:,:] = bot4
```

打印出来看看，strt和top、bottom的值是否接近。

如果不接近，说明，可能存在错误，模型无法收敛。

```py
print(strt[0,100,100])
print(strt[1,100,100])
print(strt[2,100,100])
print(strt[3,100,100])
print("top1:",top1[100,100])
print("bot1:",bot1[100,100])
print("bot2:",bot2[100,100])
print("bot3:",bot3[100,100])
print("bot4:",bot4[100,100])

# 5.0
# -2.0
# -18.0
# -35.0
# top1: 5.0
# bot1: -2.0
# bot2: -18.0
# bot3: -35.0
# bot4: -75.06968688964844
```

从f1读取渗透系数，包括：

1. HK1、HK2、HK3、HK4
2. HANI1、HANI2、HANI3、HANI4
3. VANI1、VANI2、VANI3、VANI4

```py
nums = list(f1['Arrays']['HK1'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
HK1 = np.array(tmp)

nums = list(f1['Arrays']['HK2'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
HK2 = np.array(tmp)

nums = list(f1['Arrays']['HK3'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
HK3 = np.array(tmp)

nums = list(f1['Arrays']['HK4'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
HK4 = np.array(tmp)

nums = list(f1['Arrays']['HANI1'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
HANI1 = np.array(tmp)

nums = list(f1['Arrays']['HANI2'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
HANI2 = np.array(tmp)

nums = list(f1['Arrays']['HANI3'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
HANI3 = np.array(tmp)

nums = list(f1['Arrays']['HANI4'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
HANI4 = np.array(tmp)

nums = list(f1['Arrays']['VANI1'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
VANI1 = np.array(tmp)

nums = list(f1['Arrays']['VANI2'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
VANI2 = np.array(tmp)

nums = list(f1['Arrays']['VANI3'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
VANI3 = np.array(tmp)

nums = list(f1['Arrays']['VANI4'])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
VANI4 = np.array(tmp)

HKs = np.zeros((NLAY, NROW, NCOL), dtype=float)
HKs[0,:,:] = HK1
HKs[1,:,:] = HK2
HKs[2,:,:] = HK3
HKs[3,:,:] = HK4
```

查看一下刚刚读取的内容是否正确

```py
print("HK1:",HK1[100,100])
print("HK2:",HK2[100,100])
print("HK3:",HK3[100,100])
print("HK4:",HK4[100,100])
print("HANI 和 VANI 都是比值")
print("HANI1:",HANI1[100,100])
print("HANI2:",HANI2[100,100])
print("HANI3:",HANI3[100,100])
print("HANI4:",HANI4[100,100])
print("VANI1:",VANI1[100,100])
print("VANI2:",VANI2[100,100])
print("VANI3:",VANI3[100,100])
print("VANI4:",VANI4[100,100])
```

从f1读取蒸发和补给

```py
nums = list(f1['Recharge']['07. Property'][0,:,0])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
rch = np.array(tmp)

nums = list(f1['ET']['07. Property'][0,:,0])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
et_surf = np.array(tmp)

nums = list(f1['ET']['07. Property'][1,:,0])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
et_rat = np.array(tmp)

nums = list(f1['ET']['07. Property'][2,:,0])
tmp = []
for i in range(NROW):
  tmp.append(nums[i*NCOL:(1+i)*NCOL])
et_dep = np.array(tmp)

print("rch:",rch[100,100])
print("et_surf:",et_surf[100,100])
print("et_rat:",et_rat[100,100])
print("et_dep:",et_dep[100,100])
```

#### 读取f2里面的head值，作为水头，chd就是head的值

```py
nums = list(f2['Datasets']['Head']['Values'][0])
tmp2 = []
for j in range(NLAY):
      tmp = []
      for i in range(NROW):
            tmp.append(nums[i*NCOL:(1+i)*NCOL])
      tmp2.append(tmp)
chd = np.array(tmp2)
chd.shape
# 输出测试
# chd[0,98,29]
```

看看模型中读取的水头值是否正确，直接用matplotlib绘图

```py
from matplotlib import pyplot as plt
from matplotlib import cm
import numpy.ma as npm

mask = chd < 0
arrmask = npm.array(chd, mask=mask)

plt.imshow(arrmask[3], cmap=cm.gray)
plt.colorbar()
plt.show()
```

### 输入模型基本参数

1. 模型左下角的坐标

```py
xorigin = 498149
yorigin = 3594250
```

2. 输入模型行数、列数

nlay、nrow、ncol在前面读取h5的时候已经读取过

```py
# Discretization
import numpy as np
nper = 1                  # 稳定流，周期数为 1
nlay = NLAY                  # 层数
nrow = NROW                 # 行数
ncol = NCOL                 # 列数
delr = 49.84594               # 单位行长，在out文件内部
delc = 49.82019               # 单位列长，在out文件内部
# delz = 10.0               # 单位层高
Ly = delr * nrow
Lx = delc * ncol
top = top1                 # 顶部高程
botm = bottoms
```

3. 给模型取个名称，比如1027代表10月27日

```py
# Filename
sim_ws = 'simulation_1027_A'
sim_name = 'model'          # 模拟总名称
gwfname = "gwf_" + sim_name # 渗流模型名称
gwtname = "gwt_" + sim_name # 溶质运移模型名称
```

4. 模型的单位

```py
# Units
length_units = "meters"   # 长度单位
time_units = "days"       # 时间单位
```

5. 输入弥散系数和渗透系数

```py
# 设置溶质运移模型专门参数:
# GWT
porosity = 0.2
al = 20.0                    # 纵向弥散系数
trpt = 0.3; ath1 = al * trpt # 横向弥散系数
trpv = 0.3; atv = al * trpv  # 垂直弥散系数

# 设置渗流模型专门参数:

# GWF
icelltype = [0, 0, 0, 0] # 饱和 or 非饱和
k11 = HKs
# k22overk = HANI1[100,100]
# k33overk = VANI1[100,100]
k33 = k11
# icelltype = 0
# k11 = 0.5
```

6. 输入时间离散化参数和求解器参数

```py
# 时间离散化参数:
# Temporal discretization
perlen = 100.0
nstp = 10
tsmult = 1.0
tdis_ds = []
tdis_ds.append((perlen, nstp, tsmult),)
print(tdis_ds)

# 求解器参数：
# Solver parameters
nouter, ninner = 100, 300
hclose, rclose, relax = 1e-6, 1e-6, 1.0
```

7. 在模型中设置一口井well 🗑（也可以不加❌）

```py
# wel_spd 井
qwell = 0     # 流速
cwell = 100.0 # 浓度
wel_loc = (2, 100, 100) # 分别是（层号，行号，列号）
#               (k, i, j),  flow,  conc
wel_spd = {0: [[wel_loc, qwell, cwell]]}
```

9. 在模型中加一个河流 🌊（也可以不加❌）

```py
# RIV
k_rivbott = 1 # 河床底部渗透系数，m/d
thick_rivbott = 1 # 河床沉积物厚度，m
cond = k_rivbott * (delr) * (delc) / (thick_rivbott) # conductance, m2/d
r_bott = -2 # 河底高程, 也可以设为 0
riv_stage = 2 # 河流水位，[1, 5, 2]
riv_sp_0 = [] # 应力周期 0
# riv_sp_1 = [] # 应力周期 1
# riv_sp_2 = [] # 应力周期 2
for i in range(ncol): # 河流水位, conductance, 河底高程
    if idomain[0,100,i] == 1:
        riv_sp_0.append([0, 100, i, riv_stage, cond, r_bott])
        # riv_sp_1.append([0, 14, i, riv_stage[1], cond, r_bott])
        # riv_sp_2.append([0, 14, i, riv_stage[2], cond, r_bott])`
riv_spd = {0: riv_sp_0}
print("河底高程:",r_bott)
print("河流水位:",riv_stage)
print("河床底部渗透系数:",k_rivbott,"m/d")
print("河床沉积物厚度:",thick_rivbott,"m")
print("conductance:",cond,"m2/d")
```

10. 边界水头chd的指定

要非常注意，容易不收敛！！！！啊🎃

以下2个代码块只需要跑一个

一个代表东西2侧指定，一个代表南北2侧指定，不需要2侧都指定

```py
# chd_spd = []
# cnt = 0
# for k in np.arange(nlay): # 南北两侧分别指定
#     for j in np.arange(ncol): # 
#         for i in np.arange(nrow):
#             if idomain[k][i][j] == 1:
#               chd_spd.append([(k, i, j), chd[k, i, j], 100.0]) #(l, r, c),head, conc
#               cnt += 1
#               break
#         for i in np.arange(nrow-1,-1,-1):
#             if idomain[k][i][j] == 1:
#               chd_spd.append([(k, i, j), chd[k, i, j], 100.0]) #(l, r, c),head, conc
#               cnt += 1
#               break
# chd_spd = {0: chd_spd}
# cnt
```

```py
chd_spd = []
list1 = []
list2 = []
cnt = 0
for k in np.arange(1): # 东西两侧分别指定，nlay
    for i in np.arange(nrow):
        for j in np.arange(ncol): # 西
            if idomain[k][i][j] == 1:
              chd_spd.append([(k, i, j), chd[k, i, j], 100.0])  #(l, r, c),head, conc
              cnt += 1
              list1.append(chd[k, i, j]) 
              break
        for j in np.arange(ncol-1,-1,-1): # 东
            if idomain[k][i][j] == 1:
              chd_spd.append([(k, i, j), chd[k, i, j], 100.0])  #(l, r, c),head, conc
              list2.append(chd[k, i, j]) 
              break
chd_spd = {0: chd_spd}
cnt
```

### 创建一个建模的函数

```py
import flopy
def build_model():
    sim = flopy.mf6.MFSimulation(
        sim_name=sim_name, sim_ws=sim_ws, exe_name='/usr/local/lib/modflow/mf6'
    )
    print("sim_name:",sim_name)
    print("sim_ws:",sim_ws)

    # 时间离散化
    flopy.mf6.ModflowTdis(
        sim, nper=nper, perioddata=tdis_ds, time_units=time_units
    )

    # GWF
    gwf = flopy.mf6.ModflowGwf(
        sim,
        modelname=gwfname,
        save_flows=True,
        model_nam_file="{}.nam".format(gwfname),
    )
    print("gwfname:",gwfname)

    # 渗流模型求解参数
    imsgwf = flopy.mf6.ModflowIms(
        sim,
        print_option="SUMMARY",
        outer_dvclose=hclose,
        outer_maximum=nouter,
        under_relaxation="NONE",
        inner_maximum=ninner,
        inner_dvclose=hclose,
        rcloserecord=rclose,
        linear_acceleration="CG",
        scaling_method="NONE",
        reordering_method="NONE",
        relaxation_factor=relax,
        filename="{}.ims".format(gwfname),
    )
    sim.register_ims_package(imsgwf, [gwf.name])

    # 离散化
    dis6 = flopy.mf6.ModflowGwfdis(
        gwf,
        length_units=length_units,
        xorigin=xorigin,
        yorigin=yorigin,
        nlay=nlay,
        nrow=nrow,
        ncol=ncol,
        delr=delr,
        delc=delc,
        top=top,
        botm=botm,
        idomain=idomain,
        filename="{}.dis".format(gwfname),
    )
    print("nlay:",nlay)
    print("nrow:",nrow)
    print("ncol:",ncol)
    print("delr:",delr)
    print("delc:",delc)
    print("top:",top)
    print("botm:",botm)

    # 含水层特性
    flopy.mf6.ModflowGwfnpf(
        gwf,
        save_flows=False,
        icelltype=icelltype,
        k=k11,
        # k22=1.0,
        # k22overk=k22overk,
        # k33overk=k33overk,
        k33=k33,
        save_specific_discharge=True,
        filename="{}.npf".format(gwfname),
    )
    print("icelltype:",icelltype)
    print("k11:",k11)
    print("k33:",k33)
    

    # 初始水头
    flopy.mf6.ModflowGwfic(
        gwf, strt=strt, filename="{}.ic".format(gwfname)
    )
    print("strt:",strt[0,100,100])
    print("strt:",strt[1,100,100])
    print("strt:",strt[2,100,100])
    print("strt:",strt[3,100,100])

    # 指定水头边界
    flopy.mf6.ModflowGwfchd(
        gwf,
        maxbound=len(chd_spd),
        stress_period_data=chd_spd,
        save_flows=False,
        auxiliary="CONCENTRATION",
        pname="CHD-1",
        filename="{}.chd".format(gwfname),
    )
    print("chd_spd:",chd_spd[0][:10])
 
    flopy.mf6.ModflowGwfriv(
        gwf, 
        stress_period_data=riv_spd
    )

    # 井边界
    flopy.mf6.ModflowGwfwel(
        gwf,
        print_input=True,
        print_flows=True,
        stress_period_data=wel_spd,
        save_flows=False,
        auxiliary="CONCENTRATION",
        pname="WEL-1",
        filename="{}.wel".format(gwfname),
    )
    print("wel_spd:",wel_spd)

    # 补给recharge
    flopy.mf6.ModflowGwfrcha(
        gwf,
        recharge=rch,
        filename="{}.rch".format(gwfname),
    )
    print("rch:",rch[100,100])

    # 蒸发
    flopy.mf6.ModflowGwfevta(
        gwf,
        surface=et_surf,
        rate=et_rat,
        depth=et_dep,
        filename="{}.evt".format(gwfname),
    )
    print("et_surf:",et_surf[100,100])
    print("et_rat:",et_rat[100,100])
    print("et_dep:",et_dep[100,100])

    # 输出控制
    flopy.mf6.ModflowGwfoc(
        gwf,
        head_filerecord="{}.hds".format(gwfname),
        budget_filerecord="{}.bud".format(gwfname),
        headprintrecord=[
            ("COLUMNS", 100, "WIDTH", 150, "DIGITS", 6, "GENERAL")
        ],
        saverecord=[("HEAD", "ALL"), ("BUDGET", "ALL")],
        printrecord=[("HEAD", "ALL"), ("BUDGET", "ALL")],
    )

    # GWT
    gwt = flopy.mf6.ModflowGwt(
        sim,
        modelname=gwtname,
        save_flows=True,
        model_nam_file="{}.nam".format(gwtname),
    )

    # 溶质运移模型求解参数
    imsgwt = flopy.mf6.ModflowIms(
        sim,
        print_option="SUMMARY",
        outer_dvclose=hclose,
        outer_maximum=nouter,
        under_relaxation="NONE",
        inner_maximum=ninner,
        inner_dvclose=hclose,
        rcloserecord=rclose,
        linear_acceleration="BICGSTAB",
        scaling_method="NONE",
        reordering_method="NONE",
        relaxation_factor=relax,
        filename="{}.ims".format(gwtname),
    )
    sim.register_ims_package(imsgwt, [gwt.name])

    # 离散化
    flopy.mf6.ModflowGwtdis(
        gwt,
        nlay=nlay,
        nrow=nrow,
        ncol=ncol,
        delr=delr,
        delc=delc,
        top=top,
        botm=botm,
        idomain=idomain,
        filename="{}.dis".format(gwtname),
    )

    # 初始浓度
    flopy.mf6.ModflowGwtic(
        gwt, strt=sconc, filename="{}.ic".format(gwtname)
    )

    # 对流
    flopy.mf6.ModflowGwtadv(
        gwt, scheme="UPSTREAM", filename="{}.adv".format(gwtname)
    )

    # 弥散
    flopy.mf6.ModflowGwtdsp(
        gwt,
        alh=al,
        ath1=ath1,
        atv=atv,
        filename="{}.dsp".format(gwtname),
    )

    # 化学反应 (等同于 MT3DMS 的 reaction)
    flopy.mf6.ModflowGwtmst(
        gwt,
        porosity=porosity,
        first_order_decay=False,
        decay=None,
        decay_sorbed=None,
        sorption=None,
        bulk_density=None,
        distcoef=None,
        filename="{}.mst".format(gwtname),
    )

    # 源汇项
    sourcerecarray = [
        ("WEL-1", "AUX", "CONCENTRATION"),
        ("CHD-1", "AUX", "CONCENTRATION"),
    ]
    flopy.mf6.ModflowGwtssm(
        gwt, sources=sourcerecarray, filename="{}.ssm".format(gwtname)
    )

    # 输出控制
    flopy.mf6.ModflowGwtoc(
        gwt,
        budget_filerecord="{}.cbc".format(gwtname),
        concentration_filerecord="{}.ucn".format(gwtname),
        concentrationprintrecord=[
            ("COLUMNS", 10, "WIDTH", 15, "DIGITS", 6, "GENERAL")
        ],
        saverecord=[("CONCENTRATION", "ALL"), ("BUDGET", "ALL")],
        printrecord=[("CONCENTRATION", "ALL"), ("BUDGET", "ALL")],
    )

    # GWF 和 GWT 之间的交换
    flopy.mf6.ModflowGwfgwt(
        sim,
        exgtype="GWF6-GWT6",
        exgmnamea=gwfname,
        exgmnameb=gwtname,
        filename="{}.gwfgwt".format(sim_name),
    )
    return sim, gwf, gwt, dis6
```

### 创建一个绘制结果的函数

```py
import matplotlib.pyplot as plt

def plot_results(model, result):
    fig = plt.figure(figsize = (14, 6))
    ax = fig.add_subplot(1, 1, 1)
    pmv = flopy.plot.PlotMapView(model = model, ax = ax, layer = 0)
    pmv.plot_grid(ax = ax, color = ".5", alpha = 0.2) # 绘制网格
    
    # 网格填充颜色
    plot_array = pmv.plot_array(result, masked_values = [1e30], cmap = 'bwr')
    plt.colorbar(plot_array)
    riv = pmv.plot_bc("RIV", alpha = 0.5)
    qm = pmv.plot_bc("CHD", alpha = 0.5)
    
    # 绘制等值线
    plot_contour_array = pmv.contour_array(result, masked_values = [1e30], cmap = "brg", linestyles="--")
    plt.clabel(plot_contour_array, fmt = r'%.2f') # 绘制等值线上文本标注
    # plt.colorbar(plot_contour_array)              # 等值线图例
    
    pmv.plot_bc(package = model.get_package("WEL-1"), color = 'green')
    pmv.plot_inactive(ibound = model.modelgrid.idomain, color_noflow = 'black')
    
    # 绘制水流的方向 (粉红色箭头)
    spdis = gwf.oc.output.budget().get_data(text = 'DATA-SPDIS')[0]
    qx, qy, qz = flopy.utils.postprocessing.get_specific_discharge(spdis, gwf)
    pmv.plot_vector(qx, qy, normalize = True, istep = 10, jstep = 10, color = "pink")
```


### 开始跑模型

```py
sim, gwf, gwt, dis6 = build_model()

sim.write_simulation(silent = True)

success, buff = sim.run_simulation(silent = True)
if not success:
    raise Exception("MODFLOW 6 did not terminate normally.")
```

### 读出模型的结果

```py
import matplotlib.pyplot as plt

head = gwf.oc.output.head().get_alldata()
concentration = gwt.oc.output.concentration().get_alldata()
print("head.shape:", head.shape)
print("concentration.shape:", concentration.shape)

# head.shape: (30, 4, 213, 283)
# concentration.shape: (30, 4, 213, 283)
```

```py
plot_results(model = gwf, result = concentration[9,0])
```

```py
plot_results(model = gwf, result = concentration[29,0])
```

## 假如模型需要“溯源”需要用到modpath

第一步：创建粒子群particlegroups

```py
# create particles
grid = gwf.modelgrid
partlocs = []
partids = []
for i in range(0,grid.nrow,10):
      for j in range(0,grid.ncol,10):
            if idomain[0,i,j] == 1:
                partlocs.append((0, i, j))
                partids.append(i*grid.ncol + j)
part0 = flopy.modpath.ParticleData(partlocs, structured=True, particleids=partids)
pg0 = flopy.modpath.ParticleGroup(
    particlegroupname="PG1", particledata=part0, filename="ex01a.sloc"
)

v = [(0,), (400,)]
pids = [1, 2]  # [1000, 1001]
part1 = flopy.modpath.ParticleData(v, structured=False, drape=1, particleids=pids)
pg1 = flopy.modpath.ParticleGroup(
    particlegroupname="PG2", particledata=part1, filename="ex01a.pg2.sloc"
)

particlegroups = [pg0, pg1]
```

第二步：将particlegroups放到模型中跑

```py
mp_namea = sim_name + "a_mp"
model_ws = sim_ws
# create modpath files
mp = flopy.modpath.Modpath7(
    modelname=mp_namea, flowmodel=gwf, exe_name="/usr/local/lib/modflow/mp7", model_ws=model_ws
)
flopy.modpath.Modpath7Bas(mp, porosity=porosity)

flopy.modpath.Modpath7Sim(
    mp,
    simulationtype="combined",
    trackingdirection="backward", # 重要
    weaksinkoption="pass_through",
    weaksourceoption="pass_through",
    referencetime=0.0,
    stoptimeoption="specified", # "extend"
    stoptime=365.0*20,
    zonedataoption="on",
    stopzone=2,
    zones=zone3,
    timepointdata=[100, 365.0], # the number of time points (timepointcount)
    particlegroups=particlegroups, # 粒子群参数
)

# flopy.modpath.Modpath7Sim(
#     mp,
#     simulationtype="combined",
#     trackingdirection="backward", # 重要
#     weaksinkoption="pass_through",
#     weaksourceoption="pass_through",
#     referencetime=0.0,
#     stoptimeoption="specified", # "extend"
#     stoptime=500000.0,
#     zonedataoption="on",
#     stopzone=2,
#     zones=zone3,
#     timepointdata=[500, 1000.0], # the number of time points (timepointcount)
#     particlegroups=particlegroups, # 粒子群参数
# )

# flopy.modpath.Modpath7Sim(
#     mp,
#     simulationtype="combined",
#     trackingdirection="backward", # 重要
#     weaksinkoption="pass_through",
#     weaksourceoption="pass_through",
#     referencetime=0.0,
#     stoptimeoption="extend", # "extend"
#     zonedataoption="on",
#     stopzone=2,
#     zones=zone3,
#     timepointdata=[500, 1000.0],
#     particlegroups=particlegroups, # 粒子群参数
# )

# flopy.modpath.Modpath7Sim(
#     mp,
#     simulationtype="combined",
#     trackingdirection="forward",
#     weaksinkoption="pass_through",
#     weaksourceoption="pass_through",
#     budgetoutputoption="summary",
#     budgetcellnumbers=[1049, 1259],
#     traceparticledata=[1, 1000],
#     referencetime=[0, 0, 0.0],
#     stoptimeoption="extend",
#     timepointdata=[500, 1000.0],
#     zonedataoption="on",
#     stopzone=2,
#     zones=zone3,
#     particlegroups=particlegroups,
# )

# write modpath datasets
mp.write_input()

```

第三步：跑模型

```py
# run modpath
mp.run_model()
```

第四步：获取模型数据并绘图

```py

fpth = os.path.join(model_ws, mp_namea + ".mppth")
p = flopy.utils.PathlineFile(fpth)
p0 = p.get_alldata()
fpth = os.path.join(model_ws, mp_namea + ".timeseries")
ts = flopy.utils.TimeseriesFile(fpth)
ts0 = ts.get_alldata()

# 画出endpoint
fpth = os.path.join(model_ws, mp_namea + ".mpend")
e = flopy.utils.EndpointFile(fpth)
e0 = e.get_alldata()

import matplotlib as mpl
fig = plt.figure(figsize=(13, 13), constrained_layout=True)
ax = fig.add_subplot(1, 1, 1, aspect="equal")
mm = flopy.plot.PlotMapView(modelgrid=gwf.modelgrid, ax=ax)
ax.set_xlim(0, Lx)
ax.set_ylim(0, Ly)
cmap = mpl.colors.ListedColormap(
    [
        "r",
        "g",
    ]
)
pmv = flopy.plot.PlotMapView(model = gwf, ax = ax, layer = 0)
# plot_array = pmv.plot_array(head[9,0], masked_values = [1e30], cmap = 'bwr')
# plt.colorbar(plot_array)
v = mm.plot_array(idomain[0], cmap=cmap, edgecolor="gray")
mm.plot_endpoint(e0, direction="ending", colorbar=True, shrink=0.5);
```

第五步：绘制第二张图


## 坐标转化

```py
from pyproj import Proj,transform
import math

def WGS84_CGCS2000(lon,lat,degree_type,is_zone_number):

    '''
    :param lon: 经度
    :param lat: 纬度
    :param degree_type:3度带或者6度带
    :param is_zone_number: 生成坐标是否包含带号
    :return:
    '''

    if int(lon)<75 or int(lon)>135:
        print('该区域不在中国境内')
    else:
        if degree_type==3:
            if is_zone_number==True:
                zone=math.floor( (lon-1.5)/3 )+1
                Epsg_test='45{0}'.format(str(zone-12) )
                p1 = Proj(init='epsg:4326')
                p2 = Proj(init='epsg:{0}'.format(Epsg_test))
                x, y = p1(lon,lat)
                x_prj, y_prj = transform(p1, p2, x, y)
                x_prj=int(x_prj)
                y_prj=int(y_prj)

            elif is_zone_number==False:
                zone=math.floor( (lon-1.5)/3 )+1
                Epsg_test = '45{0}'.format(str(zone+9))
                p1 = Proj(init='epsg:4326')
                p2 = Proj(init='epsg:{0}'.format(Epsg_test))
                x, y = p1(lon,lat)
                x_prj, y_prj = transform(p1, p2, x, y)
                x_prj=int(x_prj)
                y_prj=int(y_prj)

        elif degree_type==6:

            if is_zone_number==True:
                zone=math.floor(lon/6)+1
                epsg=str(4490+zone-12)
                p1 = Proj(init='epsg:4326')
                p2 = Proj(init='epsg:{0}'.format(epsg))
                x, y = p1(lon,lat)
                x_prj, y_prj = transform(p1, p2, x, y)
                x_prj=int(x_prj)
                y_prj=int(y_prj)

            elif is_zone_number==False:

                zone = math.floor(lon / 6) + 1
                epsg=4500+zone-11
                p1 = Proj(init='epsg:4326')
                p2 = Proj(init='epsg:{0}'.format(epsg))
                x, y = p1(lon,lat)
                x_prj, y_prj = transform(p1, p2, x, y)
                x_prj=int(x_prj)
                y_prj=int(y_prj)

    return x_prj,y_prj

def CGCS2000_WGS84(X,Y):

    p = Proj(init='epsg:4528')
    lon,lat = p(X,Y,inverse=True)
    lon=round(lon,6)
    lat=round(lat,6)
    return lon,lat

if __name__=='__main__':
    X_DIFF = 40094019 
    Y_DIFF = 368
    x_2000, y_2000 = WGS84_CGCS2000(121.049864464,32.551416489, 3, is_zone_number=True)
    print("对应的网格坐标:", x_2000 - X_DIFF, y_2000 - Y_DIFF)
    # ID为0的坐标值
    x0 = 498149
    y0 = 3604810
    x_WGS84, y_WGS84 = CGCS2000_WGS84(x0 + X_DIFF, y0 + Y_DIFF)
    print("对应的WGS坐标:", x_WGS84, y_WGS84)

    # print(CGCS2000_WGS84(x_2000,y_2000))
```

### 假如模型中增加一个污染源

userInput输入污染源的经纬度

```py
userInput = [121.049864464, 32.551416489]
x2000tmp, y2000tmp = WGS84_CGCS2000(userInput[0],userInput[1], 3, is_zone_number=True)
pollutionX = x2000tmp - X_DIFF
pollutionY = y2000tmp - Y_DIFF
print("对应的网格坐标:", pollutionX, pollutionY)
```

### 开始跑模型

```py
# 时间离散化参数:
# Temporal discretization
# 时间离散化参数:
nper = 3
tdis_ds = [(100.0, 10, 1.0), (900.0, 10, 1.0), (9000.0, 10, 1.0)]
print(tdis_ds)

# Filename
sim_ws = 'simulation_1026_pollution'
sim_name = 'pollution'          # 模拟总名称
gwfname = "gwf_" + sim_name # 渗流模型名称
gwtname = "gwt_" + sim_name # 溶质运移模型名称


sim, gwf, gwt, dis6 = build_model()
sim.write_simulation(silent = True)

success, buff = sim.run_simulation(silent = True)
if not success:
    raise Exception("MODFLOW 6 did not terminate normally.")

```

### 获取模型中的一些基本信息

网格点的坐标

```py
xvertice = gwf.modelgrid.xvertices
yvertice = gwf.modelgrid.yvertices
x0 = xvertice[0,0]
y0 = yvertice[0,0]
print("x0:", x0)
print("y0:", y0)
```

输出对应网格的序号

```py
pollutionC = int((pollutionX - x0) // delc)
pollutionR = int((y0 - pollutionY) // delr)

print("对应的网格序号:", pollutionC, pollutionR)
```

假设污染物在井四周9个网格点：

```py
# 边界条件: # Boundary conditions
# wel_spd 井
wel_spd = []
qwell = 0.5
cwell = 100.0
laytmp = 2
directions = [[-1,-1],[-1,0],[-1,1], \
              [0,-1],[0,0],[0,1], \
              [1,-1],[1,0],[1,1]]
for dx, dy in directions:
      wel_loc = (laytmp, pollutionR + dx, pollutionC + dy)
      wel_spd.append([wel_loc, qwell, cwell])
wel_spd = {0: wel_spd}
wel_spd
```

## 在一个监测点附近进行溯源

### 监测点的经纬度为userInput

```py
userInput = [121.049864464, 32.551416489]
x2000tmp, y2000tmp = WGS84_CGCS2000(userInput[0],userInput[1], 3, is_zone_number=True)
monitorX = x2000tmp - X_DIFF
monitorY = y2000tmp - Y_DIFF
x0 = xvertice[0,0]
y0 = yvertice[0,0]
monitorC = int((monitorX - x0) // delc)
monitorR = int((y0 - monitorY) // delr)
laytmp = 1
nodew = (laytmp, monitorR, monitorC)
```

```py
# Define names for the MODPATH 7 simulations
mp_namea = sim_name + "a_mp3"
mp_nameb = sim_name + "b_mp3"
```

### 创建粒子群，并进行计算

```py
multi = 1 # 

pcoord = np.array(
    [
        [0.000 * multi, 0.125 * multi, 0.500],
        [0.000 * multi, 0.375 * multi, 0.500],
        [0.000 * multi, 0.625 * multi, 0.500],
        [0.000 * multi, 0.875 * multi, 0.500],
        [1.000 * multi, 0.125 * multi, 0.500],
        [1.000 * multi, 0.375 * multi, 0.500],
        [1.000 * multi, 0.625 * multi, 0.500],
        [1.000 * multi, 0.875 * multi, 0.500],
        [0.125 * multi, 0.000 * multi, 0.500],
        [0.375 * multi, 0.000 * multi, 0.500],
        [0.625 * multi, 0.000 * multi, 0.500],
        [0.875 * multi, 0.000 * multi, 0.500],
        [0.125 * multi, 1.000 * multi, 0.500],
        [0.375 * multi, 1.000 * multi, 0.500],
        [0.625 * multi, 1.000 * multi, 0.500],
        [0.875 * multi, 1.000 * multi, 0.500],
    ]
)
print("pcoord.shape[0]:",pcoord.shape[0])
# pcoord.shape[0]

plocs = [nodew for i in range(pcoord.shape[0])]
print("plocs:", plocs)
# create particle data
pa = flopy.modpath.ParticleData(
    plocs,
    structured=True,
    localx=pcoord[:, 0],
    localy=pcoord[:, 1],
    localz=pcoord[:, 2],
    drape=0,
)
print("pcoord[:, 0]:",pcoord[:, 0])
print("pcoord[:, 1]:",pcoord[:, 1])
print("pcoord[:, 2]:",pcoord[:, 2])

# create backward particle group
fpth = mp_namea + ".sloc"
pga = flopy.modpath.ParticleGroup(
    particlegroupname="BACKWARD1", particledata=pa, filename=fpth
)

model_ws = sim_ws
# create modpath files
mp = flopy.modpath.Modpath7(
    modelname=mp_namea, flowmodel=gwf, exe_name="/usr/local/lib/modflow/mp7", model_ws=model_ws
)
flopy.modpath.Modpath7Bas(mp, porosity=porosity)
flopy.modpath.Modpath7Sim(
    mp,
    simulationtype="combined",
    trackingdirection="backward",
    weaksinkoption="pass_through",
    weaksourceoption="pass_through",
    referencetime=0.0,
    stoptimeoption="extend",
    timepointdata=[500, 1000.0],
    particlegroups=pga,
)

# write modpath datasets
mp.write_input()
```

### 跑模型，获取数据，并绘图

```py
# run modpath
mp.run_model()

fpth = os.path.join(model_ws, mp_namea + ".mppth")
p = flopy.utils.PathlineFile(fpth)
p0 = p.get_alldata()
fpth = os.path.join(model_ws, mp_namea + ".timeseries")
ts = flopy.utils.TimeseriesFile(fpth)
ts0 = ts.get_alldata()

# 画出endpoint
fpth = os.path.join(model_ws, mp_namea + ".mpend")
e = flopy.utils.EndpointFile(fpth)
e0 = e.get_alldata()

import matplotlib as mpl
fig = plt.figure(figsize=(13, 13), constrained_layout=True)
ax = fig.add_subplot(1, 1, 1, aspect="equal")
mm = flopy.plot.PlotMapView(modelgrid=gwf.modelgrid, ax=ax)
ax.set_xlim(0, Lx)
ax.set_ylim(0, Ly)
cmap = mpl.colors.ListedColormap(
    [
        "r",
        "g",
    ]
)
pmv = flopy.plot.PlotMapView(model = gwf, ax = ax, layer = 0)
plot_array = pmv.plot_array(head[9,0], masked_values = [1e30], cmap = 'bwr')
# plt.colorbar(plot_array)
v = mm.plot_array(idomain[0], cmap=cmap, edgecolor="gray")
mm.plot_endpoint(e0, direction="ending", colorbar=True, shrink=0.5);
```

### 绘制第二张图

```py
import matplotlib as mpl
fig = plt.figure(figsize=(8, 8), constrained_layout=True)
ax = fig.add_subplot(1, 1, 1, aspect="equal")
mm = flopy.plot.PlotMapView(modelgrid=gwf.modelgrid, ax=ax)
# pmv = flopy.plot.PlotMapView(model = gwf, ax = ax, layer = 0)
# mm.plot_grid(ax = ax, color = ".5", alpha = 0.2) # 绘制网格
ax.set_xlim(0, Lx)
ax.set_ylim(0, Ly)
cmap = mpl.colors.ListedColormap(
    [
        "r",
        "g",
    ]
)
v = mm.plot_array(gwf.modelgrid.idomain, cmap=cmap, edgecolor="white")
mm.plot_pathline(p0, layer="all", color="blue", lw=0.75)
colors = ["green", "orange", "red","blue"]
# 绘制各个颜色的点
# for k in range(nlay):
#     mm.plot_timeseries(ts0, layer=k, marker="o", lw=0, color=colors[k]);
mm.plot_timeseries(ts0, layer=0, marker="o", lw=0, color=colors[0])

# 绘制等值线
plot_contour_array = mm.contour_array(concentration[9,0], masked_values = [1e30], cmap = "brg", linestyles="--")
plt.clabel(plot_contour_array, fmt = r'%.2f') # 绘制等值线上文本标注
# plt.colorbar(plot_contour_array)              # 等值线图例

mm.plot_bc(package = gwf.get_package("WEL-1"), color = 'green')
mm.plot_inactive(ibound = gwf.modelgrid.idomain, color_noflow = 'black')


```

## 控制监控点的位置，端点，尽量保证是直线

先确定监控点的位置，也就是从GMS中，确定监控点的网格坐标

```py
这里的getHead，用的是模型中读取的值，在实际项目中，应用传感器读取到的实测值
def getHead(tmpi, tmpj):
    return head[29, 0, tmpi-1, tmpj-1] 
```

### 左侧边界拟合

```py
x = [11, 19, 27, 37, 45, 72, 87, 109, 115]
y = [getHead(11, 112), getHead(19, 104), getHead(27, 95), getHead(37, 84), getHead(45, 75), getHead(72, 49), getHead(87, 38), getHead(109, 21), getHead(115, 15)]
z1 = np.polyfit(x, y, 4) #用3次多项式拟合，输出系数从高到0
funcLeft1 = np.poly1d(z1) #使用次数合成多项式
y_pre = funcLeft1(x)

plt.plot(x,y,'.')
plt.plot(x,y_pre)
plt.show()
```

```py
x = [115, 118, 133, 154, 202]
# y = [getHead(115, 15), getHead(115, 15), getHead(115, 15), getHead(115, 15), getHead(115, 15)]
y = [getHead(115, 15), getHead(118, 37), getHead(133, 71), getHead(154, 120), getHead(202, 215)]
z1 = np.polyfit(x, y, 4) #用3次多项式拟合，输出系数从高到0
funcLeft2 = np.poly1d(z1) #使用次数合成多项式
y_pre = funcLeft2(x)

plt.plot(x,y,'.')
plt.plot(x,y_pre)
plt.show()
```

### 右侧边界拟合

```py
x = [11, 14, 20, 30, 46, 60]
y = [getHead(11, 112), getHead(14, 117), getHead(20, 119), getHead(30, 136), getHead(46, 195), getHead(60, 247)]
z1 = np.polyfit(x, y, 6) #用3次多项式拟合，输出系数从高到0
funcRight1 = np.poly1d(z1) #使用次数合成多项式
y_pre = funcRight1(x)

plt.plot(x,y,'.')
plt.plot(x,y_pre)
plt.show()
```

```py
x = [60, 119, 138, 145, 154, 162, 172, 187, 202]
y = [getHead(60, 247), getHead(119, 270), getHead(138, 262), getHead(145, 252), getHead(154, 241), getHead(162, 232), getHead(172, 228), getHead(187, 221), getHead(202, 215)]
z2 = np.polyfit(x, y, 6) #用3次多项式拟合，输出系数从高到0
funcRight2 = np.poly1d(z2) #使用次数合成多项式
y_pre = funcRight2(x)

plt.plot(x,y,'.')
plt.plot(x,y_pre)
plt.show()
```

### 创建新的模型

```py
# Filename
sim_ws = 'simulation_1028_B'
sim_name = 'model'          # 模拟总名称
gwfname = "gwf_" + sim_name # 渗流模型名称
gwtname = "gwt_" + sim_name # 溶质运移模型名称

# 边界条件: # Boundary conditions
# wel_spd 井
wel_spd = []
qwell = 0
cwell = 100.0
laytmp = 2
directions = [[-1,-1],[-1,0],[-1,1], \
              [0,-1],[0,0],[0,1], \
              [1,-1],[1,0],[1,1]]
for dx, dy in directions:
      wel_loc = (laytmp, pollutionR + dx, pollutionC + dy)
      wel_spd.append([wel_loc, qwell, cwell])
wel_spd = {0: wel_spd}

# 时间离散化参数:
# Temporal discretization
# 时间离散化参数:
nper = 1
tdis_ds = [(100.0, 10, 1.0)]

chd_spd = []
cnt = 0

for k in np.arange(nlay): 
      for i in np.arange(nrow):
            for j in np.arange(ncol): # 西边
                  if idomain[k][i][j] == 1: # i <= 103 可以把south过滤掉
                        # chd_spd.append([(k, i, j), funcLeft3(i), 100.0])  #(l, r, c),head, conc
                        # cnt += 1
                        # break
                        if i < 115:
                              chd_spd.append([(k, i, j), funcLeft1(i+1), 100.0])  #(l, r, c),head, conc
                              cnt += 1
                              break
                        else:
                              chd_spd.append([(k, i, j), funcLeft2(i+1), 100.0])  #(l, r, c),head, conc
                              cnt += 1
                              break
            for j in np.arange(ncol-1,-1,-1):  # 东边
                  if idomain[k][i][j] == 1: # i >= 84 可以把north过滤掉
                        # chd_spd.append([(k, i, j), funcRight3(i), 100.0])  #(l, r, c),head, conc
                        # cnt += 1
                        # break
                        if i < 60:
                              chd_spd.append([(k, i, j), funcRight1(i+1), 100.0])  #(l, r, c),head, conc
                              cnt += 1
                              break
                        else:
                              chd_spd.append([(k, i, j), funcRight2(i+1), 100.0])  #(l, r, c),head, conc
                              cnt += 1
                              break

# for k in np.arange(4): # 东西两侧分别指定，nlay
#       for i in np.arange(nrow):
#             for j in np.arange(ncol): # 西
#                   if idomain[k][i][j] == 1:
#                         chd_spd.append([(k, i, j), chd[k, i, j], 100.0])  #(l, r, c),head, conc
#                         break
#             for j in np.arange(ncol-1,-1,-1): # 东
#                   if idomain[k][i][j] == 1:
#                         chd_spd.append([(k, i, j), chd[k, i, j], 100.0])  #(l, r, c),head, conc
#                         break
chd_spd_dic = {0: chd_spd}
cnt
```

### 模型再跑一遍

```py


# chd_spd = []
# cnt = 0
# for k in np.arange(1): 
#     for j in np.arange(ncol): 
#         for i in np.arange(nrow): # 北边
#             if idomain[k][i][j] == 1 and j >= 125: # j >= 125 可以把west过滤掉
#               chd_spd.append([(k, i, j), funcSouth_j(j), 100.0]) #(l, r, c),head, conc
#               cnt += 1
#               break
#         for i in np.arange(nrow-1,-1,-1): # 南边
#             if idomain[k][i][j] == 1 and j <= 185: # j <= 185 可以把east过滤掉
#               chd_spd.append([(k, i, j), funcNorth_j(j), 100.0]) #(l, r, c),head, conc
#               cnt += 1
#               break
# chd_spd = {0: chd_spd}
# cnt

# RIV
# riv_sp_0 = [] # 应力周期 0
# riv_spd = {0: riv_sp_0}

import flopy
def build_model():
    sim = flopy.mf6.MFSimulation(
        sim_name=sim_name, sim_ws=sim_ws, exe_name='/usr/local/lib/modflow/mf6'
    )
    print("sim_name:",sim_name)
    print("sim_ws:",sim_ws)

    # 时间离散化
    flopy.mf6.ModflowTdis(
        sim, nper=nper, perioddata=tdis_ds, time_units=time_units
    )

    # GWF
    gwf = flopy.mf6.ModflowGwf(
        sim,
        modelname=gwfname,
        save_flows=True,
        model_nam_file="{}.nam".format(gwfname),
    )
    print("gwfname:",gwfname)

    # 渗流模型求解参数
    imsgwf = flopy.mf6.ModflowIms(
        sim,
        print_option="SUMMARY",
        outer_dvclose=hclose,
        outer_maximum=nouter,
        under_relaxation="NONE",
        inner_maximum=ninner,
        inner_dvclose=hclose,
        rcloserecord=rclose,
        linear_acceleration="CG",
        scaling_method="NONE",
        reordering_method="NONE",
        relaxation_factor=relax,
        filename="{}.ims".format(gwfname),
    )
    sim.register_ims_package(imsgwf, [gwf.name])

    # 离散化
    dis6 = flopy.mf6.ModflowGwfdis(
        gwf,
        length_units=length_units,
        xorigin=xorigin,
        yorigin=yorigin,
        nlay=nlay,
        nrow=nrow,
        ncol=ncol,
        delr=delr,
        delc=delc,
        top=top,
        botm=botm,
        idomain=idomain,
        filename="{}.dis".format(gwfname),
    )
    print("nlay:",nlay)
    print("nrow:",nrow)
    print("ncol:",ncol)
    print("delr:",delr)
    print("delc:",delc)
    print("top:",top)
    print("botm:",botm)

    # 含水层特性
    flopy.mf6.ModflowGwfnpf(
        gwf,
        save_flows=False,
        icelltype=icelltype,
        k=k11,
        # k22=1.0,
        # k22overk=k22overk,
        # k33overk=k33overk,
        k33=k33,
        save_specific_discharge=True,
        filename="{}.npf".format(gwfname),
    )
    print("icelltype:",icelltype)
    print("k11:",k11)
    print("k33:",k33)
    

    # 初始水头
    flopy.mf6.ModflowGwfic(
        gwf, strt=strt, filename="{}.ic".format(gwfname)
    )
    print("strt:",strt[0,100,100])
    print("strt:",strt[1,100,100])
    print("strt:",strt[2,100,100])
    print("strt:",strt[3,100,100])

    # 指定水头边界
    flopy.mf6.ModflowGwfchd(
        gwf,
        maxbound=len(chd_spd_dic),
        stress_period_data=chd_spd_dic,
        save_flows=False,
        auxiliary="CONCENTRATION",
        pname="CHD-1",
        filename="{}.chd".format(gwfname),
    )
    print("chd_spd:",chd_spd[0][:10])
 
    # flopy.mf6.ModflowGwfriv(
    #     gwf, 
    #     stress_period_data=riv_spd
    # )

    # 井边界
    flopy.mf6.ModflowGwfwel(
        gwf,
        print_input=True,
        print_flows=True,
        stress_period_data=wel_spd,
        save_flows=False,
        auxiliary="CONCENTRATION",
        pname="WEL-1",
        filename="{}.wel".format(gwfname),
    )
    print("wel_spd:",wel_spd)

    # 补给recharge
    flopy.mf6.ModflowGwfrcha(
        gwf,
        recharge=rch,
        filename="{}.rch".format(gwfname),
    )
    print("rch:",rch[100,100])

    # 蒸发
    flopy.mf6.ModflowGwfevta(
        gwf,
        surface=et_surf,
        rate=et_rat,
        depth=et_dep,
        filename="{}.evt".format(gwfname),
    )
    print("et_surf:",et_surf[100,100])
    print("et_rat:",et_rat[100,100])
    print("et_dep:",et_dep[100,100])

    # 输出控制
    flopy.mf6.ModflowGwfoc(
        gwf,
        head_filerecord="{}.hds".format(gwfname),
        budget_filerecord="{}.bud".format(gwfname),
        headprintrecord=[
            ("COLUMNS", 100, "WIDTH", 150, "DIGITS", 6, "GENERAL")
        ],
        saverecord=[("HEAD", "ALL"), ("BUDGET", "ALL")],
        printrecord=[("HEAD", "ALL"), ("BUDGET", "ALL")],
    )

    # GWT
    gwt = flopy.mf6.ModflowGwt(
        sim,
        modelname=gwtname,
        save_flows=True,
        model_nam_file="{}.nam".format(gwtname),
    )

    # 溶质运移模型求解参数
    imsgwt = flopy.mf6.ModflowIms(
        sim,
        print_option="SUMMARY",
        outer_dvclose=hclose,
        outer_maximum=nouter,
        under_relaxation="NONE",
        inner_maximum=ninner,
        inner_dvclose=hclose,
        rcloserecord=rclose,
        linear_acceleration="BICGSTAB",
        scaling_method="NONE",
        reordering_method="NONE",
        relaxation_factor=relax,
        filename="{}.ims".format(gwtname),
    )
    sim.register_ims_package(imsgwt, [gwt.name])

    # 离散化
    flopy.mf6.ModflowGwtdis(
        gwt,
        nlay=nlay,
        nrow=nrow,
        ncol=ncol,
        delr=delr,
        delc=delc,
        top=top,
        botm=botm,
        idomain=idomain,
        filename="{}.dis".format(gwtname),
    )

    # 初始浓度
    flopy.mf6.ModflowGwtic(
        gwt, strt=sconc, filename="{}.ic".format(gwtname)
    )

    # 对流
    flopy.mf6.ModflowGwtadv(
        gwt, scheme="UPSTREAM", filename="{}.adv".format(gwtname)
    )

    # 弥散
    flopy.mf6.ModflowGwtdsp(
        gwt,
        alh=al,
        ath1=ath1,
        atv=atv,
        filename="{}.dsp".format(gwtname),
    )

    # 化学反应 (等同于 MT3DMS 的 reaction)
    flopy.mf6.ModflowGwtmst(
        gwt,
        porosity=porosity,
        first_order_decay=False,
        decay=None,
        decay_sorbed=None,
        sorption=None,
        bulk_density=None,
        distcoef=None,
        filename="{}.mst".format(gwtname),
    )

    # 源汇项
    sourcerecarray = [
        ("WEL-1", "AUX", "CONCENTRATION"),
        ("CHD-1", "AUX", "CONCENTRATION"),
    ]
    flopy.mf6.ModflowGwtssm(
        gwt, sources=sourcerecarray, filename="{}.ssm".format(gwtname)
    )

    # 输出控制
    flopy.mf6.ModflowGwtoc(
        gwt,
        budget_filerecord="{}.cbc".format(gwtname),
        concentration_filerecord="{}.ucn".format(gwtname),
        concentrationprintrecord=[
            ("COLUMNS", 10, "WIDTH", 15, "DIGITS", 6, "GENERAL")
        ],
        saverecord=[("CONCENTRATION", "ALL"), ("BUDGET", "ALL")],
        printrecord=[("CONCENTRATION", "ALL"), ("BUDGET", "ALL")],
    )

    # GWF 和 GWT 之间的交换
    flopy.mf6.ModflowGwfgwt(
        sim,
        exgtype="GWF6-GWT6",
        exgmnamea=gwfname,
        exgmnameb=gwtname,
        filename="{}.gwfgwt".format(sim_name),
    )
    return sim, gwf, gwt, dis6


sim, gwf1, gwt, dis6 = build_model()
sim.write_simulation(silent = True)

success, buff = sim.run_simulation(silent = True)
if not success:
    raise Exception("MODFLOW 6 did not terminate normally.")


head = gwf.oc.output.head().get_alldata()
concentration = gwt.oc.output.concentration().get_alldata()

```

### 把计算结果绘制出来

```py
import matplotlib.pyplot as plt
def plot_results(model, result):
    fig = plt.figure(figsize = (14, 6))
    ax = fig.add_subplot(1, 1, 1)
    pmv = flopy.plot.PlotMapView(model = model, ax = ax, layer = 0)
    pmv.plot_grid(ax = ax, color = ".5", alpha = 0.2) # 绘制网格
    
    # 网格填充颜色
    plot_array = pmv.plot_array(result, masked_values = [1e30], cmap = 'bwr')
    plt.colorbar(plot_array)
    # riv = pmv.plot_bc("RIV", alpha = 0.5)
    qm = pmv.plot_bc("CHD", alpha = 0.5)
    
    # 绘制等值线
    plot_contour_array = pmv.contour_array(result, masked_values = [1e30], cmap = "brg", linestyles="--")
    plt.clabel(plot_contour_array, fmt = r'%.2f') # 绘制等值线上文本标注
    # plt.colorbar(plot_contour_array)              # 等值线图例
    
    pmv.plot_bc(package = model.get_package("WEL-1"), color = 'green')
    pmv.plot_inactive(ibound = model.modelgrid.idomain, color_noflow = 'black')
    
    # 绘制水流的方向 (粉红色箭头)
    spdis = gwf.oc.output.budget().get_data(text = 'DATA-SPDIS')[0]
    qx, qy, qz = flopy.utils.postprocessing.get_specific_discharge(spdis, gwf)
    pmv.plot_vector(qx, qy, normalize = True, istep = 10, jstep = 10, color = "pink")

# plot_results(model = gwf1, result = chd[0])
plot_results(model = gwf1, result = head[9,0])
```