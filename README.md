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

## Instalación de Apache Zeppelin — ÚNICAMENTE en la máquina Master

Este paso es el que distingue a la máquina Master del resto de nodos: **la instalación de Zeppelin se realiza exclusivamente aquí**, dado que actúa como el notebook desde el cual se redacta y ejecuta el código que luego se distribuye a los workers.

```
cd ~/software
wget https://downloads.apache.org/zeppelin/zeppelin-0.12.1/zeppelin-0.12.1-bin-all.tgz
tar -xzf zeppelin-0.12.1-bin-all.tgz
sudo mv zeppelin-0.12.1-bin-all /opt/zeppelin
```

Establecer las variables de entorno:

```
export ZEPPELIN_HOME=/opt/zeppelin
export PATH=$ZEPPELIN_HOME/bin:$PATH
```

---

## Configuración de Zeppelin para vincularse al cluster — únicamente en el Master

```
cd $ZEPPELIN_HOME/conf
cp zeppelin-env.sh.template zeppelin-env.sh
cp zeppelin-site.xml.template zeppelin-site.xml
nano zeppelin-env.sh
```

Definir lo siguiente:

```
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export SPARK_HOME=/opt/spark
export MASTER=spark://<IP_DEL_MASTER>:7077
```

En el archivo `zeppelin-site.xml`, permitir que el servidor quede a la escucha en todas las interfaces de red:

```
<property>
  <n>zeppelin.server.addr</n>
  <value>0.0.0.0</value>
</property>
```

---

## Puesta en marcha de Zeppelin — exclusivamente en el Master

```
$ZEPPELIN_HOME/bin/zeppelin-daemon.sh start
```

Comprobar que el proceso esté activo:

```
jps
```

Ingresar desde el navegador de cualquier equipo:

```
http://<IP_DEL_MASTER>:8080
```
<img width="1917" height="397" alt="image" src="https://github.com/user-attachments/assets/34bd01ea-a544-45a0-82a2-8ba55cad9c41" />


---

## Verificación final del cluster distribuido

Desde un notebook nuevo dentro de Zeppelin, correr:

```
%spark
val datos = spark.range(1000000)
printf("Total: %d%n", datos.count())
```

Si el cálculo se ejecuta sin errores y, al inspeccionar la Spark Application UI ( `http://<IP_DEL_MASTER>:4040` ), se aprecian tareas corriendo en paralelo sobre ambos workers, esto confirma que el cluster distribuido está operando correctamente.

*(En este punto puede añadirse una captura de la Spark UI mostrando las tareas ejecutándose en ambos workers)*

---

## Resumen de tareas por equipo

| Componente | Máquina Master | Máquina Worker 1 | Máquina Worker 2 | Máquina Worker 3 |
|---|---|---|---|---|
| Java | Sí | Sí | Sí | Sí |
| Apache Spark | Sí | Sí | Sí | Sí |
| Apache Zeppelin | Sí (única instalación) | No | No | No |
| Proceso `Master` | Sí | No | No | No |
| Proceso `Worker` | opcional | Sí | Sí | Sí |

## Intento de conexión:
### Configuracion del router
Como nuestro router no tenia para poder conectarce al internet, nos toco preguntar como hacerlo a la inteligencia artificial para que nos podamos conectar por el network y de esa manera conectar las computadoras.
Primero entramos a esta ip 192.168.1.1
y nos dio la entra para entrar por usuario y contraseña al router conectado al cnt en el google edge
<img width="1600" height="899" alt="image" src="https://github.com/user-attachments/assets/a1ff20e2-bb76-4667-a766-a1ed9762fc79" />
Esta es la intergase nos salio errores probamos con 
admin com usuario y como contraseña y nos dio errores, asi que decidimmos probar la coneccion al land buscamos la ipconfig
en el cmd y nos dio diversas ip con el default Switch.

Con esa informacion usamos la convinacion de teclas windows + r en el cual escribimos ncpa.cpl

<img width="1390" height="703" alt="image" src="https://github.com/user-attachments/assets/00cf9d54-1b4c-4325-9e1e-175cbf5f1df1" />

En este apartado estamos configurando unos exports para que todos los workers tengan la misma configuracion a parte de la configuracion de spark

<img width="1222" height="127" alt="image" src="https://github.com/user-attachments/assets/9d210fad-e01b-491b-89d1-90b285d2ac3a" />

Despues de haber ejecutado cada configuracion en las diferentes compus ahora revisamos que esten trabajando los 3 workers en la computadora master

<img width="1130" height="592" alt="image" src="https://github.com/user-attachments/assets/8820a057-dd93-415d-8871-e5d7ef3a657d" />

Luego con un csv acerca de un conjunto de datos sobre precios e ingresos del mercado de los videojuegos fuimos probando para saber cuanto consume y en cuanto tiempo esta el proceso

<img width="1191" height="617" alt="image" src="https://github.com/user-attachments/assets/9224e0db-5bd9-4a94-933b-991f1712488d" />

Debido a la poca cantidad de memoria ram tuvimos un resultado de 7 min pero a la hora de actualizar y revisar de nuevo fue aumentando el tiempo por la poca cantidad de memoria que teniamos 

<img width="1135" height="586" alt="image" src="https://github.com/user-attachments/assets/51e3ae2c-450d-480d-8b28-1d07b2c7158a" />

<img width="1132" height="592" alt="image" src="https://github.com/user-attachments/assets/0ce36ef3-cc3c-4dfa-9d14-d306da345fea" />

En la compu de mi compañero adnres vallejo debido a la poca cantidad de memoria que tiene utilizo 

<img width="1007" height="630" alt="image" src="https://github.com/user-attachments/assets/1e9eca2d-0d11-4d8a-99a1-36f1fde4b090" />




