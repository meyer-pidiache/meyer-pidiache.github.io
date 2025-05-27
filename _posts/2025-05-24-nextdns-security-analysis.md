---
title: Evaluación de la Seguridad de NextDNS
authors: [meyer, perplexity]
date: 2025-05-24 10:55:00 -0500
categories: [Cybersecurity]
tags: [dns, nextdns]
comments: true
image:
  path: /assets/img/nextdns.png
  alt: NextDNS Logo
---

NextDNS se presenta como una solución DNS revolucionaria que promete actuar como "el nuevo firewall para la Internet moderna", pero la pregunta fundamental que muchos usuarios se hacen es qué tan seguro es realmente confiar sus consultas DNS a este servicio[^1]. El análisis de la seguridad de NextDNS revela un panorama complejo que incluye tanto fortalezas significativas como algunas preocupaciones legítimas que los usuarios deben considerar antes de adoptar esta plataforma. Fundado en 2019 por dos empresarios franceses con experiencia en infraestructura de red y plataformas de streaming, NextDNS ha ganado reconocimiento por su enfoque en la privacidad y seguridad, aunque no está exento de controversias que han afectado la confianza de algunos usuarios[^2][^3].

## Características de Seguridad Técnica

NextDNS implementa múltiples capas de protección técnica que constituyen la base de su propuesta de valor en seguridad. El servicio utiliza protocolos de encriptación DNS avanzados, incluyendo DNS over HTTPS (DoH) y DNS over TLS (DoT), así como soporte para IPv6, lo que garantiza que las consultas DNS estén protegidas durante el tránsito[^2][^14]. Esta encriptación es especialmente importante en redes públicas donde las consultas DNS sin encriptar podrían ser interceptadas por operadores de red maliciosos. La plataforma también incorpora más de diez tipos diferentes de protecciones contra amenazas, utilizando fuentes de inteligencia sobre amenazas que se actualizan en tiempo real y contienen millones de dominios maliciosos conocidos[^1].

El análisis de DNS en tiempo real representa una de las características más avanzadas de NextDNS, ya que examina tanto las preguntas como las respuestas DNS en cuestión de nanosegundos para detectar y bloquear comportamientos maliciosos[^1]. Esta capacidad va más allá del simple bloqueo de dominios, permitiendo la detección de técnicas de evasión sofisticadas como el cryptojacking, ataques de phishing, algoritmos de generación de dominios (DGAs) y dominios recién registrados que a menudo se utilizan en ciberataques[^1]. La protección contra DNS rebinding y la detección de homógrafos IDN añaden capas adicionales de seguridad que protegen contra ataques más sofisticados que podrían comprometer la seguridad del dispositivo del usuario.

### Implementación de Protocolos Encriptados

La implementación técnica de los protocolos encriptados en NextDNS demuestra un compromiso serio con la seguridad de las comunicaciones. El servicio es compatible nativa con dispositivos Android 9+ a través de la función "DNS Privado" en configuraciones de red, y navegadores como Firefox y Chrome pueden hacer uso de la seguridad DNS de forma nativa[^2]. Para dispositivos que no soportan DoH/DoT de manera predeterminada, NextDNS proporciona aplicaciones oficiales para Windows, Mac, iOS y Android, asegurando que todos los usuarios puedan beneficiarse de la encriptación DNS independientemente de su plataforma[^2]. Esta amplia compatibilidad técnica significa que los usuarios pueden mantener la seguridad DNS encriptada en todos sus dispositivos sin comprometer la funcionalidad.

## Políticas de Privacidad y Gestión de Datos

La política de privacidad de NextDNS establece principios que, en teoría, deberían tranquilizar a los usuarios preocupados por la seguridad de sus datos. La empresa afirma categóricamente que no vende, licencia, sublicencia o comparte ninguno de los datos enviados directa o indirectamente por sus usuarios con ninguna persona o entidad[^19]. Además, NextDNS Inc. se describe como una empresa independiente 100% financiada, poseída y controlada por sus fundadores, prometiendo que nunca se involucrará en actividades de venta o intercambio de datos, ahora o en el futuro[^19]. El servicio también implementa una política de "Lo que ves es lo que tenemos" (WYSIWWH), permitiendo a los usuarios ver, exportar o eliminar cada bit de sus datos en cualquier momento[^19].

Sin embargo, la historia de NextDNS no está exenta de controversias relacionadas con la privacidad que han generado preocupaciones legítimas entre algunos usuarios. En 2020, se descubrió que NextDNS había violado su propia política de privacidad al compartir direcciones de correo electrónico de usuarios con Intercom, un servicio de chat de soporte de terceros, sin consentimiento explícito[^3][^7]. Lo más preocupante fue que, en lugar de corregir la violación, NextDNS optó por modificar su política de privacidad para permitir el comportamiento que había sido detectado, eliminando específicamente la palabra "compartir" de su declaración de política[^3]. Esta controversia generó desconfianza en parte de la comunidad de usuarios y planteó preguntas sobre el compromiso real de la empresa con la privacidad de los datos.

### Transparencia en el Manejo de Datos

NextDNS mantiene que cuando el servicio entra en contacto con datos de usuario que no deben ser registrados, estos se descartan lo más rápidamente posible[^19]. Durante el proceso de respuesta a una consulta DNS, el servidor supuestamente descarta todos los datos de solicitud y respuesta inmediatamente después de enviar la respuesta al usuario[^19]. Para características que requieren algún tipo de retención de datos, los usuarios reciben la opción, control y acceso completo a lo que se registra y por cuánto tiempo[^19]. El servicio también utiliza una implementación innovadora interna de EDNS Client Subnet que no expone la dirección IP del usuario a servidores DNS autoritativos, y aplica Query Name Minimisation para proteger adicionalmente la privacidad[^19].

## Arquitectura de Seguridad y Vulnerabilidades Potenciales

La arquitectura de seguridad de NextDNS presenta tanto fortalezas como posibles puntos débiles que los usuarios deben considerar. Un aspecto preocupante señalado por algunos expertos en seguridad es el uso de subdominios personalizados, que podrían crear vulnerabilidades si la cuenta de NextDNS de un usuario se ve comprometida[^8]. Si un atacante obtiene acceso a la cuenta de un usuario, podría potencialmente manipular la configuración DNS de manera que deje al usuario mucho menos seguro de lo que estaba inicialmente[^8]. Esta vulnerabilidad surge del hecho de que NextDNS utiliza identificadores de perfil personalizados para aplicar políticas de filtrado específicas, lo que crea una dependencia en la seguridad de la cuenta del usuario.

Otra consideración importante es que NextDNS, como cualquier servicio DNS centralizado, requiere que los usuarios confíen en la empresa con metadatos potencialmente sensibles sobre su actividad en línea. Aunque el servicio puede proteger contra muchas amenazas a nivel de dominio, no puede ayudar con sitios web y aplicaciones que explotan vulnerabilidades en el sistema operativo subyacente del dispositivo[^16]. Esto significa que NextDNS debe considerarse como una capa de protección adicional, no como una solución de seguridad completa que elimine la necesidad de otras medidas de seguridad.

### Limitaciones del Modelo de Seguridad

El modelo de seguridad de NextDNS tiene limitaciones inherentes que los usuarios deben comprender. El servicio no puede ocultar la dirección IP del sitio web de destino ni encriptar el tráfico que no sea DNS, como las consultas de búsqueda reales enviadas a motores como Google[^14]. Esto significa que, aunque NextDNS puede proteger las consultas DNS y bloquear dominios maliciosos, no proporciona el mismo nivel de protección que una VPN completa para todo el tráfico de Internet. Además, el hecho de que el software del servidor no sea de código abierto, a diferencia de los clientes de NextDNS, plantea preguntas sobre la capacidad de auditoría independiente del sistema[^17].

## Reconocimiento y Certificaciones de Terceros

NextDNS ha obtenido cierto reconocimiento de terceros que respalda su credibilidad en el espacio de seguridad DNS. La empresa fue aceptada como un Resolver Recursivo Confiable (TRR) de Mozilla el 17 de diciembre de 2019, lo que significa que cumple con los estrictos requisitos contractuales de Mozilla para proveedores de DNS[^11][^17]. Mozilla implementó esta certificación como parte de su iniciativa DNS-over-HTTPS, eligiendo proveedores que estuvieran dispuestos a cumplir con contratos estrictos de privacidad y seguridad[^11]. Sin embargo, es importante notar que los requisitos de TRR de Mozilla se aplican específicamente al subdominio firefox.dns.nextdns.io y no necesariamente a todos los endpoints de NextDNS[^11].

La participación en el programa TRR de Mozilla requiere que NextDNS mantenga estándares específicos de logging y privacidad que no necesariamente se aplican a todos sus servicios[^11]. Esto significa que mientras firefox.dns.nextdns.io debe cumplir con requisitos estrictos de no logging, los endpoints personalizados de NextDNS pueden tener políticas diferentes dependiendo de las configuraciones del usuario. A pesar de esta validación de terceros, algunos usuarios han notado la ausencia de auditorías de seguridad independientes o casos judiciales donde NextDNS haya podido demostrar definitivamente su política de "no logs"[^17].

### Reputación en la Comunidad de Seguridad

La reputación de NextDNS en la comunidad de seguridad es generalmente positiva, aunque no sin reservas. Muchos expertos en privacidad y seguridad reconocen el valor del servicio, especialmente cuando se compara con alternativas como Pi-hole sin Unbound[^17]. El modelo de negocio freemium de NextDNS, donde los clientes pagan por el servicio, se considera más transparente que los modelos donde el usuario es el producto[^17]. Sin embargo, la falta de auditorías independientes publicadas y algunos incidentes de privacidad han generado cierta cautela entre los usuarios más conscientes de la seguridad.

## Consideraciones Específicas de Implementación

La implementación práctica de NextDNS presenta varios escenarios que afectan la seguridad general del sistema. Para usuarios que ejecutan servidores DNS locales como Bind9, la integración con NextDNS requiere consideraciones especiales para mantener la encriptación[^4]. Bind9 solo soporta DoT y DoH para conexiones downstream, no upstream, lo que significa que los usuarios necesitan un reenviador que acepte consultas DNS no encriptadas y las envíe a través de DoH/DoT[^4]. Esta limitación técnica puede crear puntos débiles en la cadena de seguridad si no se configura correctamente.

Para implementaciones empresariales y usuarios avanzados, NextDNS ofrece capacidades de análisis que pueden ser valiosas para la seguridad, pero que también plantean preguntas sobre la retención de datos[^13]. El servicio permite a los usuarios analizar registros, modificar listas de bloqueo y comprender qué metadatos, datos o filtraciones de privacidad y seguridad pueden tener[^13]. Desde una perspectiva de InfoSec, la capacidad de reconocer las consultas DNS de salida y el volumen de las mismas es realmente útil, ya que bloquea automáticamente rastreo, dominios recién registrados, URLs de malware, etc.[^13].

### Configuración de Seguridad Óptima

Para maximizar la seguridad al usar NextDNS, los usuarios deben considerar varias configuraciones específicas. La habilitación de protección contra amenazas múltiples, incluyendo Google Safe Browsing, protección contra cryptojacking, y bloqueo de dominios recién registrados, proporciona capas adicionales de seguridad[^1]. Los usuarios también deben configurar cuidadosamente las listas de bloqueo y whitelist para equilibrar la seguridad con la funcionalidad, ya que el bloqueo excesivo puede interferir con aplicaciones y servicios legítimos[^12]. La configuración de logging debe ajustarse según las necesidades de privacidad individuales, recordando que habilitar logs detallados puede comprometer la privacidad pero proporciona mejores capacidades de análisis de seguridad.

## Comparación con Alternativas y Contexto del Mercado

En el contexto del mercado actual de servicios DNS seguros, NextDNS se posiciona como una alternativa intermedia entre servicios básicos como Cloudflare DNS y soluciones auto-hospedadas como Pi-hole. Comparado con el DNS público de Google (8.8.8.8), NextDNS ofrece ventajas significativas en términos de privacidad, ya que Google utiliza los datos DNS como parte de su modelo de negocio de vigilancia publicitaria[^13]. Cloudflare representa una buena alternativa que también protege la privacidad, pero carece de las capacidades de análisis detallado y personalización que ofrece NextDNS[^13].

La propuesta de valor de NextDNS radica en su capacidad de ofrecer funcionalidades similares a Pi-hole o AdGuard, pero sin la necesidad de mantener hardware conectado permanentemente al router ni realizar configuración y mantenimiento de software[^5]. Esto hace que NextDNS sea especialmente atractivo para usuarios que desean protección avanzada sin la complejidad técnica de las soluciones auto-hospedadas. Sin embargo, los usuarios deben considerar que confiar en un servicio en la nube significa depender de la seguridad e integridad de esa empresa, a diferencia de las soluciones auto-hospedadas donde mantienen control total.

### Análisis Costo-Beneficio de Seguridad

El modelo de precios de NextDNS afecta directamente las consideraciones de seguridad. El servicio es completamente gratuito hasta 300,000 consultas mensuales, luego requiere una suscripción de €1.99 al mes o €19.90 anuales[^5]. Para un usuario individual con dispositivos básicos conectados, 300,000 consultas mensuales son suficientes, pero para hogares con múltiples usuarios, dispositivos inteligentes y domótica, este límite puede consumirse en aproximadamente diez días[^5]. Esta limitación puede forzar a los usuarios a elegir entre pagar por el servicio o cambiar a alternativas potencialmente menos seguras cuando se agote su cuota gratuita.

## Conclusión

NextDNS representa una solución de seguridad DNS sólida que ofrece protecciones significativas contra amenazas comunes de Internet, aunque no está exento de consideraciones importantes que los usuarios deben evaluar. Las fortalezas del servicio incluyen su implementación robusta de protocolos DNS encriptados, capacidades avanzadas de detección de amenazas en tiempo real, y un modelo de negocio transparente que no depende de la venta de datos de usuarios. La participación en el programa de Resolver Recursivo Confiable de Mozilla y las características técnicas avanzadas como la protección contra cryptojacking y el análisis de comportamiento malicioso demuestran un compromiso serio con la seguridad.

Sin embargo, los usuarios deben ser conscientes de las limitaciones y controversias asociadas con el servicio. La modificación previa de la política de privacidad para acomodar violaciones detectadas plantea preguntas legítimas sobre el compromiso a largo plazo con la privacidad de datos. Las vulnerabilidades potenciales relacionadas con subdominios personalizados y la naturaleza centralizada del servicio requieren que los usuarios confíen en la seguridad operacional de NextDNS. Además, las limitaciones inherentes del modelo DNS significa que NextDNS debe considerarse como una capa de protección complementaria, no como una solución de seguridad integral.

Para la mayoría de usuarios, especialmente aquellos que buscan mejorar su seguridad DNS sin la complejidad de soluciones auto-hospedadas, NextDNS ofrece un nivel de protección considerable que supera significativamente el uso de servicios DNS predeterminados del ISP o proveedores comerciales orientados a la recolección de datos. La recomendación es utilizar NextDNS como parte de una estrategia de seguridad más amplia que incluya otras medidas como software antivirus actualizado, navegadores seguros y, para usuarios con mayores necesidades de privacidad, herramientas adicionales como VPNs de confianza.

## Referencias

[^1]: <a href="https://nextdns.io" target="_blank" rel="noopener">https://nextdns.io</a>

[^2]: <a href="https://vpnreviewer.com/nextdns-review" target="_blank" rel="noopener">https://vpnreviewer.com/nextdns-review</a>

[^3]: <a href="https://www.reddit.com/r/nextdns/comments/ju2hfw/nextdns_privacy_policy_change_now_allows_them_to/" target="_blank" rel="noopener">https://www.reddit.com/r/nextdns/comments/ju2hfw/nextdns_privacy_policy_change_now_allows_them_to/</a>

[^4]: <a href="https://help.nextdns.io/t/60yqm2w/bind9-using-encrypted-dns" target="_blank" rel="noopener">https://help.nextdns.io/t/60yqm2w/bind9-using-encrypted-dns</a>

[^5]: <a href="https://burp.es/privacidad-en-los-dns-nextdns/" target="_blank" rel="noopener">https://burp.es/privacidad-en-los-dns-nextdns/</a>

[^6]: <a href="https://www.qnapclub.es/showthread.php?tid=3977" target="_blank" rel="noopener">https://www.qnapclub.es/showthread.php?tid=3977</a>

[^7]: <a href="https://news.ycombinator.com/item?id=25070699" target="_blank" rel="noopener">https://news.ycombinator.com/item?id=25070699</a>

[^8]: <a href="https://help.nextdns.io/t/35h5jl2/improve-security-and-privacy-by-switching-from-personalized-to-generic-subdomain" target="_blank" rel="noopener">https://help.nextdns.io/t/35h5jl2/improve-security-and-privacy-by-switching-from-personalized-to-generic-subdomain</a>

[^9]: <a href="https://help.nextdns.io/t/m1hskqp/how-secure-does-nextdns-treat-its-root-ca-secrets-internally" target="_blank" rel="noopener">https://help.nextdns.io/t/m1hskqp/how-secure-does-nextdns-treat-its-root-ca-secrets-internally</a>

[^10]: <a href="https://help.nextdns.io/t/x2yz4tm/its-time-for-nextdns-to-implement-odoh" target="_blank" rel="noopener">https://help.nextdns.io/t/x2yz4tm/its-time-for-nextdns-to-implement-odoh</a>

[^11]: <a href="https://help.nextdns.io/t/x2hc1k8/mozilla-contractual-obligations-in-relation-to-public-nextdns" target="_blank" rel="noopener">https://help.nextdns.io/t/x2hc1k8/mozilla-contractual-obligations-in-relation-to-public-nextdns</a>

[^12]: <a href="https://www.redeszone.net/tutoriales/seguridad/nextdns-cortafuegos-protege-amenazas-internet/" target="_blank" rel="noopener">https://www.redeszone.net/tutoriales/seguridad/nextdns-cortafuegos-protege-amenazas-internet/</a>

[^13]: <a href="https://www.cndltd.com/blog-mobile/technical-blog/review-nextdns-privacy-security-splunk?tmpl=component\&print=1\&format=print" target="_blank" rel="noopener">https://www.cndltd.com/blog-mobile/technical-blog/review-nextdns-privacy-security-splunk?tmpl=component\&print=1\&format=print</a>

[^14]: <a href="https://www.youtube.com/watch?v=0xkGeqb07Cg" target="_blank" rel="noopener">https://www.youtube.com/watch?v=0xkGeqb07Cg</a>

[^15]: <a href="https://ciroapp.com/es/nextdns-review/" target="_blank" rel="noopener">https://ciroapp.com/es/nextdns-review/</a>

[^16]: <a href="https://help.nextdns.io/t/y4yyn0s/secure-from-cyberattack" target="_blank" rel="noopener">https://help.nextdns.io/t/y4yyn0s/secure-from-cyberattack</a>

[^17]: <a href="https://www.reddit.com/r/privacytoolsIO/comments/milkyd/do_you_trust_nextdns/" target="_blank" rel="noopener">https://www.reddit.com/r/privacytoolsIO/comments/milkyd/do_you_trust_nextdns/</a>

[^18]: <a href="https://www.incibe.es/menores/familias/control-parental/nextdns" target="_blank" rel="noopener">https://www.incibe.es/menores/familias/control-parental/nextdns</a>

[^19]: <a href="https://nextdns.io/privacy" target="_blank" rel="noopener">https://nextdns.io/privacy</a>

[^20]: <a href="https://www.youtube.com/watch?v=nGCCqOLK5uE" target="_blank" rel="noopener">https://www.youtube.com/watch?v=nGCCqOLK5uE</a>

[^21]: <a href="https://www.youtube.com/watch?v=18Ihx_K0pVg" target="_blank" rel="noopener">https://www.youtube.com/watch?v=18Ihx_K0pVg</a>

[^22]: <a href="https://www.reddit.com/r/nextdns/comments/xp1a2k/is_using_nextdns_and_a_vpn_good_bad_or_indifferent/?tl=es-es" target="_blank" rel="noopener">https://www.reddit.com/r/nextdns/comments/xp1a2k/is_using_nextdns_and_a_vpn_good_bad_or_indifferent/?tl=es-es</a>

[^23]: <a href="https://www.reddit.com/r/nextdns/comments/159ppnd/your_honest_opinon_needed_why_did_you_choose_to/?tl=es-es" target="_blank" rel="noopener">https://www.reddit.com/r/nextdns/comments/159ppnd/your_honest_opinon_needed_why_did_you_choose_to/?tl=es-es</a>

[^24]: <a href="https://www.reddit.com/r/nextdns/comments/i9zwxl/nextdns_safety_and_security/?tl=es-es" target="_blank" rel="noopener">https://www.reddit.com/r/nextdns/comments/i9zwxl/nextdns_safety_and_security/?tl=es-es</a>

[^25]: <a href="https://play.google.com/store/apps/details?id=com.doubleangels.nextdnsmanagement" target="_blank" rel="noopener">https://play.google.com/store/apps/details?id=com.doubleangels.nextdnsmanagement</a>

[^26]: <a href="https://www.reddit.com/r/nextdns/comments/1i0u6u9/is_nextdns_worth_it/?tl=es-es" target="_blank" rel="noopener">https://www.reddit.com/r/nextdns/comments/1i0u6u9/is_nextdns_worth_it/?tl=es-es</a>

[^27]: <a href="https://help.nextdns.io" target="_blank" rel="noopener">https://help.nextdns.io</a>

[^28]: <a href="https://www.reddit.com/r/nextdns/comments/14ox7hl/there_is_a_dns_security_vulnerability_dns_address/" target="_blank" rel="noopener">https://www.reddit.com/r/nextdns/comments/14ox7hl/there_is_a_dns_security_vulnerability_dns_address/</a>

[^29]: <a href="https://controld.com/blog/dnsfilter-vs-nextdns/" target="_blank" rel="noopener">https://controld.com/blog/dnsfilter-vs-nextdns/</a>

[^30]: <a href="https://www.reddit.com/r/firefox/comments/u0qfz3/what_is_nextdns_and_why_do_i_have_it/?tl=es-419" target="_blank" rel="noopener">https://www.reddit.com/r/firefox/comments/u0qfz3/what_is_nextdns_and_why_do_i_have_it/?tl=es-419</a>

[^31]: <a href="https://help.nextdns.io/category/discussions/?pg=1\&sort=active" target="_blank" rel="noopener">https://help.nextdns.io/category/discussions/?pg=1\&sort=active</a>

[^32]: <a href="https://www.reddit.com/r/ISO27001/comments/16ynwec/how_to_answer_this_please_provide_evidence/" target="_blank" rel="noopener">https://www.reddit.com/r/ISO27001/comments/16ynwec/how_to_answer_this_please_provide_evidence/</a>

[^33]: <a href="https://www.reddit.com/r/InternetBrasil/comments/v1t5rj/nextdns_%C3%A9_bom/" target="_blank" rel="noopener">https://www.reddit.com/r/InternetBrasil/comments/v1t5rj/nextdns_%C3%A9_bom/</a>

[^34]: <a href="https://www.siteconfiavel.com.br/site/nextdns-com" target="_blank" rel="noopener">https://www.siteconfiavel.com.br/site/nextdns-com</a>

[^35]: <a href="https://edgeuno.com/es/success-stories/nextdns-reduces-latency-and-increases-quality-of-service-across-latam/" target="_blank" rel="noopener">https://edgeuno.com/es/success-stories/nextdns-reduces-latency-and-increases-quality-of-service-across-latam/</a>

[^36]: <a href="https://www.youtube.com/watch?v=K02C1xsB6Ww" target="_blank" rel="noopener">https://www.youtube.com/watch?v=K02C1xsB6Ww</a>

[^37]: <a href="https://jfrog.com/blog/cve-2025-29927-next-js-authorization-bypass/" target="_blank" rel="noopener">https://jfrog.com/blog/cve-2025-29927-next-js-authorization-bypass/</a>

[^38]: <a href="https://www.cndltd.com/blog-mobile/technical-blog/review-nextdns-privacy-security-splunk" target="_blank" rel="noopener">https://www.cndltd.com/blog-mobile/technical-blog/review-nextdns-privacy-security-splunk</a>

[^39]: <a href="https://help.nextdns.io/t/h7hswav/potential-nextdns-securityprivacy-risk-due-to-ds-lite-tunnel-and-linked-ip" target="_blank" rel="noopener">https://help.nextdns.io/t/h7hswav/potential-nextdns-securityprivacy-risk-due-to-ds-lite-tunnel-and-linked-ip</a>

[^40]: <a href="https://help.nextdns.io/t/g9hjacc/enhanced-account-security" target="_blank" rel="noopener">https://help.nextdns.io/t/g9hjacc/enhanced-account-security</a>

[^41]: <a href="https://help.nextdns.io/t/q6hwwbg/dnsfilters-problematic-test-tool-that-misrepresents-every-dns-service-including-nextdns" target="_blank" rel="noopener">https://help.nextdns.io/t/q6hwwbg/dnsfilters-problematic-test-tool-that-misrepresents-every-dns-service-including-nextdns</a>

[^42]: <a href="https://help.nextdns.io/t/h7ykqth/hipaa-compliance" target="_blank" rel="noopener">https://help.nextdns.io/t/h7ykqth/hipaa-compliance</a>

[^43]: <a href="https://help.nextapp.co/en/articles/6781793-soc-2-type-ii" target="_blank" rel="noopener">https://help.nextapp.co/en/articles/6781793-soc-2-type-ii</a>

[^44]: <a href="https://help.nextdns.io/t/q6hw5td/is-there-doc-for-test-nextdns-io" target="_blank" rel="noopener">https://help.nextdns.io/t/q6hw5td/is-there-doc-for-test-nextdns-io</a>

[^45]: <a href="https://help.nextdns.io/t/g9hmv0a/how-to-install-and-trust-nextdns-root-ca" target="_blank" rel="noopener">https://help.nextdns.io/t/g9hmv0a/how-to-install-and-trust-nextdns-root-ca</a>

[^46]: <a href="https://help.nextdns.io/t/x2y31df/mozzilla-firefox-help-this-device-is-using-nextdns-with-no-profile-make-sure-you-use-the-dns-over-https-endpoint-shown-below" target="_blank" rel="noopener">https://help.nextdns.io/t/x2y31df/mozzilla-firefox-help-this-device-is-using-nextdns-with-no-profile-make-sure-you-use-the-dns-over-https-endpoint-shown-below</a>

[^47]: <a href="https://blog.mozilla.org/en/mozilla/firefox-announces-new-partner-in-delivering-private-and-secure-dns-services-to-users/" target="_blank" rel="noopener">https://blog.mozilla.org/en/mozilla/firefox-announces-new-partner-in-delivering-private-and-secure-dns-services-to-users/</a>

[^48]: <a href="https://news.sophos.com/en-us/2019/12/18/mozilla-adds-nextdns-to-list-of-dns-over-https-providers/" target="_blank" rel="noopener">https://news.sophos.com/en-us/2019/12/18/mozilla-adds-nextdns-to-list-of-dns-over-https-providers/</a>

[^49]: <a href="https://bugzilla.mozilla.org/show_bug.cgi?id=1644444" target="_blank" rel="noopener">https://bugzilla.mozilla.org/show_bug.cgi?id=1644444</a>

[^50]: <a href="https://dev.to/alvarosdev/usando-nextdns-cli-en-tu-red-1o6n" target="_blank" rel="noopener">https://dev.to/alvarosdev/usando-nextdns-cli-en-tu-red-1o6n</a>

[^51]: <a href="https://www.twingate.com/docs/nextdns-configuration" target="_blank" rel="noopener">https://www.twingate.com/docs/nextdns-configuration</a>

[^52]: <a href="https://help.nextdns.io/t/m1yfjz3/iso-27001-andor-similar-certifications" target="_blank" rel="noopener">https://help.nextdns.io/t/m1yfjz3/iso-27001-andor-similar-certifications</a>

[^53]: <a href="https://www.lautenbacher.io/en/general/recommendation-more-security-with-nextdns/" target="_blank" rel="noopener">https://www.lautenbacher.io/en/general/recommendation-more-security-with-nextdns/</a>

[^54]: <a href="https://www.teamworkims.co.uk/nextens-achieves-iso-27001-certification/" target="_blank" rel="noopener">https://www.teamworkims.co.uk/nextens-achieves-iso-27001-certification/</a>

[^55]: <a href="https://nextdc.com/hubfs/ISO_27001_Certification_Final.pdf" target="_blank" rel="noopener">https://nextdc.com/hubfs/ISO_27001_Certification_Final.pdf</a>
