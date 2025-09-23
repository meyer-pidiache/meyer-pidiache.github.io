---
title: Zero Trust + IA
authors: [meyer]
date: 2025-09-21 20:15:00 -0500
categories: [Artículo]
tags: [zero-trust, ia, seguridad, red, firewall]
comments: true
---

Zero Trust o Zero Trust Architecture (ZTA) es una estrategia de seguridad con ciertos principios: verificar de forma explícita, utilizar el acceso con menos privilegios y asunción de que hay brechas (vulnerabilidades). En lugar de confiar en el firewall que se esté usando, se comprueba todas las solicitudes independientemente del origen, es decir, cero confianza[^1].

Se puede ver como una filosofía, tanto para individuos como para organizaciones, donde se verifica de extremo a extremo el comportamiento de red en busca de amenazas, logrando con ello mayor seguridad para cuentas de usuario, dispositivos, aplicaciones y datos. Su relevancia se puede ver con la orden ejecutiva de Estados Unidos 14028, que establece su implementación en la infraestructura digital del gobierno federal[^2].

A pesar de todas estas medidas, no es suficiente para las crecientes amenazas actuales, que llegan a sobrepasar esta arquitectura de seguridad, es por eso que un nuevo complemento está jugando un fuerte rol, hablo del uso de la IA en el proceso de comprobación de todas las solicitudes, ya sea para procesos de automatización de respuesta y mitigación de amenazas, control de acceso granular en tiempo real o análisis de comportamiento en busca de anomalías como lo menciona Achanta[^3].

Tenemos que considerar que antes de la llegada de la IA, las políticas y reglas de seguridad se basaban en diccionario, es decir, en usar grandes listas de reglas que bloqueaban el comportamiento anómalo en una infraestructura, o el análisis humano del tráfico de red, siendo ello un punto débil para amenazas desconocidas como las de días cero (Zero Day), y no podemos ignorar las limitaciones de las habilidades humanas. Es ahí donde la IA juega su papel, al poder procesar grandes cantidades de datos y lograr detectar ataques sofisticados, y no solo eso, también actuar en minutos para mitigar daños, como se menciona en esta investigación de Karamchand[^4]. Las limitaciones de su implementación no son algo oculto, van desde sistemas legados hasta costos de computación (la IA lo requiere).

Uno de los líderes de seguridad e implementación de IA es Cloudflare, que tiene un Firewall de Aplicaciones Web (WAF) que ha mostrado resultados de éxito, y es que esta empresa tiene la infraestructura y el flujo de datos suficiente para entrenar sus modelos de aprendizaje automático, ya sea con técnicas que involucran supervisión o no, siendo esto una mejora significativa de los controles tradicionales (basados en reglas fijas)[^5]. Debo decir que no solo es teoría o tendencia, un resultado que destaca es el de día cero en Ivanti Connect Secure[^6], que Cloudflare con su WAF logró detectar y bloquear antes de que se hiciera pública su regla específica para este vector de ataque, en otras palabras, logró inhabilitar la vulnerabilidad Zero Day antes de que se hiciera pública, todo un triunfo.

Así como los atacantes están usando la IA para fines de hacking no ético, la industria se está moldeando para enfrentar estos desafíos y otros que un pasado eran complicados de sobrepasar, esto nos da a entender que la infraestructura digital actual requiere de atención. Como todo sistema, las actualizaciones son necesarias frente al contexto que se le ponga, y en este caso, hablamos de algo crítico, que involucra tanto a particulares como a grandes empresas. Es nuestra responsabilidad protegernos en este mundo digital cada vez más dominante.

***

## Bibliografía

[^1]: [https://learn.microsoft.com/es-mx/security/zero-trust/zero-trust-overview](https://learn.microsoft.com/es-mx/security/zero-trust/zero-trust-overview)
[^2]: [https://learn.microsoft.com/es-es/entra/standards/memo-22-09-meet-identity-requirements](https://learn.microsoft.com/es-es/entra/standards/memo-22-09-meet-identity-requirements)
[^3]: [https://cloudsecurityalliance.org/blog/2025/02/27/how-is-ai-strengthening-zero-trust](https://cloudsecurityalliance.org/blog/2025/02/27/how-is-ai-strengthening-zero-trust)
[^4]: [https://doi.org/10.30574/wjarr.2024.24.3.3883](https://doi.org/10.30574/wjarr.2024.24.3.3883)
[^5]: [https://blog.cloudflare.com/making-waf-ai-models-go-brr/](https://blog.cloudflare.com/making-waf-ai-models-go-brr/)
[^6]: [https://blog.cloudflare.com/how-cloudflares-ai-waf-proactively-detected-ivanti-connect-secure-critical-zero-day-vulnerability/](https://blog.cloudflare.com/how-cloudflares-ai-waf-proactively-detected-ivanti-connect-secure-critical-zero-day-vulnerability/)
