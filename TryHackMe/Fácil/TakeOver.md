🧠 Takeover — Write-up

Dificultad: Fácil
Sistema operativo: Linux
Tiempo estimado: ~20 minutos
Plataforma: TryHackMe

📌 Introducción

Esta es la primera máquina que resuelvo, llamada Takeover, catalogada como fácil. Y sinceramente, sí lo es… pero solo si prestas atención a los detalles.

La máquina se basa prácticamente al 100 % en reconocimiento y enumeración, sin explotación compleja. Desde el principio, la propia descripción ya te va dando pistas clave, sobre todo relacionadas con subdominios, así que decidí centrarme mucho en esa parte.

🔍 Reconocimiento inicial

Lo primero que hago siempre es un escaneo completo de puertos con nmap, para tener una visión clara de la superficie de ataque.

nmap -p- -sS --min-rate 5000 -Pn -n 10.80.173.103 -oG ports


Uso estas opciones porque:

-p- escanea todos los puertos.

--min-rate 5000 acelera el escaneo.

-Pn evita el ping.

-oG ports guarda el resultado para no tener que repetir el escaneo más adelante, algo importante tanto para informes como para evitar tráfico innecesario en un entorno real.

📊 Resultados

El escaneo mostró tres puertos abiertos:

22 (SSH)

80 (HTTP)

443 (HTTPS)

Después realicé un segundo escaneo para identificar servicios y versiones:

nmap -sCV -p 22,80,443 10.80.173.103


Aquí vi algunas versiones antiguas con posibles vulnerabilidades, pero antes de intentar explotar nada decidí seguir un enfoque más básico: mirar la web.

🌐 Enumeración web

Al entrar a la web principal no encontré nada especialmente interesante. Todo parecía bastante limpio y sin funcionalidades explotables a simple vista.

En ese momento recordé algo importante:
👉 la descripción de la máquina hacía mucho hincapié en los subdominios.

Así que pasé directamente a enumerarlos.

🔎 Enumeración de subdominios

Probé distintos métodos, pero el que finalmente me dio resultados fue este comando con gobuster:

gobuster vhost \
-w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
-u futurevera.thm \
-t 50 \
-o subdomains \
--append-domain

Resultado obtenido:

portal.futurevera.thm

Al acceder, vi que no aportaba nada útil, así que seguí investigando.

🧠 Pensando fuera de la herramienta

Después de un buen rato sin resultados, decidí hacer algo que muchas veces se pasa por alto: volver a leer la descripción de la máquina con calma.

Ahí mencionaban que la empresa escribía blogs, así que probé manualmente:

blog.futurevera.thm


El subdominio existía, pero tampoco mostraba nada relevante. Algo curioso es que todos los subdominios daban problemas con el certificado SSL, así que decidí inspeccionar los certificados HTTPS con más detalle.

🔐 Análisis del certificado SSL

Tras revisar certificados sin éxito durante un rato, volví nuevamente a la descripción… y entonces caí en la cuenta de algo clave:

Decían que estaban reconstruyendo el soporte.

Eso me hizo pensar inmediatamente en otro subdominio lógico:

support.futurevera.thm


Este sí existía. A simple vista no parecía tener nada especial, pero esta vez revisé el certificado SSL de ese subdominio con más atención.

Y ahí estaba la clave 👇

Dentro del certificado aparecía un subdominio oculto:

secrethelpdesk934752.support.futurevera.thm

🏁 Resolución de la máquina

Al acceder a ese subdominio parecía que no había nada nuevo… hasta que probé algo muy simple pero decisivo:

👉 Entrar usando HTTP en lugar de HTTPS.

Al hacerlo, la flag apareció directamente en la página, dando por resuelta la máquina.

✅ Conclusión

Esta máquina me pareció muy buena para aprender algo fundamental en ciberseguridad:

No todo es explotar vulnerabilidades.

La enumeración, la observación y leer bien las descripciones puede ser suficiente.

Los certificados SSL pueden filtrar información crítica.

Probar cosas simples (como HTTP vs HTTPS) puede marcar la diferencia.

Una máquina sencilla, pero perfecta para reforzar mentalidad y metodología.
