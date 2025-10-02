## 1. Configuración Inicial y SSH: Conexión Segura

Esto es clave para conectarnos sin tener que poner contraseñas cada dos por tres (como haríamos para clonar de GitHub con HTTPS).

### Verificando el Entorno

1.  **`pwd`**
    * **Explicación:** Simplemente para ver dónde estamos. En Windows con MINGW, nos dice la ruta de la *home* del usuario: `/c/Users/Usuario`.
![alt text](<img/Captura de pantalla 2025-09-22 083905.png>)

2.  **`ls ~/.ssh/`**
    * **Explicación:** Intentamos listar el contenido de la carpeta oculta `.ssh` en nuestro *home*. Como aún no la hemos creado, peta con un `No such file or directory`. Normal.
    ![alt text](<img/Captura de pantalla 2025-09-22 085000.png>)

### Creando y Cargando la Clave (ed25519)

3.  **`ssh-keygen -t ed25519 -C "fjimlop182@g.educaand.es"`**
    * **Explicación:**  Generamos un nuevo par de claves:
        * `-t ed25519`: Usamos el algoritmo más moderno y seguro.
        * `-C "..."`: Añadimos nuestro correo para saber de quién es esta clave si tenemos varias.
        * Esto crea las dos claves: la **privada** (`id_ed25519`) que se queda en nuestro PC y la **pública** (`id_ed25519.pub`) que subiremos a GitHub.
    ![alt text](<img/Captura de pantalla 2025-09-22 084442.png>)

4.  **`eval "$(ssh-agent -s)"`**
    * **Explicación:** Iniciamos el **agente SSH**. Es un proceso que se queda corriendo en segundo plano y almacena nuestra clave privada descifrada. Así, solo introduces la *passphrase* una vez y no cada vez que haces un `git push`.
    ![alt text](<img/Captura de pantalla 2025-09-22 084442.png>)

5.  **`ssh-add ~/.ssh/id_ed25519`**
    * **Explicación:** Añadimos nuestra clave privada al agente SSH. 
    ![alt text](<img/Captura de pantalla 2025-09-22 085000.png>)

6.  **`cat ~/.ssh/id_ed25519.pub`**
    * **Explicación:** Imprimimos por pantalla el contenido de la clave pública. **Este *string* es el que tienes que copiar y pegar en la configuración de claves SSH de tu cuenta de GitHub** para que el servidor te reconozca.
    ![alt text](<img/Captura de pantalla 2025-09-22 085243.png>)

---

## 2. Inicialización de Git y Configuración Global

Aquí empezamos con Git: creamos la carpeta, la inicializamos y nos identificamos.

### Preparando la Carpeta del Proyecto

7.  **`cd Documents/`** / **`mkdir Optativa`** / **`cd Optativa/`**
    * **Explicación:** Navegamos a *Documents*, creamos una carpeta `Optativa` para el proyecto y entramos en ella. Todo el código del proyecto irá aquí.
    ![alt text](<img/Captura de pantalla 2025-09-22 085944.png>)

8.  **`git init`**
    * **Explicación:** **Convierte la carpeta `Optativa` en un repositorio de Git.** Se crea la subcarpeta `.git/` y Git empieza a funcionar
    ![alt text](<img/Captura de pantalla 2025-09-22 085944.png>)

### Identificándonos Globalmente

9.  **`git config --global --list`**
    * **Explicación:** Intentamos listar nuestra configuración global. Falla porque es la primera vez y aún no existe el archivo de configuración global.
    ![alt text](<img/Captura de pantalla 2025-09-22 091210.png>)

10. **`git config --global user.name FranJJL05`**
    * **Explicación:** **Configuramos nuestro nombre de usuario global.** Este es el nombre que aparecerá en *todos* nuestros *commits* a partir de ahora.

11. **`git config --global user.email fjimlop182@g.educaand.es`**
    * **Explicación:** **Configuramos nuestro correo electrónico global.** Lo mismo que con el nombre, pero con el correo.
    ![alt text](<img/Captura de pantalla 2025-09-22 091347.png>)

---

## 3. Primer Commit y Pushing

Todo listo para crear nuestro primer archivo, subirlo al *staging area* y mandarlo al remoto.

12. **`echo "# Configurando SSH" > README.md`** / **`ls`**
    * **Explicación:** Creamos el archivo básico `README.md` (fundamental en cualquier proyecto) y verificamos que está ahí con `ls`.
    ![alt text](<img/Captura de pantalla 2025-09-22 091511.png>)

13. **`git status -s`**
    * **Explicación:** Miramos el estado. El `?? README.md` significa que Git ve el archivo, pero no le estamos haciendo **seguimiento** (*untracked*).
    ![alt text](<img/Captura de pantalla 2025-09-22 091752.png>)

14. **`git add .`**
    * **Explicación:** **Añadimos todos los archivos nuevos o modificados al *staging area* (área de preparación).** Esto le dice a Git: "Quiero incluir este archivo en mi próximo *commit*".
    ![alt text](<img/Captura de pantalla 2025-09-22 091752.png>)

15. **`git status -s`**
    * **Explicación:** Ahora el `A README.md` significa que el archivo ya está en el *staging area*, listo para ser *comiteado*.
    ![alt text](<img/Captura de pantalla 2025-09-22 092017.png>)

16. **`git commit -m "Created: README.md"`**
    * **Explicación:** **Creamos el *commit* definitivo.** Los cambios en el *staging area* se empaquetan y se guardan en el historial local del repositorio. El mensaje `-m` explica qué hemos hecho.
    ![alt text](<img/Captura de pantalla 2025-09-22 092017.png>)

17. **`git log --oneline`**
    * **Explicación:** Verificamos que nuestro *commit* se ha guardado correctamente. Muestra el historial de forma concisa.
    ![alt text](<img/Captura de pantalla 2025-09-22 092141.png>)

### Enlazando con GitHub y Subiendo

18. **`git remote add origin git@github.com:FranJJL05/Optativa.git`**
    * **Explicación:** **Definimos el repositorio remoto.** Le decimos a nuestro Git local: "El servidor principal donde subiremos el código (el *upstream*) se llama `origin` y está en esta URL de SSH".
    ![alt text](<img/Captura de pantalla 2025-09-22 092930.png>)

19. **`git remote -v`**
    * **Explicación:** Verificamos que el *remote* se ha configurado bien.
    ![alt text](<img/Captura de pantalla 2025-09-22 092930.png>)

20. **`git push origin master`**
    * **Explicación:** **Subimos el *commit* local al repositorio remoto.** Mandamos la rama `master` (la principal) a `origin`. 
    ![alt text](<img/Captura de pantalla 2025-09-22 093116.png>)
