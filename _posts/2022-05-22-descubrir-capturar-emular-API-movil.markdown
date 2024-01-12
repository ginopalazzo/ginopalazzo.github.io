---
layout: post
title:  "101 API de aplicaciones móviles"
lang: es
ref: 101-Mobile-API
description: "Cómo descubrir, capturar y emular llamadas API de aplicaciones móviles."
date:   2022-05-22 11:07:21 +0100
categories: ["Scraper"]
tags: ["API", "Data"]
---

## Introducción
El objetivo de este post es describir los pasos necesarios para descubrir y utilizar la API de una aplicación móvil de terceros (o propia para debuggear).

He dividido el problema en los siguientes puntos, que serán futuras entradas en el blog:

1. **Descubrimiento del API**: Descubir qué llamadas se realizan desde una aplicación móvil.

    `1.1 Problema` ¿Cómo capturo las llamadas al API que hace una aplicación móvil?\
    `1.1 Solución` Programa de captura de tráfico
    
    `1.2 Problema` Vaya, veo las llamadas pero están encriptadas...\
    `1.2 Solución` Man in the middle con certificados. Sólo en iOS.
        
2. **Autenticación en el API**: De nada sirve saber cuáles son las llamadas que se realizan si no podemos recrearlas. Para ello necesitamos descubrir qué mecanismos tiene la aplicación para autenticarse.
    
    `2.1 Problema` No sé cómo se optienen todos esos datos de autenticación.\
    `2.1 Solución` Ingeniería inversa. Decompilar la aplicación móvil.
    
    `2.2 Problema` No puedo leer el código.\
    `2.2 Solución` Seguir los métodos y variables decompiladas.

3. **Utilizar el API**

    Una vez sabemos qué llamadas pueden hacerse al API y cómo nos autenticamos para realizarlas, solo queda probar.

