---
author: Hernán David Jalabert
---

# Sistemas Operativos con enfoque en Linux
## Presentación interactiva via shell de Linux. 

```
         _nnnn_                      
        dGGGGMMb          |  
       @p~qp~~qMb         | Sabias que...
       M|@||@) M|         | 
       @,----.JM|         | Cuando Linus Torvalds estaba trabajando en Linux,
      JS^\__/  qKL        | alguien propuso que el proyecto necesitaba una mascota.
     dZP        qKRb      | Durante una visita a un zoológico en Australia, Linus
    dZP          qKKb     | fue mordido por un pequeño pingüino. Le pareció tan gracioso 
   fZP            SMMb    | y simpático el animal que dijo: "¡el logo debería ser un
   HZM            MMMM    | pingüino feliz y gordito!".
   FqM            MMMMi   |
 __| ".        |\dS"qML   | Así nació Tux, el pingüino, diseñado originalmente por
 |    `.       | `' \Zq   | Larry Ewing en 1996.
_)      \.___.,|     .'   | 
\____   )MMMMMM|   .'     | https://en.wikipedia.org/wiki/Tux_(mascot)
     `-'       `--' hdj   |
```

---

## Índice

- ¿Qué es un sistema operativo?
- Tipos de sistemas operativos
- Historia de Linux
- Distribuciones Linux
- Gestión de procesos
- Sistema de archivos
- Memoria
- Usuarios y permisos

---

## ¿Qué es un sistema operativo?

- **Definición**
- **Componentes principales**
  - Kernel
  - Interfaz con el usuario
  - Sistema de archivos
  - Manejador de dispositivos 

**Demostración**: Veamos el kernel de este sistema

```bash
# Información del kernel
uname -a
# Verificar versión instalada
cat /proc/version
```

---
## kernel

Una representacion simple del kernel y sus componentes.

```bash
echo '[Hardware] -> [Kernel]
[Kernel] -> [Aplicaciones]
[Kernel] -> [Gestor de Procesos]
[Kernel] -> [Gestor de Memoria]
[Kernel] -> [Sistema de Archivos]
[Kernel] -> [Controladores]
[Controladores] -> [Hardware]
[Gestor de Procesos] -> [Aplicaciones]
[Sistema de Archivos] -> [Almacenamiento]' | graph-easy
```

---

## Tipos de sistemas operativos

| Clasificación        | Opción 1      | Opción 2        |
|----------------------|---------------|-----------------|
| **Por usuarios**     | Monousuario   | Multiusuario    |
| **Por tareas**       | Monotarea     | Multitarea      |
| **Por arquitectura** | Centralizado  | Distribuido     |

**Demostración**: Linux es multiusuario por diseño

```bash
# Ver usuarios del sistema
cat /etc/passwd | head
# Ver usuarios conectados actualmente
who
# Quien soy yo
whoami
```

*Mas adelante veremos con mas detalle lo de multiusuario*

---

## Historia de Linux

- **Unix**: Laboratorios Bell de AT&T, 1969
- **Proyecto GNU**: Richard Stallman, 1983
- **Kernel de Linux**: Linus Torvalds, 1991


> `Linux es el proyecto comunitario mas grande del mundo!`
> https://github.com/torvalds/linux
> ☆ *191k* ⎇ *55.4k* ⦿ *1.3M* ≡*30+M*

**Demostración**: Clonar el repositorio del kernel y ver los primeros 10 cambios.

```bash
# Obtener los primeros cambios del kernel (desde que se alojo en git)
git clone https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
cd linux
git log --reverse
```

---

## Distribuciones Linux

- **Concepto**
- **Categorías** (según su enfoque)
  - Servidores
  - Escritorio
  - Especializadas

**Demostración**: Identificando nuestra distribución

```bash
# Información de la distribución
cat /etc/os-release
lsb_release -a
# Alternativa con más detalles visuales (si disponible)
neofetch
# o
screenfetch
```

---

## Distribuciones Linux: Arbol de Distribuciones

* Ejemplo de distros basadas en Debian y Ubuntu.

```
~~~bash
echo "[ Debian ] --> [ Raspbian ]
[ Debian ] --> [ Ubuntu ]
[ Debian ] --> [ Mint ]
[ Ubuntu ] --> [ Pop! ]
[ Ubuntu ] --> [ Elementary ]
[ Ubuntu ] --> [ ZorinOS ]
[ Ubuntu ] --> [ Mint ]" | graph-easy
~~~
```

---

## Distribuciones Linux: Gestores de paquetes

- **Debian/Ubuntu**: apt (archivos .deb)
- **Red Hat/Fedora**: dnf/yum (archivos .rpm)
- **Arch**: pacman

**Demostración**: Usando el gestor de paquetes

```bash
# En sistemas basados en Debian/Ubuntu
apt list --installed | head
# Buscar un paquete
apt search htop
# Instalar un programa
apt install cowsay
# Ejecutar programa
cowsay "Un gran poder conlleva una gran responsabilidad - Tío Ben"
```

---

## Gestión de procesos en Linux

- **Procesos**
- **Características**
  - Identificador único (PID)
  - Prioridad y estado
  - Jerarquía (padre-hijo)

**Demostración**: Explorando procesos

```bash
# Listar procesos en ejecución
ps aux
# Ver procesos en tiempo real
top
# Árbol de procesos
pstree -Asp
ps -ejH
# Otra forma de mostrar procesos
htop
```

---

## Experimentando con procesos

- **Estados**: Ejecución, espera, bloqueado, zombie
- **Señales**: Instrucciones enviadas a procesos

**Demostración**: Creación y control de un proceso

```bash
# Crear un proceso en segundo plano
sleep 100 &
# Ver su PID
echo $!
# Mostar con htop (F3)
htop
# Terminar el proceso
kill $!
# Verificar que se terminó
ps -p $! || echo "Proceso terminado"
# Ver señales
cat /proc/1/status | grep Sig
```

---

## Sistema de archivos Linux

- **Estructura jerárquica**: Todo comienza en '/'
- **Directorios principales**:
  - /etc: Configuración
  - /home: Datos de usuarios
  - /var: Datos variables
  - /usr: Aplicaciones


> *batcat ./include/folder_structure.txt --language=yml*

**Demostración**: Explorando el sistema de archivos

```bash
# Estructura principal
ls -la / | head
# Espacio en dispositivos
df -h
```

---

## Trabajando con archivos

- **Operaciones**: Crear, leer, modificar, eliminar
- **Permisos**: Lectura, escritura, ejecución

**Demostración**: Manipulación de archivos

```bash
# Crear un archivo
echo "Demostración Linux" > demo.txt
# Ver sus propiedades
ls -l demo.txt
# Cambiar permisos (solo lectura)
chmod 400 demo.txt
# Ver permisos
ls -l demo.txt
# Verificar permisos
echo "lalala" > demo.txt
```

---

## Gestión de memoria

- **Memoria física y virtual**
- **Swap**
- **Paginación**

**Demostración**: Analizando el uso de memoria

```bash
# Ver uso de memoria
free -m
# Procesos que más memoria consumen
ps aux --sort=-%mem | head -5
# Estado de swap
swapon --show
# Memoria virtual
vmstat
```

---

## Usuarios y multiusuario

- **Sistema multiusuario por diseño**
- **Usuarios y grupos**
- **Root**: Superusuario con privilegios totales

**Demostración**: Gestión de usuarios

```bash
# Ver usuario actual
whoami
# Información detallada
id
# Crear un usuario (requiere privilegios)
sudo useradd -m -s /bin/bash usuario_demo
# Establecer contraseña
sudo passwd usuario_demo
# Conectarse con el nuevo usuario
su - usario_demo
```

---

## Permisos y seguridad

- **Modelo de permisos**
  - r: Lectura 
  - w: Escritura 
  - x: Ejecución
- **Propietario, grupo, otros**

**Demostración**: Compartir archivos entre usuarios

```bash
# Crear archivo para compartir
touch archivo_compartido.txt
# Cambiar permisos para permitir escritura al grupo
chmod 664 archivo_compartido.txt
# Ver resultado
ls -l archivo_compartido.txt
```

---

## Hardware y recursos del sistema

- **CPU, memoria, almacenamiento**
- **Dispositivos** 
- **Comunicación con hardware**

**Demostración**: Información de hardware

```bash
# Información sobre CPU
lscpu | head -10
# Dispositivos de bloque
lsblk
# Información sobre dispositivos PCI
lspci | head
```

---

## Conclusiones

- Sistema Operativo
    - `Un sistema operativo es como el sistema circulatorio de tu computadora. Distribuye recursos vitales (como memoria y tiempo de procesador) donde se necesitan, conecta todas las partes del cuerpo tecnológico y mantiene un flujo constante de información entre el hardware, los programas y el usuario.`
- **Linux**: Sistema potente, flexible y transparente
    - **Ventajas principales**
    - Control total del sistema
    - Seguridad y estabilidad
    - Personalización
    - Gran comunidad


> *En un mundo donde la tecnología tiende a encerrarnos, Linux nos recuerda que la verdadera innovación nace de la colaboración y la libertad."* - Linus Torvalds

---

## Sobre este slide

Para mostrar este slide se trabajo con `slides` (https://github.com/maaslalani/slides), una aplicación por linea de comandos para crear diapositivas mediante un archivo de texto plano en Markdown (MD).

```bash
batcat "./presentation.md"
```

Todo esto realizado directamente de la terminal sin necesidad de levantar el mouse, por eso los desarrolladores ❤  la terminal, almenos hasta que la IA haga todo por nosotros en un futuro cada dia mas cercano.

- Herramientas
    - Shell
    - slides (aplicacion de diapositivas por linea de comandos)
    - Vim (Editor de textos para terminal)
    - easy-graph (Libreria escrita en perl para la generacion de diagramas ASCII)
    - WSL (Windows Linux Subsystem)
    - Ubuntu 24.04 Server 
- Fuentes de información
    - Algo de wikipedia
    - Claude AI
    - Experiencia personal

---

## ¿Preguntas?

```bash
echo "¡Gracias por su atención!"
```


