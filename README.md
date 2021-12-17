# iot_cap10

Alumnos:

  Cueva Flores, Jonathan Brandon
  
  Garcia Diaz, German Flavio
  
  Montesinos Apaza, Sergio

  Herrera Cooper, Miguel Alexander

# PASOS A SEGUIR
## Antes de iniciar el docker
Verificar si no esta corriendo mysql en su maquina
```
sudo netstat -nlpt |grep 3306
```
Si hay un proceso en ejecucion detener
```
sudo service mysql stop
```
## Ejecutar nuestro archivo .yml
### Crear red interna
```
docker network create <<nombre de red>>
```
para nuestro caso:
```
docker network create iot-network
```
### Inicializar nuestro docker
```
docker-compose up
```
### Ingresamos a nuestro contenedor o VM
```
docker exec -it <<nombre-asignado-por-docker>> bash
```
En nuestro caso
```
docker exec -it iotsecond_mysql_1 bash
```
### Ingresamos a Mysql
```
mysql -u root -p
```
la contraseña de nuestro mysql la especificamos en nuestro archivo .yml para nuestro caso es "password"
### Ingresando a la base de datos "TimeSeries"
```
use TimeSeries
```
### Creando la tabla en nuestra BD
```
CREATE TABLE thingData ( id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, topic varchar(1024), payload varchar(2048), timestamp varchar(15), deleted binary(1) ) 
```
![Captura realizada el 2021-11-29 10 03 11](https://user-images.githubusercontent.com/20243155/143892008-c0e08842-c796-4340-9b59-249d94832ac5.png)
)
### Creando la nueva tabla 'ruleEngine' en la base de datos
```
CREATE TABLE  `TimeSeries`.`ruleEngine` ( `id` int(11) NOT NULL AUTO_INCREMENT, `ruleName` varchar(255) NOT NULL, `active` binary(1) NOT NULL DEFAULT '\0', `topicPattern` varchar(1024) NOT NULL, `payloadPattern` varchar(2048) NOT NULL, `method` varchar(7) NOT NULL DEFAULT 'GET', `webHook` varchar(1024) NOT NULL, PRIMARY KEY (`id`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
```

```
INSERT INTO `ruleEngine` (`id`, `ruleName`, `active`, `topicPattern`, `payloadPattern`, `method`, `webHook`) VALUES
(1, 'timestamp rule', 0x31, 'timestamp%', '%', 'POST', 'localhost:1880/pub/modifiedTime/rule-engine-working'),
(2, 'timestamp rule 2', 0x31, 'timestamp%', '%', 'POST', 'localhost:1880/pub/modifiedTime/rule-engine-working-again'),
(3, 'again rule', 0x31, '%', '%again', 'POST', 'localhost:1880/pub/modifiedTime/again found');
```

# GET
![Captura realizada el 2021-12-12 22 31 58](https://user-images.githubusercontent.com/20243155/145752632-8b22ad51-1a2f-4ccb-87ff-74569bd90c04.png)
![Captura realizada el 2021-12-12 23 52 51](https://user-images.githubusercontent.com/20243155/145754516-c8af3054-67d9-4794-91c8-69c907cd8238.png)

# GET FILTERS
![Captura realizada el 2021-12-12 22 44 57](https://user-images.githubusercontent.com/20243155/145752615-dec2ac7f-e87f-4593-8009-3895e8c8fb4e.png)
![Captura realizada el 2021-12-12 22 44 28](https://user-images.githubusercontent.com/20243155/145752627-522bf02e-af33-4fe2-b0f5-2c5d98d35f03.png)

# DELETE
![Captura realizada el 2021-12-12 23 02 48](https://user-images.githubusercontent.com/20243155/145752578-84868faf-6608-46db-8e3e-85f9dbd62970.png)
![Captura realizada el 2021-12-12 23 02 27](https://user-images.githubusercontent.com/20243155/145752605-3f9d4e27-57cd-473b-9ff0-1fe820a2046b.png)

# PURGE
![Captura realizada el 2021-12-12 23 17 09](https://user-images.githubusercontent.com/20243155/145752562-b6b17683-0693-4149-8eb9-0097a343526c.png)
