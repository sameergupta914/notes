# What is Docker

- Docker is an open-source platform that enables developers to automate the deployment, scaling, and management of applications inside lightweight, portable containers. These containers bundle an application with all its dependencies (libraries, configuration files, etc.), ensuring that it runs consistently across different environments.

- What is a Docker Image?
    - A Docker image is a lightweight, standalone, and executable package that contains everything needed to run an application, including:

        - The application code
        - System libraries
        - Dependencies
        - Configuration files

- What is a Docker Container?
    - A Docker container is a lightweight, isolated environment that runs an application along with all its dependencies. It is created from a Docker image and provides a consistent runtime across different systems.

- Containers (Docker)-> Containers share the host OS kernel and isolate applications at the process level. They are lightweight, as they do not require a full operating system. Docker manages containers using Docker Engine, making it easy to deploy, scale, and destroy them quickly.

- Virtual Machines (VMs)-> Each VM includes a full OS, libraries, and dependencies, making it much heavier than a container. VMs provide better isolation but require more resources and take longer to start. Example:
    - Running a Linux VM inside VirtualBox requires installing the OS, setting up system resources, and booting it up.

- 