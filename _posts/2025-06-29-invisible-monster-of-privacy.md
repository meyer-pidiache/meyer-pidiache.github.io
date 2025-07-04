---
title: Privacidad y seguridad, detrás de un monstruo invisible 
authors: [meyer]
date: 2025-07-04 11:37:00 -0500
categories: [Artículo]
tags: [privacidad, seguridad, dns]
comments: true
---

> Publicado también en [El Pozo Es-Séptico](https://elpozoeseptico.com/privacidad-y-seguridad-detras-de-un-monstruo-invisible/){:target="_blank"}
{: .prompt-info }

En la industria del software, recopilar datos es el pan de cada día, la fachada inocente que nos muestran ha sido los datos de telemetría, para la mejora de la experiencia de usuario al usar los servicios que nos ofrecen, ello bajo los términos y condiciones que aceptamos sin leer, cada vez que nos registramos en un sitio nuevo. Nadie está exento de esto, desde que empezamos a usar un sistema operativo, ya sea Windows, MacOS, algunas distribuciones de Linux, Android o iOS, etc., estamos compartiendo todo lo que el software puede recopilar bajo las leyes establecidas, leyes que no están a la par del avance del mundo de la informática.

La lista de datos que compartimos es extensa, y depende del servicio que usemos, pero así por encima encontramos datos personales, como nombres, dirección de correo electrónico, número de teléfono, edad, género, estado civil, orientación sexual y otros detalles demográficos. Suelen recopilarse también identificadores únicos del dispositivo (como el IMEI, MAC address o ID de publicidad), información de ubicación (GPS, dirección IP), datos financieros (métodos de pago, historial de compras), datos de salud y fitness (historial médico, actividad física), mensajes y comunicaciones (correos, SMS, MMS), así como actividad dentro de las aplicaciones y servicios web (tiempo de uso, clics, preferencias, historial de búsqueda). Eso no es todo, se incluyen datos del dispositivo (modelo, sistema operativo, versión del navegador), información de aplicaciones instaladas, datos de navegación y cookies (preferencias, inicio de sesión), datos de formularios, listas de contactos, archivos y documentos personales, e incluso datos biométricos como huellas dactilares o reconocimiento facial.

¿Y eso de qué les sirve? La respuesta es sencilla, para ganar más dinero. Somos el producto, nada es gratis, internet no vive en las nubes, vive en servidores físicos que necesitan electricidad, mantenimiento, que conllevan costos, costos que son minúsculos en comparación con las ganancias que obtienen de nuestros datos.

La historia no termina, navegar en internet también tiene sus riesgos, el rey del océano es el _phishing_ (pesca de humanos), donde el engaño es el anzuelo de nuestras mentes, nadie está exento de caer en esas redes. Un solo clic puede ser el comienzo de una catástrofe. ¿Y entonces? ¿Qué podemos hacer?

Ningún sistema es seguro, pero sí podemos reforzar nuestra seguridad para mitigar daños. Todo nuestro tráfico de red pasa por nuestro ISP (Internet Service Provider), el cual es el primer pescador con sus propios servidores DNS (Domain Name System), todo sitio web que visitemos, queda a la vista de ellos. Si manualmente cambiamos a otros más generales, como los servidores de Google (8.8.8.8) o Cloudflare (1.1.1.1), estaremos en manos más grandes.

Nosotros debemos tener el control, y para esto, la mejor opción es tener un Servidor DNS privado, ¿suena complicado? En realidad, es más sencillo de lo que podríamos imaginar, aunque nos podemos divertir armando un Servidor DNS en un microcontrolador con PiHole o AdGuard Home, pero en esta ocasión, veremos NextDNS, un servicio gratuito para uso personal limitado a 300,000 peticiones por mes, que es más que suficiente.

Lo primero que debemos hacer es ingresar a [nextdns.io](https://nextdns.io) y seguir las instrucciones, podemos probar el servicio sin crear una cuenta, pero es recomendado para guardar las configuraciones que realizaremos. Con esta plataforma lograremos filtrar todas las peticiones web que se realicen desde nuestro dispositivo o red (si configuramos nuestro router), podemos elegir medidas de seguridad y privacidad, bloquear aplicaciones, bloquear anuncios, datos de telemetría, usarlo como control parental, entre otras muchas opciones que son completamente gratuitas (estamos en las manos de una empresa, pero no de muchas, eso ya es un avance).

En los primeros minutos de tener configurado el DNS privado, veremos en la sección de analíticas todo el tráfico permitido y bloqueado, los resultados son sorprendentes al ver la cantidad de tráfico que sucede sin que nos demos cuenta, inclusive si no estamos usando nuestro dispositivo, la recopilación de información no duerme, es un gigante hambriento, del cual podemos protegernos tomando las medidas adecuadas.

La solución comentada es efectiva para la navegación y uso general, pero no todo el tráfico de red es web y por consiguiente este no requiere una traducción de dominio, es aquí donde soluciones avanzadas aparecen, como lo son reglas de firewall, VPN privadas, zonas de cero confianza, proxies inversos, entre otras, pero por el momento, este filtro DNS es una gran batalla ganada.
