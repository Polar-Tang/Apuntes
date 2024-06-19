- Dificultad: Esperma (menor que feto)
- from: Vulnhub
- Vulnerability: SQLI
## Descargarse la máquina
Descargamos la máquina desde el siguiente enlace:
https://www.vulnhub.com/entry/owasp-broken-web-applications-project-12,46/
Una vez desplegada y extraída le damos a open machine en vmware y nos logueamos
user: root
password: owaspbwa
Una vez dentro vemos la IP con un `ifconfig`
![[Pasted image 20240619182445.png]]
Y la ip que tengamos la googleamos con `ip/`
Pero esta en TLS 3 o 1.2 y las web les da asco las versiones tan viejas, así que tenemos que configurarla dependiendo de nuestro browser

	#### Microsoft Internet Explorer

1. Open **Internet Explorer**
2. From the menu bar, click **Tools** >  **Internet Options** > **Advanced** tab
3. Scroll down to **Security** category, manually check the option box for **Use TLS 1.1** and **Use TLS 1.2   
      
    ![](https://knowledge.digicert.com/content/dam/kb/attachments/ssl-tls-certificates/enabling-tls-1-1-and-tls-1-2-on-web-browsers/internet-explorer.jpg)** 
4. Click **OK**
5. Close your browser and restart Internet Explorer  
      
    

#### Google Chrome

1. Open **Google Chrome**
2. Click **Alt F** and select **Settings**
3. Scroll down and select **Show advanced settings...**
4. Scroll down to the **Network** section and click on **Change proxy settings...**
5. Select the **Advanced** tab
6. Scroll down to **Security** category, manually check the option box for **Use TLS 1.1** and **Use TLS 1.2   
      
    ![](https://knowledge.digicert.com/content/dam/kb/attachments/ssl-tls-certificates/enabling-tls-1-1-and-tls-1-2-on-web-browsers/chrome.jpg)** 
7. Click **OK**
8. Close your browser and restart Google Chrome

####   
  
Mozilla Firefox

1. Open **Firefox**
2. In the address bar, type **about:config** and press Enter
3. In the **Search** field, enter **tls**. Find and double-click the entry for **security.tls.version.max**
4. Set the integer value to **3** to force protocol of TLS 1.2   (y la versión mínima también bajale si es mayor o igual a 3)
      
    ![](https://knowledge.digicert.com/content/dam/kb/attachments/ssl-tls-certificates/enabling-tls-1-1-and-tls-1-2-on-web-browsers/firefox.png)  
      
    
5. Click **OK**
6. Close your browser and restart Mozilla Firefox

####   
  
Opera

1. Open **Opera**
2. Click **Ctrl** plus **F12**
3. Scroll down to the **Network** section and click on **Change proxy settings...**
4. Select the **Advanced** tab
5. Scroll down to **Security** category, manually check the option box for **Use TLS 1.1** and **Use TLS 1.2   
      
    ![](https://knowledge.digicert.com/content/dam/kb/attachments/ssl-tls-certificates/enabling-tls-1-1-and-tls-1-2-on-web-browsers/chrome.jpg)** 
6. Click **OK**
7. Close your browser and restart Opera
Ahora en la página le damos a bricks
![[Pasted image 20240619182946.png]]
Le das ahí y después a login pages
![[Pasted image 20240619183020.png]]
Capaz lo tengas que descargar primero, en tal caso solo lo descargas y lo extraes
```
7z x gobuster_Linux_x86_64.tar
```

- **Login 1.**
Si intentamos en el name y password poner el mismo comando para que lo tome, tal vez veamos en burpsuite un `Warning: mysql_num_rows() expects parameter 1 to be resource, boolean given in /owaspbwa/owaspbricks-svn/login-3/index.php on line 9`  lo cual significa que nos interpreta el lenguaje pero retorna false.
La query sería algo así:
```SQL
SELECT * FROM users WHERE name='' and password=''
```

Ya está configurado para lo que escribamos en el campo name vaya dentro del name de la petición y lo que vaya en el campo password vaya dentro de password, ósea que es necesario que el password y name digan lo mismo
Está securizado contra un [[Undirectory Path Traversal]]. Pero al ser una versión antigua de php (anterior a las 5.3 el input del login es vulnerable al nullbait).
- **Login 2.**
Ahora si intentamos escribir un código sqli no podríamo, ya que hay un código en JS que nos rompe las bolas para que no lo interprete. Entonces abrimos burpsuite para chusmear el código (Intercept on > Refrescar la página de Login > Click derecho > change request method )
![[Pasted image 20240619170726.png]]
Enviado vemos que el código trabaja con el id passwd, y está propiedad es accesible a ser modificada
![[Pasted image 20240619170059.png]]
![[Pasted image 20240619165944.png]]
- **Login 3**
Ahora interceptamos, enviamos al repeter, send y en la respuesta del servidor se ve el SQL que hay detrás
![[Pasted image 20240619175248.png]]
La query por defecto es algo así , no es una [[consulta paremetrizada]]
```SQL
SELECT * FROM users WHERE name=('') and password=('')
```

- **Login 4**
Es lo mismo que la 3 pero en vez de comillas simples son comillas dobles xD
![[Pasted image 20240619180731.png]]

- **Login 5**
	Misma metodología, interceptamos y vemos que la quiery cambia levemente 
	
```SQL
SELECT * FROM users WHERE name='' and password=''
```
Ósea que solo hay que comentar, sencillo

![[Pasted image 20240619181252.png]]
Ya por último, pongas lo que pongas en el login 6 te va a dar correcto