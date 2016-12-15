# USB sticky bootable windows using Linux

## Firstly this tutorial will be shown in English and below in Portuguese.
##English

CREATE PENDRIVE BOOTABLE WITH WINDOWS 10

This tip is update from an earlier available in: Windows 7 bootable by the pendrive [Tip]

Tip tested on the Freya (Ubuntu) and Fedora 24 Elementary distribution.

To create a bootable Windows 10 pendrive using Linux you need to have:
ISO image of Windows 10 or DVD of it
Ms-sys program
Cfdisk program
Pendrive with at least 8GB

If your distribution has ms-sys in the repositories, great, just install. But ms-sys does not exist in Ubuntu repositories, so installing it on Ubuntu needs to be "manual".

INSTALLING MS-SYS
http://ms-sys.sourceforge.net/#Download
or you can get this file in this repository

Download the latest stable version. It should be a package in ms-sys-VERSION-tar.gz format.

Unzip it with:

 Tar -xzvf ms-sys * .tgz

Compile:

 Cd ms-sys
$ Make

Become root and install:

 Your
# Make install

Note: instead of "su", in Ubuntu by default it would be "sudo su" because it does not create a root password during installation.

PENDRIVE PREPARATION

Connect the pendrive. To know where it is use the command as root:

#fdisk -l

Let's assume that it stayed in "/ dev / sdb". Then run as root:

# Cfdisk / dev / sdb

Using cfdisk, delete all the partitions on the pendrive and create a single partition (marked as bootable and type 7).

Note: If you do not know how to use cfdisk, follow this tip: use the left-right arrows to navigate the options that appear at the bottom, use the up and down arrows to navigate between partitions and Enter to select.

So you have created a partition called "/ dev / sdb1". After that exit cfdisk.

Format the partition created as NTFS. To do this use the command, still as root:

# Mkfs.ntfs -f / dev / sdb1

USING MS-SYS

Burn the Windows MBR to the newly formatted pendrive. To do this use the command:

# Ms-sys -7 / dev / sdb

Note: in the above command use "/ dev / sdb" instead of "/ dev / sdb1". That is, use without the number. The use of "-7" is for Windows 7, 8 and 10. For other options use the "ms-sys --help" command.

CREATING THE BOOTABLE PENDRIVE

Mount the ISO image (or DVD) of Windows 7 to a directory of your liking. In this example I will use the directory "/ mnt / iso".

Create two directories for assembly:

# Mkdir / mnt / iso
# Mkdir / mnt / usb

To mount the ISO image:

# Mount -o loop windows.iso / mnt / iso

Or, in the case of being a DVD:

# Mount / dev / sr0 / mnt / iso

And also mount the partition that is on the pendrive in another directory. In this example I will use "/ mnt / usb":

# Mount / dev / sdb1 / mnt / usb

Copy all files from the DVD, or ISO image, from Windows to the USB drive:

# Cp -r / mnt / iso / * / mnt / usb /

Wait. It may take a long time, because there are many files.

When finished, you can boot the pendrive that will start Windows 10.

Optionally you can save the files as an image to use on other USB drives without having to perform the whole procedure again. Just use as root:

# Dd if = / dev / sdb of = / home / windows.img

This will create a bootable system image inside / home.

To restore this image to another pendrive it would be enough to execute the opposite of the previous command, that would be:

# Dd if = / home / windows.img of = / dev / sdb

It is!

##Pt-Br
CRIAR PENDRIVE BOOTÁVEL COM WINDOWS 10

Esta dica é atualização de uma anterior disponível em: Windows 7 bootável pelo pendrive [Dica] 

Dica testada na distribuição Elementary OS Freya (Ubuntu) e Fedora 24. 

Para criar um pendrive bootável de Windows 10 usando o Linux é necessário ter:
Imagem ISO do Windows 10 ou o DVD dele
Programa ms-sys
Programa cfdisk
Pendrive com pelo menos 8GB

Se sua distribuição tem o ms-sys nos repositórios, ótimo, basta instalar. Mas o ms-sys não existe nos repositórios do Ubuntu, por isso a instalação dele no Ubuntu precisa ser "manual". 

INSTALAÇÃO DO MS-SYS

Acesse:
http://ms-sys.sourceforge.net/#Download
ou você pode obter este arquivo neste repositório.

Baixe a última versão estável. Deve ser um pacote no formato ms-sys-VERSÃO-tar.gz. 

Descompacte-o com: 

// tar -xzvf ms-sys*.tgz 

Compile: 

// cd ms-sys
// $ make 

Torne-se root e instale: 

 su
 // # make install 

Obs.: ao invés de "su", no Ubuntu por padrão seria "sudo su", pois o mesmo não cria senha de root durante a instalação. 

PREPARAÇÃO DO PENDRIVE

Conecte o pendrive. Para saber onde ele está use o comando como root: 

// # fdisk -l 

Vamos supor que ele ficou em "/dev/sdb". Então execute, como root: 

// # cfdisk /dev/sdb 

Usando o cfdisk apague todas as partições do pendrive e crie uma única partição (marcada como bootável e do tipo 7). 

Obs.: se não sabe como usar o cfdisk, siga esta dica: use as setas esquerda direita para navegar nas opções que aparecem na parte inferior, use as setas cima e baixo para navegar entre as partições e Enter para selecionar. 

Assim você terá criado uma partição chamada "/dev/sdb1". Depois disso saia do cfdisk. 

Formate a partição criada como NTFS. Para isso use o comando, ainda como root: 

// # mkfs.ntfs -f /dev/sdb1 

USANDO O MS-SYS

Grave o MBR do Windows no pendrive recém formatado. Para isso use o comando: 

// # ms-sys -7 /dev/sdb 

Obs.: no comando acima use "/dev/sdb" e não "/dev/sdb1". Ou seja, use sem o número. O uso de "-7" serve para Windows 7, 8 e 10. Para outras opções use o comando "ms-sys --help". 

CRIANDO O PENDRIVE BOOTÁVEL

Monte a imagem ISO (ou o DVD) do Windows 7 em um diretório do seu agrado. Neste exemplo usarei o diretório "/mnt/iso". 

Crie dois diretórios para montagem: 

// # mkdir /mnt/iso
// # mkdir /mnt/usb 

Para montar a imagem ISO: 

// # mount -o loop windows.iso /mnt/iso 

Ou, no caso de ser um DVD: 

// # mount /dev/sr0 /mnt/iso 

E monte também a partição que está no pendrive em outro diretório. Neste exemplo usarei "/mnt/usb": 

// # mount /dev/sdb1 /mnt/usb 

Copie todos os arquivos do DVD, ou da imagem ISO, do Windows para a partição do pendrive: 

// # cp -r /mnt/iso/* /mnt/usb/ 

Aguarde. Pode demorar bastante, pois são muitos arquivos. 

Quando terminar, você pode dar boot pelo pendrive que irá iniciar o Windows 10. 

Opcionalmente você poderá salvar os arquivos como imagem para usar em outros pendrives sem precisar executar todo o procedimento de novo. Basta usar como root: 

// # dd if=/dev/sdb of=/home/windows.img 

Assim será criado uma imagem do sistema, bootável, dentro de /home. 

Para restaurar essa imagem em outro pendrive bastaria executar o contrário do comando anterior, que seria: 

// # dd if=/home/windows.img of=/dev/sdb 

É isso! 
