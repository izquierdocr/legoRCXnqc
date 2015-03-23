Referencia
http://www.17od.com/2013/01/13/using-lego-mindstorms-on-ubuntu/

**Instalar el programa desde el centro de software. Buscar nqc

**Dar permisos al grupo del usuario que se quiera. Para usar la GUI del explorador en lugar de comandos, abrir una ventana de terminal y teclear
sudo nautilus
Ir a /dev/usb y dar permisos a legousbtower0 con boton derecho del mouse

**Los permisos de la instrucción anterior se mantienen hasta que se desconecta el USB Tower o se reinicia la máquina. Si no se quieren otorgar los permisos cada vez que se reinicia copiar (o crear) el archivo 90-legotower.rules (que solo contiene la linea: ATTRS{idVendor}=="0694",ATTRS{idProduct}=="0001",MODE="0666",GROUP="<group>" donde <group> es el grupo del usuario) dentro de /etc/udev/rules.d
Verificar que el idVendor e idProduct correspondan al puerto usb del IR Tower ejecutando lsusb en la terminal. Dar al archivo permisos de root en usuario y grupo (Tanto los permisos como la copia pueden hacerse con sudo nautilus y usar el explorador).


**Ejecutar siempre nqc con la opción -Susb para enviar comandos al USB en lugar del puerto serial (por defecto)


**Prender el RCX y colocarlo en linea de vista con la IR Tower


**Seleccionar el slot donde se guardará el programa (1-5)
nqc -Susb -pgm 5

**Compilar y enviar el programa al RCX en el slot indicado por la instruccion anterior
nqc -Susb -d 11_slave.nqc

**Correr el programa
nqc -Susb -run

**Enviar comandos al programa (0-255)
nqc -Susb -msg 3


**Ejemplo de programa que escucha comandos
task main()          
{
  while (true)
  {
    ClearMessage();
    until (Message() != 0);
    if (Message() == 1) {OnFwd(OUT_A+OUT_C);}
    if (Message() == 2) {OnRev(OUT_A+OUT_C);}
    if (Message() == 3) {Off(OUT_A+OUT_C);}
  }
}
