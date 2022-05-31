---
layout: single
title: Terraform Cheat Sheet
date: 2022-02-31
classes: wide
header:
  teaser: /assets/images/cloud.png
categories:
  - terraform
tags:
  - iaas
  - infrastructure as a code
  - terraform cheat sheet
---

### Terraform Cheat Sheet
---------------

```terraform
# Formato de Código de Terraform
terraform fmt #formateo
terraform validate #validar
terraform validate -backend=false #validar sin backend


# Inicializar Terraform
terraform init #inicializar
terraform init -get-plugins=false #inicializar sin plugins
terraform init -verify-plugins=false #inicializar sin verificar plugins
terraform init -var-file='vars.tfvars' #inicializar con fichero de variables

# Plan, Deploy y Destroy de infraestructura
terraform apply --auto-approve #aplicar cambios sin requerir confirmacion
terraform destroy --auto-approve #destroy del despliegue sin requerir confirmacion
terraform plan -out plan.out #guardar plan en fichero plan.out
terraform apply plan.out #usar para el plan el fichero plan.out para desplegar la infraestructura
terraform plan -destroy #plan de destroy de un despliegue
terraform apply -target=azurerm_virtual_machine.vmPrueba #apply por target de un recurso determinado
terraform apply -var my_region_variable=west-europe #pasar variable en un apply
terraform apply -lock=true #hacer un lock del tfstate
terraform apply refresh=false # no refrescar el fichero tfstate
terraform apply --parallelism=5 #numbero de operaciones simultaneas a 5
terraform refresh #refrescar el tfstate
terraform providers #obtener informacion de los providers


# Terraform Workspaces
terraform workspace new mynewworkspace #crear workspace
terraform workspace select default #seleccionar workspace
terraform workspace list #listar todos los workspace


# Terraform State
terraform state show azurerm_virtual_machine.vmPrueba #ver los detalles del recurso en el tfstate
terraform state pull > terraform.tfstate #descargar tfstate a un fichero
terraform state mv azurerm_role_assignment.reader module.custom_module #mover recurso a un modulo diferente
terraform state replace-provider hashicorp/azurerm  hashicorp/terraform-provider-azurestack  #reemplazar provider
terraform state list #listar recursos del tfstate
terraform state rm azurerm_virtual_machine.vmPrueba #borrar recurso del tfstate


# Terraform Import-Outputs
terraform import azurerm_virtual_machine.vmPrueba /subscriptions/aada33q4-s22f-6353-s63-1h2v4logz2g3/resourceGroups/RESOURCE_GROUP_PRUEBA/providers/Microsoft.Compute/virtualMachines/vmPrueba #importar recurso por id
terraform import 'azurerm_virtual_machine.vmPrueba[0]' #hace lo mismo que el comando anterior
terraform output #muestra todos los outputs
terraform output azurerm_virtual_machine # muestra todas las maquinas virtuales de los outputs
terraform output -json #muestra todos los outputs en formato json


# Terraform Comados Varios
terraform version #version de terraform
terraform get -update=true #actualiza modulos de terraform


# Consola de Terraform
echo 'join(",",["foo","bar"])' | terraform console #echo de la expresion en la consola de terraform
echo '1 + 5' | terraform console #echo interactivo de la consola de terraform
echo "azurerm_virtual_machine.vmPrueba" | terraform console #muestra en la consola de terraform el recurso


# Terraform Graphics
terraform graph | dot -Tpng > graph.png #genera grafico con las dependencias y relaciones de los recursos


# Terraform Taint/Untaint
terraform taint aws_instance.my_ec2 #marca recursos que sera recreado en el siguiente apply
terraform untaint aws_instance.my_ec2 #quita marcado de recurso
terraform force-unlock LOCK_ID #fuerza el unlock de un recurso por LOCK_ID


# Terraform Cloud
terraform login #Logarse en Terraform cloud
terraform logout #Log out de Terraform Cloud
```