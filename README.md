# ESXi670-201912001

** Falta saber si con la ISO nativa de vSphere 6.7 deja instalar... **

INTRODUCCIÓN
En este repo voy a enseñar como he customizado la ISO de instalación de VMware vShpere 6.7 para que reconozca la tarjeta de red de mi placa base

HARDWARE
Tengo la siguiente placa base: X79-F1 (INFO: https://es.aliexpress.com/item/1005002767889809.html)
Tarjeta de red integrada: Realtek RTL8111F (INFO: https://vibsdepot.v-front.de/wiki/index.php/Net55-r8168)
Controlador de tarjeta de red: r8168 (INFO: https://vibsdepot.v-front.de/wiki/index.php/Net55-r8168)

DESCARGAS
El depot de VMware vShpere 6.7 lo he descargado de la pagina oficial de VMware en el mismo apartado donde descargas la ISO hay una opción para descargar el "offline-bundle.zip"
El controlador depot del controlador lo de descargado de vibsdepot.v-front.de (URL DESCARGA: http://vibsdepot.v-front.de/depot/bundles/net55-r8168-8.045a-napi-offline_bundle.zip)

PASOS A SEGUIR:
Copiar el depot de VMware y el depot del controlador a un directorio llamado CustomISO

Abrir un PS como administrador y ejecutar las siguientes ordenes:
cd C:\Users\lluis\Downloads\CustomISO\
Add-EsxSoftwareDepot .\ESXi670-201912001.zip
Add-EsxSoftwareDepot .\net55-r8168-8.045a-napi-offline_bundle.zip
New-EsxImageProfile -CloneProfile "ESXi-6.7.0-20191204001-standard" -Name "ESXi67-IntelNUC" -Vendor "GnanCloudGarage"
Set-EsxImageProfile -ImageProfile "ESXi67-IntelNUC" -AcceptanceLevel CommunitySupported
Add-EsxSoftwarePackage -ImageProfile "ESXi67-IntelNUC" -SoftwarePackage "net55-r8168"
Export-EsxImageProfile -ImageProfile "ESXi67-IntelNUC" -ExportToIso -FilePath ESXi67-IntelNUC.iso

PARA DEBUGGEAR:
Obtener el nombre de las imagenes:
Get-EsxImageProfile

Obtener el nombre del package:
Get-EsxSoftwarePackage -PackageUrl net55-r8168-8.045a-napi.x86_64.vib (INFO: Se encuentra dentro del depot.zip del driver, se debe extraer a fuera)
