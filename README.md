# Digital-Communications
I implement my work here
clc
close all
v = [48,46,44,42,40,38,36,34,32,30,28,26,24,23,22,21];
p = [29.125,30.5625,31.9375,33.5,35.3125,37,39.3125,41.625,44.1875,47.0625,50.3125,54.3125,58.8125,61.3125,64.0625,67.0625];
plot(v,p)
figure(1)
title('Voulme vs Pressure','FontSize',12,'FontWeight',"bold");
xlabel('Volume','FontSize',12,'FontWeight',"bold");
ylabel('Pressure','FontSize',12,'FontWeight',"bold");
figure(2)
stem(v,p); % plot of sampled signal
title('SAMPLED SIGNAL','FontSize',12,'FontWeight',"bold");
xlabel('Volume','FontSize',12,'FontWeight',"bold");
ylabel('Pressure','FontSize',12,'FontWeight',"bold");
n1=5;%number of bits per sample
L=2^n1;%number of quantization levels
xmax=1.3e+08;%max value of quantization vector
xmin=0;%min value of quantization vector
del=(xmax-xmin)/L; %Difference between each quantization level
partition=xmin:del:xmax% definition of decision lines
codebook=xmin-(del/2):del:xmax+del/2; % definition of representation levels
[index,quants]=quantiz(p,partition,codebook);%quantiz is inbuilt function which is used to quantize the signal
% gives rounded off values of the samples
figure(3)
set(gca,'Fontsize',8,'Fontweight','bold')
stem(quants,"color",'r');%plotting of quantized signal
title('QUANTIZED SIGNAL');
xlabel('Volume','FontSize',8,'FontWeight',"bold");
ylabel('Pressure','FontSize',8,'FontWeight',"bold");
 % NORMALIZATION
l1=length(index); % to convert 1 to n as 0 to n-1 indicies
for i=1:l1
if (index(i)~=0)
index(i)=index(i)-1;
end
end
l2=length(quants);
for i=1:l2 % to convert the end representation levels within the range.
if(quants(i)==xmin-(del/2))
quants(i)=xmin+(del/2);
end
if(quants(i)==xmax+(del/2))
quants(i)=xmax-(del/2);
end
end
% ENCODING
code=de2bi(quants,'left-msb'); % Decimal to binary conversion of quantization vector
k=1;
for i=1:l1 % to convert column vector to row vector
for j=1:n1
coded(k)=code(i,j);
j=j+1;
k=k+1;
end
i=i+1;
end
figure(4);
stairs(coded);%plotting digital signal
xlim([0,400])
ylim([-1 2])
%plot of digital signal
title('DIGITAL SIGNAL');
set(gca,'Fontsize',8,'Fontweight','bold')
xlabel('volume','FontSize',8,'FontWeight',"bold");
ylabel('pressure','FontSize',8,'FontWeight',"bold");
%Demodulation
code1=reshape(coded,n1,(length(coded)/n1));
index1=bi2de(code1,'left-msb');
resignal=del*index+xmin+(del/2);%decoding the binary sequence
figure(5);%plot of demodulated signal compared to original signl
subplot(2,1,1)%plot of demodulated signal
plot(v,resignal,"color",'k');
set(gca,'Fontsize',8,'Fontweight','bold')
title('DEMODULATAED SIGNAL');
xlabel('volume','FontSize',8,'FontWeight',"bold");
ylabel('pressure','FontSize',8,'FontWeight',"bold");
subplot(2,1,2)
plot(v,p,"color",'m');%plot of original signal
set(gca,'Fontsize',8,'Fontweight','bold')
title('ORIGINALSIGNAL');
xlabel('volume','FontSize',8,'FontWeight',"bold");
ylabel('pressure','FontSize',8,'FontWeight',"bold");
