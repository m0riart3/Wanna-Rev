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
por lo que podemos medio saber por donde empezar, tendríamos que cambiar la extensión de nuestro archivo, quitarle el .exe y arreglar a nivel hexadecimal el archivo para poder pasar al análisis dinámico, para ello abrimos el binario con el comando bless. Después de cambiar la extension y de cambiar cada EVX! que hubiera en el hexadecimal por UPX! desempaquetamos el binario con UPX para poder ver por fin el código utilizando el siguiente comando
```(m0riart3㉿kali)-[~/Desktop/upx-3.96-arm64_linux]
└─$ upx -d ../chall
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX 3.96        Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 23rd 2020

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
    933968 <-    362604   38.82%   linux/amd64   chall

Unpacked 1 file.
```
## Análisis Dinámico
Ya teniendolo desempaquetado, lo primero sería comprobar que tipo de protecciones tiene el binario, para ello usamos el comando checksec
```(m0riart3㉿kali)-[~/Desktop]
└─$ checksec --file chall
[*] '/home/m0riart3/Desktop/chall'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    Canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
```
viendo el output del comando, podemos ver que tiene alguna proteccion en la pila, el siguiente paso sería ejecutarlo, pero al hacerlo no ocurre nada, así que el siguiente paso sería utilizar el debugger,pero al hacerlo obtenemos lo siguiente
![gdb](https://github.com/m0riart3/Wanna-Rev/blob/main/gdb.PNG)

con este output podemos suponer que también tiene antidebugger, por último queda utilizar ghidra y ver el código fuente tal cual
