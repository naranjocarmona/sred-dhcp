# Configuración DHCP en Debian

## Instalacion del Servidor

Para instalar el servidor en Debian, ejecutar el siguiente comando:

```bash
apt install isc-dhcp-server
```

![](capturas/instalarServidor.png)

Sale el siguiente error:

![](capturas/instalacionError.png)

Parece que no se puede iniciar el servicio porque hay que configurarlo.

Para obtener mas detalles sobre el error, se puede consultar el log del systema con:

```bash
cat /var/log/syslog | grep "dhcp"
```

![](capturas/logDhcp.png)

## Configuracion

Para empezar a configurar, ponemos nuestra IP de manera estatica modificando el siguiente fichero:

```bash
nano /etc/network/interfaces
```

![](capturas/configuracionEstatica.png)

A continuacion, modificamos el archivo de configuracion de isc-dhcp-server


```bash
nano nano /etc/default/isc-dhcp-server
```

Descomentamos todas las lineas relacionadas con v4, le anadimos a uno de los interfaces del final v4 y terminamos anadiendole el nombre de nuestra tarjeta de red y guardamos.

![](capturas/configuracionDefaultIsc-dhcp-server.png)

A continuacion, modificamos el archivo de configuracion del servidor DHCP, aunque antes haremos una copia de seguridad por si algo sale mal:

```bash
cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.ORIGINAL
```

```bash
nano /etc/dhcp/dhcpd.conf
```

Aqui configuraremos todo lo descomentado, recordando que el rango de IPs que vayas a elegir pertenezcan a tu red.

![](capturas/configuracionServidorDHP.png)

Finalmente reiniciamos el servidor para confirmar los cambios

```bash
service isc-dhcp-server restart
```

## Comprobacion

Para poder comprobar la configuracion del servidor DHCP, necesitamos un cliente que se conecte a el. Entonces, veremos si el servidor le asigna una IP automaticamente al cliente.

Para ello, vamos a ejecutar una maquina virtual con Windows XP.

> Nota: La instalacion y configuracion del servidor DHCP se ha realizado en una maquina virtual con Debian.

> Nota: Si vemos un error parecido al siguiente, significa que falta un punto y coma (semicolon) en la línea indicada.
```
Oct  2 18:53:18 debian isc-dhcp-server[25084]: /etc/dhcp/dhcpd.conf line 10: semicolon expected.
```

En Virtual Box, se configuran las tarjetas de red de ambas maquinas como redes internas y en Avanzado, le damos a permitir todo.

![](capturas/configuracionRedInterna.png)

Iniciamos la maquina de Debian y miramos el log del sistema con

```bash
tail -f /var/log/syslog
```

Iniciamos la maquina de Windows XP y vemos que el servidor le asigna una IP del rango automaticamente.

![](capturas/ipasignada.png)

En el log de Debian podemos ver lo siguiente:

![](capturas/vistaLogFinal.png)




## Referencias

* [Blog Jesus](https://jesusfernandeztoledo.com/configurar-servidor-dhcp-en-debian-ubuntu/)
* [IES Mar de Cadiz](https://www.fpgenred.es/DHCP/index.html)

