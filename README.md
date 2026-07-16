# Informe-Multipass
Se comprobo q si se tiene instalado multi pass

Sin embargo al querer usar el cmd para crear una virtual machine de 24.04 no dejo pero se esta a la espera de abrirse desde multipasmismo.

Aparentemente ha sido éxito la creación del multipas 24.04.

Uso de sudo apt update

Tras usar el apt update parece q existen pendientes que son el kernal 

<img width="975" height="430" alt="image" src="https://github.com/user-attachments/assets/c52cea9d-23b1-492a-98e0-227e62d671d3" />

La solución parece mas cerca de lo esperado usando el sudo reboot para reiniciar la virtual machine por el mensaje mostrado de que necesita ser reiniciado.

Tras el reinicio la actualización del kernal ha sido un total éxito ya se tiene el kernal esperado el 6.8.0-134-generic

<img width="850" height="114" alt="image" src="https://github.com/user-attachments/assets/b15c2250-e9b2-41f9-9763-a8b01ebc96b2" />

Ahora se seguirá con el procedimiento de la instalacio de las herramientas básicas

La primera ejecución es el  
```bash

sudo apt install -y wget curl tar nano unzip net-tools openssh-client openssh-server

```

Ha sido completamente exitosa sin la necesidad de reiniciar algo de la maquina virtual ya que el kernel ya lo hemos actualizado.

Ahora seguiremos con la instalación de java Para un laboratorio estable se puede usar OpenJDK 17.

```bash

sudo apt install -y openjdk-17-jdk

java -version

```

Tras haber realizado este proceso nos percatamos que no teníamos Windows pro así q decidimos activarlo de la cmd con los siguientes comandos:

sc config wuauserv start= auto & net start wuauserv

changepk.exe /productkey VK7JG-NPHTM-C97JM-9MPGT-3V66T

Para que despues de que se reinicie correcatamente ejecutar el siguiente comando en el powershell:
irm https://get.activated.win/ | iex
Que nos dara la sigueinte imagen 

<img width="975" height="860" alt="image" src="https://github.com/user-attachments/assets/a0bbbf73-025e-4c1d-97e0-861aecb5de90" />

Con esa imagen usamos el método de activación HWID que es la tecla 1, Para que trans esperar se nos ejecutara lo siguiente y ya tendremos Windows pro

Tras solucionar el problema del Windows, nos toco desistalar y reinstlar el multipass para que se pueda instalr el hiper-V.

Ahora antes de reinstalar el multipass usamos el comando en el powershell: Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All, para activar el Hiper-v.

Luego de que se haya reiniciado reinstalamos las herramientas y java.

Se debe ejecutar el comando: nano ~/.bashrc

Y al final poner:

export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64

export PATH=\$JAVA_HOME/bin:\$PATH

Para que se peuda instalra en el java_home luego de eso se aplican los cambios con este código  
source ~/.bashrc

printf "JAVA_HOME=%s\\n" "\$JAVA_HOME"

Ahora se intala spark

Crear directorio de software:

```bash

mkdir -p ~/software

cd ~/software

```

Descargar Spark. Ajustar el nombre del archivo si en la página oficial aparece una versión más reciente:

```bash

wget <https://downloads.apache.org/spark/spark-4.1.2/spark-4.1.2-bin-hadoop3.tgz>

```

Intento de conexión:
