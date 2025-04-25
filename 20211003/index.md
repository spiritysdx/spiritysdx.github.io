# 2021年全国大学生数学建模竞赛C题省一等奖代码部分


## 前言

论文配套的代码均为本人编写，第三四问存在部分代码问题，需要改动细节部分才能运行成功，后面两问的代码时间成本很高，长则数小时，短则十几二十分钟，自己跑酌情修改迭代次数，转换赛题要求的填表格式得通过最后部分的代码才能转换。

完整材料详见博客相关资源

## 代码

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec
import time,random

plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号

# 第一问


#原始数据
data1 = pd.read_excel(r'附件1 近5年402家供应商的相关数据.xlsx',sheet_name='企业的订货量（m³）')
data2 = pd.read_excel(r'附件1 近5年402家供应商的相关数据.xlsx',sheet_name='供应商的供货量（m³）')

#企业订货量
temp1 = data1.set_index('供应商ID').drop(['材料分类'],axis=1)

#供应商供货量
temp2 = data2.set_index('供应商ID').drop(['材料分类'],axis=1)

#订货量与供货量的差值
temp3 = temp2 - temp1

#材料分类系数
Dtype = list(data1['材料分类'])
Ddata = [0.95,0.86,0.79]
D = []
for i in Dtype:
    if i == 'A':
        D.append(Ddata[0])
    elif i == 'B':
        D.append(Ddata[1])
    elif i == 'C':
        D.append(Ddata[2])
    else:
        continue

#构造趋势函数
def f(x):
    if x < 0:
        return 4-4*(0.8)**x 
    elif x >= 0:
        return (0.5)**x
    else:
        pass
#画趋势图
plt.figure()
x=list(np.linspace(-1,1))
y = [f(i) for i in x]
plt.plot(x,y)
plt.xlabel('t')
plt.ylabel('y')
plt.savefig('qushi.jpg')
plt.show()

#供货量与订货量的差值比企业订货量
temp4=temp3/temp1
temp4 = temp4.fillna(0)

#计算供货商每周得分
roworder = 0
ttp = []
for row in temp3.itertuples():
    lineorder = 0
    rowlist = []
    for i in list(row)[1:]:
        #计算供应商每周对应综合得分
        tp = f(np.array(temp4)[roworder][lineorder])*abs(np.array(temp2)[roworder][lineorder])*D[roworder]
        rowlist.append(tp)
        lineorder +=1
    roworder +=1
    ttp.append(rowlist)

#计算供货商总得分
d1 = []
for i in ttp:
    d1.append(sum(i))

#构造供货商总得分表
df1 = pd.DataFrame(d1)

#构造问题一供货商综合得分表
ttpp = ttp.copy()
count = 0
for i in ttpp:
    ct = 0
    i.append(Dtype[count])
    i.append(d1[count])
    count +=1
index = temp1.index.values
columns = data1.columns.values[2:]
index = list(index)
columns = list(columns)
columns.append('材料分类')
columns.append('得分')
w1_defen = pd.DataFrame(ttpp,index=index,columns=columns)
#w1_defen.to_csv('问题一供货商得分表.csv')

#供货商综合得分表排序后得到最优的50个供货商
df1_data = df1.sort_values(by=[0],ascending=False).head(50)

#最优的50家供货商得到的订购量
tptp = []
for i in list(df1_data.index):
    tptp.append(pd.DataFrame(list(data1.iloc[i])).T)
dff1 = pd.concat(tptp)
#dff1.to_csv('dff1.csv')

#最优的50家供货商提供的供货量
tptp = []
for i in list(df1_data.index):
    tptp.append(pd.DataFrame(list(data2.iloc[i])).T)
dff1 = pd.concat(tptp)
#dff1.to_csv('df1.csv')

#第二问

#第二问
#50家供应商的一周总平均提供
temp5 = dff1.set_index(0).drop([1],axis=1)

tp = temp5.std(axis=1)#具体方差表
tmp = temp5.median(axis=1)#具体中位数
index = list(tp.index)
fc_values = list(tp)
zvs_values = list(tmp)

#方差中位数图
plt.figure(figsize=(25, 10), dpi=60)
x = np.linspace(1,len(index))
y1 = fc_values#方差
y2 = zvs_values#中位数
plt.plot(x,y1,x,y2)
plt.show()


#突变稳定初筛选
tubian_list = []
wending_list = []
for i in fc_values:
    if i >= 300:#方差大于300
        addres = fc_values.index(i)
        if zvs_values[addres] <= 100:#中位数小于100 
            tubian_list.append(index[addres])
        else:
            wending_list.append(index[addres])
    else:
        addres = fc_values.index(i)
        wending_list.append(index[addres])

#处理类型成本
Dtype50 = []
for i in np.array(dff1):
    Dtype50.append(i[1])
Ddata50 = [0.6,0.66,0.72]
D50 = []
for i in Dtype50:
    if i == 'A':
        D50.append(Ddata50[0])
    elif i == 'B':
        D50.append(Ddata50[1])
    elif i == 'C':
        D50.append(Ddata50[2])
    else:
        continue

#已订购的平均值
you0 = list((temp5 == 0).astype(int).sum(axis=1))
sum_temp5 = list(temp5.sum(axis=1).values)
fei0 = [240 - i for i in you0]
mean_temp5 = np.array(sum_temp5)/np.array(fei0)

#按顺序提取最低限度的供应商
index = list(dff1[0])
pj_values = list(mean_temp5)
sum_cn = 0
ct = 0
count = 0
pjcn = []
ljcn = []
for i in mean_temp5:
    sum_cn = sum_cn + (i/D50[ct])
    ljcn.append(sum_cn)
    pjcn.append((i/D50[ct]))
    if sum_cn < 28200:
        count+=1
        ct+=1
        continue
    else:
        break
gys = list(dff1[0].values)[0:count+1]
cpt = Dtype50[0:count+1]
gys23 = pd.DataFrame([gys,cpt,pjcn,ljcn],index=['供应商ID','材料分类','供应商平均产能','累计产能']).T.set_index('供应商ID')
#gys23.to_excel('选择的23个供应商.xlsx')

#看看原始23家的供货商供货量图

#for i in gys:
    #if i in tubian_list
tpw = []
for i in gys:
    tpw.append(np.array(temp5.T[i]))
biao23 = pd.DataFrame(tpw)
count = 0

for j in tpw: 
    y = tpw[count]
    x = biao23.columns.values
    plt.figure()
    plt.plot(x,y)
    plt.xlabel(gys[count])
    plt.show()
    time.sleep(2)
    count+=1

#biao23.to_excel('选择的23个供应商具体数据.xlsx')

#第一周随机序列
def first_suiji():
    #23家分类
    tubian_id = ['S140','S139','S307','S395']
    yichang_id = ['S201','S348','S330','S308','S151']
    teshu_id = ['S108']
    wending_id = []
    wending_type=[]
    
    for i in gys:
        if (i not in yichang_id )and (i not in tubian_id) and (i not in teshu_id):
            wending_id.append(i)
            wending_type.append(gys23.loc[i]['材料分类'])

    #异常值取行与去除异常值后加入稳定值
    yic = []
    yic_type = []
    for j in yichang_id:
        addres = gys.index(j)
        yic.append(np.array(biao23.loc[addres]))
        yic_type.append(gys23.loc[j]['材料分类'])
    for k in yic:
        for m in k:
            if m > 6000:
                addres = list(k).index(m)
                k[addres] = np.median(k)#异常值使用中位数
    yic_suiji_xulie = []
    for t in yic:
        yic_max=t.max()
        yic_min=t.min()
        yic_suiji_xulie.append(np.random.randint(yic_max*0.65,yic_max, 1))


    #特殊值取行
    addres = gys.index(teshu_id[0])
    tes = np.array(biao23.loc[addres])

    #稳定值取行
    wen = []
    for j in wending_id:
        addres = gys.index(j)
        wen.append(np.array(biao23.loc[addres]))

    #稳定值取随机序列
    wen_up = []
    wen_down = []
    for i in wen:
        wen_up.append(i.max())
        wen_down.append(i.max()*0.65)

    wen_suiji_xulie = []
    for i in wen_up:
        addres = wen_up.index(i)
        wen_suiji_xulie.append(np.random.randint(wen_down[addres],i, 1))

    #总的稳定值随机序列
    wenzong = wen_suiji_xulie+yic_suiji_xulie
    wenzong_type = wending_type+yic_type
    for i in yichang_id:
        wending_id.append(i)
    
    #特殊值取随机序列后加上最大值
    text = []
    tq = []
    te_type = gys23.loc[teshu_id[0]]['材料分类']
    for i in tes:
        if i != tes[list(tes).index(tes.max())]:
            text.append(i)
            tq.append(i)
        else:
            text.append('s')
    text[text.index('s')] = np.array(tq).mean()
    tes_up = np.array(text).max()
    tes_down = np.array(text).min()
    te_suiji_xulie = np.random.randint(tes_up*0.65,tes_up, 24)
    tes_max = list(tes).index(tes.max())
    #te_suiji_xulie[random.randint(0,24)] = tes_max
    #te_suiji = te_suiji_xulie[random.randint(0,23)]


    #三个突变值取最大值的两个周
    tb = []
    tb_type = []
    for j in tubian_id:
        tb_type.append(gys23.loc[j]['材料分类'])
        tp = np.linspace(0,0,24)
        addres = np.random.randint(0,24, 2)
        if (addres[0] == 0) or (addres[1] == 23): 
            tb.append(np.random.randint(5999,6000, 1))
        else:
            tb.append(0)

    return wenzong,tb,np.array([6000]),wenzong_type,tb_type,te_type,[wending_id,tubian_id,teshu_id]

#后面几周随机序列
def suiji():
    #23家分类
    tubian_id = ['S140','S139','S307','S395']
    yichang_id = ['S201','S348','S330','S308','S151']
    teshu_id = ['S108']
    wending_id = []
    wending_type=[]
    for i in gys:
        if (i not in yichang_id )and (i not in tubian_id) and (i not in teshu_id):
            wending_id.append(i)
            wending_type.append(gys23.loc[i]['材料分类'])

    #异常值取行与去除异常值后加入稳定值
    yic = []
    yic_type = []
    for j in yichang_id:
        addres = gys.index(j)
        yic.append(np.array(biao23.loc[addres]))
        yic_type.append(gys23.loc[j]['材料分类'])
    for k in yic:
        for m in k:
            if m > 6000:
                addres = list(k).index(m)
                k[addres] = np.median(k)#异常值使用中位数
    yic_suiji_xulie = []
    for t in yic:
        yic_max=t.max()
        yic_min=t.min()
        yic_suiji_xulie.append(np.random.randint(yic_max*0.2,yic_max*0.7, 1))


    #特殊值取行
    addres = gys.index(teshu_id[0])
    tes = np.array(biao23.loc[addres])

    #稳定值取行
    wen = []
    for j in wending_id:
        addres = gys.index(j)
        wen.append(np.array(biao23.loc[addres]))

    #稳定值取随机序列
    wen_up = []
    wen_down = []
    for i in wen:
        wen_up.append(i.max()*0.7)
        wen_down.append(i.max()*0.2)

    wen_suiji_xulie = []
    for i in wen_up:
        addres = wen_up.index(i)
        wen_suiji_xulie.append(np.random.randint(wen_down[addres],i, 1))

    #总的稳定值随机序列
    wenzong = wen_suiji_xulie+yic_suiji_xulie
    wenzong_type = wending_type+yic_type
    for i in yichang_id:
        wending_id.append(i)
    
    #特殊值取随机序列
    text = []
    tq = []
    te_type = gys23.loc[teshu_id[0]]['材料分类']
    for i in tes:
        if i != tes[list(tes).index(tes.max())]:
            text.append(i)
            tq.append(i)
        else:
            text.append('s')
    text[text.index('s')] = np.array(tq).mean()
    tes_up = np.array(text).max()
    tes_down = np.array(text).min()
    te_suiji_xulie = np.random.randint(500,1000, 24)
    
    tes_max = list(tes).index(tes.max())
    #te_suiji_xulie[random.randint(0,24)] = tes_max
    te_suiji = te_suiji_xulie[random.randint(0,23)]


    #三个突变值取最大值的两个周
    tb = []
    tb_type = []
    #for i in tubian_id:
    #    addres = gys.index(i)
    #    tubian.append(np.array(biao23.loc[addres]))
    for j in tubian_id:
        tb_type.append(gys23.loc[j]['材料分类'])
        tp = np.linspace(0,0,24)
        addres = np.random.randint(0,24, 2)
        if (addres[0] == 0) or (addres[1] == 23): 
            tb.append(np.random.randint(2000,6000, 1))
        else:
            tb.append(0)

    return wenzong,tb,np.array(te_suiji),wenzong_type,tb_type,te_type,[wending_id,tubian_id,teshu_id]

#第一周
result1 = []
mubiao1_list = []
P1 = []
for i in range(1000):
    suiji_list = first_suiji()
    count +=0
    t2 = []
    t2.append(int(suiji_list[2]))
    suiji_data = suiji_list[0]+suiji_list[1]+t2
    suiji_type = suiji_list[3]+suiji_list[4]
    suiji_type.append(suiji_list[5])
    gh = pd.DataFrame([suiji_type,suiji_data]).T
    A = gh[gh[0] == 'A'][1].values.sum()
    B = gh[gh[0] == 'B'][1].values.sum()
    C = gh[gh[0] == 'C'][1].values.sum()
    if ((A+B+C) <= 48000) and ((A/0.6+B/0.66+C/0.72)>=(28200*2)):
        P1.append(A/0.6+B/0.66+C/0.72)
        mubiao1 = 1.2*A+1.1*B+C
        mubiao1_list.append(mubiao1)
        result1.append(suiji_list)
    else:
        continue
addres = mubiao1_list.index(min(mubiao1_list)[0])
fliter = []
for i in result1[addres][0]:
    fliter.append(int(i))
for i in result1[addres][1]:
    fliter.append(i)
fliter.append(int(result1[addres][2]))
fliter.append(int(P1[addres]))
fliter.append(int(min(mubiao1_list)))
fliter1 = fliter.copy()

#后面几周
#筛选对应材料的供货商
p = int(P1[addres])
fliter_list = []
P_list = [int(P1[addres])]
CT = 0
for i in range(2,25):
    result2 = []
    mubiao2_list = []
    Pn = []
    p=P_list[CT]
    for i in range(500):
        suiji_list = suiji()
        count +=0
        t2 = []
        t2.append(int(suiji_list[2]))
        suiji_data = suiji_list[0]+suiji_list[1]+t2
        suiji_type = suiji_list[3]+suiji_list[4]
        suiji_type.append(suiji_list[5])
        gh = pd.DataFrame([suiji_type,suiji_data]).T
        A = gh[gh[0] == 'A'][1].values.sum()
        B = gh[gh[0] == 'B'][1].values.sum()
        C = gh[gh[0] == 'C'][1].values.sum()
        if ((A+B+C) <= 48000) and ((A/0.6+B/0.66+C/0.72)>=(28200*2)-p):
            Pn.append(A/0.6+B/0.66+C/0.72)
            mubiao2 = 1.2*A+1.1*B+C
            mubiao2_list.append(mubiao1)
            result2.append(suiji_list)
        else:
            continue
    addres = mubiao2_list.index(min(mubiao2_list)[0])
    fliter = []
    for i in result2[addres][0]:
        fliter.append(i[0])
    for i in result2[addres][1]:
        tmp = []
        try:
            for j in i:
                if j != 0:
                    tmp.append(j[0])
                else:
                    tmp.append(j)
            for k in tmp:
                fliter.append(k)
        except:
            fliter.append(i)
    fliter.append(int(result2[addres][2]))
    fliter.append(int(Pn[addres]))
    fliter.append(int(min(mubiao2_list)))
    fliter_list.append(fliter)
    P_list.append(int(Pn[addres]))
    CT +=1

#整体订购方案
t = [fliter1]
for i in fliter_list:
    t.append(i)
ttp = []
for i in suiji_list[-1]:
    for j in i:
        ttp.append(j)
ttp.append('原料对应产能')
ttp.append('原材料费用')
dinggoufangan=pd.DataFrame(t,columns=ttp,index=list(np.linspace(1,24,24))).T
t = []
for i in dinggoufangan.values:
    ttttt = []
    for j in i:
        ttttt.append(int(j))
    t.append(np.array(ttttt))
dinggoufangan = pd.DataFrame(np.array(t),index=dinggoufangan.index.values,columns=dinggoufangan.columns.values)
dinggoufangan.to_excel('订购草案.xlsx')
#dinggoufangan


#转换填表格式
temp7 = pd.read_excel(r'订购草案.xlsx')
t23 = temp7.iloc[0:23,:]
tp = []
for i in np.linspace(0,24,25):
    tp.append(int(i))
t23.columns = tp
tp = []
for i in data1['供应商ID'].values:
    c = [i]
    for j in np.linspace(0,0,24):
        c.append(int(j))
    tp.append(c)
s402 = pd.DataFrame(tp)
ct = 0
for i in t23[0].values:
    index_23 = int(i[-3:])-1
    s402.iloc[index_23,:] = t23.iloc[ct,:]
    ct += 1
s402.to_csv(r'订购草案终.csv')

temp7 = pd.read_excel('订购草案.xlsx')
data3 = pd.read_excel('附件2 近5年8家转运商的相关数据.xlsx')

temp6 = data3.set_index('转运商ID')
tpq = []
for i in np.array(temp6):
    ttp = []
    for j in i:
        if j != 0:
            ttp.append(j*0.01)
        else:
            continue
    tpq.append(ttp)

ct = 1

for i in tpq:
    x = np.linspace(0,len(i),len(i))
    y = list(i)
    fig = plt.figure(figsize=(8,11),dpi=98)
    na=plt.subplot(4,1,1)
    na.plot(x,y,label='转运商{}'.format(ct))
    na.legend()
    ct+=1
    plt.show()
#fig.savefig('zhuanyun.jpg')

#计算转运商信息
tp = []
for i in np.linspace(1,8,8):
    tp.append(int(i))
tpq_mean = []
for i in tpq:
    tpq_mean.append(np.array(i).mean())
plt.figure(dpi=95)
plt.ylabel('平均转运损耗')
plt.xlabel('转运商')
plt.plot(tp,tpq_mean)
plt.savefig('zhunayunpj.jpg')
plt.show()
sh_p = pd.DataFrame([tp,tpq_mean]).T.sort_values(by=1)

#随机转运的转运商选择结果  24
def suiji_zhuanyun(count):
    zy1 = []
    for i in range(0,count):
        zy1.append(int(np.random.randint(1,9,1)))
    return zy1

#随机损耗 1~8
def suiji_sunhao():
    ss = []
    tpq_suiji = []
    for i in tpq:
        tpq_suiji.append(random.sample(list(np.array(i)),1)[0])
    return tpq_suiji

#对应的概率
sjz = suiji_zhuanyun(23)
sh = suiji_sunhao()
temp = []
for i in sjz:
    temp.append(sh[i-1])
c=0
q = []
for i in temp7.values[0:-2]:#.sort_values(by=k-1)[::-1][k-1]
    t = list(i)
    t.append(c)
    q.append(t)
    c+=1
temp1 = pd.DataFrame(q)
pm = sh_p[0].values.copy()
tp111 = [] #记录选中供货商值
tp222 = []  #记录选中供货商和
tp333 = [] #记录供货商名字
tp444 = [] #记录转运商名字
#tp555 = 0  #记录检索到哪一位置供应商了
tp666 = [] #记录供货商供货量
for k in range(2,26):
    ghs_tp = temp1.sort_values(k-1)[::-1].iloc[:,k-1]#供货商数据 从大到小
    ghs_mz = temp1.sort_values(k-1)[::-1].iloc[:,0].values#供货商名字  从大到小
    #zys_tp = temp1.sort_values(k-1)[::-1].iloc[:,-1]#对应损失率
    tp11 = [] #记录选中供货商值
    tp22 = []  #记录选中供货商和
    tp33 = [] #记录供货商名字
    tp44 = [] #记录转运商名字
    #tp55 = 0  #记录检索到哪一位置供应商了
    tp66 = [] #记录供货商供货量
    sjz_t = sjz.copy()
    pm = sh_p[0].values.copy()
    for j in pm:
        #try:
        tp1 = [] #记录选中供货商值
        tp2 = 0  #记录选中供货商和
        tp3 = [] #记录供货商名字
        tp4 = [] #记录转运商名字
        #tp5 = 0  #记录检索到哪一位置供应商了
        tp6 = [] #记录供货商供货量
        count = 0
        yz = []
        for i in ghs_mz:
            try:
                yz.append([i,ghs_tp[count]])
            except:
                yz.append([i,0])
            count+=1
        for i in yz:
            tp2 +=i[1]
            if tp2 <= 6000:
                tp3.append(i[0])
                tp6.append(i[1])
            else:
                tp2 -=i[1]
            tp5+=1
            tp5 = j
            ghs_tp = [x for x in ghs_tp if x not in tp6]
            pm = pm[1:]
            ghs_mz = [x for x in ghs_mz if x not in tp3]
        tp11.append(tp1)
        tp22.append([tp2])
        tp33.append(tp3)
        tp44.append(tp4)
        #tp55.append([tp5])
        tp66.append(tp6)
        #except:
            #continue
    tp111.append(tp11)
    tp222.append([tp22])
    tp333.append(tp33)
    tp444.append(tp44)
    #tp555.append([tp55])
    tp666.append(tp66)

kkk = []
for k in range(24):
    pm = sh_p[0].values.copy()
    t = []
    for j in pm:
        tp = []
        for i in range(8):
            name = tp333[k][i]
            shuju = tp666[k][i]
            tp.append(pd.DataFrame([name,shuju],index=[0,j]).T.set_index(0))
        t.append(tp)#3 6 2 8 4 1 7 5  
                    #0 1 2 3 4 5 6 7  
    kkk.append(pd.concat([t[0][5],t[0][2],t[0][0],t[0][4],t[0][7],t[0][1],t[0][6],t[0][3]],axis=1))
result = pd.concat(kkk,axis=1)
result.to_csv('转运方案2终.csv')

temp7 = pd.read_excel('订购草案.xlsx')
t23 = temp7.iloc[0:23,:]
tp = []
for i in np.linspace(0,24,25):
    tp.append(int(i))
t23.columns = tp
tp = []
for i in data2['供应商ID'].values:
    c = [i]
    for j in np.linspace(0,0,24):
        c.append(int(j))
    tp.append(c)
s402 = pd.DataFrame(tp)
ct = 0
for i in t23[0].values:
    index_23 = int(i[-3:])-1
    s402.iloc[index_23,:] = t23.iloc[ct,:]
    ct += 1
s402.to_csv('转运方案2终终.csv')

#第三问

t = dff1[0:46] #.set_index(0)
t1 = pd.concat([t[t[1] == 'A'],t[t[1] == 'B'],t[t[1] == 'C']])

#取每行平均值
t2 = t1.set_index(0).drop([1],axis=1)
you0 = list((t2 == 0).astype(int).sum(axis=1))
sum_t2 = list(t2.sum(axis=1).values)
fei0 = [240 - i for i in you0]
mean_t2 = np.array(sum_t2)/np.array(fei0)
index = list(t1.index.values)
pj_values = list(mean_t2)
sum_cn = 0
ct = 0
count = 0
pjcn = []
ljcn = []
for i in mean_t2:
    sum_cn = sum_cn + (i/D50[ct])
    ljcn.append(sum_cn)
    pjcn.append((i/D50[ct]))
    if sum_cn < 28200:
        count+=1
        ct+=1
        continue
    else:
        break
gys = t2.index.values[0:len(pjcn)]
cpt = t1[1].values[0:len(gys)]
gyswz = pd.DataFrame([gys,cpt,pjcn,ljcn],index=['供应商ID','材料分类','供应商平均产能','累计产能']).T.set_index('供应商ID')
#gyswz

#看看原始31家的供货商供货量图
'''
#for i in gys:
    #if i in tubian_list
tpw = []
for i in gys:
    tpw.append(np.array(t2.T[i]))
biao31 = pd.DataFrame(tpw)
count = 0
for j in tpw: 
    y = tpw[count]
    x = biao31.columns.values
    plt.figure()
    plt.plot(x,y)
    plt.xlabel(gys[count])
    plt.show()
    time.sleep(2)
    count+=1
'''

# 分步式考虑

#第一周随机序列
gys = list(gyswz.index.values)
def first_suiji_3():
    #31家分类
    tubian_id = ['S140','S307','S395','S139']
    yichang_id = ['S151','S308','S330','S348','S201','S143']
    teshu_id = ['S108']
    wending_id = []
    wending_type=[]
    
    for i in gys:
        if (i not in yichang_id )and (i not in tubian_id) and (i not in teshu_id):
            wending_id.append(i)
            wending_type.append(gyswz.loc[i]['材料分类'])

    #异常值取行与去除异常值后加入稳定值
    yic = []
    yic_type = []
    for j in yichang_id:
        addres = gys.index(j)
        yic.append(np.array(biao31.loc[addres]))
        yic_type.append(gyswz.loc[j]['材料分类'])
    for k in yic:
        for m in k:
            if m > 2500:
                addres = list(k).index(m)
                k[addres] = np.median(k)#异常值使用中位数
    yic_suiji_xulie = []
    for t in yic:
        yic_max=t.max()
        yic_min=t.min()
        yic_suiji_xulie.append(np.random.randint(yic_max*0.65,yic_max, 1))


    #特殊值取行
    addres = gys.index(teshu_id[0])
    tes = np.array(biao31.loc[addres])

    #稳定值取行
    wen = []
    for j in wending_id:
        addres = gys.index(j)
        wen.append(np.array(biao31.loc[addres]))

    #稳定值取随机序列
    wen_up = []
    wen_down = []
    for i in wen:
        wen_up.append(i.max())
        wen_down.append(i.max()*0.65)

    wen_suiji_xulie = []
    for i in wen_up:
        addres = wen_up.index(i)
        wen_suiji_xulie.append(np.random.randint(wen_down[addres],i, 1))

    #总的稳定值随机序列
    wenzong = wen_suiji_xulie+yic_suiji_xulie
    wenzong_type = wending_type+yic_type
    for i in yichang_id:
        wending_id.append(i)
    
    #特殊值取随机序列后加上最大值
    text = []
    tq = []
    te_type = gyswz.loc[teshu_id[0]]['材料分类']
    for i in tes:
        if i != tes[list(tes).index(tes.max())]:
            text.append(i)
            tq.append(i)
        else:
            text.append('s')
    text[text.index('s')] = np.array(tq).mean()
    tes_up = np.array(text).max()
    tes_down = np.array(text).min()
    te_suiji_xulie = np.random.randint(tes_up*0.65,tes_up, 24)
    tes_max = list(tes).index(tes.max())
    #te_suiji_xulie[random.randint(0,24)] = tes_max
    #te_suiji = te_suiji_xulie[random.randint(0,23)]


    #三个突变值取最大值的两个周
    tb = []
    tb_type = []
    #for i in tubian_id:
    #    addres = gys.index(i)
    #    tubian.append(np.array(biao31.loc[addres]))
    for j in tubian_id:
        tb_type.append(gyswz.loc[j]['材料分类'])
        tp = np.linspace(0,0,24)
        addres = np.random.randint(0,24, 2)
        if (addres[0] == 0) or (addres[1] == 23): 
            tb.append(np.random.randint(5999,6000, 1))
        else:
            tb.append(0)

    return wenzong,tb,np.array([6000]),wenzong_type,tb_type,te_type,[wending_id,tubian_id,teshu_id]

#后面几周随机序列
def suiji_3():
    #31家分类
    tubian_id = ['S140','S307','S395','S139']
    yichang_id = ['S151','S308','S330','S348','S201','S143']
    teshu_id = ['S108']
    wending_id = []
    wending_type=[]
    for i in gys:
        if (i not in yichang_id )and (i not in tubian_id) and (i not in teshu_id):
            wending_id.append(i)
            wending_type.append(gyswz.loc[i]['材料分类'])

    #异常值取行与去除异常值后加入稳定值
    yic = []
    yic_type = []
    for j in yichang_id:
        addres = gys.index(j)
        yic.append(np.array(biao31.loc[addres]))
        yic_type.append(gyswz.loc[j]['材料分类'])
    for k in yic:
        for m in k:
            if m > 2500:
                addres = list(k).index(m)
                k[addres] = np.median(k)#异常值使用中位数
    yic_suiji_xulie = []
    for t in yic:
        yic_max=t.max()
        yic_min=t.min()
        yic_suiji_xulie.append(np.random.randint(yic_max*0.2,yic_max*0.7, 1))


    #特殊值取行
    addres = gys.index(teshu_id[0])
    tes = np.array(biao31.loc[addres])

    #稳定值取行
    wen = []
    for j in wending_id:
        addres = gys.index(j)
        wen.append(np.array(biao31.loc[addres]))

    #稳定值取随机序列
    wen_up = []
    wen_down = []
    for i in wen:
        wen_up.append(i.max()*0.7)
        wen_down.append(i.max()*0.2)

    wen_suiji_xulie = []
    for i in wen_up:
        addres = wen_up.index(i)
        wen_suiji_xulie.append(np.random.randint(wen_down[addres],i, 1))

    #总的稳定值随机序列
    wenzong = wen_suiji_xulie+yic_suiji_xulie
    wenzong_type = wending_type+yic_type
    for i in yichang_id:
        wending_id.append(i)
    
    #特殊值取随机序列
    text = []
    tq = []
    te_type = gyswz.loc[teshu_id[0]]['材料分类']
    for i in tes:
        if i != tes[list(tes).index(tes.max())]:
            text.append(i)
            tq.append(i)
        else:
            text.append('s')
    text[text.index('s')] = np.array(tq).mean()
    tes_up = np.array(text).max()
    tes_down = np.array(text).min()
    te_suiji_xulie = np.random.randint(500,1000, 24)
    
    tes_max = list(tes).index(tes.max())
    #te_suiji_xulie[random.randint(0,24)] = tes_max
    te_suiji = te_suiji_xulie[random.randint(0,23)]


    #三个突变值取最大值的两个周
    tb = []
    tb_type = []
    #for i in tubian_id:
    #    addres = gys.index(i)
    #    tubian.append(np.array(biao31.loc[addres]))
    for j in tubian_id:
        tb_type.append(gyswz.loc[j]['材料分类'])
        tp = np.linspace(0,0,24)
        addres = np.random.randint(0,24, 2)
        if (addres[0] == 0) or (addres[1] == 23): 
            tb.append(np.random.randint(2000,6000, 1))
        else:
            tb.append(0)

    return wenzong,tb,np.array(te_suiji),wenzong_type,tb_type,te_type,[wending_id,tubian_id,teshu_id]

#随机转运的转运商选择
def suiji_zhuanyun_3():
    zy1 = []
    for i in range(0,31):
        zy1.append(int(np.random.randint(1,9,1)))
    return zy1

#第一周
result1 = []
mubiao1_list = []
P1 = []
for i in range(1000):
    suiji_list = first_suiji_3()
    count +=0
    t2 = []
    t2.append(int(suiji_list[2]))
    suiji_data = suiji_list[0]+suiji_list[1]+t2
    suiji_type = suiji_list[3]+suiji_list[4]
    suiji_type.append(suiji_list[5])
    gh = pd.DataFrame([suiji_type,suiji_data]).T
    A = gh[gh[0] == 'A'][1].values.sum()
    B = gh[gh[0] == 'B'][1].values.sum()
    C = gh[gh[0] == 'C'][1].values.sum()
    if ((A+B+C) <= 48000) and ((A/0.6+B/0.66+C/0.72)>=(28200*2)):
        P1.append(A/0.6+B/0.66+C/0.72)
        mubiao1 = 1.2*A+1.1*B+C
        mubiao1_list.append(mubiao1)
        result1.append(suiji_list)
    else:
        continue
addres = mubiao1_list.index(min(mubiao1_list)[0])
fliter = []
for i in result1[addres][0]:
    fliter.append(int(i))
for i in result1[addres][1]:
    fliter.append(i)
fliter.append(int(result1[addres][2]))
fliter.append(int(P1[addres]))
fliter.append(int(min(mubiao1_list)))
fliter1 = fliter.copy()

#后面几周
#筛选对应材料的供货商
p = int(P1[addres])
fliter_list = []
P_list = [int(P1[addres])]
CT = 0
for i in range(2,25):
    result2 = []
    mubiao2_list = []
    Pn = []
    p=P_list[CT]
    for i in range(200):
        try:
            suiji_list = suiji_3()
            count +=0
            t2 = []
            t2.append(int(suiji_list[2]))
            suiji_data = suiji_list[0]+suiji_list[1]+t2
            suiji_type = suiji_list[3]+suiji_list[4]
            suiji_type.append(suiji_list[5])
            gh = pd.DataFrame([suiji_type,suiji_data]).T
            A = gh[gh[0] == 'A'][1].values.sum()
            B = gh[gh[0] == 'B'][1].values.sum()
            C = gh[gh[0] == 'C'][1].values.sum()
            if ((A+B+C) <= 48000) and ((A/0.6+B/0.66+C/0.72)>=(28200*2)-p):
                Pn.append(A/0.6+B/0.66+C/0.72)
                mubiao2 = 1.2*A+1.1*B+C
                mubiao2_list.append(mubiao1)
                result2.append(suiji_list)
            else:
                continue
        except:
            continue
    addres = mubiao2_list.index(min(mubiao2_list)[0])
    fliter = []
    for i in result2[addres][0]:
        fliter.append(i[0])
    for i in result2[addres][1]:
        tmp = []
        try:
            for j in i:
                if j != 0:
                    tmp.append(j[0])
                else:
                    tmp.append(j)
            for k in tmp:
                fliter.append(k)
        except:
            fliter.append(i)
    fliter.append(int(result2[addres][2]))
    fliter.append(int(Pn[addres]))
    fliter.append(int(min(mubiao2_list)))
    fliter_list.append(fliter)
    P_list.append(int(Pn[addres]))
    CT +=1


#整体订购方案
t = [fliter1]
for i in fliter_list:
    t.append(i)
ttp = []
for i in suiji_list[-1]:
    for j in i:
        ttp.append(j)
ttp.append('原料对应产能')
ttp.append('原材料费用')
dinggoufangan=pd.DataFrame(t,columns=ttp,index=list(np.linspace(1,24,24))).T
t = []
for i in dinggoufangan.values:
    ttttt = []
    for j in i:
        ttttt.append(int(j))
    t.append(np.array(ttttt))
dinggoufangan3 = pd.DataFrame(np.array(t),index=dinggoufangan.index.values,columns=dinggoufangan.columns.values)
dinggoufangan3.to_csv('订购方案')

temp7 = dinggoufangan3.copy()
data3 = pd.read_excel('附件2 近5年8家转运商的相关数据.xlsx')

temp6 = data3.set_index('转运商ID')
tpq = []
for i in np.array(temp6):
    ttp = []
    for j in i:
        if j != 0:
            ttp.append(j*0.01)
        else:
            continue
    tpq.append(ttp)

ct = 1

for i in tpq:
    x = np.linspace(0,len(i),len(i))
    y = list(i)
    fig = plt.figure(figsize=(8,11),dpi=98)
    na=plt.subplot(4,1,1)
    na.plot(x,y,label='转运商{}'.format(ct))
    na.legend()
    ct+=1
    plt.show()
#fig.savefig('zhuanyun.jpg')

#计算转运商信息
tp = []
for i in np.linspace(1,8,8):
    tp.append(int(i))
tpq_mean = []
for i in tpq:
    tpq_mean.append(np.array(i).mean())
plt.figure(dpi=95)
plt.ylabel('平均转运损耗')
plt.xlabel('转运商')
plt.plot(tp,tpq_mean)
plt.savefig('zhunayunpj.jpg')
plt.show()
sh_p = pd.DataFrame([tp,tpq_mean]).T.sort_values(by=1)
#随机转运的转运商选择结果  24
def suiji_zhuanyun(count):
    zy1 = []
    for i in range(0,count):
        zy1.append(int(np.random.randint(1,9,1)))
    return zy1

#随机损耗 1~8
def suiji_sunhao():
    ss = []
    tpq_suiji = []
    for i in tpq:
        tpq_suiji.append(random.sample(list(np.array(i)),1)[0])
    return tpq_suiji

#对应的概率
sjz = suiji_zhuanyun(23)
sh = suiji_sunhao()
temp = []
for i in sjz:
    temp.append(sh[i-1])
c=0
q = []
for i in temp7.values[0:-2]:#.sort_values(by=k-1)[::-1][k-1]
    t = list(i)
    t.append(c)
    q.append(t)
    c+=1
temp1 = pd.DataFrame(q)
pm = sh_p[0].values.copy()
tp111 = [] #记录选中供货商值
tp222 = []  #记录选中供货商和
tp333 = [] #记录供货商名字
tp444 = [] #记录转运商名字
#tp555 = 0  #记录检索到哪一位置供应商了
tp666 = [] #记录供货商供货量
for k in range(2,26):
    ghs_tp = temp1.sort_values(k-1)[::-1].iloc[:,k-1]#供货商数据 从大到小
    ghs_mz = temp1.sort_values(k-1)[::-1].iloc[:,0].values#供货商名字  从大到小
    #zys_tp = temp1.sort_values(k-1)[::-1].iloc[:,-1]#对应损失率
    tp11 = [] #记录选中供货商值
    tp22 = []  #记录选中供货商和
    tp33 = [] #记录供货商名字
    tp44 = [] #记录转运商名字
    #tp55 = 0  #记录检索到哪一位置供应商了
    tp66 = [] #记录供货商供货量
    sjz_t = sjz.copy()
    pm = sh_p[0].values.copy()
    for j in pm:
        #try:
        tp1 = [] #记录选中供货商值
        tp2 = 0  #记录选中供货商和
        tp3 = [] #记录供货商名字
        tp4 = [] #记录转运商名字
        #tp5 = 0  #记录检索到哪一位置供应商了
        tp6 = [] #记录供货商供货量
        count = 0
        yz = []
        for i in ghs_mz:
            try:
                yz.append([i,ghs_tp[count]])
            except:
                yz.append([i,0])
            count+=1
        for i in yz:
            tp2 +=i[1]
            if tp2 <= 6000:
                tp3.append(i[0])
                tp6.append(i[1])
            else:
                tp2 -=i[1]
            tp5+=1
            tp5 = j
            ghs_tp = [x for x in ghs_tp if x not in tp6]
            pm = pm[1:]
            ghs_mz = [x for x in ghs_mz if x not in tp3]
        tp11.append(tp1)
        tp22.append([tp2])
        tp33.append(tp3)
        tp44.append(tp4)
        #tp55.append([tp5])
        tp66.append(tp6)
        #except:
            #continue
    tp111.append(tp11)
    tp222.append([tp22])
    tp333.append(tp33)
    tp444.append(tp44)
    #tp555.append([tp55])
    tp666.append(tp66)

kkk = []
for k in range(24):
    pm = sh_p[0].values.copy()
    t = []
    for j in pm:
        tp = []
        for i in range(8):
            name = tp333[k][i]
            shuju = tp666[k][i]
            tp.append(pd.DataFrame([name,shuju],index=[0,j]).T.set_index(0))
        t.append(tp)#3 6 2 8 4 1 7 5  
                    #0 1 2 3 4 5 6 7  
    kkk.append(pd.concat([t[0][5],t[0][2],t[0][0],t[0][4],t[0][7],t[0][1],t[0][6],t[0][3]],axis=1))
result = pd.concat(kkk,axis=1)
result.to_csv('转运方案3同终.csv')

#格式转换
temp7 = result.copy()
t23 = temp7.iloc[0:23,:]
tp = []
for i in np.linspace(0,24,25):
    tp.append(int(i))
t23.columns = tp
tp = []
for i in data2['供应商ID'].values:
    c = [i]
    for j in np.linspace(0,0,24):
        c.append(int(j))
    tp.append(c)
s402 = pd.DataFrame(tp)
ct = 0
for i in t23[0].values:
    index_23 = int(i[-3:])-1
    s402.iloc[index_23,:] = t23.iloc[ct,:]
    ct += 1
s402.to_csv('转运方案3终终.csv')

#分步目标值
cs_type = list(gyswz['材料分类'].values)
suihaobiao = []
for i in range(24):
    suihaobiao.append(suiji_sunhao())
ck = suihaobiao[0]
for j in suihaobiao[1:]:
    ck = ck + j
roworder = 0
rowlist = []
for row in fenbu.fillna(0).itertuples():
    ttp = []
    temp = np.array(row[1:])*np.array(ck)+np.array(row[1:])
    roworder+=1
    rowlist.append(temp)
count = 0
tptp = []
for i in cs_type:
    if i == 'A':
        tptp.append(rowlist[count].sum()*1.2)
    elif i == 'B':
        tptp.append(rowlist[count].sum()*1.1)
    else:
        tptp.append(rowlist[count].sum())
    count+=1
#min(np.array(tptp))
sum(tptp)


# 同步式考虑

#第一周随机序列
gys = list(gyswz.index.values)
def first_suiji_3():
    #31家分类
    tubian_id = ['S140','S307','S395','S139']
    yichang_id = ['S151','S308','S330','S348','S201','S143']
    teshu_id = ['S108']
    wending_id = []
    wending_type=[]
    
    for i in gys:
        if (i not in yichang_id )and (i not in tubian_id) and (i not in teshu_id):
            wending_id.append(i)
            wending_type.append(gyswz.loc[i]['材料分类'])

    #异常值取行与去除异常值后加入稳定值
    yic = []
    yic_type = []
    for j in yichang_id:
        addres = gys.index(j)
        yic.append(np.array(biao31.loc[addres]))
        yic_type.append(gyswz.loc[j]['材料分类'])
    for k in yic:
        for m in k:
            if m > 2500:
                addres = list(k).index(m)
                k[addres] = np.median(k)#异常值使用中位数
    yic_suiji_xulie = []
    for t in yic:
        yic_max=t.max()
        yic_min=t.min()
        yic_suiji_xulie.append(np.random.randint(yic_max*0.65,yic_max, 1))


    #特殊值取行
    addres = gys.index(teshu_id[0])
    tes = np.array(biao31.loc[addres])

    #稳定值取行
    wen = []
    for j in wending_id:
        addres = gys.index(j)
        wen.append(np.array(biao31.loc[addres]))

    #稳定值取随机序列
    wen_up = []
    wen_down = []
    for i in wen:
        wen_up.append(i.max())
        wen_down.append(i.max()*0.65)

    wen_suiji_xulie = []
    for i in wen_up:
        addres = wen_up.index(i)
        wen_suiji_xulie.append(np.random.randint(wen_down[addres],i, 1))

    #总的稳定值随机序列
    wenzong = wen_suiji_xulie+yic_suiji_xulie
    wenzong_type = wending_type+yic_type
    for i in yichang_id:
        wending_id.append(i)
    
    #特殊值取随机序列后加上最大值
    text = []
    tq = []
    te_type = gyswz.loc[teshu_id[0]]['材料分类']
    for i in tes:
        if i != tes[list(tes).index(tes.max())]:
            text.append(i)
            tq.append(i)
        else:
            text.append('s')
    text[text.index('s')] = np.array(tq).mean()
    tes_up = np.array(text).max()
    tes_down = np.array(text).min()
    te_suiji_xulie = np.random.randint(tes_up*0.65,tes_up, 24)
    tes_max = list(tes).index(tes.max())
    #te_suiji_xulie[random.randint(0,24)] = tes_max
    #te_suiji = te_suiji_xulie[random.randint(0,23)]


    #三个突变值取最大值的两个周
    tb = []
    tb_type = []
    #for i in tubian_id:
    #    addres = gys.index(i)
    #    tubian.append(np.array(biao31.loc[addres]))
    for j in tubian_id:
        tb_type.append(gyswz.loc[j]['材料分类'])
        tp = np.linspace(0,0,24)
        addres = np.random.randint(0,24, 2)
        if (addres[0] == 0) or (addres[1] == 23): 
            tb.append(np.random.randint(5999,6000, 1))
        else:
            tb.append(0)

    return wenzong,tb,np.array([6000]),wenzong_type,tb_type,te_type,[wending_id,tubian_id,teshu_id]

#后面几周随机序列
def suiji_3():
    #31家分类
    tubian_id = ['S140','S307','S395','S139']
    yichang_id = ['S151','S308','S330','S348','S201','S143']
    teshu_id = ['S108']
    wending_id = []
    wending_type=[]
    for i in gys:
        if (i not in yichang_id )and (i not in tubian_id) and (i not in teshu_id):
            wending_id.append(i)
            wending_type.append(gyswz.loc[i]['材料分类'])

    #异常值取行与去除异常值后加入稳定值
    yic = []
    yic_type = []
    for j in yichang_id:
        addres = gys.index(j)
        yic.append(np.array(biao31.loc[addres]))
        yic_type.append(gyswz.loc[j]['材料分类'])
    for k in yic:
        for m in k:
            if m > 2500:
                addres = list(k).index(m)
                k[addres] = np.median(k)#异常值使用中位数
    yic_suiji_xulie = []
    for t in yic:
        yic_max=t.max()
        yic_min=t.min()
        yic_suiji_xulie.append(np.random.randint(yic_max*0.2,yic_max*0.7, 1))


    #特殊值取行
    addres = gys.index(teshu_id[0])
    tes = np.array(biao31.loc[addres])

    #稳定值取行
    wen = []
    for j in wending_id:
        addres = gys.index(j)
        wen.append(np.array(biao31.loc[addres]))

    #稳定值取随机序列
    wen_up = []
    wen_down = []
    for i in wen:
        wen_up.append(i.max()*0.7)
        wen_down.append(i.max()*0.2)

    wen_suiji_xulie = []
    for i in wen_up:
        addres = wen_up.index(i)
        wen_suiji_xulie.append(np.random.randint(wen_down[addres],i, 1))

    #总的稳定值随机序列
    wenzong = wen_suiji_xulie+yic_suiji_xulie
    wenzong_type = wending_type+yic_type
    for i in yichang_id:
        wending_id.append(i)
    
    #特殊值取随机序列
    text = []
    tq = []
    te_type = gyswz.loc[teshu_id[0]]['材料分类']
    for i in tes:
        if i != tes[list(tes).index(tes.max())]:
            text.append(i)
            tq.append(i)
        else:
            text.append('s')
    text[text.index('s')] = np.array(tq).mean()
    tes_up = np.array(text).max()
    tes_down = np.array(text).min()
    te_suiji_xulie = np.random.randint(500,1000, 24)
    
    tes_max = list(tes).index(tes.max())
    #te_suiji_xulie[random.randint(0,24)] = tes_max
    te_suiji = te_suiji_xulie[random.randint(0,23)]


    #三个突变值取最大值的两个周
    tb = []
    tb_type = []
    #for i in tubian_id:
    #    addres = gys.index(i)
    #    tubian.append(np.array(biao31.loc[addres]))
    for j in tubian_id:
        tb_type.append(gyswz.loc[j]['材料分类'])
        tp = np.linspace(0,0,24)
        addres = np.random.randint(0,24, 2)
        if (addres[0] == 0) or (addres[1] == 23): 
            tb.append(np.random.randint(2000,6000, 1))
        else:
            tb.append(0)

    return wenzong,tb,np.array(te_suiji),wenzong_type,tb_type,te_type,[wending_id,tubian_id,teshu_id]

#随机转运的转运商选择
def suiji_zhuanyun_3():
    zy1 = []
    for i in range(0,31):
        zy1.append(int(np.random.randint(1,9,1)))
    return zy1

min_2 = []
dinggou_list3 = []
zhunayun_list3 = []
for mmm in range(8):
    try:
        #第一周
        result1 = []
        mubiao1_list = []
        P1 = []
        for i in range(30):
            suiji_list = first_suiji_3()
            count +=0
            t2 = []
            t2.append(int(suiji_list[2]))
            suiji_data = suiji_list[0]+suiji_list[1]+t2
            suiji_type = suiji_list[3]+suiji_list[4]
            suiji_type.append(suiji_list[5])
            gh = pd.DataFrame([suiji_type,suiji_data]).T
            A = gh[gh[0] == 'A'][1].values.sum()
            B = gh[gh[0] == 'B'][1].values.sum()
            C = gh[gh[0] == 'C'][1].values.sum()
            if ((A+B+C) <= 48000) and ((A/0.6+B/0.66+C/0.72)>=(28200*2)):
                P1.append(A/0.6+B/0.66+C/0.72)
                mubiao1 = 1.2*A+1.1*B+C
                mubiao1_list.append(mubiao1)
                result1.append(suiji_list)
            else:
                continue
        addres = mubiao1_list.index(min(mubiao1_list)[0])
        fliter = []
        for i in result1[addres][0]:
            fliter.append(int(i))
        for i in result1[addres][1]:
            fliter.append(i)
        fliter.append(int(result1[addres][2]))
        fliter.append(int(P1[addres]))
        fliter.append(int(min(mubiao1_list)))
        fliter1 = fliter.copy()

        #后面几周
        #筛选对应材料的供货商
        p = int(P1[addres])
        fliter_list = []
        P_list = [int(P1[addres])]
        CT = 0
        for i in range(2,25):
            result2 = []
            mubiao2_list = []
            Pn = []
            p=P_list[CT]
            for i in range(150):
                suiji_list = suiji_3()
                count +=0
                t2 = []
                t2.append(int(suiji_list[2]))
                suiji_data = suiji_list[0]+suiji_list[1]+t2
                suiji_type = suiji_list[3]+suiji_list[4]
                suiji_type.append(suiji_list[5])
                gh = pd.DataFrame([suiji_type,suiji_data]).T
                A = gh[gh[0] == 'A'][1].values.sum()
                B = gh[gh[0] == 'B'][1].values.sum()
                C = gh[gh[0] == 'C'][1].values.sum()
                if ((A+B+C) <= 48000) and ((A/0.6+B/0.66+C/0.72)>=(28200*2)-p):
                    Pn.append(A/0.6+B/0.66+C/0.72)
                    mubiao2 = 1.2*A+1.1*B+C
                    mubiao2_list.append(mubiao1)
                    result2.append(suiji_list)
                else:
                    continue
            addres = mubiao2_list.index(min(mubiao2_list)[0])
            fliter = []
            for i in result2[addres][0]:
                fliter.append(i[0])
            for i in result2[addres][1]:
                tmp = []
                try:
                    for j in i:
                        if j != 0:
                            tmp.append(j[0])
                        else:
                            tmp.append(j)
                    for k in tmp:
                        fliter.append(k)
                except:
                    fliter.append(i)
            fliter.append(int(result2[addres][2]))
            fliter.append(int(Pn[addres]))
            fliter.append(int(min(mubiao2_list)))
            fliter_list.append(fliter)
            P_list.append(int(Pn[addres]))
            CT +=1


        #整体订购方案
        t = [fliter1]
        for i in fliter_list:
            t.append(i)
        ttp = []
        for i in suiji_list[-1]:
            for j in i:
                ttp.append(j)
        ttp.append('原料对应产能')
        ttp.append('原材料费用')
        dinggoufangan=pd.DataFrame(t,columns=ttp,index=list(np.linspace(1,24,24))).T
        t = []
        for i in dinggoufangan.values:
            ttttt = []
            for j in i:
                ttttt.append(int(j))
            t.append(np.array(ttttt))
        dinggoufangan3 = pd.DataFrame(np.array(t),index=dinggoufangan.index.values,columns=dinggoufangan.columns.values)
        dinggou_list3.append(dinggoufangan3)

        #转运方案
        t31 = dinggoufangan3[0:31]
        result2 = []
        for k in range(24):
            index = list(dinggoufangan3[0:31].index.values)
            ttpp = list(t31.iloc[:,k].values)
            ss_ghs = pd.DataFrame([index,ttpp]).T.sort_values(by=1).iloc[::-1]
            ghsmd = ss_ghs[0].values #供货商名单
            zysmdv = sh_p[0].values  #转运商名字
            ghsmdv= ss_ghs[1].values  #供货商从大到小排的供货量
            ghslist = []
            for i in zysmdv:
                tp = 0
                ghsl = []
                ct = 0
                ghsmdv_tp = []
                for j in ghsmdv:
                    tp += j
                    if tp <= 6000:
                        ghsl.append(ghsmd[ct])
                        ghsmdv_tp.append(j)
                    ct +=1
                ghslist.append(ghsl)
                ghsmd = [x for x in ghsmd if x not in ghsl]
                zysmdv = zysmdv[1:]
                ghsmdv = [x for x in ghsmdv if x not in ghsmdv_tp]
            count = 0
            ys = pd.DataFrame(index = ss_ghs[0].values,columns=[1,2,3,4,5,6,7,8])
            count2 = 0
            for i in ghslist:
                for j in i:
                    ys.loc[j][(sh_p[0].values)[count]] = ss_ghs[1].values[count2]
                    count2 +=1
                count +=1
            result2.append(ys)
        chushi = result2[0]
        for i in result2[1:]:
            chushi = pd.concat([chushi,i],axis=1)
        zhunayun_list3.append(chushi)
        #chushi

        #一次循环目标值
        cs_type = list(gyswz['材料分类'].values)
        suihaobiao = []
        for i in range(24):
            suihaobiao.append(suiji_sunhao())
        ck = suihaobiao[0]
        for j in suihaobiao[1:]:
            ck = ck + j
        roworder = 0
        rowlist = []
        for row in chushi.fillna(0).itertuples():
            ttp = []
            temp = np.array(row[1:])*np.array(ck)+np.array(row[1:])
            roworder+=1
            rowlist.append(temp)
        count = 0
        tptp = []
        for i in cs_type:
            if i == 'A':
                tptp.append(rowlist[count].sum()*1.2)
            elif i == 'B':
                tptp.append(rowlist[count].sum()*1.1)
            else:
                tptp.append(rowlist[count].sum())
            count+=1
        min_2.append(sum(tptp))
    except:
        pass

address = min_2.index(min(min_2))

dinggou_list3[address].to_csv('wenti3_ghs.csv')

zhunayun_list3[address].to_csv('wenti3_zys.csv')

#第四题

df4_data = df1.sort_values(by=[0],ascending=False)#评分表
tptp = []
for i in list(df4_data.index):
    tptp.append(pd.DataFrame(list(data2.iloc[i])).T)
dff4 = pd.concat(tptp)#评分表对应供货商数据
df4_std = []
for i in np.array(dff4.loc[:,2:]):
    df4_std.append(np.array(i).std())
#df4_data

#构建所有数据表
count = 0
tp = []
for i in dff4.values:
    t = list(i)
    t.append(df4_data.values[count][0])
    t.append(df4_std[count])
    count += 1
    tp.append(t)
origin4 = pd.DataFrame(tp)
#data4 = origin4.loc[:,2:241]
#origin4

#筛选突变进行平稳化处理
x4 = np.linspace(0,402,402)
fc4 = origin4[243].values
adr = []
count = 0
for i in fc4:
    if i > 1300:#取出突变型
        adr.append(count)
    count +=1

#max --->  rand(0.66m~0.33m)/(m/6000)
ttp = []
for i in adr:
    ttp.append(origin4.iloc[i])
cn = pd.DataFrame(ttp).iloc[:,2:242]
roworder = 0
ttp = []
max_l4 = []
for row in np.array(cn):
    max_l4.append(row.max())
cout = 0
tp = []
for row in np.array(cn):
    ct = 0
    for i in row:
        if i > 6000:
            row[ct] = random.uniform(0.6*max_l4[cout],max_l4[cout])/(max_l4[cout]/6000)
        ct +=1
    cout +=1
    tp.append(row)
values_t = origin4.iloc[:,2:242]    
#values_t

#处理后的数据表
count = 0
tpv = []
for i in np.array(values_t):
    ct = 0
    if count in cn.index.values:
        tpv.append(tp[ct])
        ct +=1
    else:
        tpv.append(i)
    count+=1
temp = pd.DataFrame(tpv,index=values_t.index.values,columns=values_t.columns.values)
#temp

ghs4 = origin4.copy()
#处理类型成本
Dtype402 = []
for i in np.array(ghs4):
    Dtype402.append(i[1])
Ddata3 = [0.6,0.66,0.72]
D402 = []
for i in Dtype402:
    if i == 'A':
        D402.append(Ddata3[0])
    elif i == 'B':
        D402.append(Ddata3[1])
    elif i == 'C':
        D402.append(Ddata3[2])
    else:
        continue

#取每行平均值
t4 = ghs4.set_index(0).drop([1],axis=1)
you0 = list((t4 == 0).astype(int).sum(axis=1))
sum_t4 = list(t4.sum(axis=1).values)
fei0 = [240 - i for i in you0]
mean_t4 = np.array(sum_t4)/np.array(fei0)
index = list(t1.index.values)
pj_values = list(mean_t4*1.14)
ghs4.iloc[:,242] = pj_values
ghs4 = ghs4.sort_values(by=242)[::-1]
#ghs4 = pd.concat([ghs4[ghs4[1] == 'A'],ghs4[ghs4[1] == 'B'],ghs4[ghs4[1] == 'C']])
#ghs4

sum_cn = 0
ct = 0
count = 0
pjcn = []
ljcn = []
for i in pj_values:
    sum_cn = sum_cn + i
    ljcn.append(sum_cn)
    pjcn.append(i)
    if sum_cn < 48000:
        count+=1
        ct+=1
        continue
    else:
        break
gys = t4.index.values[0:len(pjcn)]
cpt = ghs4[1].values[0:len(gys)]
gyslim = pd.DataFrame([gys,cpt,pjcn,ljcn],index=['供应商ID','材料分类','供应商平均供应能力','累计供应能力']).T.set_index('供应商ID')
#gyslim = pd.concat([gyslim[gyslim['材料分类'] == 'A'],gyslim[gyslim['材料分类'] == 'B'],gyslim[gyslim['材料分类'] == 'C']])
gyslim.to_csv('重要参数4.csv')
#14/15 6/5

def onew():
    while True:
        one_w = []
        for i in pjcn:
            a = random.uniform((i*0.8),(i*1.3))
            one_w.append(a)
        if sum(one_w) > 48000:
            return one_w

#概率表
tq = []
for i in range(24):
    for j in suiji_sunhao():
        tq.append(j)

kb = []
#订购方案
tp = []
for i in range(24):
    tp.append(onew())
t286 = pd.DataFrame(tp,columns=gyslim.index.values).T
t286.to_csv('订购4.csv')

temp7 = pd.read_csv('订购4.csv')
data3 = pd.read_excel('附件2 近5年8家转运商的相关数据.xlsx')

temp6 = data3.set_index('转运商ID')
tpq = []
for i in np.array(temp6):
    ttp = []
    for j in i:
        if j != 0:
            ttp.append(j*0.01)
        else:
            continue
    tpq.append(ttp)

ct = 1

for i in tpq:
    x = np.linspace(0,len(i),len(i))
    y = list(i)
    fig = plt.figure(figsize=(8,11),dpi=98)
    na=plt.subplot(4,1,1)
    na.plot(x,y,label='转运商{}'.format(ct))
    na.legend()
    ct+=1
    plt.show()
#fig.savefig('zhuanyun.jpg')

#计算转运商信息
tp = []
for i in np.linspace(1,8,8):
    tp.append(int(i))
tpq_mean = []
for i in tpq:
    tpq_mean.append(np.array(i).mean())
plt.figure(dpi=95)
plt.ylabel('平均转运损耗')
plt.xlabel('转运商')
plt.plot(tp,tpq_mean)
plt.savefig('zhunayunpj.jpg')
plt.show()
sh_p = pd.DataFrame([tp,tpq_mean]).T.sort_values(by=1)
#随机转运的转运商选择结果  24
def suiji_zhuanyun(count):
    zy1 = []
    for i in range(0,count):
        zy1.append(int(np.random.randint(1,9,1)))
    return zy1

#随机损耗 1~8
def suiji_sunhao():
    ss = []
    tpq_suiji = []
    for i in tpq:
        tpq_suiji.append(random.sample(list(np.array(i)),1)[0])
    return tpq_suiji

#对应的概率
sjz = suiji_zhuanyun(23)
sh = suiji_sunhao()
temp = []
for i in sjz:
    temp.append(sh[i-1])
c=0
q = []
for i in temp7.values[0:-2]:#.sort_values(by=k-1)[::-1][k-1]
    t = list(i)
    t.append(c)
    q.append(t)
    c+=1
temp1 = pd.DataFrame(q)
pm = sh_p[0].values.copy()
tp111 = [] #记录选中供货商值
tp222 = []  #记录选中供货商和
tp333 = [] #记录供货商名字
tp444 = [] #记录转运商名字
#tp555 = 0  #记录检索到哪一位置供应商了
tp666 = [] #记录供货商供货量
for k in range(2,26):
    ghs_tp = temp1.sort_values(k-1)[::-1].iloc[:,k-1]#供货商数据 从大到小
    ghs_mz = temp1.sort_values(k-1)[::-1].iloc[:,0].values#供货商名字  从大到小
    #zys_tp = temp1.sort_values(k-1)[::-1].iloc[:,-1]#对应损失率
    tp11 = [] #记录选中供货商值
    tp22 = []  #记录选中供货商和
    tp33 = [] #记录供货商名字
    tp44 = [] #记录转运商名字
    #tp55 = 0  #记录检索到哪一位置供应商了
    tp66 = [] #记录供货商供货量
    sjz_t = sjz.copy()
    pm = sh_p[0].values.copy()
    for j in pm:
        #try:
        tp1 = [] #记录选中供货商值
        tp2 = 0  #记录选中供货商和
        tp3 = [] #记录供货商名字
        tp4 = [] #记录转运商名字
        #tp5 = 0  #记录检索到哪一位置供应商了
        tp6 = [] #记录供货商供货量
        count = 0
        yz = []
        for i in ghs_mz:
            try:
                yz.append([i,ghs_tp[count]])
            except:
                yz.append([i,0])
            count+=1
        for i in yz:
            tp2 +=i[1]
            if tp2 <= 6000:
                tp3.append(i[0])
                tp6.append(i[1])
            else:
                tp2 -=i[1]
            tp5+=1
            tp5 = j
            ghs_tp = [x for x in ghs_tp if x not in tp6]
            pm = pm[1:]
            ghs_mz = [x for x in ghs_mz if x not in tp3]
        tp11.append(tp1)
        tp22.append([tp2])
        tp33.append(tp3)
        tp44.append(tp4)
        #tp55.append([tp5])
        tp66.append(tp6)
        #except:
            #continue
    tp111.append(tp11)
    tp222.append([tp22])
    tp333.append(tp33)
    tp444.append(tp44)
    #tp555.append([tp55])
    tp666.append(tp66)

kkk = []
for k in range(24):
    pm = sh_p[0].values.copy()
    t = []
    for j in pm:
        tp = []
        for i in range(8):
            name = tp333[k][i]
            shuju = tp666[k][i]
            tp.append(pd.DataFrame([name,shuju],index=[0,j]).T.set_index(0))
        t.append(tp)#3 6 2 8 4 1 7 5  
                    #0 1 2 3 4 5 6 7  
    kkk.append(pd.concat([t[0][5],t[0][2],t[0][0],t[0][4],t[0][7],t[0][1],t[0][6],t[0][3]],axis=1))
result = pd.concat(kkk,axis=1)
result.to_csv('转运方案4终.csv')


#格式转换
temp7 = result.copy()
qm = data2.iloc[:,0].values
qm_list = [x[1:] for x in qm]
k_index = [x[1:] for x in qm if x not in temp7.index.values]
temp = pd.DataFrame(qm).set_index(0)
y_index = [x[1:] for x in temp7.index.values]
for i in range(192):
    temp[i] = None
k = 0
tp = []
for i in qm_list:
    if i in y_index:
        tp.append(temp7.iloc[k,:])
        k+=1
    else:
        tp.append(temp.iloc[int(i)-1,:])
tpp = []
for i in tp:
    tpp.append(np.array(i))
temp = pd.DataFrame(tpp)
temp.to_csv('转运方案4终终.csv')

#转换填表格式
temp7 = pd.read_csv(r'订购4.csv')
t23 = temp7.iloc[0:135,:]
tp = []
for i in np.linspace(0,24,25):
    tp.append(int(i))
t23.columns = tp
tp = []
for i in data1['供应商ID'].values:
    c = [i]
    for j in np.linspace(0,0,24):
        c.append(int(j))
    tp.append(c)
s402 = pd.DataFrame(tp)
ct = 0
for i in t23[0].values:
    index_23 = int(i[-3:])-1
    s402.iloc[index_23,:] = t23.iloc[ct,:]
    ct += 1
s402.to_csv(r'订购4终.csv')

type_z = data1['材料分类'].values

#平均产能
data1 = pd.read_excel(r'附件1 近5年402家供应商的相关数据.xlsx',sheet_name='企业的订货量（m³）')
type_z = data1['材料分类'].values

z4 = pd.read_csv('订购4终.csv')

z4 = z4.drop(['Unnamed: 0'],axis=1)
z4 = z4.set_index('0')
#z4 = z4.T

Dtype = type_z
Ddata = [1/0.6,1/0.66,1/0.72]
D = []
for i in Dtype:
    if i == 'A':
        D.append(Ddata[0])
    elif i == 'B':
        D.append(Ddata[1])
    elif i == 'C':
        D.append(Ddata[2])
    else:
        continue
t = []
for i in range(24):
    t.append(D)
#temp = /24
z4 = pd.DataFrame(np.array(z4)*np.array(t).T)
res = z4.apply(lambda x: x.sum(),axis=0).T
res.to_csv('重要数据4.csv')
print('第四问平均产能：{}'.format(res.sum()/24))
```
