## Instructions to remove old software versions

To upgrade iSpy, spyne, IRMA or DAIS-Ribosome, you first need to remove your current version from your machine.

Usually you will only need to update some of the 4 required containers.

Follow the removal instructions based on your version, either the original [IRMA-SPY](#irma-spy-uninstall) or [iSpy](#ispy-uninstall).

### iSpy uninstall

1. Open an Ubuntu terminal
2. ```bash 
    # Show running containers
    docker ps
    
    # Stop running container you want to update. In this example we are updating ispy and spyne.
    docker stop ispy spyne

    # Delete containers
    docker rm ispy spyne

    # Delete images
    docker rmi ispy spyne
    ```
3. Install the new iSpy and spyne containers: [https://nbx0.github.io/NGS_training/#container-setup](https://nbx0.github.io/NGS_training/#container-setup)

### IRMA-SPY uninstall
1. Open an Ubuntu terminal
2. ```bash 
    # Show running containers
    docker ps
    
    # Stop running container you want to update. In this example we are updating ispy and spyne.
    docker stop irma-spy sc2-spike-seq

    # Delete containers
    docker rm irma-spy sc2-spike-seq

    # Delete images
    docker rmi irma-spy sc2-spike-seq
    ```
3. Install the new iSpy and spyne containers: [https://nbx0.github.io/NGS_training/#container-setup](https://nbx0.github.io/NGS_training/#container-setup)