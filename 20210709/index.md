# 改进的SIR差分模型及三个模型的应用

## 前言

感谢组员的共同协作，做组长的有些东西帮不上实在抱歉。


## 改进的SIR差分模型

```matlab
%% SIR差分模型
frame=importdata('美国covid19疫情数据l-history改.csv');
date_row=size(frame.data,1);
data=frame.data;
date=frame.textdata(:,1);
real_date=date(2:date_row+1,:);
E=zeros(2,90);
l=0.000125;    %日接触率
m=0.01;   %日治愈率0.0005
E(1,1)=data(345,12)/300000000;
E(2,1)=1-E(1,1);
for i=1:90
    E(1,i+1)=l*E(2,i)-m*E(1,i)+E(1,i);
    E(2,i+1)=E(2,i)-l*E(1,i)*E(2,i);
end
X=flip(data(311:345,12)');% 5.31  4.2 
rate=flip(data(311:345,12)')%现阳性率
rate(isnan(rate))=0;
n=length(rate);
rt=0:1:n-1;
%a0=[100,10]; %初值
figure(1)
h1 = plot(rt,X/300000000,'*'); %画点
hold on;
a=E(1,:);
b=E(2,:);
h2 = plot(a,'r')
xlabel('日期');         %设置横坐标名
ylabel('感染人数');   %设置纵坐标名
legend([h1 h2],'3~5月感染人数','SIR模型迭代曲线','Location','NorthWest');
hold on
%%  预测
X=flip(data(255:345,12)');%311行后为第一题
rate=flip(data(255:345,12)')%现阳性率
rate(isnan(rate))=0;
n=length(rate);
rt=0:1:n-1;
figure(2)
h3 = plot(rt,X/300000000,'*'); %画点
hold on;
a=E(1,:);
b=E(2,:);
h4=plot(a,'r')
xlabel('日期');
ylabel('感染人数'); 
legend([h3 h4],'3~6月感染人数','SIR模型迭代曲线','Location','NorthWest');
hold on
%% 接种疫苗前
frame=importdata('usc.csv');
date_row=size(frame.data,1);
data=frame.data;
date=frame.textdata(:,1);
real_date=date(2:date_row+1,:);
E=zeros(3,150);
l=0.0018;    %日接触率 0.0018
m=0.01;   %日治愈率 0.01
E(1,1)=data(320,1)/300000000;
E(3,1)=28664448/300000000%43714928  33714928  28664448
E(2,1)=1-E(1,1)-E(3,1);

for i=1:150
    E(1,i+1)=E(1,i)+l*E(2,i)-m*E(1,i);
    E(2,i+1)=E(2,i)-l*E(1,i)*E(2,i);
    E(3,i+1)=1-E(1,i+1)- E(2,i+1);
end
X=data(320:486,1)';%80-482  3.1-7.1
rate=data(320:486,1)'%
rate(isnan(rate))=0;
n=length(rate);
rt=0:1:n-1;
figure(6)
h11 = plot(rt,X/300000000,'*'); %画点
hold on;
a=E(1,:);
b=E(2,:);
c=E(3,:);
t=1:151;
h1=plot(t,a,'r')%感染人数
%plot(t,b,'b')%健康人数
plot(t,c,'r')%移除者
%legend('i(t)','s(t)','r(t)')
hold on
%% 接种疫苗后
E=zeros(3,150);
l=0.0018;    %日接触率 0.0018
m=0.01;   %日治愈率 0.01
E(1,1)=data(320,1)/300000000;
E(3,1)=53714928/300000000%43714928  33714928  28664448
E(2,1)=1-E(1,1)-E(3,1);
for i=1:150
    E(1,i+1)=E(1,i)+l*E(2,i)-m*E(1,i);
    E(2,i+1)=E(2,i)-l*E(1,i)*E(2,i);
    E(3,i+1)=1-E(1,i+1)- E(2,i+1);
end
X=data(320:486,1)';%80-482  3.1-7.1
rate=data(320:486,1)'%
rate(isnan(rate))=0;
n=length(rate);
rt=0:1:n-1;
figure(6)
%h11 = plot(rt,X/300000000,'*'); %画点
hold on;
a=E(1,:);
b=E(2,:);
c=E(3,:);
t=1:151;
h2=plot(t,a,'b')%感染人数
%plot(t,b,'b')%健康人数
plot(t,c,'b')%移除者
%legend('i(t)','s(t)','r(t)')
hold on
xlabel('日期');         %设置横坐标名
ylabel('r(t)与i（t）');   %设置纵坐标名
legend([h1 h2 h11],'接种疫苗前','接种疫苗后','感染人数原始数据','Location','NorthWest');
```

## logistic模型预测美国人口

```matlab
%改进的指数增长模型
x=flip([324985536 322941312 320635168 318300992 315993728 313830976 311556864 309321664 306771520 304093952 301231200 298379904 295516608 292805312 290107936 287625184 284968960 282162400 279040000 275854016 272656992])
y=flip([0.006330017,0.007192424,0.007333235,0.007301613,0.006891455,0.007299188,0.007226135,0.008312845,0.008805068,0.009503504,0.009555925,0.00968912,0.009259723,0.009297836,0.008631901,0.009321099,0.009946612,0.011189794,0.011549529,0.011725443,0.012112401])
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
plot(t,y,'r*')
figure(2)
hold on;
xlabel('year');         %设置横坐标名
ylabel('population');   %设置纵坐标名
grid on                 %网格线
plot(t,x,'r*',t,X)
figure(3)
hold on;
xlabel('year');         %设置横坐标名
ylabel('rate');         %设置纵坐标名
grid on                 %网格线
plot(t,y,'r*',t,R')
%2018
t1=t(21)+1;
x2018=x0*exp((r0*t1.^3)/3+(r1*t1.^2)/2+r2*t1)
y2018=r0*t1.^2+r1*t1+r2
%2019
t1=t(21)+2;
x2019=x0*exp((r0*t1.^3)/3+(r1*t1.^2)/2+r2*t1)
y2019=r0*t1.^2+r1*t1+r2
```

## 药物中毒急救建模

```matlab
clc;
clear all;
syms x t;
f(t)=6*(exp(-0.1155*t)-exp(-0.1386*t));
h(t)=exp(-0.1386*t);
figure(1);
fplot(f,[0 25]);
hold on;
fplot(h,[0,25]);
grid on;
xlabel('时间t/h');
ylabel('药量');
legend('血液中的药量y/mg','胃肠道中的药量x/mg');
figure(2)
fplot(f,[0,25]);
hold on;
fplot(h,[0,25]);
f(t)=-(exp(-0.693*t)-exp(-0.1386*t))/4;
fplot(f,[0,25]);
xlabel('时间t/h');
ylabel('药量');
legend('血液中的药量y/mg','胃肠道中的药量x/mg','施救后血液中的药量y/mg');
```

## 热传导差分偏微分模型应用

```matlab
clear;
close all;
clc;
pho=[300;862;74.2;1.18];
c=[1377;2100;1726;1005];
lamda=[0.082;0.37;0.045;0.028];
a=lamda./(pho.*c);
 
d=[0.6;6;3.6;5]*10^-3;
 
TT=273.15;
T_in=37;
T_out=75;
T_s=48.08;
 
xmin=0;
xmax=sum(d);
 
N=5400;
h=0.05*10^-3;
k=1;
r=k/h^2;
I=round((xmax-xmin)/h);
 
A=zeros(1,I);
B=zeros(1,I+1);
C=zeros(1,I);
 
N1=round(d(1)/h);
N2=round(d(2)/h);
N3=round(d(3)/h);
N4=round(d(4)/h);
 
for i=1:N1
    A(i)=-a(1)*r;
    B(i)=2+2*r*a(1);
    C(i)=-r*a(1);
end
for i=N1+1:N1+N2
    A(i)=-a(2)*r;
    B(i)=2+2*r*a(2);
    C(i)=-r*a(2);
end
for i=N1+N2+1:N1+N2+N3
    A(i)=-a(3)*r;
    B(i)=2+2*r*a(3);
    C(i)=-r*a(3);
end
for i=N1+N2+N3+1:N1+N2+N3+N4
    A(i)=-a(4)*r;
    B(i)=2+2*r*a(4);
    C(i)=-r*a(4);
end
 
T=zeros(I+1,N+1);
T(:,1)=(T_in+TT)*ones(I+1,1);
 
T_xt=xlsread('CUMCM-2018-Problem-A-Chinese-Appendix.xlsx');
 
h_min=110;
h_max=120;
delta_h=0.1;
H1=h_min:delta_h:h_max;
delta=zeros(1,length(H1));
 
for j=1:length(H1)
    h1=h_min+(j-1)*delta_h;
    k1=lamda(1);k2=lamda(2);k3=lamda(3);k4=lamda(4);
    x1=d(1);x2=d(1)+d(2);x3=d(1)+d(2)+d(3);x4=d(1)+d(2)+d(2)+d(4);
    t1=T_out+TT;t2=T_in+TT;t3=T_s+TT;
    
    h5=-((h1*k2*k3*k4*t1)/(k1*k2*k3*k4-h1*k1*k2*k3*x3-h1*k1*k2*k4*x2 ...
        -h1*k1*k3*k4*x1+h1*k1*k2*k3*x4+h1*k1*k2*k4*x3+h1*k1*k3*k4*x2+h1*k2*k3*k4*x1)-(h1*k2*k3*k4*t3)...
        /(k1*k2*k3*k4-h1*k1*k2*k3*x3-h1*k1*k2*k4*x2-h1*k1*k3*k4*x1+h1*k1*k2*k3*x4+h1*k1*k2*k4*x3+h1*k1*k3*k4*x2+h1*k2*k3*k4*x1))...
        /(t2/k1-t3/k1);
    
    AA=diag(B)+diag(A,1)+diag(C,-1);
    AA(1,1)=lamda(1)/h+h1;
    AA(1,2)=-lamda(1)/h;
    AA(I+1,I)=-lamda(4)/h;
    AA(I+1,I+1)=lamda(4)/h+h5;
    
    AA(N1+1,N1)=-lamda(1);
    AA(N1+1,N1+1)=lamda(1)+lamda(2);
    AA(N1+1,N1+2)=-lamda(2);
    
    AA(N1+N2+1,N1+N2)=-lamda(2);
    AA(N1+N2+1,N1+N2+1)=lamda(2)+lamda(3);
    AA(N1+N2+1,N1+N2+2)=-lamda(3);
    
    AA(N1+N2+N3+1,N1+N2+N3)=-lamda(3);
    AA(N1+N2+N3+1,N1+N2+N3+1)=lamda(3)+lamda(4);
    AA(N1+N2+N3+1,N1+N2+N3+2)=-lamda(4);
    
    for n=1:k:N
        D=zeros(I+1,1);
        D(1)=h1*(T_out+TT);
        D(I+1)=h5*(T_in+TT);
        for i=2:1:N1
            D(i)=r*a(1)*T(i-1,n)+(2-2*r*a(1))*T(i,n)+r*a(1)*T(i+1,n);
        end
        for i=N1+1:1:N1+N2
            D(i)=r*a(2)*T(i-1,n)+(2-2*r*a(2))*T(i,n)+r*a(2)*T(i+1,n);
        end
        for i=N1+N2+1:1:N1+N2+N3
            D(i)=r*a(3)*T(i-1,n)+(2-2*r*a(3))*T(i,n)+r*a(3)*T(i+1,n);
        end
        for i=N1+N2+N3+1:1:N1+N2+N3+N4
            D(i)=r*a(4)*T(i-1,n)+(2-2*r*a(4))*T(i,n)+r*a(4)*T(i+1,n);
        end
        D(N1+1)=0;
        D(N1+N2+1)=0;
        D(N1+N2+N3+1)=0;
        T(:,n+1)=AA\D;
    end
   delta(j)=sqrt(sum((T_xt(:,2)-T(end,:)'+TT).^2)/length(T_xt(:,1)));
end
%图二 
figure(1);
mesh(0:k:N,1000*(0:h:sum(d)),(T-TT));
%图三
T_problem1=zeros(N+1,4);
T_problem1(:,1)=T(1,:)';
T_problem1(:,2)=T(N1+1,:)';
T_problem1(:,3)=T(N2+N1+1,:)';
T_problem1(:,4)=T(N3+N2+N1+1,:)';
T_problem1=T_problem1-TT;
figure(2);
plot(0:k:N,T_problem1(:,1)',0:k:N,T_problem1(:,2)',0:k:N,T_problem1(:,3)',0:k:N,T_problem1(:,4)',0:k:N,T_xt(:,2)');
```

## python数据处理(写的很乱)

为了写第一个模型整合各种数据。。。有点乱，将就看吧。

```python
#导入需要的数据库和文件
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib.pyplot import MultipleLocator
odata=pd.read_csv(r'us_state_vaccinations.csv')
plt.rcParams['font.sans-serif'] = ['SimHei'] 
plt.rcParams['axes.unicode_minus'] = False 
```


```python
r_hex = '#dc2624'     # red,       RGB = 220,38,36
dt_hex = '#2b4750'    # dark teal, RGB = 43,71,80
tl_hex = '#45a0a2'    # teal,      RGB = 69,160,162
r1_hex = '#e87a59'    # red,       RGB = 232,122,89
tl1_hex = '#7dcaa9'   # teal,      RGB = 125,202,169
g_hex = '#649E7D'     # green,     RGB = 100,158,125
o_hex = '#dc8018'     # orange,    RGB = 220,128,24
tn_hex = '#C89F91'    # tan,       RGB = 200,159,145
g50_hex = '#6c6d6c'   # grey-50,   RGB = 108,109,108
bg_hex = '#4f6268'    # blue grey, RGB = 79,98,104
g25_hex = '#c7cccf'   # grey-25,   RGB = 199,204,207
```


```python
odata
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>location</th>
      <th>total_vaccinations</th>
      <th>total_distributed</th>
      <th>people_vaccinated</th>
      <th>people_fully_vaccinated_per_hundred</th>
      <th>total_vaccinations_per_hundred</th>
      <th>people_fully_vaccinated</th>
      <th>people_vaccinated_per_hundred</th>
      <th>distributed_per_hundred</th>
      <th>daily_vaccinations_raw</th>
      <th>daily_vaccinations</th>
      <th>daily_vaccinations_per_million</th>
      <th>share_doses_used</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-01-12</td>
      <td>Alabama</td>
      <td>78134.0</td>
      <td>377025.0</td>
      <td>70861.0</td>
      <td>0.15</td>
      <td>1.59</td>
      <td>7270.0</td>
      <td>1.45</td>
      <td>7.69</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.207</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-01-13</td>
      <td>Alabama</td>
      <td>84040.0</td>
      <td>378975.0</td>
      <td>74792.0</td>
      <td>0.19</td>
      <td>1.71</td>
      <td>9245.0</td>
      <td>1.53</td>
      <td>7.73</td>
      <td>5906.0</td>
      <td>5906.0</td>
      <td>1205.0</td>
      <td>0.222</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-01-14</td>
      <td>Alabama</td>
      <td>92300.0</td>
      <td>435350.0</td>
      <td>80480.0</td>
      <td>NaN</td>
      <td>1.88</td>
      <td>NaN</td>
      <td>1.64</td>
      <td>8.88</td>
      <td>8260.0</td>
      <td>7083.0</td>
      <td>1445.0</td>
      <td>0.212</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-01-15</td>
      <td>Alabama</td>
      <td>100567.0</td>
      <td>444650.0</td>
      <td>86956.0</td>
      <td>0.28</td>
      <td>2.05</td>
      <td>13488.0</td>
      <td>1.77</td>
      <td>9.07</td>
      <td>8267.0</td>
      <td>7478.0</td>
      <td>1525.0</td>
      <td>0.226</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-01-16</td>
      <td>Alabama</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7557.0</td>
      <td>7498.0</td>
      <td>1529.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>11328</th>
      <td>2021-06-30</td>
      <td>Wyoming</td>
      <td>421749.0</td>
      <td>516025.0</td>
      <td>226911.0</td>
      <td>34.38</td>
      <td>72.87</td>
      <td>198958.0</td>
      <td>39.21</td>
      <td>89.16</td>
      <td>55.0</td>
      <td>913.0</td>
      <td>1578.0</td>
      <td>0.817</td>
    </tr>
    <tr>
      <th>11329</th>
      <td>2021-07-01</td>
      <td>Wyoming</td>
      <td>423238.0</td>
      <td>516325.0</td>
      <td>227741.0</td>
      <td>34.51</td>
      <td>73.13</td>
      <td>199743.0</td>
      <td>39.35</td>
      <td>89.21</td>
      <td>1489.0</td>
      <td>1113.0</td>
      <td>1923.0</td>
      <td>0.820</td>
    </tr>
    <tr>
      <th>11330</th>
      <td>2021-07-02</td>
      <td>Wyoming</td>
      <td>424025.0</td>
      <td>516865.0</td>
      <td>228162.0</td>
      <td>34.59</td>
      <td>73.26</td>
      <td>200184.0</td>
      <td>39.42</td>
      <td>89.31</td>
      <td>787.0</td>
      <td>847.0</td>
      <td>1463.0</td>
      <td>0.820</td>
    </tr>
    <tr>
      <th>11331</th>
      <td>2021-07-03</td>
      <td>Wyoming</td>
      <td>431008.0</td>
      <td>517365.0</td>
      <td>230914.0</td>
      <td>35.34</td>
      <td>74.47</td>
      <td>204522.0</td>
      <td>39.90</td>
      <td>89.39</td>
      <td>6983.0</td>
      <td>1680.0</td>
      <td>2903.0</td>
      <td>0.833</td>
    </tr>
    <tr>
      <th>11332</th>
      <td>2021-07-04</td>
      <td>Wyoming</td>
      <td>431101.0</td>
      <td>517365.0</td>
      <td>230993.0</td>
      <td>35.35</td>
      <td>74.49</td>
      <td>204598.0</td>
      <td>39.91</td>
      <td>89.39</td>
      <td>93.0</td>
      <td>1682.0</td>
      <td>2906.0</td>
      <td>0.833</td>
    </tr>
  </tbody>
</table>
<p>11333 rows × 14 columns</p>
</div>




```python
states = list(set(odata['location']))
len(states)
```




    65




```python
date = list(set(odata['date']))
date.sort()
date
```


```python
date[0]
```



    '2020-12-20'




```python
US = odata.loc[odata['date'] == '2021-01-12']
US
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>location</th>
      <th>total_vaccinations</th>
      <th>total_distributed</th>
      <th>people_vaccinated</th>
      <th>people_fully_vaccinated_per_hundred</th>
      <th>total_vaccinations_per_hundred</th>
      <th>people_fully_vaccinated</th>
      <th>people_vaccinated_per_hundred</th>
      <th>distributed_per_hundred</th>
      <th>daily_vaccinations_raw</th>
      <th>daily_vaccinations</th>
      <th>daily_vaccinations_per_million</th>
      <th>share_doses_used</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-01-12</td>
      <td>Alabama</td>
      <td>78134.0</td>
      <td>377025.0</td>
      <td>70861.0</td>
      <td>0.15</td>
      <td>1.59</td>
      <td>7270.0</td>
      <td>1.45</td>
      <td>7.69</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.207</td>
    </tr>
    <tr>
      <th>174</th>
      <td>2021-01-12</td>
      <td>Alaska</td>
      <td>35838.0</td>
      <td>141600.0</td>
      <td>22486.0</td>
      <td>0.74</td>
      <td>4.90</td>
      <td>5400.0</td>
      <td>3.07</td>
      <td>19.36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.253</td>
    </tr>
    <tr>
      <th>348</th>
      <td>2021-01-12</td>
      <td>American Samoa</td>
      <td>2124.0</td>
      <td>10650.0</td>
      <td>842.0</td>
      <td>0.47</td>
      <td>3.81</td>
      <td>260.0</td>
      <td>1.51</td>
      <td>19.12</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.199</td>
    </tr>
    <tr>
      <th>522</th>
      <td>2021-01-12</td>
      <td>Arizona</td>
      <td>141355.0</td>
      <td>563025.0</td>
      <td>95141.0</td>
      <td>0.11</td>
      <td>1.94</td>
      <td>8343.0</td>
      <td>1.31</td>
      <td>7.74</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.251</td>
    </tr>
    <tr>
      <th>696</th>
      <td>2021-01-12</td>
      <td>Arkansas</td>
      <td>40879.0</td>
      <td>274400.0</td>
      <td>39357.0</td>
      <td>0.00</td>
      <td>1.35</td>
      <td>8.0</td>
      <td>1.30</td>
      <td>9.09</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.149</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>10463</th>
      <td>2021-01-12</td>
      <td>Virginia</td>
      <td>190607.0</td>
      <td>797150.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.23</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9.34</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.239</td>
    </tr>
    <tr>
      <th>10637</th>
      <td>2021-01-12</td>
      <td>Washington</td>
      <td>195567.0</td>
      <td>567725.0</td>
      <td>162105.0</td>
      <td>0.23</td>
      <td>2.57</td>
      <td>17689.0</td>
      <td>2.13</td>
      <td>7.46</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.344</td>
    </tr>
    <tr>
      <th>10811</th>
      <td>2021-01-12</td>
      <td>West Virginia</td>
      <td>103330.0</td>
      <td>160975.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.77</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.98</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.642</td>
    </tr>
    <tr>
      <th>10985</th>
      <td>2021-01-12</td>
      <td>Wisconsin</td>
      <td>137253.0</td>
      <td>429500.0</td>
      <td>125895.0</td>
      <td>0.19</td>
      <td>2.36</td>
      <td>11343.0</td>
      <td>2.16</td>
      <td>7.38</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.320</td>
    </tr>
    <tr>
      <th>11159</th>
      <td>2021-01-12</td>
      <td>Wyoming</td>
      <td>16467.0</td>
      <td>47800.0</td>
      <td>13577.0</td>
      <td>0.37</td>
      <td>2.85</td>
      <td>2116.0</td>
      <td>2.35</td>
      <td>8.26</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.344</td>
    </tr>
  </tbody>
</table>
<p>65 rows × 14 columns</p>
</div>




```python
US['people_vaccinated'].sum()
```




    15746663.0




```python
data=[]
```


```python
for i in date:
    temp = odata.loc[odata['date'] == i]
    data.append(temp['people_vaccinated'].sum())
data
```

```python
dataframe=pd.DataFrame(data=data, index=date, columns=['vaccinations'], dtype=None, copy=False)
dataframe
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vaccinations</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-12-20</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-12-21</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-12-22</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-12-23</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-12-24</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2021-06-30</th>
      <td>366017545.0</td>
    </tr>
    <tr>
      <th>2021-07-01</th>
      <td>367355455.0</td>
    </tr>
    <tr>
      <th>2021-07-02</th>
      <td>367983174.0</td>
    </tr>
    <tr>
      <th>2021-07-03</th>
      <td>368905977.0</td>
    </tr>
    <tr>
      <th>2021-07-04</th>
      <td>369524458.0</td>
    </tr>
  </tbody>
</table>
<p>197 rows × 1 columns</p>
</div>




```python
dataframe.dropna(subset=['vaccinations'],inplace=True)
dataframe.columns = ['vaccinations']
dataframe.drop(dataframe[dataframe['vaccinations']<0.01].index)
vus = dataframe
vus
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vaccinations</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-12-20</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-12-21</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-12-22</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-12-23</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-12-24</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2021-06-30</th>
      <td>366017545.0</td>
    </tr>
    <tr>
      <th>2021-07-01</th>
      <td>367355455.0</td>
    </tr>
    <tr>
      <th>2021-07-02</th>
      <td>367983174.0</td>
    </tr>
    <tr>
      <th>2021-07-03</th>
      <td>368905977.0</td>
    </tr>
    <tr>
      <th>2021-07-04</th>
      <td>369524458.0</td>
    </tr>
  </tbody>
</table>
<p>197 rows × 1 columns</p>
</div>




```python
fig = plt.figure( figsize=(16,6), dpi=100)
ax = fig.add_subplot(1,1,1)
x = dataframe.index
y = dataframe['vaccinations']
ax.plot( x, y, color=dt_hex, linewidth=2, linestyle='-' )
ax.set_title('接种疫苗人数(/千万)',fontdict={
      'color':'black',
      'size':24
})
ax.set_xticks( range(0,len(x),30))
```




    [<matplotlib.axis.XTick at 0x208ab850c18>,
     <matplotlib.axis.XTick at 0x208ab850be0>,
     <matplotlib.axis.XTick at 0x208ab8426a0>,
     <matplotlib.axis.XTick at 0x208ad89a588>,
     <matplotlib.axis.XTick at 0x208ad89aa20>,
     <matplotlib.axis.XTick at 0x208ad89aeb8>,
     <matplotlib.axis.XTick at 0x208ad89a978>]




![png](output_12_1.png)



```python
cdata=pd.read_csv(r'DXYArea.csv')
```


```python
cdata
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>continentName</th>
      <th>continentEnglishName</th>
      <th>countryName</th>
      <th>countryEnglishName</th>
      <th>provinceName</th>
      <th>provinceEnglishName</th>
      <th>province_zipCode</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>updateTime</th>
      <th>cityName</th>
      <th>cityEnglishName</th>
      <th>city_zipCode</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>33718538</td>
      <td>0.0</td>
      <td>29096816</td>
      <td>605526</td>
      <td>2021-07-05 19:13:09</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>欧洲</td>
      <td>Europe</td>
      <td>法国</td>
      <td>France</td>
      <td>法国</td>
      <td>France</td>
      <td>961002</td>
      <td>5921696</td>
      <td>0.0</td>
      <td>353370</td>
      <td>111190</td>
      <td>2021-07-05 19:12:06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>南美洲</td>
      <td>South America</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>巴西</td>
      <td>Brazil</td>
      <td>973003</td>
      <td>18769808</td>
      <td>0.0</td>
      <td>17082876</td>
      <td>524475</td>
      <td>2021-07-05 19:12:06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>欧洲</td>
      <td>Europe</td>
      <td>瑞典</td>
      <td>Sweden</td>
      <td>瑞典</td>
      <td>Sweden</td>
      <td>962005</td>
      <td>1090880</td>
      <td>0.0</td>
      <td>4971</td>
      <td>14631</td>
      <td>2021-07-05 19:12:06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>欧洲</td>
      <td>Europe</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>俄罗斯</td>
      <td>Russia</td>
      <td>964006</td>
      <td>5635294</td>
      <td>0.0</td>
      <td>5083441</td>
      <td>138579</td>
      <td>2021-07-05 19:12:06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>554367</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>辽宁省</td>
      <td>Liaoning</td>
      <td>210000</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
      <td>0</td>
      <td>2020-01-22 03:28:10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>554368</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>台湾</td>
      <td>Taiwan</td>
      <td>710000</td>
      <td>1</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>2020-01-22 03:28:10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>554369</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>Hongkong</td>
      <td>香港</td>
      <td>Hongkong</td>
      <td>810000</td>
      <td>0</td>
      <td>117.0</td>
      <td>0</td>
      <td>0</td>
      <td>2020-01-22 03:28:10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>554370</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>黑龙江省</td>
      <td>Heilongjiang</td>
      <td>230000</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
      <td>0</td>
      <td>2020-01-22 03:28:10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>554371</th>
      <td>亚洲</td>
      <td>Asia</td>
      <td>中国</td>
      <td>China</td>
      <td>湖南省</td>
      <td>Hunan</td>
      <td>430000</td>
      <td>1</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>2020-01-22 03:28:10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>554372 rows × 19 columns</p>
</div>




```python
cus = cdata.loc[cdata['countryName'] == '美国']
comfirm=cus.copy()
cus
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>continentName</th>
      <th>continentEnglishName</th>
      <th>countryName</th>
      <th>countryEnglishName</th>
      <th>provinceName</th>
      <th>provinceEnglishName</th>
      <th>province_zipCode</th>
      <th>province_confirmedCount</th>
      <th>province_suspectedCount</th>
      <th>province_curedCount</th>
      <th>province_deadCount</th>
      <th>updateTime</th>
      <th>cityName</th>
      <th>cityEnglishName</th>
      <th>city_zipCode</th>
      <th>city_confirmedCount</th>
      <th>city_suspectedCount</th>
      <th>city_curedCount</th>
      <th>city_deadCount</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>33718538</td>
      <td>0.0</td>
      <td>29096816</td>
      <td>605526</td>
      <td>2021-07-05 19:13:09</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>106</th>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>33716933</td>
      <td>0.0</td>
      <td>29096816</td>
      <td>605526</td>
      <td>2021-07-05 11:05:16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>308</th>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>33716933</td>
      <td>0.0</td>
      <td>29096816</td>
      <td>605526</td>
      <td>2021-07-05 10:01:19</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>437</th>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>33716933</td>
      <td>0.0</td>
      <td>29096816</td>
      <td>605526</td>
      <td>2021-07-05 09:39:02</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>728</th>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>33714928</td>
      <td>0.0</td>
      <td>29087421</td>
      <td>605524</td>
      <td>2021-07-04 21:41:13</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>541769</th>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>9</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>2020-02-03 09:28:34</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>543505</th>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>8</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>2020-02-02 07:41:43</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>545557</th>
      <td>北美洲</td>
      <td>North America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>6</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>2020-02-01 02:48:13</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>547090</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>6</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>2020-01-31 07:17:36</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>550409</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>美国</td>
      <td>United States of America</td>
      <td>971002</td>
      <td>5</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>2020-01-27 17:20:43</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>3241 rows × 19 columns</p>
</div>




```python
cus['updateTime'] = pd.to_datetime(cus.updateTime,format="%Y-%m-%d",errors='coerce').dt.date
cus.drop_duplicates(subset=['provinceName', 'updateTime'], keep='first', inplace=True)
cus.drop(['provinceName','continentName','continentEnglishName','countryEnglishName','cityName','province_deadCount','province_suspectedCount','city_deadCount','city_curedCount','city_suspectedCount','city_confirmedCount','province_confirmedCount','countryName','provinceEnglishName','province_zipCode','cityEnglishName','city_zipCode'],axis=1,inplace=True)
cus.dropna(subset=['province_curedCount'],inplace=True)
cus = cus.drop(cus[cus['province_curedCount']<0.01].index)
#cus = cus.drop(cus[cus['updateTime']<0.01].index)
cus = cus.sort_values(by='updateTime', ascending=True)
cus
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>province_curedCount</th>
      <th>updateTime</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>520054</th>
      <td>3</td>
      <td>2020-02-12</td>
    </tr>
    <tr>
      <th>517924</th>
      <td>3</td>
      <td>2020-02-13</td>
    </tr>
    <tr>
      <th>515650</th>
      <td>3</td>
      <td>2020-02-14</td>
    </tr>
    <tr>
      <th>514566</th>
      <td>3</td>
      <td>2020-02-15</td>
    </tr>
    <tr>
      <th>511738</th>
      <td>3</td>
      <td>2020-02-16</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3425</th>
      <td>29026688</td>
      <td>2021-07-01</td>
    </tr>
    <tr>
      <th>2131</th>
      <td>29052087</td>
      <td>2021-07-02</td>
    </tr>
    <tr>
      <th>1499</th>
      <td>29072881</td>
      <td>2021-07-03</td>
    </tr>
    <tr>
      <th>728</th>
      <td>29087421</td>
      <td>2021-07-04</td>
    </tr>
    <tr>
      <th>0</th>
      <td>29096816</td>
      <td>2021-07-05</td>
    </tr>
  </tbody>
</table>
<p>477 rows × 2 columns</p>
</div>




```python
cus['province_curedCount']
```




    520054           3
    517924           3
    515650           3
    514566           3
    511738           3
                ...   
    3425      29026688
    2131      29052087
    1499      29072881
    728       29087421
    0         29096816
    Name: province_curedCount, Length: 477, dtype: int64




```python
#cus.set_index('updateTime',inplace=True)
#cus.index.name = None
temp = cus['updateTime'].copy()
cout=0
temp_list = []
for i in temp:
    temp_list.append(str(i))
    cout+=1
cus["updateTime"]=temp_list
cus.set_index('updateTime',inplace=True)
cus
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>province_curedCount</th>
    </tr>
    <tr>
      <th>updateTime</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-02-12</th>
      <td>3</td>
    </tr>
    <tr>
      <th>2020-02-13</th>
      <td>3</td>
    </tr>
    <tr>
      <th>2020-02-14</th>
      <td>3</td>
    </tr>
    <tr>
      <th>2020-02-15</th>
      <td>3</td>
    </tr>
    <tr>
      <th>2020-02-16</th>
      <td>3</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2021-07-01</th>
      <td>29026688</td>
    </tr>
    <tr>
      <th>2021-07-02</th>
      <td>29052087</td>
    </tr>
    <tr>
      <th>2021-07-03</th>
      <td>29072881</td>
    </tr>
    <tr>
      <th>2021-07-04</th>
      <td>29087421</td>
    </tr>
    <tr>
      <th>2021-07-05</th>
      <td>29096816</td>
    </tr>
  </tbody>
</table>
<p>477 rows × 1 columns</p>
</div>




```python
fig, axes = plt.subplots(1,1,figsize=(16, 4))
x=cus.index
y=cus.values
plot=axes.plot(x,y,color=dt_hex,linewidth=2,linestyle='-',label='治愈')
axes.set_xticks(range(0,len(x),40))
plt.xlabel('日期',fontsize=10)
plt.ylabel('人数',fontsize=10)
axes.legend(loc=0,frameon=True)
plt.show()
```


![png](output_19_0.png)



```python
v_cus = cus.loc[cus.index > '2021-03-01']
v_cus.columns = ['curedCount']
v_cus
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>curedCount</th>
    </tr>
    <tr>
      <th>updateTime</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-03-02</th>
      <td>19817532</td>
    </tr>
    <tr>
      <th>2021-03-03</th>
      <td>19905322</td>
    </tr>
    <tr>
      <th>2021-03-04</th>
      <td>19997983</td>
    </tr>
    <tr>
      <th>2021-03-05</th>
      <td>20093442</td>
    </tr>
    <tr>
      <th>2021-03-06</th>
      <td>20183329</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2021-07-01</th>
      <td>29026688</td>
    </tr>
    <tr>
      <th>2021-07-02</th>
      <td>29052087</td>
    </tr>
    <tr>
      <th>2021-07-03</th>
      <td>29072881</td>
    </tr>
    <tr>
      <th>2021-07-04</th>
      <td>29087421</td>
    </tr>
    <tr>
      <th>2021-07-05</th>
      <td>29096816</td>
    </tr>
  </tbody>
</table>
<p>106 rows × 1 columns</p>
</div>




```python
v_vus=vus.loc[vus.index > '2021-03-01']
v_vus.index.name = "updateTime"
v_vus
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vaccinations</th>
    </tr>
    <tr>
      <th>updateTime</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-03-02</th>
      <td>105774893.0</td>
    </tr>
    <tr>
      <th>2021-03-03</th>
      <td>108010927.0</td>
    </tr>
    <tr>
      <th>2021-03-04</th>
      <td>110417023.0</td>
    </tr>
    <tr>
      <th>2021-03-05</th>
      <td>113492161.0</td>
    </tr>
    <tr>
      <th>2021-03-06</th>
      <td>117171344.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2021-06-30</th>
      <td>366017545.0</td>
    </tr>
    <tr>
      <th>2021-07-01</th>
      <td>367355455.0</td>
    </tr>
    <tr>
      <th>2021-07-02</th>
      <td>367983174.0</td>
    </tr>
    <tr>
      <th>2021-07-03</th>
      <td>368905977.0</td>
    </tr>
    <tr>
      <th>2021-07-04</th>
      <td>369524458.0</td>
    </tr>
  </tbody>
</table>
<p>125 rows × 1 columns</p>
</div>




```python
result = pd.merge( v_cus, v_vus, how='left',on='updateTime')
result.dropna(subset=['curedCount'],inplace=True)
result.dropna(subset=['vaccinations'],inplace=True)
result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>curedCount</th>
      <th>vaccinations</th>
    </tr>
    <tr>
      <th>updateTime</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-03-02</th>
      <td>19817532</td>
      <td>105774893.0</td>
    </tr>
    <tr>
      <th>2021-03-03</th>
      <td>19905322</td>
      <td>108010927.0</td>
    </tr>
    <tr>
      <th>2021-03-04</th>
      <td>19997983</td>
      <td>110417023.0</td>
    </tr>
    <tr>
      <th>2021-03-05</th>
      <td>20093442</td>
      <td>113492161.0</td>
    </tr>
    <tr>
      <th>2021-03-06</th>
      <td>20183329</td>
      <td>117171344.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2021-06-30</th>
      <td>29007495</td>
      <td>366017545.0</td>
    </tr>
    <tr>
      <th>2021-07-01</th>
      <td>29026688</td>
      <td>367355455.0</td>
    </tr>
    <tr>
      <th>2021-07-02</th>
      <td>29052087</td>
      <td>367983174.0</td>
    </tr>
    <tr>
      <th>2021-07-03</th>
      <td>29072881</td>
      <td>368905977.0</td>
    </tr>
    <tr>
      <th>2021-07-04</th>
      <td>29087421</td>
      <td>369524458.0</td>
    </tr>
  </tbody>
</table>
<p>105 rows × 2 columns</p>
</div>




```python
#result.to_csv("cvus.csv")
```


```python
pusdata=pd.read_csv(r'美国covid19疫情数据l-history改.csv')
pusdata['date'] = pd.to_datetime(pusdata.date,format="%Y-%m-%d",errors='coerce').dt.date
pusdata = pusdata.sort_values(by='date', ascending=True)
pusdata
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>death</th>
      <th>deathIncrease</th>
      <th>inIcuCumulative</th>
      <th>inIcuCurrently</th>
      <th>hospitalizedIncrease</th>
      <th>hospitalizedCurrently</th>
      <th>hospitalizedCumulative</th>
      <th>negative</th>
      <th>negativeIncrease</th>
      <th>onVentilatorCumulative</th>
      <th>onVentilatorCurrently</th>
      <th>positive</th>
      <th>positiveIncrease</th>
      <th>Unnamed: 14</th>
      <th>states</th>
      <th>totalTestResults</th>
      <th>totalTestResultsIncrease</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>419</th>
      <td>2020-01-13</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>418</th>
      <td>2020-01-14</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>417</th>
      <td>2020-01-15</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>416</th>
      <td>2020-01-16</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>415</th>
      <td>2020-01-17</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-03-03</td>
      <td>508665.0</td>
      <td>2449</td>
      <td>45214.0</td>
      <td>9359.0</td>
      <td>2172</td>
      <td>45462.0</td>
      <td>873073.0</td>
      <td>73857281.0</td>
      <td>267001</td>
      <td>4260.0</td>
      <td>3094.0</td>
      <td>28520365.0</td>
      <td>66836</td>
      <td>0.002349</td>
      <td>56</td>
      <td>357888671</td>
      <td>1406795</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-03-04</td>
      <td>510408.0</td>
      <td>1743</td>
      <td>45293.0</td>
      <td>8970.0</td>
      <td>1530</td>
      <td>44172.0</td>
      <td>874603.0</td>
      <td>74035238.0</td>
      <td>177957</td>
      <td>4267.0</td>
      <td>2973.0</td>
      <td>28585852.0</td>
      <td>65487</td>
      <td>0.002296</td>
      <td>56</td>
      <td>359479655</td>
      <td>1590984</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-03-05</td>
      <td>512629.0</td>
      <td>2221</td>
      <td>45373.0</td>
      <td>8634.0</td>
      <td>2781</td>
      <td>42541.0</td>
      <td>877384.0</td>
      <td>74307155.0</td>
      <td>271917</td>
      <td>4275.0</td>
      <td>2889.0</td>
      <td>28654639.0</td>
      <td>68787</td>
      <td>0.002406</td>
      <td>56</td>
      <td>361224072</td>
      <td>1744417</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-03-06</td>
      <td>514309.0</td>
      <td>1680</td>
      <td>45453.0</td>
      <td>8409.0</td>
      <td>503</td>
      <td>41401.0</td>
      <td>877887.0</td>
      <td>74450990.0</td>
      <td>143835</td>
      <td>4280.0</td>
      <td>2811.0</td>
      <td>28714654.0</td>
      <td>60015</td>
      <td>0.002094</td>
      <td>56</td>
      <td>362655064</td>
      <td>1430992</td>
    </tr>
    <tr>
      <th>0</th>
      <td>2021-03-07</td>
      <td>515151.0</td>
      <td>842</td>
      <td>45475.0</td>
      <td>8134.0</td>
      <td>726</td>
      <td>40199.0</td>
      <td>878613.0</td>
      <td>74582825.0</td>
      <td>131835</td>
      <td>4281.0</td>
      <td>2802.0</td>
      <td>28756489.0</td>
      <td>41835</td>
      <td>0.001457</td>
      <td>56</td>
      <td>363825123</td>
      <td>1170059</td>
    </tr>
  </tbody>
</table>
<p>420 rows × 18 columns</p>
</div>




```python
temp = pusdata['date'].copy()
cout=0
temp_list = []
for i in temp:
    temp_list.append(str(i))
    cout+=1
pusdata["date"]=temp_list
pusdata.set_index('date',inplace=True)
pusdata=pusdata[['positive']]
pusdata.index.name = "updateTime"
pusdata
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>positive</th>
    </tr>
    <tr>
      <th>updateTime</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-01-13</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2020-01-14</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-15</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-16</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2020-01-17</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2021-03-03</th>
      <td>28520365.0</td>
    </tr>
    <tr>
      <th>2021-03-04</th>
      <td>28585852.0</td>
    </tr>
    <tr>
      <th>2021-03-05</th>
      <td>28654639.0</td>
    </tr>
    <tr>
      <th>2021-03-06</th>
      <td>28714654.0</td>
    </tr>
    <tr>
      <th>2021-03-07</th>
      <td>28756489.0</td>
    </tr>
  </tbody>
</table>
<p>420 rows × 1 columns</p>
</div>




```python
#pd.merge( result,pusdata, how='left',on='updateTime').dropna(subset=['positive'],inplace=True)
```


```python
#confirm
comfirm['updateTime'] = pd.to_datetime(comfirm.updateTime,format="%Y-%m-%d",errors='coerce').dt.date
comfirm.drop_duplicates(subset=['provinceName', 'updateTime'], keep='first', inplace=True)
comfirm.drop(['provinceName','continentName','continentEnglishName','countryEnglishName','cityName','province_deadCount','province_suspectedCount','city_deadCount','city_curedCount','city_suspectedCount','city_confirmedCount','province_curedCount','countryName','provinceEnglishName','province_zipCode','cityEnglishName','city_zipCode'],axis=1,inplace=True)
comfirm.dropna(subset=['province_confirmedCount'],inplace=True)
comfirm = comfirm.drop(comfirm[comfirm['province_confirmedCount']<0.01].index)
comfirm = comfirm.sort_values(by='updateTime', ascending=True)
temp = comfirm['updateTime'].copy()
cout=0
temp_list = []
for i in temp:
    temp_list.append(str(i))
    cout+=1
comfirm["updateTime"]=temp_list
comfirm.set_index('updateTime',inplace=True)
comfirm.columns = ['confirmedCount']
comfirm
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>confirmedCount</th>
    </tr>
    <tr>
      <th>updateTime</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2020-01-27</th>
      <td>5</td>
    </tr>
    <tr>
      <th>2020-01-31</th>
      <td>6</td>
    </tr>
    <tr>
      <th>2020-02-01</th>
      <td>6</td>
    </tr>
    <tr>
      <th>2020-02-02</th>
      <td>8</td>
    </tr>
    <tr>
      <th>2020-02-03</th>
      <td>9</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2021-07-01</th>
      <td>33665047</td>
    </tr>
    <tr>
      <th>2021-07-02</th>
      <td>33679489</td>
    </tr>
    <tr>
      <th>2021-07-03</th>
      <td>33709176</td>
    </tr>
    <tr>
      <th>2021-07-04</th>
      <td>33714928</td>
    </tr>
    <tr>
      <th>2021-07-05</th>
      <td>33718538</td>
    </tr>
  </tbody>
</table>
<p>486 rows × 1 columns</p>
</div>




```python
result = pd.merge( result,comfirm, how='left',on='updateTime')
result.dropna(subset=['confirmedCount'],inplace=True)
result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>curedCount</th>
      <th>vaccinations</th>
      <th>confirmedCount</th>
    </tr>
    <tr>
      <th>updateTime</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-03-02</th>
      <td>19817532</td>
      <td>105774893.0</td>
      <td>28664448</td>
    </tr>
    <tr>
      <th>2021-03-03</th>
      <td>19905322</td>
      <td>108010927.0</td>
      <td>28719624</td>
    </tr>
    <tr>
      <th>2021-03-04</th>
      <td>19997983</td>
      <td>110417023.0</td>
      <td>28771556</td>
    </tr>
    <tr>
      <th>2021-03-05</th>
      <td>20093442</td>
      <td>113492161.0</td>
      <td>28827140</td>
    </tr>
    <tr>
      <th>2021-03-06</th>
      <td>20183329</td>
      <td>117171344.0</td>
      <td>28894787</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2021-06-30</th>
      <td>29007495</td>
      <td>366017545.0</td>
      <td>33653426</td>
    </tr>
    <tr>
      <th>2021-07-01</th>
      <td>29026688</td>
      <td>367355455.0</td>
      <td>33665047</td>
    </tr>
    <tr>
      <th>2021-07-02</th>
      <td>29052087</td>
      <td>367983174.0</td>
      <td>33679489</td>
    </tr>
    <tr>
      <th>2021-07-03</th>
      <td>29072881</td>
      <td>368905977.0</td>
      <td>33709176</td>
    </tr>
    <tr>
      <th>2021-07-04</th>
      <td>29087421</td>
      <td>369524458.0</td>
      <td>33714928</td>
    </tr>
  </tbody>
</table>
<p>105 rows × 3 columns</p>
</div>




```python
#result.to_csv("cvus.csv")
```

**完整文档详见：[博客相关资源-常微分偏微分建模练习](https://www.spiritlhl.top/ziyuan/)**
