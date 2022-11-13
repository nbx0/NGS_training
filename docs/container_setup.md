## IRMA-SPY Container Setup
<hr>
IRMA-Spy relies on four Docker **_containers_** to run, each of which must be installed using the `docker pull` command inside Linux or Mac Terminal to download the docker **_images_** from the Quay.io repository (IRMA and DAIS-Ribosome are presently stored in AWS's ECR). These **_images_** are then built into runnable **_containers_** with the `docker run` command.
<br/><br/>
If using a Windows PC, you should have already installed WSL2, Docker Desktop, **and** Docker CLI inside WSL2. If you have not, [please return to those instructions.](../README.md#computer-requirements)
<br/><br/>

1. Open an Ubuntu or Mac Terminal
2. Pull the container for our genome assembler: [IRMA](https://wonder.cdc.gov/amd/flu/irma/)
    {% include codeHeader.html %}
    ```bash
    docker pull public.ecr.aws/n3z8t4o2/irma:1.0.2p3
    ```
3. Pull the container for our genome annotator: DAIS-Ribosome
    {% include codeHeader.html %}
    ```bash
    docker pull public.ecr.aws/n3z8t4o2/dais-ribosome:1.2.1
    ```
4. Pull the container for IRMA-SPY's backend [Snakemake workflow manager](https://snakemake.readthedocs.io/en/stable/)
    {% include codeHeader.html %}
    ```bash
    docker pull quay.io/nbx0_cdc/sc2-spike-seq:v1.0.0
    ```
5. Pull the container for IRMA-SPY
    {% include codeHeader.html %}
    ```bash
    docker pull quay.io/nbx0_cdc/irma-spy:v1.0.1
    ```
6. Create a folder inside Ubuntu that will store your sequencing runs' data.
    {% include codeHeader.html %}
    ```bash
    mkdir ~/FLU_SC2_SEQUENCING
    ```
    - **_Optional:_** Navigate to `File Explorer` and find this folder inside the Ubuntu mount on the left sidebar. Click on this folder to open its contents and then open `home` and then open the folder named for your WSL _username_. Right click FLU_SC2_SEQUENCING and `Create Shortcut`. Move the shortcut folder to an memorable location such as your Desktop.
7. Build the IRMA container
    {% include codeHeader.html %}
    ```bash
    docker run -v $(readlink -f ~/FLU_SC2_SEQUENCING):/data --name irma-1.0.2p3 -t -d public.ecr.aws/n3z8t4o2/irma:1.0.2p3
    ```
8. Build the DAIS-Ribosome container
    {% include codeHeader.html %}
    ```bash
    docker run -v $(readlink -f ~/FLU_SC2_SEQUENCING):/data --name dais-ribosome-1.2.1 -t -d public.ecr.aws/n3z8t4o2/dais-ribosome:1.2.1
    ```
9. Build the Snakemake container
    {% include codeHeader.html %}
    ```bash
    docker run -v $(readlink -f ~/FLU_SC2_SEQUENCING):/data -v /var/run/docker.sock:/var/run/docker.sock --name sc2-spike-seq -t -d quay.io/nbx0_cdc/sc2-spike-seq:v1.0.0
    ```
10. Build the IRMA-SPY container
    {% include codeHeader.html %}
    ```bash
    docker run -v $(readlink -f ~/FLU_SC2_SEQUENCING):/data -v /var/run/docker.sock:/var/run/docker.sock -d -p 8050:8050 --name irma-spy quay.io/nbx0_cdc/irma-spy:v1.0.1
    ```

You are now ready to run IRMA-SPY! You can open it from `Docker Desktop` by clicking on the `Containers` tab on the left sidebar and clicking the icon of the box with the arrow pointing to the top left. This will open IRMA-SPY into your default internet browser.
![alt text](../images/dockerdesktop_launch_irmaspy.png)