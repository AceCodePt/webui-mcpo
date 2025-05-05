# OpenWebUI Setup with Docker Compose

This README provides instructions on how to set up OpenWebUI using the provided `docker-compose.yml` file. OpenWebUI is a web interface for interacting with Large Language Models (LLMs). This setup uses Docker Compose to manage the required services.

## Prerequisites

Before you begin, ensure you have the following installed:

- Docker: [Install Docker](https://docs.docker.com/get-docker/ "null")

- Docker Compose: [Install Docker Compose](https://docs.docker.com/compose/install/ "null") (Usually comes with Docker Desktop)

## Setup Instructions

Follow these steps to set up OpenWebUI:

1.  Clone the Repository (Optional): If you have obtained the `docker-compose.yml` file from a repository, clone it to your local machine. If you have the `docker-compose.yml` file directly, you can skip this step.

    ```
    git clone https://github.com/AceCodePt/webui-mcpo.git
    cd https://github.com/AceCodePt/webui-mcpo.git
    ```

2.  Create Configuration File:

    - Create a file named `config.json` in the same directory as the `docker-compose.yml` file.

    - Add the necessary configuration for `mcpo` to this file. The contents of this file will depend on the specific MCP servers you want to use. Refer to the mcpo documentation for the correct format: [mcpo Documentation](https://github.com/open-webui/mcpo "null")

    - You can use the provided `example.config.json` as a template. You will need to modify it to match your specific MCP server configuration, especially the `<DATABASE_URL>` placeholder.

    - Example `config.json` (This is just an example, you'll need to adapt it):

      ```
      {
        "mcpServers": {
          "example_server": {
            "command": "your_mcp_server_command",
            "args": ["--arg1", "value1", "--arg2", "value2"]
          }
        }
      }

      ```

3.  Create .env File:

    - Create a file named `.env` in the same directory as the `docker-compose.yml` file.

    - Add the `MCPO_API_KEY` to this file. This API key is used for authentication with the `mcpo` service. Choose a strong, secure key.

    - You can use the provided `example.env` file as a template. You should change the value of `MCPO_API_KEY_FROM_ENV` to a secure, unique key.

    - Example `.env`:

      ```
      MCPO_API_KEY=your_secure_api_key_here

      ```

      _(Replace `your_secure_api_key_here` with your actual API key)_

4.  Start the Services:

    - Open a terminal in the directory containing the `docker-compose.yml` file.

    - Run the following command to start the OpenWebUI and mcpo services:

      ```
      docker compose up -d

      ```

      The `-d` flag runs the services in detached mode (in the background).

5.  Access OpenWebUI:

    - Once the services are running, you can access the OpenWebUI web interface by opening your web browser and navigating to: `http://localhost:3000`

## Configuration Notes

- config.json: This file configures the `mcpo` service. Refer to the official mcpo documentation for details on how to configure it for your specific MCP servers. The provided `docker-compose.yml` mounts this file as read-only (`:ro`). If `mcpo` needs to write to this file, remove the `:ro` from the volume mount in the `docker-compose.yml`.

- .env: This file sets the `MCPO_API_KEY` environment variable, which is used by the `mcpo` service for authentication. Ensure you replace `your_secure_api_key_here` with a strong, randomly generated key.

- Ports:

  - OpenWebUI is accessible on port `3000` of your host machine.

  - mcpo is accessible on port `8000` of your host machine.

- Volumes:

  - The `open-webui` volume is used to persist OpenWebUI data.

  - The `./config.json:/config.json` volume mount makes your local `config.json` file available to the `mcpo` container.

- Networking: The `webui-network` allows the `openwebui` and `mcpo` services to communicate with each other.

- Updating: To update OpenWebUI and mcpo, pull the latest images and restart the containers:

  ```
  docker compose pull
  docker compose up -d

  ```

## Troubleshooting

- Container Startup Issues: If you encounter errors during startup, check the container logs for more information. You can view the logs using the following command:

  ```
  docker compose logs <service_name>

  ```

  Replace `<service_name>` with either `openwebui` or `mcpo`.

- Connection Issues: If you cannot access OpenWebUI in your browser, ensure that the containers are running correctly and that there are no port conflicts.

- mcpo Configuration: Ensure your `config.json` is correctly configured for your MCP servers. Incorrect configuration is the most common cause of issues with mcpo.

## MCPO API Key

The `MCPO_API_KEY` is essential for securing your mcpo service. Treat it like a password. Anyone who has this key can access your MCP servers through mcpo.
