# 小额贷款的划算问题(matlab数学建模)

## 代码

```matlab
clc;close all;

%% 获取数据

day_i=3/10000 %日息

number_of_periods_i=day_i*30 %月利率

P=10000 %借款金额

N = 12 %还款期数

%% 模型选取及计算

Amount=P %借款金额

rate=number_of_periods_i %月利率

number_of_periods=N %贷款期数

command=input(‘输入0为等额本金还款，输入1为等额本息还款，输入2为等本等息还款，输入3为先息后本还款： ‘)

if command==0

%等额本金还款方式

M=Amount/number_of_periods %每月偿还本金

for j=1:number_of_periods-1

Amount(j+1)=Amount(j)-M %月初余额

end

R=Amount*rate %月底的利息

debt=Amount+R %月底欠款

M1=M+Amount*rate %月还款额

sum_R=sum(R) %总利息

sum_Amount=sum(M1) %还款总额

M=M-zeros(1,number_of_periods)

data=[Amount’,R’,debt’,M’,M1’]%月初所欠金额 利息额 月末所欠金额 偿还本金 月末付款

[n,m]=size(data)

result= cell(n+1,m)

result(1,:)={‘月初所欠金额’,’利息额’,’月末所欠金额’,’偿还本金’,’月末付款’}

result(2:end,:) = num2cell(data)

xlswrite(‘data.xlsx’,result,’等额本金还款方式’)%输出excel

fprintf(‘sum_R=%f’,sum_R)

fprintf(‘sum_Amount=%f’,sum_Amount)

elseif command==1

%等额本息还款方式

M=Amount*(rate*(1+rate)^number_of_periods)/((1+rate)^number_of_periods-1)%月还款额

for i=1:number_of_periods-1

Amount(i+1)=Amount(i)*(1+rate)-M %月初金额

end

R=Amount*rate %月底的利息

mon=M-R %每月偿还本金

debt=Amount+R %月底欠款

sum_R=sum(R) %总利息

sum_Amount=M*number_of_periods %还款总额

M=M-zeros(1,number_of_periods)

data=[Amount’,R’,debt’,M’-R’,M’]%月初所欠金额 利息额 月末所欠金额 偿还本金 月末付款

[n,m]=size(data)

result= cell(n+1,m)

result(1,:)={‘月初所欠金额’,’利息额’,’月末所欠金额’,’偿还本金’,’月末付款’}

result(2:end,:) = num2cell(data)

xlswrite(‘data.xlsx’,result,’等额本息还款方式’)%输出excel

fprintf(‘sum_R=%f’,sum_R)

fprintf(‘sum_Amount=%f’,sum_Amount)

elseif command==2

%等本等息还款方式

M=Amount/number_of_periods %月还本金

for i=1:number_of_periods-1

Amount(i+1)=Amount(i)-M %月初金额

end

R=[P*rate,P*rate,P*rate,P*rate,P*rate,P*rate,P*rate,P*rate,P*rate,P*rate,P*rate,P*rate] %月底的利息

debt=Amount+R %月底欠款

M2=M+R %月还款额

sum_R=sum(R) %总利息

sum_Amount=M*number_of_periods+P*rate*number_of_periods %还款总额

M=M-zeros(1,number_of_periods)

data=[Amount’,R’,debt’,M’-R’,M2’]%月初所欠金额 利息额 月末所欠金额 偿还本金 月末付款

[n,m]=size(data)

result= cell(n+1,m)

result(1,:)={‘月初所欠金额’,’利息额’,’月末所欠金额’,’偿还本金’,’月末付款’}

result(2:end,:) = num2cell(data)

xlswrite(‘data.xlsx’,result,’等本等息还款方式’)%输出excel

fprintf(‘sum_R=%f’,sum_R)

fprintf(‘sum_Amount=%f’,sum_Amount)

elseif command==3

%先息后本还款方式

M=0 %月还本金

for i=1:number_of_periods-1

Amount(i+1)=Amount(i)-M %月初金额

end

R=Amount*rate %月底的利息

debt=Amount+R %月底欠款

M3=R %月还款额

sum_R=sum(R) %总利息

M4=debt

sum_Amount=P+sum_R %还款总额

last_number_of_periods_debt=P+P*rate %最后一个月还款金额

M=M-zeros(1,number_of_periods)

data=[Amount’,R’,debt’,M’,M3’]%月初所欠金额 利息额 月末所欠金额 偿还利息 月末付款

[n,m]=size(data)

result= cell(n+1,m)

result(1,:)={‘月初所欠金额’,’利息额’,’月末所欠金额’,’偿还本金’,’月末付款利息’}

result(2:end,:) = num2cell(data)

xlswrite(‘data.xlsx’,result,’先息后本还款方式’)%输出excel

fprintf(‘sum_R=%f’,sum_R)

fprintf(‘sum_Amount=%f’,sum_Amount)

end
```

## 源文档

**完整文档详见：[博客相关资源-小额贷款的划算问题](https://www.spiritlhl.top/ziyuan/)**
