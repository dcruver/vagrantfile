# Starting The VMs

This vagrantfile can be used to create both Linux and Windows VMs. You can use it to 
start up to 25 boxes of each OS. 

Examples:

    vagrant up linux1
    vagrant up linux25

    vagrant up windows1
    vagrant up windows25


# Provisioning

Each VM will be provisioned with Oracle JDK8, Maven 3.3 and git.

