
# ServerFanSpeedControl
Objective: 1U,2U rack server change fan speed

$\color{#FF3030}{UseByYourOwnRisk!!!}$  
Basic Information  
Server：Dell PowerEdge R720  
iDRAC Version：7  
Operation System(To manage Server)：CentOS 8 / Windows  

1.Enable IPMI Control  
==================================================================
iDRAC 7  
1.Enter System Setup  
Use F2 to enter System Setup UI  
![System Setup](https://img-blog.csdnimg.cn/20210417133901664.png)  
2.Enter iDRAC Settings  
![iDRAC Settings](https://img-blog.csdnimg.cn/20210417134254886.png)  
3.Set IPMI enabled  
![IPMI](https://user-images.githubusercontent.com/128015241/225513933-2f728997-fc7c-417c-9d30-e5df5ad3cc61.png)  
Select Network,Set IPMI become enabled  
![Network](https://user-images.githubusercontent.com/128015241/225514012-3003977c-2961-4683-8d8c-8c615b6ed679.png)  


2.Adjust Fan Speed  
==================================================================
CentOS 8 User  
1.Install IPMI  


```
yum install epel-release -y 
yum install ipmitool -y  
```

2.Check IPMI is installed   
```rpm -qa | grep ipmi```  
![IPMI](https://img-blog.csdnimg.cn/20210417135438275.png)  

3.Change Fan Speed  
  
```ipmitool -I lanplus -U ipmiUsername -P ipmiPassword -H ServerAddress raw 0x30 0x30 0x01 0x00```
ipmiUsername：Username to login iDRAC，default = root   
ipmiPassword：Password to login IDRAC，default = calvin  
ServerAddress： IP of IDRAC address，NOT the operation systemIP or other VMs IP   

0x00 = DISABLE manual operation  
0x01 ENABLE manual operation  

3.2 Change Spped  
```
ipmitool -I lanplus -U ipmiUsername -P ipmiPassword -H ServerAddress raw 0x30 0x30 0x02 0xff 0x18   
```

0xff = Apply to ALL fan  
0x18 = Percentage of the fan operate，The orginal Fan should be 12000rpm。0x18 is eqaul to 24 in decimal number.  

0xff 0x18 = All Fan will operate in 24% of rpm, you can change 0xff to adjust each fan.  

10% should be suitable for not high load operation (i.e. NAS or HomeAssistant)  




==================================================================
Windows User  
You can use software by @cw1997  

https://github.com/cw1997/dell_fans_controller/releases/  


User：IDRAC LoginName，default = root  
Password：IDRAC password，default = calvin  
IP： iDRAC server IP，NOT VMs IP  
 




