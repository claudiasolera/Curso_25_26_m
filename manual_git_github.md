# Manual Configurar github por ssh

## Crear la carpeta del directorio

- Crea un directorio específico para github.
    ```bash
    $ mkdir 'nombre-de-la-carpeta'
    ```

- Entra dentro de la carpeta que se ha creado.

    ```bash
    $ cd 'nombre-de-la-carpeta'/
    ```
---

## Configuración inicial de git

- Comprueba si están bien configurados el nombre (user.name) y el gmail (user.email) de nuestro git.

    ```bash
    $ git config --list
    ```
    En caso de no estarlo, lo configuramos mediante los siguientes comandos:

    ```bash
    $ git config --global user.name "tu-nombre"
    $ git config --global user.email "tu-gmail"
    ```

>Es importante que tanto el nombre de usuario como el gmail coincidan con los que usamos en nuestro github.

- También hay que configurar que la rama al inicializar un repositorio se llame main y no master.

    ```bash
    $ git config --global init.defaultBranch main
    ```
---

## Creación del repositorio local y archivo README.md

- Inicia un nuevo repositorio en la carpeta actual.

    ```bash
    $ git init
    ```
- Crea un archivo README.md.
    ```bash
    $ echo "# título del archivo" > README.md
    ```
    - Vemos el estado del repositorio y nos tiene que salir el nombre del archivo en rojo.
    ```bash
    $ git status
    ```
    ![imagen ssh](/img/readme.png)

    >Para ver el estado del repositorio en modo resumido:
    >```bash
    >$ git status -s
    >```

- Añade el archivo creado al área de preparación (stagin area).
    ```bash
    $ git add .
    ```
    >Haciendo uso del punto, se están añadiendo todos los archivos nuevos o modificados al stagin area. Si quieres añadir un archivo en específico susituye el punto por el nombre del archivo.

    - Puedes hacer un commit comentando lo que has hecho.
        ```bash
        $ git commit -m "Created README"
        ```
    - Verifica que no quedan cambios pendientes.
        ```bash
        $ git status
        ```


---

## Instalación de la clave en Github

- Genera una clave SSH asociada a tu correo.
    ```bash
    ssh-keygen -t ed25519 -C "tu-correo@gmail.com"
    ```

- Obten la clave y copiala entera sin incluir espacios finales.
    ```bash
    cat ~/.ssh/id_ed25519.pub
    ```
- En tu github, ve a Settings > SSH and GPG keys > New SSH key
![imagen ssh](/img/ssh.png)
    Ponle un título con el que identificar esa clave y pégala en el apartado Key
    ![imagen ssh](/img/key.png)

---

## Añadir la clave a Agent

- Inicia el agente ssh
    ```bash
    $ eval "$(ssh-agent -s)"
    ```
    Y añade la clave al agente para poder usarla. 
    ```bash
    $ ssh-add ~/.ssh/id_ed25519
    ```
    >En Windows, tienes que activar el servicio ssh-agent en PowerShell ejecutandolo como administrador:
    ```powershell
    Get-Service ssh-agent | Set-Service -StartupType Automatic
    Start-Service ssh-agent
    ```
- Prueba la conexión con github.
    ```bash
    $ ssh -T git@github.com
    ```
---

## Crear el repositorio remoto en GitHub y obtener la url SSH

- En tu github ve a Home > New . Le pones el nombre y una descripción al repositorio, el resto de configuraciones no se tocan. 
    ![imagen ssh](/img/remoto.png)
    >El nombre del repositorio remoto debe ser el mismo que el del directorio que has creado al principio para usar en git

    Finalmente le das a Create repository.
- Dentro del repositorio que has creado en github, te vas a Code y copias la URL de SSH para conectarla con el repositorio local.
    ![imagen ssh](/img/url_ssh.png)
---

## Conectar el repositorio local con GitHub


- Enlaza el repositorio local con el remoto en tu github sustituyendo 'tu_url_ssh' por la url copiada en el paso anterior.
```bash
$ git remote add origin tu_url_ssh
```

---

## Confirmar cambios al repositorio remoto

- Antes de confirmar los cambios, puedes ver el historial de commits de los archivos creados, en este caso el de README.md .
    - Manera detallada:
    ```bash
    $ git log
    ```
    - En una sola línea:
    ```bash
    $ git log --oneline
    ```

- Finalmente, envía los camibios confirmados al repositorio remoto de github.
```bash
$ git push origin main
```
- Verifica que se han hecho los cambios correctamente en tu github:
    ![imagen ssh](/img/github.png)

---