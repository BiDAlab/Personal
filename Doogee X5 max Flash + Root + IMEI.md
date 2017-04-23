### English:
**Steps to be followed in order to flash the latest software for `Doogee X5 max`, root and recover `IMEI` number:**

1. Download and extract this file: [http://www.mediafire.com/file/cx40cjbzng038nm/Files_-_X5_max.zip](http://www.mediafire.com/file/cx40cjbzng038nm/Files_-_X5_max.zip).
2. Install the drivers by running `InstallDriver.exe` or `installdrv64.exe` from `MT6577 USB VCOM drivers`.
3. Run `flash_tool.exe` from `SP-Flash-Tool`.
4. Load the `*.txt` of the ROM to be flashed by pressing `Scatter-loading. It's located in `DOOGEE\MP_mt6580_w370_a48-01_daoge_cc_64gbitp8d3_m_wcdma_mul_20170330-112347_songlixin_PC`.
5. Choose `Format All + Download` option.
6. Run `Download`.
7. Plug in the device by USB.
8. Wait untill the message `Download Ok` shows up.
9. Plug out the device.
10. Load the next `*.txt` scatter from `TWRP`. 
11. Choose `Download only` and run `Download`.
12. Plug in the device by USB.
13. Wait until the message `Download Ok` shows up.
14. Plug out the device.
15. Turn on the device by pressing `Power + VolUp`.
16. Choose `Recovery mode` with `VolDown`.
17. Swipe to confirm.
18. `Install`.
19. Choose the path to the storage unit that has SuperSU inside.
20. Swipe to confirm the installation of SuperSU.
21. Wait until SuperSU is installed.
22. `Reboot system`.
23. Wait until the phone turns on normally.
24. If it doesn't turn on, step back to 3.
25. Do not turn on WiFi connection yet.
26. Confirm installing third party apps and save them to the SD card from your computer.
27. Modify the software. If something requires root access, confirmation will be asked: confirm.
28. Malicious apps list: [com.gangyun.beautysnap, com.android.Pet.mediaproxy, com.dataviz.docstogo, com.lbe.parallel.intl, com.mediatek.factorymode].
29. Uninstalled anyway: [com.adups.fota, com.android.galaxy4, com.google.android.setupwizard, com.android.noisefield, com.android.managedprovisioning, com.android.wallpaper.holospiral, com.mediatek.hetcomm, cn.xender, com.adups.fota.sysoper].
30. Reboot.
31. Check that there's no `IMEI` number in `Tools/"Phone information"/Status/"IMEI information"/`. If there is an `IMEI` number, step forward to 36.
32. Install `Chamelephone`.
33. Run Chamelephone with root privileges.
34. Type in the `IMEI` number of your device. They are printed in the phone's case and it is illegal to use different ones.
35. Reboot the device.
36. Turn on WiFi.
37. Normal usage.

### Español:
**Los pasos a seguir para flashear la última versión de software para el terminal `Doogee X5 max`, rootearlo *(adquirir privilegios de administrador)* y recuperar el número `IMEI` *(necesario para utilizar servicios de telefonía)* son los siguientes:**  

1. Descargar y descomprimir la siguiente carpeta comprimida: [http://www.mediafire.com/file/cx40cjbzng038nm/Files_-_X5_max.zip](http://www.mediafire.com/file/cx40cjbzng038nm/Files_-_X5_max.zip).
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
24. Si no arranca, volver al paso 3.
25. Para modificar el terminal con seguridad, no conectar a internet.
26. Es recomendable transferir las aplicaciones mediante USB a la tarjeta SD, por lo que al instalar habrá que confirmar `instalar aplicaciones de sitios externos`.
27. Hacer las modificaciones necesarias. Si algo requiere acceso root, se solicitará confirmación: confirmar.
28. Apps maliciosas en X5 max: [com.gangyun.beautysnap, com.android.Pet.mediaproxy, com.dataviz.docstogo, com.lbe.parallel.intl, com.mediatek.factorymode].
29. Desinstaladas igualmente: [com.adups.fota, com.android.galaxy4, com.google.android.setupwizard, com.android.noisefield, com.android.managedprovisioning, com.android.wallpaper.holospiral, com.mediatek.hetcomm, cn.xender, com.adups.fota.sysoper].
30. Reiniciar.
31. Comprobar que en `Ajustes/"Información del teléfono"/Estado/"Información del IMEI"/` no hay `IMEI`. En caso contrario saltar al paso 36.
32. Instalar `Chamelephone`.
33. Ejecutar Chamelephone con permisos root.
34. Escribir los `IMEI` del terminal. Éstos vienen en la caja original del terminal, son únicos por dispositivo y no es legal utilizar uno que no te pertenece.
35. Reiniciar el terminal.
36. Conectar a Wi-Fi.
37. Uso normal del terminal.
