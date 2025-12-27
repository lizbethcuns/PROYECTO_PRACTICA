### PASOS PREVIOS DE LA PRACTICA
Primero se procede instalando ROS-based dependencies, siguiendo los pasos del siguiente repositorio https://github.com/widegonz/unitree-go2-ros2 

```bash
sudo apt install ros-humble-gazebo-ros2-control
sudo apt install ros-humble-xacro
sudo apt install ros-humble-robot-localization
sudo apt install ros-humble-ros2-controllers
sudo apt install ros-humble-ros2-control
sudo apt install ros-humble-velodyne
sudo apt install ros-humble-velodyne-gazebo-plugins
sudo apt-get install ros-humble-velodyne-description
```

```bash
sudo apt install -y python3-rosdep
rosdep update
mkdir -p go2_cun/src
cd go2_cun/src
git clone https://github.com/widegonz/unitree-go2-ros2.git
cd ~/go2_cun
rosdep install --from-paths src --ignore-src -r -y
```
construir mi workspace

```bash
cd ~/go2_cun
colcon build
source ~/go2_cun/install/setup.bash
```
##                                  VERIFICACION DE LA INSTALACION
```bash
lizbeth@lizbeth-Laptop:~/go2_cun$ echo $COLCON_PREFIX_PATH
/home/lizbeth/go2_cun/install:/home/lizbeth/turtlebot4_ws/install
lizbeth@lizbeth-Laptop:~/go2_cun$ 
```
### VERIFICACION 

```bash
/go2_cun$ ros2 pkg list | grep champ
ros2 pkg list | grep go2
```

Aparecera algo como

```bash
champ
champ_base
champ_bringup
champ_config
champ_description
champ_gazebo
champ_msgs
champ_navigation
champ_teleop
go2_config
go2_description
```

Aunque el repositorio se llama: unitree-go2-ros2, NINGÚN paquete ROS se llama unitree_*.

Paquetes del framework CHAMP (9 paquetes) (control de robots cuadrúpedos) : 

```bash
champ - Framework base
champ_base - Controlador base
champ_bringup - Lanzadores
champ_config - Configuraciones
champ_description - Descripciones URDF
champ_gazebo - Integración con Gazebo
champ_msgs - Mensajes personalizados
champ_navigation - Navegación
champ_teleop - Teleoperación
```

Paquetes específicos del GO2 (2 paquetes) Paquetes del Go2 :

```bash
go2_config - Configuración del GO2
go2_description - Descripción URDF del GO2
```

Estado actual:    
- Workspace go2_cun creado correctamente;
- 11 paquetes compilados exitosamente;
- Sourced correctamente;
- Paquetes disponibles en ROS2;

Por eso la mayoría de paquetes son champ_*, no unitree_*.

### Qué es CHAMP   

CHAMP es un:

framework de locomoción para robots cuadrúpedos

El Go2 usa CHAMP para:

- Control de patas;
- gait (caminar);
- simulación en Gazebo;
- teleoperación;

👉 Por eso la mayoría de paquetes son champ_*, no unitree_*.

### Problemas previos que se tuvo despues de ejecutar el gazebo  
Ejecute 
```bash
ros2 launch go2_config gazebo.launch.py
```
Funcionó → Gazebo se abrió

luego ejecute 

```bash
ros2 launch go2_config gazebo.launch.py rviz:=true
```
y aparecio Package 'go2_config' not found, esto se debe a que ROS solo buscó en 

```bash
/home/lizbeth/turtlebot4_ws/install/...
/opt/ros/humble
```

 y NO buscó en 

 ```bash
 ~/go2_cun/install
```

## Solucion Temporal :

```bash
source ~/go2_cun/install/setup.bash
```

Luego verifica:

```bash
ros2 pkg list | grep go2
```

Debe salir:

```bash
go2_config
go2_description
```

Y ahora sí ejecuta:

```bash
ros2 launch go2_config gazebo.launch.py rviz:=true
```

# Regla de oro 

❗ Cada terminal nueva necesita su source ya que ROS no recuerda workspaces entre terminales

# Solucion 1 Permanente para evitar que vuelva a pasar (opcional)
Edita tu ~/.bashrc y deja el orden correcto:

```bash
source /opt/ros/humble/setup.bash
source ~/turtlebot4_ws/install/setup.bash
source ~/go2_cun/install/setup.bash
```
El último tiene prioridad → Go2 queda activo.

```bash
source ~/.bashrc
```
# Solucion 2 Permanente para evitar que vuelva a pasar (MAS RECOMENDADA LA QUE APLIQUE)

```bash
cd ~/go2_cun
```
Abre el archivo (si no lo tienes abierto ya):

```bash
nano ~/.bashrc
```
Usa las flechas del teclado para bajar hasta el final. Edita para que este bloque de abajo y trata de que tu archivo se vea igual. 
## Esto debe de aparecer en las ultimas lineas del nano

# ... (todo lo de arriba déjalo igual) ...
```bash
source /opt/ros/humble/setup.bash
export PATH="$HOME/.local/bin:$PATH"

# source ~/turtlebot4_ws/install/setup.bash
source ~/go2_cun/install/setup.bash
```

(Pon un # delante de la línea del turtlebot. Esto lo "apaga" sin borrarlo (se vuelve un comentario).)

# Guarda los cambios:

    Presiona Ctrl + O (la letra O de oso).

    Presiona Enter (para confirmar el nombre del archivo).

    Presiona Ctrl + X (para salir).

# Aplica los cambios: Para que la terminal se entere de lo que acabas de hacer, escribe esto en la terminal:

```bash
source ~/.bashrc
```

## Verificación total 

```bash
echo $COLCON_PREFIX_PATH
```

Debe salir:

```bash
/home/lizbeth/go2_cun/install
```

Y tambien ejecutar

```bash
ros2 pkg list | grep go2
```

Debe listar:

```bash
go2_config
go2_description
```

# A partir de ahora    

Cada terminal nueva:

- Ya tiene ROS Humble;
- Ya tiene GO2;
- NO tendrás que hacer source manual, Cuando quieras turtlebot;

## luego de haber arreglado con la solucion permanente puedo iniciar asi

Ir al workspace 

```bash
cd ~/go2_cun
```

Lanza Gazebo (terminal 1)

```bash
ros2 launch go2_config gazebo.launch.py
```

y ejecutar los demas terminales... 
Para poder ver mas informacion abrir el siguiente archivo que se encuentra en la carpeta de UnitreeGo2 


### PARA EL LIDDAR 2D
Yo trabajare con el LIDDAR 2D por lo cual primero es 

PASO 1: VER QUÉ TIENES REALMENTE

```bash
cd ~/go2_cun
ls
```
Debes ver algo como:


```bash
src  install  build  log

```
PASO 2: BUSCAR go2_description DE VERDAD

```bash
find ~/go2_cun/src -name go2_description
```

Si todo está bien, verás algo como:

```bash
/home/lizbeth/go2_cun/src/unitree-go2-ros2/robots/descriptions/go2_description
```

PASO 3: IR A LA CARPETA CORRECTA
ejecuta 

```bash
cd ~/go2_cun/src/unitree-go2-ros2/robots/descriptions/go2_description/xacro
```
luego

```bash
ls
```

Ahí SÍ debes ver archivos como:

```bash
robot_VLP.xacro
velodyne.xacro
laser.xacro
```

PASO 4: AHORA SÍ EDITAR EL ARCHIVO

```bash
nano robot_VLP.xacro
```

Y aquí haces el cambio o se tiene que verificar que las ultimas lineas del codigo esten asi:

```bash
<!-- <xacro:include filename="$(find go2_description)/xacro/velodyne.xacro"/> -->
<xacro:include filename="$(find go2_description)/xacro/laser.xacro"/>
```

Reconstruir el workspace
¡Tengo una excelente noticia! ¡Tu archivo ya está configurado correctamente! 🎉

Si miras al final del código que me acabas de mandar, verás esto:

```bash
XML

    <xacro:include filename="$(find go2_description)/xacro/laser.xacro"/>
```

    La línea del Velodyne tiene `` alrededor. (Esto significa que está APAGADO / Comentado).

    La línea del Laser (Hokuyo) NO tiene flechas alrededor. (Esto significa que está ENCENDIDO).

Así que no tienes que escribir nada más en el archivo. Ya está listo para usar el láser 2D.
Sigue estos pasos para probarlo ahora mismo:
1. Salir de Nano

Como el archivo ya está bien, solo sal sin guardar (o guarda, da igual porque no hiciste cambios):

    Presiona Ctrl + X.

2. Reconstruir el espacio de trabajo

```bash
cd ~/go2_cun
colcon build
source install/setup.bash
```
Ejecuta el comando para abrir Gazebo y RViz:
Bash

```bash
ros2 launch go2_config gazebo_velodyne.launch.py rviz:=true
```
En primera instancia no se va a visualizar el LIDAR , por lo cual hay que hacer los siguientes cambios que se visualizan en el video, como parte adicional se agrega un cilindro para poder comprobar de mejor manera los resultados.

https://github.com/user-attachments/assets/df0f7599-941f-4899-b5a0-d9b4385ae425

### Cambiar el mundo 
Empecemos por configurar y lanzar el mundo "factory" (tu asignado). Esto es el paso inicial para todo: simulación en el entorno correcto, con LiDAR y RViz. Después, pasamos a mapeo (Parte A),

# Configura el GAZEBO_MODEL_PATH

```bash
echo 'export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/home/lizbeth/go2_cun/src/unitree-go2-ros2/robots/configs/go2_config/models/' >> ~/.bashrc
```
# Lanza la Simulación con el Mundo "Factory" y LiDAR/RViz:

```bash
source ~/.bashrc
```

# Lanza la Simulación con el Mundo "Factory" y LiDAR/RViz:

```bash
ros2 launch go2_config gazebo_velodyne.launch.py rviz:=true world:=factory
```
# ------------ USE ESTE CODIGO ------------------
Lanzar el simulador con el mundo deseado y LiDAR habilitado:

```bash
ros2 launch go2_config gazebo_velodyne.launch.py world:=factory
```

# ------------ empezar de 0 ------------------

```bash
cd ~/go2_cun
source install/setup.bash
ros2 launch go2_config gazebo_velodyne.launch.py world:=factory rviz:=true
```

# ------------ opcion 1 ------------------ 

cd ~/go2_cun
source install/setup.bash
ros2 launch go2_config slam.launch.py use_sim_time:=true
# ----

ros2 launch go2_config slam.launch.py use_sim_time:=true

# ------------ opcion 2 ------------------ 
ESTE (genérico / avanzado)
ros2 launch slam_toolbox online_async_launch.py use_sim_time:=true

# ------------ opcion 3 - SIMILAR  ------------------ 
source ~/go2_cun/install/setup.bash
ros2 launch slam_toolbox online_async_launch.py use_sim_time:=True

# Mueve el robot (teleop)

ros2 run teleop_twist_keyboard teleop_twist_keyboard

# Guarda el mapa

ros2 run nav2_map_server map_saver_cli -f factory_map


###

###
###
###
###

##











source ~/turtlebot4_ws/install/setup.bash


```bash
source ~/go2_cun/install/setup.bash
```

Aplica los cambios: Para que la terminal se entere de lo que acabas de hacer, escribe esto en la terminal:

```bash
ros2 launch go2_config gazebo.launch.py rviz:=true
```

###  ======================================== ojo =======================



---------------------------------------------ABRIR TERMINALES----------------------------------------------------
Gazebo demo: Run the Gazebo environment

ros2 launch go2_config gazebo.launch.py

2.2 Walking demo in RVIZ: Run the gazebo along with rviz

source ~/go2_cun/install/setup.bash

Luego verifica:

ros2 pkg list | grep go2

Debe salir:

go2_config

go2_description

-------------------------------------EMPEZAR DESDE LA TEMRINAL -----------------------------------------------------
Ve a tu workspace go2_cun
cd ~/go2_cun
------------------------------------------- IMPORTANTE --------------------------------------------------------------
2️⃣ Ve a tu workspace go2_cun
cd ~/go2_cun

3️⃣ Carga el entorno de ROS 2 y tu workspace

⚠️ Este paso es obligatorio después de reiniciar

source install/setup.bash

(Si usas ROS Humble y no lo tienes en tu .bashrc, primero:)
source /opt/ros/humble/setup.bash

Luego otra vez:
source install/setup.bash

Y ahora sí ejecuta:
ros2 launch go2_config gazebo.launch.py rviz:=true
 
EN MI CASO NO SABIA SI TENIA EL .bash es decir es la "memoria automática" de tu terminal. Lo que ves ahí son las instrucciones que la computadora ejecuta 
por sí sola cada vez que abres una ventana nueva.asi que lo que hice fue ejecutar nano ~/.bashrc , lo cual me salio ME SALIO ESTO EN EL NANO OK ME SALIO if [ -f ~/.bash_aliases ]; then


  fi

fi

source /opt/ros/humble/setup.bash

export PATH="$HOME/.local/bin:$PATH"

source /opt/ros/humble/setup.bash

source ~/turtlebot4_ws/install/setup.bash

lo cual estaba incorrecto ya que tenia un duplicado la línea source /opt/ros/humble/setup.bash, no tenia el Go2 activado: Falta la línea del go2_cun y lo que si tenia activado era 
Turtlebot: La línea source ~/turtlebot4_ws/install/setup.bash está activa.
--------------------------------------------------------------------------
pasos para correguir 
------------------------------------------------------------------------
2.3 Run the teleop node:
ros2 run teleop_twist_keyboard teleop_twist_keyboard
