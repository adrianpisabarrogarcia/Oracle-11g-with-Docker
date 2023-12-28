
# Oracle 11g with Docker

REF: [https://hub.docker.com/r/oracleinanutshell/oracle-xe-11g](link:%20https://hub.docker.com/r/oracleinanutshell/oracle-xe-11g)

## Instalación

 1. Descarga de **Docker Desktop** para [Windows](https://docs.docker.com/desktop/install/windows-install/), [Linux](https://docs.docker.com/desktop/install/linux-install/) y [MacOs](https://docs.docker.com/desktop/install/mac-install/)

 2. Instala **colima** sólo si tienes un procesador Apple Silicon (M). Lo puedes instalar haciendo uso del gestor de paquetes **[Homebrew](https://brew.sh/)** :

 ```bash
 brew install colima
 ```
 
 Colima es una especie de Rosseta, simulador de arquitecturas amd64 o x86_84 que lo hace retrocompatible con arquitectura M de Apple Silicon (arm)

 Una vez instalado existosamente, lanza este comando para **iniciar colima** *(el proceso tarda un rato)*

 ```bash
 colima start --arch x86_64 --memory 4
 ```

 3. Ahora **inicaremos el contenedor** con el siguiete comando de docker:

 ```bash
 docker run -d -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true oracleinanutshell/oracle-xe-11g
 ```

## Datos de conexión

Datos de conexión para utilizarlo en [SQL Developer](https://www.oracle.com/es/database/sqldeveloper/), [DBeaver](https://dbeaver.io/download/) (recomendado) o en cualquier proyecto:

- **Hostname**: localhost
- **Port**: 49161
- **SID**: xe
- **Username**: system
- **Password**: oracle

## Crear un esquema, usuario y contraseña

```sql
-- crear usuario
CREATE USER <username> IDENTIFIED BY <password>;

-- asignar privilegios
GRANT CREATE TABLE, CREATE PROCEDURE TO <username>;

-- ver las tablespaces que ya están creadas
SELECT tablespace_name, file_name FROM dba_data_files;

-- crear tablespace
CREATE TABLESPACE <tablespace_name>
DATAFILE '/u01/app/oracle/oradata/XE/<tablespace_name>.dbf' SIZE 100M
AUTOEXTEND ON NEXT 100M MAXSIZE 1G;  
 
-- asignar el tablespace a ese usuario
ALTER USER <username> DEFAULT TABLESPACE <tablespace_name>;

-- asignar cuota ilimitada 
ALTER USER <username> QUOTA UNLIMITED ON <tablespace_name>;
GRANT CREATE SESSION TO <username>;
```
