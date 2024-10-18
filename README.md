# P3-Tarefa-Dig
1.Realiza unha consulta dig danielcastelao.org" e identifica cada parte da resposta (IN, CNAME, A, QUERY SECTION, ANSWER SECTION, AUTHORITY SECTION, etc)

**Despois de executar o comando** 
```
dig danielcastelao.org, 
```
**veremos diferentes partes como o header, a seccion de consultas ou question section, a sección de respostas ou answer section e as sección de estadisticas ou stadistics. A continuación se mostrará o que aparece en cada una de ela.**

Header:
; <<>> DiG 9.18.28-0ubuntu0.22.04.1-Ubuntu <<>> danielcastelao.org
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 4566
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

Question Section:
;; QUESTION SECTION:
;danielcastelao.org. IN A

Answer Section:
;; ANSWER SECTION:
danielcastelao.org. 900 IN A 178.211.133.37
danielcastelao.org: é o dominio solicitado
900: é o tempo de vida en segundos onde a resposta é valida ata que deba consultarse outra vez o DNS
in: é o tipo ou clase de rexistro, neste caso internet
A: é o tipo de rexistro, neste caso direción IPv4.
178.211.133.37: é a direción IPv4 do dominio.

Statistics:
;; Query time: 165 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Fri Oct 04 21:04:13 CEST 2024
;; MSG SIZE rcvd: 63

**No meu caso non aparece o apartado de AUTHORITY SECTION.**

2.Realiza consutas dos seguintes nomes e identifica as diferencias: moodle.danielcastelao.org, www.danielcastelao.org

**Ao realizar as consultas especificadas as diferencias vense reflexadas no apartado de AUTHORITY SECTION. Neste caso aparece o seguinte:**
;; AUTHORITY SECTION:
danielcastelao.org. 300 IN SOA ns1.hover.com. dnsmaster.hover.com. 1720467415 1800 900 604800 300

A diferencia é que no moodle aparece o tipo de rexistro "NS" que indica os servidores autoritativos.

3.Averigua o nome e IP dos servidores de DNS autoritativos de www.danielcastelao.org, por qué soen ser 2 servidores autoritativos?

**Para averiguar o nome e IP dos servidores de DNS escribiremos dous comandos, o primeriro para averiguar o nome e o seguinte para obter a IP a través de esos nomes.**
O primeiro é
```
dig ns danielcastelo.org
```
**Con este podremos obter os nomes dos 2 servidores autoritativos.**
;; ANSWER SECTION:
danielcastelao.org. 900 IN NS ns2.hover.com.
danielcastelao.org. 900 IN NS ns1.hover.com.
A continuación obteremos as suas IPs co comando dig A (nome do servidor DNS).
;; ANSWER SECTION: ns2.hover.com. 6961 IN A 64.98.148.13 ;; ANSWER SECTION: ns1.hover.com. 7065 IN A 216.40.47.26
Este son os dous servidores no meu caso.

Soen ser dous servidores por diferentes motivos entre eles:
1- Redundancia e disponibilidad
2-Balanceo de carga
3-Resiliencia ante antaques

4.Realiza as consultas de nomes inversas: 130.206.164.68 e de outras dúas IPs que se che ocorran.

**Para realizar unha consulta inversa o parametro é -x. O comando reultante é dig -x 130.206.164.68.**
;; ANSWER SECTION:
68.164.206.130.in-addr.arpa. 7200 IN PTR pluto.tlm.unavarra.es.
68.164.206.130.in-addr.arpa. 7200 IN PTR s164m68.unavarra.es.

dig -x 130.206.165.68
;; ANSWER SECTION: 68.165.206.130.in-addr.arpa. 7200 IN PTR s165m68.unavarra.es.

dig -x 130.206.163.68
;; ANSWER SECTION: 68.163.206.130.in-addr.arpa. 7157 IN PTR s163m68.unavarra.es.

5.A qué servidor DNS estás consultando? Cómo o podes cambiar sen tocar os ficheiros de configuración do sistema?

**Para saber a que servidor DNS estou a consultar teño que realizar o comando cat /etcresolv.conf. Este comando accede a un arquivo de configuración onde se definen os servidores DNS que o sistema usa para resolver nomes do dominio.**
**Para cambialo podemos utilziar o comando dig @8.8.8.8 www.danielcastelao.org, o simbolo "@" indica o servidor DNS ao que se debe enviar a consulta, neste caso se está utilizando o servidor DNS público de Google.**

6.Obtén o rexistro SOA (Start of Authority) do dominio moodle.danielcastelao.org preguntándolle ó servidor DNS de google e logo preoguntándollo directamente ó servidor primario do dominio danielcastelao.org.

Para consultar o rexistro SOA preguntandolle ó servidor DNS de Google debemos escribir o comando 
```
dig @8.8.8.8 SOA moodle.danielcastelao.org
```
;; AUTHORITY SECTION:
danielcastelao.org. 300 IN SOA ns1.hover.com. dnsmaster.hover.com. 1720467415 1800 900 604800 300

**E preguntandolle directamente ó servidor primario do dominio danielcastelao.org o comando**
```
 dig @ns1.hover.com. SOA moodle.danielcastelao.org.
```
;; AUTHORITY SECTION:
danielcastelao.org. 300 IN SOA ns1.hover.com. dnsmaster.hover.com. 1720467415 1800 900 604800 300

7.Consulta a IP de www.elpais.com. Cánto tempo queda almaceado o rexistro de recurso no DNS local?, se preguntas ó DNS local por este recurso, qué observas no TTL do rexistro?

**Para obter a IP e os demais datos da web do pais empregaremos o seguinte comando:**
```
dig www.elpais.com
```
**Obteremos o parametros necesarios para esponder no meu caso a IP do server que me aparece é: 127.0.0.53 e empregou un tempo de rexistro de 23 msecs.**

8.Busca o TTL de distintos nomes de dominio de servicios que escollas, a qué se poden deber as diferencias?

**Para a proba ditos se seleccionaron google, facebook e amazon. As diferencias no TTl poden deberse á política de cache do servicio ou os servizos con contenido dinámico soen ter TTl máis baixos para actualziarse con máis frecuencia.**

9.Determina o TTL máximo (original) dun nome de dominio.

**Fixemos unha consulta a microsoft utilizando o comando.**
```
dig www.microsoft.com
```
**O resultado foi de 3578 mcs**

10.Averigua cántas máquinas con distintas IPs están detrás do dominio web www.google.es, sempre son as mesmas e na mesma orde? por qué?

**Para saber cantas máquinas estan detras de un dominio debemos facer o seguinte comando:**
```
dig www.google.es
```
**Para responder a pregunta si, si deberian de ser sempre as mismas maquinas xa que as veces que probei eu recibía sempre o mesmo resultado.**

11.Pregunta o mesmo a un server raiz (J.ROOTSERVERS.NET por exemplo) e comproba na resposta se o server acepta o modo recursivo

**Neste caso a facer o comando dig a propia raiz debemos poñer o seguinte**
```
dig A J.ROOTSERVERS.net
```
**Logo podemos observar as diferentes IPs os meu resultados foros os seguintes**
J.ROOTSERVERS.net. 3600 IN A 15.197.204.56
J.ROOTSERVERS.net. 3600 IN A 3.33.243.145
**Podemos realizar esto de manera recursiva co seguinte método**
```
dig -x 15.197.204.56
```
e
```
dig -x 3.33.243.145
```

12.Se queremos ver tóda-las queries que fai o servidor de DNS, qué opción temos que usar? averigua a IP de www.timesonline.co.uk, especifica os pasos dados

**Para ver todas as respuestas do servidor, debemos poñer o comando de dig con un +trace, por exemplo nesta direccion**
```
dig +trace www.mediavida.com
```

13.Usando a información dispoñible a traveso do DNS especifica a máquina (nome e IP) ou máquinas que actúan como servers de correo do dominio danielcastelao.org
**Para descubrir estas maquinas debemos comezar polo comando :**
```
dig MX danielcastelao.org
```
**E a resposta e a seguinte**
danielcastelao.org. 900 IN MX 130 aspmx4.googlemail.com.
danielcastelao.org. 900 IN MX 90 alt1.aspmx.l.google.com.
danielcastelao.org. 900 IN MX 110 aspmx2.googlemail.com.
danielcastelao.org. 900 IN MX 100 alt2.aspmx.l.google.com.
danielcastelao.org. 900 IN MX 140 aspmx5.googlemail.com.
danielcastelao.org. 900 IN MX 80 aspmx.l.google.com.
danielcastelao.org. 900 IN MX 120 aspmx3.googlemail.com.


14.Podes obter os rexistros AAAA de www.facebook.com? a qué corresponden?

**Los registros AAAA en DNS corresponden a las direcciones IPv6 asociadas con un dominio. Para obtener los registros AAAA de facebook utilizaremos o seguinte comando :**
```
 dig AAAA www.facebook.com
 ``` 



14.Podes obter os rexistros AAAA de www.facebook.com? a qué corresponden?
