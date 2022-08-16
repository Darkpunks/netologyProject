__________________________________________________________________________
Домашнее задание к занятию "Облачные провайдеры и синтаксис Terraform"
__________________________________________________________________________

Задача 1 (Вариант с Yandex.Cloud). Регистрация в ЯО и знакомство с основами (необязательно, но крайне желательно).

Сделано.

 <img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/7.2/7.2.1.jpg">


Задача 2. Создание yandex_compute_instance через терраформ.

Ответ: при помощи какого инструмента (из разобранных на прошлом занятии) можно создать свой образ ami?

У меня получается случай с YandexCloud. Да, можно моздавай образы при помощи packer. 


```
[ivan@localhost cloud-terraform]$ terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the
following symbols:
  + create

Terraform will perform the following actions:

  # yandex_compute_instance.vm-1 will be created
  + resource "yandex_compute_instance" "vm-1" {
      + created_at                = (known after apply)
      + folder_id                 = (known after apply)
      + fqdn                      = (known after apply)
      + hostname                  = "host1.netology.yc"
      + id                        = (known after apply)
      + metadata                  = {
          + "ssh-keys" = <<-EOT
                ubuntu:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCwMedyQzjLQg6W3ijiizXcsHtDaK6np1qUc+GrFkH6K2BKMYnlx7Bzl0Nu8XB1XyVQiYjbQHTLSg9owMGU+NR3pc6RXW9YFhv3M0fXYUinxr0YmbQMqNyk1gTaRICn0iKlKhFjE1NSoi2nbBZmNNXg6yc2r6jQQ+YmmuEg6nsyQn0fib+Gt+wLo4JYeA9PJYNMiTJiYRO6DzG9Nzx4lQpdRdMPHDsyE5ZCbPIFp2jmLFiGeU6BjdmwthSSXjgyqAfskBbaqhCfnRZrZZlwa391DCLOoLD4aEo/oBatLSn62N1C89JgINQ9gwMOVKR//SY1xwv/OUxJ8j47CCJPTeQT ivan@localhost.localdomain
            EOT
        }
      + name                      = "piv-terraform"
      + network_acceleration_type = "standard"
      + platform_id               = "standard-v1"
      + service_account_id        = (known after apply)
      + status                    = (known after apply)
      + zone                      = (known after apply)

      + boot_disk {
          + auto_delete = true
          + device_name = (known after apply)
          + disk_id     = (known after apply)
          + mode        = (known after apply)

          + initialize_params {
              + block_size  = (known after apply)
              + description = (known after apply)
              + image_id    = "fd8fmkvnu3csfh8lbpau"
              + name        = (known after apply)
              + size        = (known after apply)
              + snapshot_id = (known after apply)
              + type        = "network-hdd"
            }
        }
```

 <img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/7.2/7.2.jpg">

Ссылку на репозиторий с исходной конфигурацией терраформа.

https://github.com/Darkpunks/netologyProject/blob/main/second%20part/7.2/main.tf
