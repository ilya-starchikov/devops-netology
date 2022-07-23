# Домашнее задание к занятию "7.3. Основы и принцип работы Терраформ"

```commandline
$ terraform workspace list
  default
* prod
  stage
```

```commandline
$ terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # yandex_compute_instance.count[0] will be created
  + resource "yandex_compute_instance" "count" {
      + created_at                = (known after apply)
      + folder_id                 = (known after apply)
      + fqdn                      = (known after apply)
      + hostname                  = (known after apply)
      + id                        = (known after apply)
      + metadata                  = {
          + "ssh-keys" = <<-EOT
                ubuntu:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDbGYTp7olCC6qvKiCDdJO0g5BFlCIXetmUC1OI0OZsaJuW0Cg4Tfk6iAYumxQdimXrRuYR6f/wY1/1NDDn73touludf9/1RfvKPwFlSs5URR2tMbFd0qGXaP93TtPKHsiIE0KxBzNd22/QIPIcEcJduQEGoiwzYtvKaSLPtmdfdwBlIu8I/sikwrzoZt6IguOmDiEQDZDTciz+952dezXBRO1tFHpB05gE1L6qgFBCBoJD8petcFK177gTu5wrqhbKGLZZcnQdWqBe58+vVjDm2IcBZCt43CsrIPOBgrXBBFEUuU4fTU5NfG0klnNq/o8BCSohUrfvSFKCZa2qCeqRgPf1W7+JLwCyraGFalxF22dDtGVr5uwgOdOEmwn2xSw9iygofeij5cmO+n2Sn7Zt5j8tQRZEl/TJoEe2ML49+15a7gQTOPGk7Q3SdyloC0ajcW56X/GFusmjq6W5XKc5HmSJMuEd6pZ23MOO70mF0IOtSDc08I3NTSxaxnxZbg8= ilya@MacBook-Pro-ilya.local
            EOT
        }
      + name                      = "prod-count-0"
      + network_acceleration_type = "standard"
      + platform_id               = "standard-v1"
      + service_account_id        = (known after apply)
      + status                    = (known after apply)
      + zone                      = "ru-central1-a"

      + boot_disk {
          + auto_delete = true
          + device_name = (known after apply)
          + disk_id     = (known after apply)
          + mode        = (known after apply)

          + initialize_params {
              + block_size  = (known after apply)
              + description = (known after apply)
              + image_id    = "fd8vdidmktqos56nb991"
              + name        = (known after apply)
              + size        = (known after apply)
              + snapshot_id = (known after apply)
              + type        = "network-hdd"
            }
        }

      + network_interface {
          + index              = (known after apply)
          + ip_address         = (known after apply)
          + ipv4               = true
          + ipv6               = (known after apply)
          + ipv6_address       = (known after apply)
          + mac_address        = (known after apply)
          + nat                = true
          + nat_ip_address     = (known after apply)
          + nat_ip_version     = (known after apply)
          + security_group_ids = (known after apply)
          + subnet_id          = (known after apply)
        }

      + placement_policy {
          + host_affinity_rules = (known after apply)
          + placement_group_id  = (known after apply)
        }

      + resources {
          + core_fraction = 100
          + cores         = 2
          + memory        = 4
        }

      + scheduling_policy {
          + preemptible = (known after apply)
        }
    }

  # yandex_compute_instance.count[1] will be created
  + resource "yandex_compute_instance" "count" {
      + created_at                = (known after apply)
      + folder_id                 = (known after apply)
      + fqdn                      = (known after apply)
      + hostname                  = (known after apply)
      + id                        = (known after apply)
      + metadata                  = {
          + "ssh-keys" = <<-EOT
                ubuntu:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDbGYTp7olCC6qvKiCDdJO0g5BFlCIXetmUC1OI0OZsaJuW0Cg4Tfk6iAYumxQdimXrRuYR6f/wY1/1NDDn73touludf9/1RfvKPwFlSs5URR2tMbFd0qGXaP93TtPKHsiIE0KxBzNd22/QIPIcEcJduQEGoiwzYtvKaSLPtmdfdwBlIu8I/sikwrzoZt6IguOmDiEQDZDTciz+952dezXBRO1tFHpB05gE1L6qgFBCBoJD8petcFK177gTu5wrqhbKGLZZcnQdWqBe58+vVjDm2IcBZCt43CsrIPOBgrXBBFEUuU4fTU5NfG0klnNq/o8BCSohUrfvSFKCZa2qCeqRgPf1W7+JLwCyraGFalxF22dDtGVr5uwgOdOEmwn2xSw9iygofeij5cmO+n2Sn7Zt5j8tQRZEl/TJoEe2ML49+15a7gQTOPGk7Q3SdyloC0ajcW56X/GFusmjq6W5XKc5HmSJMuEd6pZ23MOO70mF0IOtSDc08I3NTSxaxnxZbg8= ilya@MacBook-Pro-ilya.local
            EOT
        }
      + name                      = "prod-count-1"
      + network_acceleration_type = "standard"
      + platform_id               = "standard-v1"
      + service_account_id        = (known after apply)
      + status                    = (known after apply)
      + zone                      = "ru-central1-a"

      + boot_disk {
          + auto_delete = true
          + device_name = (known after apply)
          + disk_id     = (known after apply)
          + mode        = (known after apply)

          + initialize_params {
              + block_size  = (known after apply)
              + description = (known after apply)
              + image_id    = "fd8vdidmktqos56nb991"
              + name        = (known after apply)
              + size        = (known after apply)
              + snapshot_id = (known after apply)
              + type        = "network-hdd"
            }
        }

      + network_interface {
          + index              = (known after apply)
          + ip_address         = (known after apply)
          + ipv4               = true
          + ipv6               = (known after apply)
          + ipv6_address       = (known after apply)
          + mac_address        = (known after apply)
          + nat                = true
          + nat_ip_address     = (known after apply)
          + nat_ip_version     = (known after apply)
          + security_group_ids = (known after apply)
          + subnet_id          = (known after apply)
        }

      + placement_policy {
          + host_affinity_rules = (known after apply)
          + placement_group_id  = (known after apply)
        }

      + resources {
          + core_fraction = 100
          + cores         = 2
          + memory        = 4
        }

      + scheduling_policy {
          + preemptible = (known after apply)
        }
    }

  # yandex_compute_instance.for_each["2"] will be created
  + resource "yandex_compute_instance" "for_each" {
      + created_at                = (known after apply)
      + folder_id                 = (known after apply)
      + fqdn                      = (known after apply)
      + hostname                  = (known after apply)
      + id                        = (known after apply)
      + metadata                  = {
          + "ssh-keys" = <<-EOT
                ubuntu:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDbGYTp7olCC6qvKiCDdJO0g5BFlCIXetmUC1OI0OZsaJuW0Cg4Tfk6iAYumxQdimXrRuYR6f/wY1/1NDDn73touludf9/1RfvKPwFlSs5URR2tMbFd0qGXaP93TtPKHsiIE0KxBzNd22/QIPIcEcJduQEGoiwzYtvKaSLPtmdfdwBlIu8I/sikwrzoZt6IguOmDiEQDZDTciz+952dezXBRO1tFHpB05gE1L6qgFBCBoJD8petcFK177gTu5wrqhbKGLZZcnQdWqBe58+vVjDm2IcBZCt43CsrIPOBgrXBBFEUuU4fTU5NfG0klnNq/o8BCSohUrfvSFKCZa2qCeqRgPf1W7+JLwCyraGFalxF22dDtGVr5uwgOdOEmwn2xSw9iygofeij5cmO+n2Sn7Zt5j8tQRZEl/TJoEe2ML49+15a7gQTOPGk7Q3SdyloC0ajcW56X/GFusmjq6W5XKc5HmSJMuEd6pZ23MOO70mF0IOtSDc08I3NTSxaxnxZbg8= ilya@MacBook-Pro-ilya.local
            EOT
        }
      + name                      = "prod-foreach-2"
      + network_acceleration_type = "standard"
      + platform_id               = "standard-v1"
      + service_account_id        = (known after apply)
      + status                    = (known after apply)
      + zone                      = "ru-central1-a"

      + boot_disk {
          + auto_delete = true
          + device_name = (known after apply)
          + disk_id     = (known after apply)
          + mode        = (known after apply)

          + initialize_params {
              + block_size  = (known after apply)
              + description = (known after apply)
              + image_id    = "fd89ovh4ticpo40dkbvd"
              + name        = (known after apply)
              + size        = (known after apply)
              + snapshot_id = (known after apply)
              + type        = "network-hdd"
            }
        }

      + network_interface {
          + index              = (known after apply)
          + ip_address         = (known after apply)
          + ipv4               = true
          + ipv6               = (known after apply)
          + ipv6_address       = (known after apply)
          + mac_address        = (known after apply)
          + nat                = true
          + nat_ip_address     = (known after apply)
          + nat_ip_version     = (known after apply)
          + security_group_ids = (known after apply)
          + subnet_id          = (known after apply)
        }

      + placement_policy {
          + host_affinity_rules = (known after apply)
          + placement_group_id  = (known after apply)
        }

      + resources {
          + core_fraction = 100
          + cores         = 2
          + memory        = 4
        }

      + scheduling_policy {
          + preemptible = (known after apply)
        }
    }

  # yandex_compute_instance.for_each["3"] will be created
  + resource "yandex_compute_instance" "for_each" {
      + created_at                = (known after apply)
      + folder_id                 = (known after apply)
      + fqdn                      = (known after apply)
      + hostname                  = (known after apply)
      + id                        = (known after apply)
      + metadata                  = {
          + "ssh-keys" = <<-EOT
                ubuntu:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDbGYTp7olCC6qvKiCDdJO0g5BFlCIXetmUC1OI0OZsaJuW0Cg4Tfk6iAYumxQdimXrRuYR6f/wY1/1NDDn73touludf9/1RfvKPwFlSs5URR2tMbFd0qGXaP93TtPKHsiIE0KxBzNd22/QIPIcEcJduQEGoiwzYtvKaSLPtmdfdwBlIu8I/sikwrzoZt6IguOmDiEQDZDTciz+952dezXBRO1tFHpB05gE1L6qgFBCBoJD8petcFK177gTu5wrqhbKGLZZcnQdWqBe58+vVjDm2IcBZCt43CsrIPOBgrXBBFEUuU4fTU5NfG0klnNq/o8BCSohUrfvSFKCZa2qCeqRgPf1W7+JLwCyraGFalxF22dDtGVr5uwgOdOEmwn2xSw9iygofeij5cmO+n2Sn7Zt5j8tQRZEl/TJoEe2ML49+15a7gQTOPGk7Q3SdyloC0ajcW56X/GFusmjq6W5XKc5HmSJMuEd6pZ23MOO70mF0IOtSDc08I3NTSxaxnxZbg8= ilya@MacBook-Pro-ilya.local
            EOT
        }
      + name                      = "prod-foreach-3"
      + network_acceleration_type = "standard"
      + platform_id               = "standard-v1"
      + service_account_id        = (known after apply)
      + status                    = (known after apply)
      + zone                      = "ru-central1-a"

      + boot_disk {
          + auto_delete = true
          + device_name = (known after apply)
          + disk_id     = (known after apply)
          + mode        = (known after apply)

          + initialize_params {
              + block_size  = (known after apply)
              + description = (known after apply)
              + image_id    = "fd89ovh4ticpo40dkbvd"
              + name        = (known after apply)
              + size        = (known after apply)
              + snapshot_id = (known after apply)
              + type        = "network-hdd"
            }
        }

      + network_interface {
          + index              = (known after apply)
          + ip_address         = (known after apply)
          + ipv4               = true
          + ipv6               = (known after apply)
          + ipv6_address       = (known after apply)
          + mac_address        = (known after apply)
          + nat                = true
          + nat_ip_address     = (known after apply)
          + nat_ip_version     = (known after apply)
          + security_group_ids = (known after apply)
          + subnet_id          = (known after apply)
        }

      + placement_policy {
          + host_affinity_rules = (known after apply)
          + placement_group_id  = (known after apply)
        }

      + resources {
          + core_fraction = 100
          + cores         = 2
          + memory        = 4
        }

      + scheduling_policy {
          + preemptible = (known after apply)
        }
    }

  # yandex_vpc_network.default will be created
  + resource "yandex_vpc_network" "default" {
      + created_at                = (known after apply)
      + default_security_group_id = (known after apply)
      + folder_id                 = (known after apply)
      + id                        = (known after apply)
      + labels                    = (known after apply)
      + name                      = "net"
      + subnet_ids                = (known after apply)
    }

  # yandex_vpc_subnet.default will be created
  + resource "yandex_vpc_subnet" "default" {
      + created_at     = (known after apply)
      + folder_id      = (known after apply)
      + id             = (known after apply)
      + labels         = (known after apply)
      + name           = "subnet"
      + network_id     = (known after apply)
      + v4_cidr_blocks = [
          + "192.168.101.0/24",
        ]
      + v6_cidr_blocks = (known after apply)
      + zone           = "ru-central1-a"
    }

Plan: 6 to add, 0 to change, 0 to destroy.
```