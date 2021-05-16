# Wanna-Rev
## Descripción
![Descripcion](https://github.com/m0riart3/Wanna-Rev/blob/main/Descripcion.PNG)


## Análisis estático del binario

Nos encontrarmos con un ejecutable con extensión .exe, lo primero que deberíamos hacer es utilizar el comando file para ver realmente ante que nos encontramos, el comando nos da el siguiente output:
  ```m0riart3㉿kali)-[~/Desktop]
└─$ file chall.exe 
chall.exe: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), statically linked, no section header
```
por lo tanto nos encontramos ante un binario y no ante un ejecutable .exe. El siguiente paso sería usar el comando strings para ver si podemos ver algo interesante antes de pasar al analisis dinámico. Entre todo el output encontramos lo siguiente
```PROT_EXEC|PROT_WRITE failed.
$Info: This file is packed with the UPX executable packer http://upx.sf.net $
$Id: UPX 3.96 Copyright (C) 1996-2020 the UPX Team. All Rights Reserved. $
```
esto nos indica que se ha usado UPX para empaquetar el código, en otra parte del output del comando podemos ver también lo siguiente
```
EVx!
EVx!
```
por lo que podemos medio saber por donde empezar, tendríamos que cambiar la extensión de nuestro archivo, quitarle el .exe y arreglar a nivel hexadecimal el archivo para poder pasar al análisis dinámico
