---
layout: single
title: Restaurar Disco del Sistema de VM con Snapshot en Azure
date: 2022-02-30
classes: wide
header:
  teaser: /assets/images/cloud.png
categories:
  - azure
  - backup
  - restaurar vm
tags:
  - azure
  - azure powershell
  - restaurar snapshot vm azure
---
Si tenemos una máquina virtual de la que queremos restaurar una copia de seguridad del disco del sistema operativo, lo podemos hacer a partir de una Snapshot del disco del sistema que hayamos realizado previamente usando Azure PowerShell. 
No es necesario detener o desasignar la máquina virtual. Tenemos que asegurarnos de que el tipo de almacenamiento y el tamaño de la máquina virtual son compatibles con el disco de la snapshot que hemos realizado previamente.

Se realizara de la siguiente manera:

A partir de la Snapshot del disco del sistema que hemos creado previamente, creamos un disco del Sistema.

![image-20220530160032670](/assets/images/restaurar-snapshot-vm-azure/snapshot_01.png)

### Obtenemos el disco que hemos creado a partir de la Snapshot mediante Get-AzDisk pasandole el Resource Group y el Nombre.
---------------

```powershell
Get-AzDisk -ResourceGroupName myResourceGroup | Format-Table -Property Name
```

Cuando tengamos el disco que queremos usar, lo establecemos como el disco del sistema operativo de la VM. Se detiene o desasigna la máquina virtual llamada myVM y se asigna el disco llamado newDisk como el nuevo disco del sistema operativo.

### Sustituimos el disco del sistema por el disco creado a partir de la snapshot con Powershell
```powershell
# Get the VM 
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 

# (Optional) Stop/ deallocate the VM
Stop-AzVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name 

# Update the VM with the new OS disk
Update-AzVM -ResourceGroupName myResourceGroup -VM $vm 

# Start the VM
Start-AzVM -Name $vm.Name -ResourceGroupName myResourceGroup
```
