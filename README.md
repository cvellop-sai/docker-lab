# Ejercicio 1: Creando imágenes

## Paso 1

1. **Ejecuta un contenedor basado en la imagen:**
   `ubuntu`

   - `$ docker run --name ubuntu-devops ubuntu`


   ![Resultado de ejecución](capturas/captura-1-1-1.png)

2. **Accede a la terminal del contenedor.**
   - `$ docker run -it --name ubuntu-devops ubuntu /bin/bash`
   ![Resultado de ejecución](capturas/captura-1-1-2.png)


3. **Instala `curl`:**
   ```bash
   apt-get update
   apt-get install curl

   ![Resultado de ejecución](capturas/captura-1-1-3a.png)
   ![Resultado de ejecución](capturas/captura-1-1-3b.png)
   ![Resultado de ejecución](capturas/captura-1-1-3c.png)

4. **Comprueba que funciona**

   curl --version

   ![Resultado de ejecución](capturas/captura-1-1-4.png)

**Pregunta**
¿Con qué comando podrías guardar los cambios del contenedor como una nueva imagen?

   - `docker commit ubuntu-devops ubuntu-con-curl`
   ![Resultado de ejecución](capturas/captura-1-1-pregunta.png)

## Paso 2 --- Dockerfile

Crea un `Dockerfile` que haga lo mismo automáticamente.

Ejemplo:

```dockerfile
FROM ubuntu

RUN apt-get update && apt-get install -y curl
```

Construye la imagen y ejecuta un contenedor.
- `$ docker build -t ubuntu-con-curl-build .`
- `$ docker run -it ubuntu-con-curl-build /bin/bash`

   ![Resultado de ejecución](capturas/captura-1-2-1.png)

Comprueba que `curl` está instalado.

   ![Resultado de ejecución](capturas/captura-1-2-2.png)

---

## Pregunta

¿Qué comando permite ver las **capas de una imagen Docker**?

   - `docker history ubuntu-con-curl-build`

---

# 3. Volúmenes persistentes

Ejecuta un contenedor de:

    postgres

Usa un volumen Docker montado en:

    /var/lib/postgresql/data

   - `$ docker volume create postgres-data`
   - `$ docker run --name postgres -v postgres-data:/var/lib/postgresql/data -e POSTGRES_PASSWORD=mipassword -d postgres`

   ![Resultado de ejecución](capturas/captura-1-3-1a.png)

   ![Resultado de ejecución](capturas/captura-1-3-1b.png)

## Crear tabla

Conéctate a la base de datos.

   - `$ docker exec -it postgres psql -U postgres`

Crea la tabla:

```sql
CREATE TABLE items (
 id SERIAL PRIMARY KEY,
 name TEXT
);
```

Inserta un registro:

```sql
INSERT INTO items(name) VALUES ('item1');
```

   ![Resultado de ejecución](capturas/captura-1-3-2.png)

## Comprobación

1.  Para el contenedor
2.  Elimina el contenedor
3.  Crea un nuevo contenedor usando **el mismo volumen**

   ![Resultado de ejecución](capturas/captura-1-3-3-1.png)

Comprueba que los datos siguen existiendo.

   ![Resultado de ejecución](capturas/captura-1-3-3-2.png)
   

# 4. Bind mounts

Crea un archivo en tu máquina:

    index.html

Ejemplo:

```html
<h1>Hola Docker</h1>
```

   ![Resultado de ejecución](capturas/captura-1-4-1.png)
---

Ejecuta un contenedor `nginx`:

- mapea el puerto `80`
- monta el archivo en:

```{=html}
<!-- -->
```

    /usr/share/nginx/html/index.html

Abre el navegador.

   ![Resultado de ejecución](capturas/captura-1-4-2.png)
---

Pregunta:

¿Qué ocurre si modificas el archivo `index.html` en tu máquina?

   - Automáticamente se modifica en el contenedor, actualizandose en la web del navegador.

   ![Resultado de ejecución](capturas/captura-1-4-3.png)
   ![Resultado de ejecución](capturas/captura-1-4-4.png)
---

# 5. Auditando volúmenes (opcional)

Investiga:

¿Qué comando permite ver **dónde guarda Docker los datos de un
volumen**?
   - `$ docker volume inspect postgres-data`

   ![Resultado de ejecución](capturas/captura-1-5-1.png)
---

# 6. Creando redes privadas

Crea una red llamada:

    my-net

---

Arranca dos contenedores `ubuntu` en esa red.

Instala `ping` si es necesario.

Desde un contenedor intenta hacer:

```bash
ping otro_contenedor
```

---

Pregunta

¿Los contenedores pueden comunicarse entre sí?

---

# 7. Red none (opcional)

Investiga:

¿Para qué serviría ejecutar un contenedor con red:

    none

---

# 8. Multi-network (opcional)

Crea dos redes:

    secure-zone
    public-zone

Arranca un contenedor en `public-zone`.

Pregunta:

¿Puedes conectarlo también a `secure-zone`?

¿Qué comando usarías?

---

# 9. Docker Compose --- Compartiendo volúmenes

Crea un fichero:

    docker-compose.yml

Con dos servicios.

---

## writer

Debe:

- montar un volumen en `/app/logs`
- escribir un timestamp cada 30 segundos

---

## reader

Debe:

- montar el volumen en modo solo lectura
- mostrar el contenido en consola

---

# 10. Docker Compose Profiles (opcional)

Crea un `docker-compose.yml` con:

- `postgres`
- `pgadmin`

Haz que `pgadmin` pueda conectarse a `postgres`.

---

Crea dos perfiles:

### Perfil completo

Levanta:

- postgres
- pgadmin

### Perfil base

Levanta solo:

- postgres

---
