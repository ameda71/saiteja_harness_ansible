provider "google" {
  project = "sam-458313"        # ← your GCP project ID
  region  = "us-west1"          # optional, for regional resources
  zone    = "us-west1-a"        # for zonal resources like compute instances
}



resource "tls_private_key" "my_ssh_key" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

resource "google_compute_instance" "instance" {
  name         = "instance-1"
  machine_type = "e2-standard-2"
  zone = "us-west1-a"

  boot_disk {
    initialize_params {
      image = "centos-stream-9"
    }
  }

  network_interface {
    network = "default"
    access_config {}
  }

  metadata = {
    ssh-keys = "ansible:${tls_private_key.my_ssh_key.public_key_openssh}"
  }
}

output "private_key" {
  value     = tls_private_key.my_ssh_key.private_key_pem
  sensitive = true
}
output "vm_external_ip" {
   
  value = google_compute_instance.instance.network_interface[0].access_config[0].nat_ip
}
