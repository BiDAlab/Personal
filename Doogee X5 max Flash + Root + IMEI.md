Los pasos a seguir para flashear la última versión de software para el terminal `Doogee X5 max`, rootearlo (adquirir privilegios de administrador) y recuperar el número `IMEI` (necesario para utilizar servicios de telefonía) son los siguientes:  

1. Descargar y descomprimir la siguiente carpeta comprimida: [<enlace>]().
2. Instalar los drivers en el ordenador ejecutando `InstallDriver.exe` o `installdrv64.exe` del directorio `MT6577 USB VCOM drivers`.
3. Abrir flash_tool.exe de `SP-Flash-Tool`.
4. Examinar el *.txt del flash en `Scatter-loading. Éste se encuentra en `DOOGEE\MP_mt6580_w370_a48-01_daoge_cc_64gbitp8d3_m_wcdma_mul_20170330-112347_songlixin_PC`.
5. Seleccionar la opción `Format All + Download`.
6. Iniciar `Download`.
7. Conectar el terminal mediante USB.
8. Esperar a que `Download Ok`.
9. Desconectar el terminal.
10. Cargar el segundo *.txt scatter de `TWRP`.
11. Iniciar `Download`.
12. Conectar el terminal mediante USB.
13. Esperar a que `Download Ok`.
14. Desconectar el terminal.
15. Encender el terminal presionando `Power + VolUp`.
16. Seleccionar `Recovery mode` con `VolDown`.
17. Deslizar el dedo para confirmar.
18. Tocar `Install`.
19. Seleccionar la ruta a la carpeta comprimida que contiene SuperSU.
20. Confirmar instalación de SuperSU.
21. Esperar a que instale.
22. `Reboot system`.
23. Esperar a que se inicie el terminal normalmente.
24. Si no arranca, volver al paso 1.
25. Para modificar el terminal con seguridad, no conectar a internet.
26. Es recomendable transferir las aplicaciones mediante USB a la tarjeta SD, por lo que al instalar habrá que confirmar `instalar aplicaciones de sitios externos`.
27. Hacer las modificaciones necesarias. Si algo requiere acceso root, se solicitará confirmación: confirmar.
28. Apps maliciosas en X5 max: [com.gangyun.beautysnap, com.android.Pet.mediaproxy, com.dataviz.docstogo, com.lbe.parallel.intl, com.mediatek.factorymode].
29. Desinstaladas igualmente: [com.adups.fota, com.android.galaxy4, com.google.android.setupwizard, com.android.noisefield, com.android.managedprovisioning, com.android.wallpaper.holospiral, com.mediatek.hetcomm, cn.xender, com.adups.fota.sysoper].
30. Reiniciar.
31. Comprobar que en `Ajustes/"Información del teléfono"/Estado/"Información del IMEI"/` no hay `IMEI`. En caso contrario saltar al paso 33.
32. Instalar `Chamelephone`.
33. Ejecutar Chamelephone con permisos root.
34. Escribir los `IMEI` del terminal. Éstos vienen en la caja original del terminal, son únicos por dispositivo y no es legal utilizar uno que no te pertenece.
35. Reiniciar el terminal.
36. Conectar a Wi-Fi.
37. Uso normal del terminal.
