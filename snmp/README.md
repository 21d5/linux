##SNMP��ʲô
SNMP�ǻ���TCP/IPЭ�������������׼������ǰ���Ǽ����ؼ��Э��(SGMP)��������ͨ����·���й���
##Net-SNMP
Net-SNMP��һ����ѵġ�����Դ���SNMPʵ��.[����](http://www.net-snmp.org/)
##����
    wget http://sourceforge.net/projects/net-snmp/files/net-snmp/5.3.3/net-snmp-5.3.3.tar.gz
##��װ����
��װ֮ǰȷ��libtool��openssl��zlib����Ѿ���װ

    gunzip net-snmp-5.3.3.tar.gz
    tar -xvf net-snmp-5.3.3.tar
    cd net-snmp-5.3.3
    ./configure --prefix=/usr/local/net-snmp --enable-mfd-rewrites --with-default-snmp-version="2" --with-sys-location="China" --with-sys-contact="Email:xxxx@xxxx.com" --with-logfile="/usr/local/net-snmp/log/snmpd.log"  --with-persistent-directory="/var/net-snmp"
ע�ͣ�

* prefix��net-snmp��Ҫ��װ��·����
* enable-mfd-rewrites���������µ�MFD��д���õ�midģ��
* with-default-snmp-version��Ĭ�ϵ�SNMP�汾
* with-sys-contact���������ø��豸����ϵ��
* with-sys-location�����豸��λ��
* with-logfile����־�ļ�·��
* with-persistent-directory���������ݴ洢Ŀ¼
 
##���밲װ

    make && make install

##����snmpd.conf

����snmpd.conf�ļ�
�������ǰ�Դ�ļ��е�EXAMPLE.conf�ļ����Ƶ�/usr/local/net-snmp/share/snmpĿ¼�²�����Ϊsnmp.conf

    cp EXAMPLE.conf /usr/local/net-snmp/share/snmp/snmpd.conf
    
�༭snmp.conf�ļ�

```
#       sec.name  source          community ���������
#(sec.name����ȫ������ 
#source�������������Դ����IPЭ���У����������IP��ַ����net-snmp����������ԴIP���Կ��ƣ���������Բ���SNMP�涨�ģ���net-snmp��չ�� .
#community����ͬ������ )
#ԭ����
com2sec local     localhost       COMMUNITY
com2sec mynetwork NETWORK/24      COMMUNITY

#�޸ĺ��
com2sec local     localhost       public
com2sec mynetwork 192.168.8.30   public
com2sec mynetwork 192.168.11.29   public
```
##����net-snmp������     
��/etc/rc.local�ļ�ĩβ�������´���    -c���������������ļ�����

    /usr/local/net-snmp/sbin/snmpd -c /usr/local/net-snmp/share/snmp/snmpd.conf &  
##���û�������     
��/etc/profileĩβ�������´���    

    PATH=/usr/local/net-snmp/bin:/usr/local/net-snmp/sbin:$PATH
    
ʹ��������������Ч  

    source /etc/profile    
##����snmp

    /usr/local/net-snmp/sbin/snmpd -d     
    #�鿴�����Ƿ�����     
    Netstat -na | grep 161 
    #snmpʹ�õĶ˿�161
    
##����

    snmpwalk -v 2c -c public localhost if
    #������������Ϣ������ȷ����snmpd����
    IF-MIB::ifIndex.1 = INTEGER: 1
    IF-MIB::ifIndex.3 = INTEGER: 3
    IF-MIB::ifIndex.4 = INTEGER: 4
    IF-MIB::ifIndex.5 = INTEGER: 5
    IF-MIB::ifIndex.6 = INTEGER: 6
    IF-MIB::ifDescr.1 = STRING: lo
    IF-MIB::ifDescr.3 = STRING: eth0
    IF-MIB::ifDescr.4 = STRING: eth1
    IF-MIB::ifDescr.5 = STRING: sit0
    IF-MIB::ifDescr.6 = STRING: usb0
