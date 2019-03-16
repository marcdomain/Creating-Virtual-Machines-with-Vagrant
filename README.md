# Vagrantfiles

This handles creating a server and creating multi virtual machines using Vagrant for managing VMs and VirtualBox as the provider tool that allows your local machine to accommodate other operating systems.

## vagrant-apache-server

- Create a project directory and run `vagrant init` inside it to create a Vagrantfile
- Edit the Vagrant file to contain the scripts suitable for the project purpose. The scripts contained in the Vagrantfile executes the following:
  - Install an Ubuntu/trusty64 operating system on the VirtualBox provider.
  - Set GUI display to false as the progress can be read directly from the terminal.
  - Allocate a memory of 2GB and 3 CPUS to the server.
  - Set a static IP address on a private network.
  - Sync the Apache web root with the present working directory of the image.
  - Install Apache web server on the virtual machine using the provision script.
- Run `vagrant up api-vm` to initialise the image and the server.
- Use vim to edit the Apache index.html file by running `sudo vim /var/www/html` on the terminal to access the file.

## vagrant-multiVM

### Backend Virtual Machine

- Create a config script for the backend VM, named `api-vm`. Use the same name as the hostname.
- Create a private network static IP address `10.0.0.10` so that you can easily grab it.
- Expose the VM on port 80 to port 5030 on your local machine - my app runs on port 5030.
- Create a vagrant synced folder to sync the file on a local machine to the VM and add read-write permissions security purpose.
- Use VirtualBox as the provider and set the memory, number of CPU.
- Create a provisioning script that would help install the project dependencies.
- Create environment variables using bash scripts and source the variables using `source .env` inside the vagrant provision script. Remember to export each environment variable when creating them.
- Test the application with postman to be sure that the api-vm is properly configured.

### Frontend Virtual Machine

- Create a config script for the backend VM, named `client-vm`. Use the same name as the hostname so as to differentiate it from the backend VM.
- Create a private network static IP address `10.0.0.20` so that you can easily grab it.
- Expose the VM on port 80 to port 5020 on your local machine - my fronten app runs on port 5020
- Create a vagrant synced folder to sync the file on a local machine to the VM and add read-write permissions to beef up security.
- Use VirtualBox as the provider and set the memory, number of CPU.
- Create a provisioning script that would help install the project dependencies, build webpack and start the application.

### Networking Frontend and Backend Virtual Machines

- Run `vagrant up` to get the Virtual Machines running. This also installs the avahi-daemon into each VM, thereby, making each machine accessible on the network.
- Test the networking by pinging each Machine from the other one. That is pinging `ping api-vm.local` inside the client-vm ssh or pinging `ping client-vm.local` inside the api-vm ssh. If the ping is successful, it indicates that the Multi VMs networking was created successfully.
- Now ssh into the client-vm and cd into the file containing your baseURL and change the URL from Heroku to the IP address of the api-vm.
- Test the implementation on your browser using the IP address and port of the api-vm on your local machine.
