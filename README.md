# Protocol for Influenza genome and SARS-CoV-2 spike-only assembly and curation
### Start with demultiplexed sequencing reads from an Illumina or Oxford Nanopore Technologies sequencer and finish with high quality genomes ready for submission to public repositories!
<br/><br/>


<html lang="en">
   <head>
	 <script src="https://cdnjs.cloudflare.com/ajax/libs/mermaid/8.0.0/mermaid.min.js"></script>
    </head>
	 
<body>
 <pre><code class="language-mermaid">flowchart LR

title[<font size=6>Process Flowchart Diagram]
title --> B
linkStyle 0 stroke:#FFF,stroke-width:0;

subgraph SPY[<font size=6>IRMA-SPY]
S{<font size=5>Samplesheet\nGeneration} --> B
B --> C[<font size=5>DAIS-Ribosome\nannotation]
C --> D{<font size=5>Quality\nAssessment}
end
D -.-> E[<font size=5>SeqSender]
D --> GU{<font size=5>GISAID\nGUI}
GU --> F
D --> NU{<font size=5>NCBI\nGUI}
NU --> G
E --> F[(<font size=5>GISAID)]
E --> G[(<font size=5>NCBI)]
A[<font size=5>Demulitplexed\nReads] --> B[<font size=5>IRMA\nassembly]


style A fill:#153b60,color:#FFFFFF,stroke:#333,stroke-width:5px
style B fill:#89b342,color:#000000,stroke:#333,stroke-width:5px
style B fill:#89b342,color:#000000,stroke:#333,stroke-width:5px
style C fill:#89b342,color:#000000,stroke:#333,stroke-width:5px
style S fill:#06a4d5,color:#000000,stroke-width:10px
style D fill:#06a4d5,color:#000000,stroke-width:10px
style E fill:#89b342,color:#000000,stroke:#333,stroke-width:5px
style F fill:#e3e3e2,color:#000000,stroke:#333,stroke-width:5px
style G fill:#e3e3e2,color:#000000,stroke:#333,stroke-width:5px
style NU fill:#06a4d5,color:#000000,stroke-width:10px
style GU fill:#06a4d5,color:#000000,stroke-width:10px
style SPY fill:#608c5a,color:#FFFFFF,stroke:#353,stroke-width:5px
</code></pre>

<div class="mermaid">flowchart LR
subgraph KEY[<font size=5>Key]
I[<font size=4>Input Data]
U{<font size=4>User\nInteraction}
SG[<font size=4>Software\nGUI]
SC[<font size=4>Software\nCLI]
P[(<font size=4>Public\nRepository)]
end
style I fill:#153b60,color:#FFFFFF,stroke:#333,stroke-width:5px
style SC fill:#89b342,color:#000000,stroke:#333,stroke-width:5px
style SG fill:#608c5a,color:#FFFFFF,stroke:#353,stroke-width:5px
style U fill:#06a4d5,color:#000000,stroke-width:10px
style P fill:#e3e3e2,color:#000000,stroke:#333,stroke-width:5px

</div>

</body>
<script>
var config = {
    startOnLoad:true,
    theme: 'forest',
    flowchart:{
            useMaxWidth:false,
            htmlLabels:true
        }
};
mermaid.initialize(config);
window.mermaid.init(undefined, document.querySelectorAll('.language-mermaid'));
</script>

</html>
#

## Computer requirements
- A minimum of 16GB of memory is required. >=32GB is recommended.
- A minimum of 8 CPU cores is recommended.
- **Administrative privileges are required on a Windows operating system _to run linux_.**
- A linux/unix (includes Intel-based* MacOS) operating system is required.
    - *This software has not yet been tested on Apple's M-chip based OS
#

## How to install linux on a Windows 10/11 computer
You can get a full linux environment using Windows Subsystem for Linux, or WSL. The second version of WSL is WSL2 and is the recommended version to use.

- [ ] Check your Windows version and build number, select Windows logo key + R, type winver, select OK.
  - Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11 is required.
  - You can update to the latest Windows version by selecting Start > Settings > Windows Update > Check for updates.
- [ ] [Run powershell **as administrator**](./images/powershell_open.png)
- [ ] Run the following command in Powershell:
    ```bash
    wsl --install
    ```
- [ ] Restart your computer
- [ ] Reopen Powershell and enter the following commands:
    ```bash
    wsl --set-default-version 2
    wsl --install -d Ubuntu-18.04
    ```
    Following successful installation, an Ubuntu terminal should pop up that looks like:
    ![alt text](./images/ubuntu_setub_1.png)
- [ ] Enter a username that will be exclusive for WSL. Press `Enter` and then enter a password. A "prompt" will then appear in the screen like:
    ![alt text](./images/commandprompt_wsl.png) with `nbx0` replaced by your entered username and `L349232` replaced with your computer's name.
<br/><br/>
<br/><br/>
Further details can be found on Microsoft's website here: [https://docs.microsoft.com/en-us/windows/wsl/install](https://docs.microsoft.com/en-us/windows/wsl/install)
#
## `Map network drive` to be able to use Window's `File Explorer` to see folders and files inside WSL
- [ ] Open [`File Explorer`](./images/file_explorer.png)
- [ ] Right click [`This PC` and click `Map network drive`](./images/map_drive_1.png)
- [ ] Enter `\\wsl$` into `Folder: ` [and click `Browse`](./images/map_drive_2.png)
- [ ] Click on `wsl$` to unfold directories, select `Ubuntu-18.04` [and click `OK`](./images/map_drive_3.png) and then `Finish`. You should now see your WSL "drive" available in `File Explorer`:
    ![alt text](./images/map_drive_4.png)
    
#

## [Install Docker](https://www.docker.com/products/docker-desktop/)
Docker allows you to run software inside an isolated "container image" on your computer with all of that application's needed dependencies. Make sure to install the version for your operating system. Download for [Windows](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe?utm_source=docker&utm_medium=webreferral&utm_campaign=dd-smartbutton&utm_location=header) or [Mac-Intel](https://desktop.docker.com/mac/main/amd64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=dd-smartbutton&utm_location=module) or [Mac-AppleChip](https://desktop.docker.com/mac/main/arm64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=dd-smartbutton&utm_location=module) or [Linux](https://docs.docker.com/desktop/linux/install/).
#

## Install a sequence viewer
[Windows Bio Edit](https://bioedit.software.informer.com/) or [Mac Aliview](https://ormbunkar.se/aliview/#DOWNLOAD)
#

## Instructions for running IRMA-SPY coming soon!

