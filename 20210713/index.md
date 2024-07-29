# 多元回归和熵值评价法及多目标遗传优化算法

## 前言

感谢组员书写书面报告，代码部分由我书写，我写的很烂，将就看吧。

## 第一届长三角数学建模

## B题  锅炉水冷壁温度曲线

## 1Stopt多元拟合

个人体会：

只能确定2个自变量1个因变量的拟合函数形式，更高维的无法寻找公式进行拟合。

优点是优化算法基本包含，可以拿来做模型优化（简单的函数形式）。

多于2个自变量的拟合可以使用逐步线性回归法（matlab内置工具箱），缺点是拟合项数会很多，难以写出函数形式。

**ps：相关资源在右上角博客资源页面**

## 逐步线性回归

```matlab
clc
clear all
X1=xlsread('附件1.xlsx');%因变量
data1=X1(3172:end,11);
X2=xlsread('附件2.xlsx');%自变量
data2=X2(3172:end,2:50);
%1 2 3 4 6 12 13 31 34 37 38 39
%temp=[data2,data2.^2,data2.^3,log10(data2)];
%stepwise(temp,data1);%0.6990
%temp1=data2;
temp1=[data2(:,1),data2(:,2),data2(:,3),data2(:,4),data2(:,6),data2(:,12),data2(:,13),data2(:,31),data2(:,34),data2(:,37),data2(:,38),data2(:,39)];
temp2=[temp1,temp1.^2,temp1.^3,temp1.^4,temp1.^5,log10(temp1),1./temp1];
%stepwise(temp2,data1);%0.6990
stepwise(temp2,data1);
%12列拟合相关度54%
%stats beta
%temp3=[x1,x2,x3,x4,x6,x12,x13,x31,x34,x37,x38,x39];
```

重点是这个：

```matlab
stepwise(自变量函数项,因变量);
```

## 皮尔逊相关系数

```matlab
clc
clear all
X1=xlsread('附件1.xlsx');
data1=X1(:,11);
X2=xlsread('附件2.xlsx');
data2=X2(:,44:end);
data=[data1,data2];
%data=zscore(data);%对变量标准化
temp=corrcoef(data);%生成皮尔森相关性系数计算公式
pershen=temp(1,2:end);%求附件一管道十对附件二111个操作变量的皮尔森相关性系数
pershen=abs(pershen); %将皮尔森相关性系数求取绝对值
csvwrite('wenti4.csv',pershen);
```

类似照着写10个管道的

```matlab
clc
clear all
X1=xlsread('附件1.xlsx');
data1=X1(:,2:11);
X2=xlsread('附件2.xlsx');
data2=X2(:,2:end);
data=[data1,data2];
%data=zscore(data);%对变量进行标准化
temp=corrcoef(data);% 生成皮尔森相关性系数计算公式
pershen=temp(1:10,11:end);%求附件一十个管道对附件二111个操作变量和53个状态变量的皮尔森相关性系数
pershen=abs(pershen);%将皮尔森相关性系数求取绝对值
csvwrite('wenti3.csv',pershen);
%经过Excel筛选皮尔森相关性系数最大列数分别为119 112 114 5 115 121 91 92 1 73
result=[data2(:,119),data2(:,112)];
csvwrite('data10.csv',result);
拟合函数图像代码：
clc;
clear
subplot(5,2,1)
x1=(1:1:5000);
M=csvread('C:\Users\86152\Desktop\data\data1.csv');
x=M(:,1);
y=M(:,2);
p1=-366.623574952674;
p2=-6309.81591583794;
p3=-57878.3062818374;
p4=15903.1406517026;
p5=-53.4927163299915;
p6=0.0889609164030065;
p7=542.236136002024;
p8=-218.986158913513;
p9=17.338434284516;
z=(p1+p2.*log(x)+p3.*(log(x)).^2+p4*y+p5*y.^2+p6*y.^3)./(1+p7*log(x)+p8*(log(x)).^2+p9.*y);
plot(x1,z);
xlabel('采样点');
ylabel('温度');
title('管道一温度曲线')
subplot(5,2,2)
x1=(1:1:5000);
M=xlsread('C:\Users\86152\Desktop\data\data2.xlsx');
x=M(:,1);
y=M(:,2);
p1=-760228.816854935;
p2=9.40803736929389;
p3=-0.0110468670816608;
p4=2.36049470120192E-7;
p5=10924.026332869;
p6=-58.9224433132279;
p7=0.141102010823114;
p8=-0.000126582641617285;
z=p1+p2.*x+p3.*x.^2+p4.*x.^3+p5.*y+p6.*y.^2+p7.*y.^3+p8.*y.^4;
plot(x1,z);
xlabel('采样点');
ylabel('温度');
title('管道二温度曲线')
subplot(5,2,3)
x1=(1:1:5000);
M=csvread('C:\Users\86152\Desktop\data\data3.csv');
x=M(:,1);
y=M(:,2);
p1=-278364.765528611;
p2=-8053.13293300719;
p3=702.700979973376;
p4=4525.26042721373;
p5=-25.4017726090204;
p6=0.063270352619814;
p7=-5.9005816668393E-5;
z=p1+p2.*log(x)+p3.*(log(x)).^2+p4.*y+p5.*y.^2+p6.*y.^3+p7.*y.^4;
plot(x1,z);
xlabel('采样点');
ylabel('温度');
title('管道三温度曲线')
subplot(5,2,4)
x1=(1:1:5000);
M=csvread('C:\Users\86152\Desktop\data\data4.csv');
x=M(:,1);
y=M(:,2);
p1=-72.9520473758939;
p2=-556.535840804353;
p3=-6289.99299391888;
p4=38088.2044118192;
p5=-187.688895088328;
p6=-0.314791034083831;
p7=-36.6632213229309;
p8=-7693.38639121209;
p9=65.3375954195133;
p10=135.981090347525;
p11=4418.78480395924;
z=(p1+p3.*x+p5.*log(y)+p7*x.^2+p9.*(log(y)).^2+p11.*x.*log(y))./(1+p2.*x+p4.*log(y)+p6.*x.^2+p8.*(log(y)).^2+p10.*x.*log(y));
plot(x1,z);
xlabel('采样点');
ylabel('温度');
title('管道四温度曲线')
subplot(5,2,5)
x1=(1:1:5000);
M=csvread('C:\Users\86152\Desktop\data\data5.csv');
x=M(:,1);
y=M(:,2);
p1=6636.72503715224;
p2=826099.455060947;
p3=-498.995296492192;
p4=1.80238545972406;
p5=-1143812.65641828;
p6=3030.85414746531;
p7=-4932.54637234801;
p8=3.5943846329167;
z=(p1+p2.*x+p3.*x.^2+p4.*x.^3+p5.*y)./(1+p6.*x+p7.*y+p8.*y.^2);
plot(x1,z);
xlabel('采样点');
ylabel('温度');
title('管道五温度曲线')
subplot(5,2,6)
x1=(1:1:5000);
M=csvread('C:\Users\86152\Desktop\data\data6.csv');
x=M(:,1);
y=M(:,2);
p1=-5883.284880402;
p2=60.424921963928;
p3=-0.164225401010505;
p4=0.000149967919524129;
p5=61332.3184515964;
p6=79523.3753333665;
p7=-54368.4125968335;
p8=10405.5019030654;
p9=-639.850116036068;
z=p1+p2.*x+p3.*x.^2+p4.*x.^3+p5.*log(y)+p6.*(log(y)).^2+p7.*(log(y)).^3+p8.*(log(y)).^4+p9.*(log(y)).^5;
plot(x1,z);
xlabel('采样点');
ylabel('温度');
title('管道六温度曲线')
subplot(5,2,7)
x1=(1:1:5000);
M=csvread('C:\Users\86152\Desktop\data\data7.csv');
x=M(:,1);
y=M(:,2);
p1=201478.39441699;
p2=372414.059548135;
p3=-147089.218042618;
p4=12485.7289898091;
p5=-224.561025015429;
p6=3.32272031088369;
p7=-173.088899267814;
p8=4.01866601526878;
z = (p1+p2.*log(x)+p3.*(log(x)).^2+p4.*(log(x)).^3+p5.*y+p6.*y.^2)./(1+p7.*log(x)+p8.*y);
plot(x1,z);
xlabel('采样点');
ylabel('温度');
title('管道七温度曲线')
subplot(5,2,8)
x1=(1:1:5000);
M=csvread('C:\Users\86152\Desktop\data\data8.csv');
x=M(:,1);
y=M(:,2);
p1=682051.099193563;
p2=-166.30866016075;
p3=0.443241999640146;
p4=-0.000392578722960921;
p5=-340091.23447923;
p6=58193.7202898162;
p7=-3310.5722076251;
z=p1+p2.*x+p3.*x.^2+p4.*x.^3+p5.*log(y)+p6.*(log(y)).^2+p7.*(log(y)).^3;
plot(x1,z);
xlabel('采样点');
ylabel('温度');
title('管道八温度曲线')
subplot(5,2,9)
x1=(1:1:5000);
M=csvread('C:\Users\86152\Desktop\data\data9.csv');
x=M(:,1);
y=M(:,2);
p1=421.576820618631;
p2=386.777802412627;
p3=119.426378501925;
p4=481.83371005309;
p5=254.888468073613;
p6=5.98063835171261;
p7=-525.072127917344;
z=p1./(1+((x-p2)./p3).^2)+p4./(1+((y-p5)./p6).^2)+p7./((1+((x-p2)./p3).^2).*(1+((y-p5)./p6).^2));
plot(x1,z);
xlabel('采样点');
ylabel('温度');
title('管道九温度曲线')
subplot(5,2,10)
x1=(1:1:5000);
M=csvread('C:\Users\86152\Desktop\data\data10.csv');
x=M(:,1);
y=M(:,2);
p1=-1217.63395897089;
p2=-27430.5826029483;
p3=380.735591383903;
p4=-8.70371508967982;
p5=1639.83545496426;
p6=276.790380488085;
p7=423.356786189341;
p8=27449.3757790585;
z = p1+p2./(1+((x-p3)./p4).^2)+p5./(1+((y-p6)./p7).^2)+p8./((1+((x-p3)./p4).^2).*(1+((y-p6)./p7).^2));
plot(x1,z);
xlabel('采样点');
ylabel('温度');
title('管道十温度曲线')
```

## 熵值法

```
function y=guiyi(x,type,ymin,ymax)
%实现正向或负向指标归一化，返回归一化后的数据矩阵
%x为原始数据矩阵, 一行代表一个样本, 每列对应一个指标
%type设定正向指标1,负向指标2
%ymin,ymax为归一化的区间端点
[n,m]=size(x);
y=zeros(n,m);
xmin=min(x);
xmax=max(x);
switch type
    case 1
        for j=1:m
            y(:,j)=(ymax-ymin)*(x(:,j)-xmin(j))/(xmax(j)-xmin(j))+ymin;
        end
    case 2
        for j=1:m
            y(:,j)=(ymax-ymin)*(xmax(j)-x(:,j))/(xmax(j)-xmin(j))+ymin;
        end
end
clc
clear all
X=xlsread('附件1.xlsx');
data=X(:,2:end);
[max_data,index1]=max(data);
[min_data,index]=min(data);
fangcha=var(data);
[max_data,index1]=max(data);
D=[fangcha;max_data]';
temp=[2 2];
[pj w]=shang(D,temp);
pj%pj 评价分
w%w 权重

function [s,w]=shang(x,ind)
%实现用熵值法求各指标(列）的权重及各数据行的得分
%x为原始数据矩阵, 一行代表一个样本, 每列对应一个指标
%ind指示向量，指示各列正向指标还是负向指标，1表示正向指标，2表示负向指标
%s返回各行（样本）得分，w返回各列权重
[n,m]=size(x); % n个样本, m个指标
%%数据的归一化处理
for i=1:m
    if ind(i)==1 %正向指标归一化
        X(:,i)=guiyi(x(:,i),1,0.002,0.996);    %若归一化到[0,1], 0会出问题
    else %负向指标归一化
        X(:,i)=guiyi(x(:,i),2,0.002,0.996);
    end
end
%%计算第j个指标下，第i个样本占该指标的比重p(i,j)
for i=1:n
    for j=1:m
        p(i,j)=X(i,j)/sum(X(:,j));
    end
end
%%计算第j个指标的熵值e(j)
k=1/log(n);
for j=1:m
    e(j)=-k*sum(p(:,j).*log(p(:,j)));
end
d=ones(1,m)-e; %计算信息熵冗余度
w=d./sum(d); %求权值w
s=100*w*X'; %求综合得分
```

## 第一问代码

抄的某公众号

```matlab
Y=xlsread('C:\Users\86152\Desktop\附件1.xlsx');
X=Y(:,[2:11]);
x1=mean(X,1)   %求十个管道温度平均值
[max_X,index]=max(X)  %求十个管道温度最大值
[min_X,index]=min(X)  %求十个管道温度最小值
DX=var(X)      %求十个管道温度方差
```

## 多目标遗传优化算法

先写目标函数，再给定约束，求符合的自变量取值。

```matlab
function y=fun(x)
    y(1)=-(x(1)*100/3 + x(3)*90/3  + x(2)*80/2+x(4)*70/2);
    y(2)=x(3)+x(4);
end

clear
clc
fitnessfcn=@fun;
% 变量个数
nvars=4;
% lb<= X <= ub
lb=[0,0,0,0];
ub=[];
% A*X <= b 
A = [0    0 1 1
    -1/3  0 0 0
     0 -1/2 0 0
     0    0 0 0];
 
b = [48 ; 30 ; 30 ; 0];
 
% Aeq*X = beq
Aeq=[1 1 0 0;0 0 0 0; 0 0 0 0; 0 0 0 0];
beq=[120;0;0;0];
%最优个体系数paretoFraction
%种群大小populationsize
%最大进化代数generations
%停止代数stallGenLimit
%适应度函数偏差TolFun
%函数gaplotpareto：绘制Pareto前沿 options=gaoptimset('paretoFraction',0.3,'populationsize',200,'generations',300,'stallGenLimit',200,'TolFun',1e-10,'PlotFcns',@gaplotpareto);
 
[x,fval]=gamultiobj(fitnessfcn,nvars,A,b,Aeq,beq,lb,ub,options)
 
plot(-fval(:,1),fval(:,2),'pr')
xlabel('f_1(x)')
ylabel('f_2(x)')
title('Pareto front')
grid on
```

**完整文档详见：[博客相关资源-长三角数学建模B题](https://www.spiritlhl.top/ziyuan/)**
