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
CREATE TABLE  `TimeSeries`.`ruleEngine` ( 
  `id` int(11) NOT NULL AUTO_INCREMENT, 
  `ruleName` varchar(255) NOT NULL, 
  `active` binary(1) NOT NULL DEFAULT '\0', 
  `topicPattern` varchar(1024) NOT NULL, 
  `payloadPattern` varchar(2048) NOT NULL, 
  `method` varchar(7) NOT NULL DEFAULT 'GET', 
  `webHook` varchar(1024) NOT NULL, 
  PRIMARY KEY (`id`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
```
![Captura realizada el 2021-12-17 18 40 25](https://user-images.githubusercontent.com/20243155/146620671-dac2a00b-6610-472c-946c-3619cb351ff2.png)

### Insertamos 3 reglas a la tabla 'ruleEngine'
```
INSERT INTO `ruleEngine` (`id`, `ruleName`, `active`, `topicPattern`, `payloadPattern`, `method`, `webHook`) VALUES
(1, 'timestamp rule', 0x31, 'timestamp%', '%', 'POST', 'localhost:1880/pub/modifiedTime/rule-engine-working'),
(2, 'timestamp rule 2', 0x31, 'timestamp%', '%', 'POST', 'localhost:1880/pub/modifiedTime/rule-engine-working-again'),
(3, 'again rule', 0x31, '%', '%again', 'POST', 'localhost:1880/pub/modifiedTime/again found');
```
![Captura realizada el 2021-12-17 18 52 17](https://user-images.githubusercontent.com/20243155/146620690-8def1095-fdb8-41f1-955e-113fcf7013cd.png)

# Rule Engine
![Captura realizada el 2021-12-17 18 58 26](https://user-images.githubusercontent.com/20243155/146620787-6c64d3d1-a13f-4e6e-b079-39439bb05498.png)

