# dev-helper-util

### NOTEBOOK-MI

#### WIFI
Use rfkill list to see the list of available wireless devices, the result will be something likeM

   rfkill list
        0: phy0: Wireless LAN
            Soft blocked: no
            Hard blocked: no
        1: acer-wireless: Wireless LAN
            Soft blocked: yes
            Hard blocked: no
        2: hci0: Bluetooth
            Soft blocked: no
            Hard blocked: no
then you will see that an acer wireless lan is also there and this loaded module is interrupting the correct one so you can deactivate it.

with this command you can see which Kernel modules are loaded lsmod | egrep '_acpi|_bluetooth|intel_oaktrail|_laptop|_rfkill|_wmi'

my result was:

acer_wmi               20480  0
dell_wmi_aio           16384  0
sparse_keymap          16384  2 dell_wmi_aio,acer_wmi
intel_lpss_acpi        16384  0
intel_lpss             16384  1 intel_lpss_acpi
mxm_wmi                16384  1 nouveau
wmi                    20480  4 dell_wmi_aio,acer_wmi,mxm_wmi,nouveau
video                  40960  3 i915_bpo,acer_wmi,nouveau
so the problem was the module called acer_wmi so I deactivate it sudo modprobe -r acer_wmi

then the problem was solved temporally but after a restart the module will be loaded again so as suggested in the comments the solution is to put it in a black list

Add to **/etc/modprobe.d/blacklist.conf** the following line blacklist **acer-wmi** and done.

_________________________________________________________________________________
### INSTALAR PACOTE DEB
dpkg -i file.deb

_________________________________________________________________________________
### MONGODB

mongodb - https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/

sudo service mongod start
sudo service mongod stop

_________________________________________________________________________________
### COLDFUSION

#### Webservice

* Configurar **https**

File: cfusion\wwwroot\WEB-INF\axis2.xml

**original**
```xml
<transportReceiver name="http"
                       class="coldfusion.xml.rpc.CFAxisServletListener"/>
```

**changed**
```xml
 <transportReceiver name="http" class="coldfusion.xml.rpc.CFAxisServletListener">
   <parameter name="port">80</parameter>
 </transportReceiver>


 <transportReceiver name="https" class="coldfusion.xml.rpc.CFAxisServletListener">
   <parameter name="port">443</parameter>
 </transportReceiver>
```

#### Schedule

File: cfusion\lib\neo-cron.xml
_________________________________________________________________________________
### SQL
https://docs.microsoft.com/pt-br/sql/linux/quickstart-install-connect-ubuntu

#### Check the status of mssql-server service.

systemctl status mssql-server


#### Stop mssql-server service.

sudo systemctl stop mssql-server 

#### Start mssql-server service.

sudo systemctl Start mssql-server


#### Stop and Disable mssql-server service.

sudo systemctl stop mssql-server

sudo systemctl disable mssql-server

#### Enable and start mssql-server service.

sudo systemctl enable mssql-server

sudo systemctl start mssql-server]
_________________________________________________________________________________
### Linux: Speed Config.: transferir arquivos via USB

http://belenos.me/linux-trava-transferencia-pendrive/?i=1

sudo gedit /etc/sysctl.conf	

#### Diminuir taxa de dirty pages
vm.dirty_background_ratio = 5
vm.dirty_ratio = 10

sudo sysctl -p	

_________________________________________________________________________________
### OPEN VPN

sudo apt-get install openvpn openssl

Para configurar o cliente VPN, basta que editem/criem o ficheiro /etc/openvpn/client.conf e realizem a seguinte configuração:

client
remote <ENDEREÇO_IP_SEVIDOR> 1194
ca ca.crt
cert client.crt
key client.crt
cipher DES-EDE3-CBC
comp-lzo
dev tun
proto udp
nobind
persist-key
persist-tun
user nobody
group nogroup


Nota: Os ficheiros ca.crt, client1.crt e client.crt foram criado no servidor e devem passados para o cliente. Para isso podem, por exemplo, usar o comando scp.

Depois de realizada a configuração basta que que iniciem o serviço carregado a configuração realizada anteriormente.

openvpn –config /etc/openvpn/client.conf
sudo openvpn --config /etc/openvpn/client.conf
sudo openvpn --config /etc/openvpn/client/seeaway.config


https://pplware.sapo.pt/linux/openvpn-como-configurar-um-cliente-vpn-no-ubuntu/
https://www.qnap.com/pt-pt/how-to/faq/article/como-se-conectar-ao-servidor-openvpn-via-arquivo-de-configura%C3%A7%C3%A3o-ovpn


_________________________________________________________________________________
### ZSNES - http://www.hardware.com.br/comunidade/zsnes-executar/1461137/
cd Documentos/snes/zsnes142/
LD_LIBRARY_PATH="/home/weslei/Documentos/zsnes142:$LD_LIBRARY_PATH" ./zsnes

1337

sudo apt-get install gcc-multilib g++-multilib
sudo apt-get install libao4 libc6 libgcc1 libgl1-mesa-glx libpng12-0 libsdl1.2debian libstdc++6 zlib1g

https://askubuntu.com/questions/53310/is-it-possible-to-install-zsnes-emulator-from-default-software-sources

https://askubuntu.com/questions/60094/compile-32-bit-on-64-bitsystem
./configure --build=x86_64-pc-linux-gnu --host=i686-pc-linux-gnu
https://www.vivaolinux.com.br/topico/C-C++/instalando-SDL-no-Ubuntu

### PPA

**Listar:**

```bash
grep -RoPish "ppa.launchpad.net/[^/]+/[^/ ]+" /etc/apt | sort -u | sed -r 's/\.[^/]+\//:/'
```

**Remover**

http://www.diolinux.com.br/2016/07/como-remover-programas-instalados-via-ppa-ubuntu.html
