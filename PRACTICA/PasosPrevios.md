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
##                                 Abrir el terminal desde cero para poder trabajar

###  ======================================== Problemas previos que se tuvo despues de ejecutar el gazebo  =======================
Abrir el workspace

```bash
cd ~/go2_cun
```

Si quieres usar SOLO go2_cun

Abre una terminal nueva y ejecuta solo:

```bash
source ~/go2_cun/install/setup.bash
```

O si tienes en tu ~/.bashrc:

```bash
source /opt/ros/humble/setup.bash
source ~/go2_cun/install/setup.bash
```

### (no sourcear turtlebot ahí)
 
```bash
~/go2_cun$ cd ~/go2_cun colcon build
```

###  ======================================== ojo =======================

### VERIFICACION 

```bash
/go2_cun$ ros2 pkg list | grep champ
ros2 pkg list | grep go2
```

Aparecera algo como

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

Aunque el repositorio se llama: unitree-go2-ros2, NINGÚN paquete ROS se llama unitree_*.

Paquetes del framework CHAMP (9 paquetes) (control de robots cuadrúpedos) : 

champ - Framework base
champ_base - Controlador base
champ_bringup - Lanzadores
champ_config - Configuraciones
champ_description - Descripciones URDF
champ_gazebo - Integración con Gazebo
champ_msgs - Mensajes personalizados
champ_navigation - Navegación
champ_teleop - Teleoperación

Paquetes específicos del GO2 (2 paquetes) Paquetes del Go2 :

go2_config - Configuración del GO2
go2_description - Descripción URDF del GO2

Estado actual:
✅ Workspace go2_cun creado correctamente
✅ 11 paquetes compilados exitosamente
✅ Sourced correctamente
✅ Paquetes disponibles en ROS2

Por eso la mayoría de paquetes son champ_*, no unitree_*.

### Qué es CHAMP


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
