# 基于常微分及时间序列模型的印度人口增长预测

## 前言

感谢组员的共同协作。

## 问题
以人口总数、人口年度增长率为研究对象，利用并根据世界人口网搜索1959年到2018年印度国家人口统计数据进行参数估计，即数据拟合，并对2019年印度人口进行增长预测。
用数学建模预测人口增长的方法主要有差分方程、微分方程、回归分析、时间序列等，结合题目、搜索到的数据以及《常微分方程》课本中所学知识，本小组以微分方程形式表示的改进指数增长模型、logistic模型为基础，以时间序列模型为拓展课题，建立以时间为自变量的印度人口增长模型。利用历史数据带入模型求解并做出预测。

## 代码

### 改进指数增长模型

```matlab
%改进的指数增长模型
data=csvread('data.csv',1,0);
x=data(:,2)';
real_rate=data(:,3)*0.01;
n=length(x);
t=0:1:n-1;
rk=zeros(1,n);
rk(1)=(-3*x(1)+4*x(2)-x(3))/2;
rk(n)=(x(n-2)-4*x(n-1)+3*x(n))/2;
for i=2:n-1
rk(i)=(x(i+1)-x(i-1))/2;
end
rk=rk./x;
p=polyfit(t,rk,2);
r0=p(1);
r1=p(2);
r2=p(3);
x0=x(1);
R=r0*t.^2+r1*t+r2;
X=x0*exp((r0*t.^3)/3+(r1*t.^2)/2+r2*t);
figure(1)
hold on;
xlabel('year');         %设置横坐标名
ylabel('rate');         %设置纵坐标名
grid on                 %网格线
plot(t,real_rate,'r*')
title('印度人口的年增长率')
figure(2)
hold on;
xlabel('year');         %设置横坐标名
ylabel('population');   %设置纵坐标名
grid on                 %网格线
plot(t,x,'r*',t,X)
title('改进的指数模型拟合的人口')
figure(3)
hold on;
xlabel('year');         %设置横坐标名
ylabel('rate');         %设置纵坐标名
grid on                 %网格线
plot(t,real_rate,'r*',t,R')
title('改进的指数模型拟合的增长率')
t1=t(59)+1;
y2019=x0*exp((r0*t1.^3)/3+(r1*t1.^2)/2+r2*t1)
rate2019=r0*t1.^2+r1*t1+r2
```

### logistic模型

```matlab
%  线性最小二乘法拟合
data=csvread('data.csv',1,0);
x=data(:,2)';
real_rate=data(:,3)*0.01;
n=length(x);
t=0:1:n-1;
rk=zeros(1,n);
rk(1)=(-3*x(1)+4*x(2)-x(3))/2;
rk(n)=(x(n-2)-4*x(n-1)+3*x(n))/2;
for i=2:n-1
rk(i)=(x(i+1)-x(i-1))/2;
end
rk=rk./x;
p=polyfit(t,rk,1);%一阶拟合
b=p(1);
a=p(2);
r=a;
xm=-r/b;
x0=x(1);
for i=1:n
X(i)=xm/(1+((xm/x0)-1)*exp(-r*t(i)));%各个年份的预测人口总数
end
figure(1)
hold on;
xlabel('year');         %设置横坐标名
ylabel('population');   %设置纵坐标名
grid on                 %网格线
plot(t,x,'r*',t,X)
title('线性最小二乘法模型拟合的人口')
figure(2)
hold on;
xlabel('year');         %设置横坐标名
ylabel('rate');         %设置纵坐标名
grid on                 %网格线
plot(t,real_rate,'r*',t,b*t+a)
title('线性最小二乘法模型拟合的增长率')
```

```matlab
%非线性最小二乘法拟合：
function f=logistic_fun(a,t)
f=a(1)./(1+(a(1)/3.9-1)*exp(-a(2)*(t)));
end
clc;
clear all;
data=csvread('data.csv',1,0);
x=data(:,2)'*(10^(-8));
real_rate=data(:,3)*0.01;
n=length(x);
t=0:1:58;
a0=[100,7.6]; %初值
plot(t,x,'*',t,x); %画点，并且画一条直线把各点连起来
hold on;
%最重要的函数，第1个参数是函数名(一个同名的m文件定义)，第2个参数是初值，第3、4个参数是已知数据点
a=lsqcurvefit('logistic_fun',a0,t, x);
disp([' a=' num2str(a)]); %显示结果
%画图检验结果
ti=0:1:59;
xi=logistic_fun(a,ti);
xlabel('year');         %设置横坐标名
ylabel('population');   %设置纵坐标名
plot(ti,xi,'r');
%预测2019
t2019=59;
x2019=logistic_fun(a,t2019)
title('非线性最小二乘法模型拟合的人口')
```

### 时间序列模型
```matlab
Y=[1038058156,1056575549,1075000085,1093317189,1111523144,1129623456,1147609927,1165486291,1183209472,1200669765,1217726215,1234288729,1265782790,1280846129,1295604184,1310152403,1324509589,1338658835,1352617328];
adf=adftest(Y');
kpss=kpsstest(Y');
Yd1=diff(Y');
adf1=adftest(Yd1);
kpss1=kpsstest(Yd1);
Yd2=diff(Yd1);
adf2=adftest(Yd2);
kpss2=kpsstest(Yd2);
Yd3=diff(Yd2);
adf3=adftest(Yd3);
kpss3=kpsstest(Yd3);
figure
autocorr(Yd3);
figure
parcorr(Yd3);
p=1;
q=1;
Mdl = arima(p, 3, q); 
EstMdl=estimate(Mdl,Yd3);
[res,logL] = infer(EstMdl,Yd3);
figure
subplot(2,2,1)
plot(res./sqrt(EstMdl.Variance))
title('Standardized Residuals')
subplot(2,2,2),qqplot(res)
subplot(2,2,3),autocorr(res)
subplot(2,2,4),parcorr(res)
[yF,yMSE]=forecast(EstMdl,1,'y0',Y');
UB=yF+1.96*sqrt(yMSE);
LB=yF-1.96*sqrt(yMSE);
data=Y';step=1;
figure()
plot(data,'Color',[.7,.7,.7]);
hold on
h1 = plot(length(data):length(data)+step,[data(end);LB],'r:','LineWidth',2);
plot(length(data):length(data)+step,[data(end);UB],'r:','LineWidth',2)
h2 = plot(length(data):length(data)+step,[data(end);yF],'k','LineWidth',2);
legend([h1 h2],'95% ÖÃÐÅÇø¼ä','Ô¤²âÖµ',...
         'Location','NorthWest')
title('Forecast')
hold off
```

### 时间序列预测
```matlab
Y=[1.81,1.77,1.73,1.69,1.65,1.62,1.58,1.55,1.51,1.46,1.41,1.35,1.29,1.23,1.18,1.15,1.12,1.09,1.06,1.04];
adf=adftest(Y');
kpss=kpsstest(Y');
Yd1=diff(Y');
adf1=adftest(Yd1);
kpss1=kpsstest(Yd1);
Yd2=diff(Yd1);
adf2=adftest(Yd2);
kpss2=kpsstest(Yd2);
figure
autocorr(Yd2);
figure
parcorr(Yd2);
p=1;
q=1;
Mdl = arima(p, 2, q); 
EstMdl=estimate(Mdl,Yd2);
[res,logL] = infer(EstMdl,Yd2);
figure
subplot(2,2,1)
plot(res./sqrt(EstMdl.Variance))
title('Standardized Residuals')
subplot(2,2,2),qqplot(res)
subplot(2,2,3),autocorr(res)
subplot(2,2,4),parcorr(res)
[yF,yMSE]=forecast(EstMdl,1,'y0',Y');
UB=yF+1.96*sqrt(yMSE);
LB=yF-1.96*sqrt(yMSE);
data=Y';step=1;
figure()
plot(data,'Color',[.7,.7,.7]);
hold on
h1 = plot(length(data):length(data)+step,[data(end);LB],'r:','LineWidth',2);
plot(length(data):length(data)+step,[data(end);UB],'r:','LineWidth',2)
h2 = plot(length(data):length(data)+step,[data(end);yF],'k','LineWidth',2);
legend([h1 h2],'95% ÖÃÐÅÇø¼ä','Ô¤²âÖµ',...
         'Location','NorthWest')
title('Forecast')
hold off
```

### 总结

改进的指数增长模型和时间序列的拟合效果较好。

**完整文档详见：[博客相关资源-常微分项目文件](https://www.spiritlhl.top/ziyuan/)**
