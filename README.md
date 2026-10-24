# MinIO Core

*An educational project showcasing how to use MinIO with Python, covering both synchronous and asynchronous approaches.*

> [!WARNING]  
> **Project Archived**
> 
> This project is no longer actively maintained. 
> 
> **Context:**  
> Originally, this repository began as a modular component of a large-scale university assignment. Rather than simply completing the task and discarding the code, I chose to expand it into a structured mini-project to deeply explore how to work with MinIO in practice. Over time, however, I decided to consolidate all the modular knowledge I gained in web technologies into a single, unified educational project. Maintaining "perfect educational code" for MinIO across multiple separate repositories became inefficient and fragmented.
> 
> **Reason for Archiving:**  
> Consequently, I made the decision to archive this standalone project to consolidate and maintain the highest quality, up-to-date code in a single location. 
> 
> This repository remains available purely as an educational reference for those interested in a foundational understanding of how to work with MinIO in practice. For the most up-to-date and comprehensive implementation, please refer to my main unified project:
> 
> - [Pose Estimation Service](https://github.com/snegoviol/pose-estimation-service)

## Project Structure

```
minio-core/
├── app/                   # Main application code
├── docker-composes/       # Docker Compose configurations for MinIO servers
├── .env.example           # Example environment variables file
├── justfile               # Project-specific commands using Just
├── pixi.lock              # Locked dependency versions for reproducible environments
├── pixi.toml              # Pixi project configuration: environments, dependencies, and platforms
└── playground-testing/    # Jupyter notebooks for playground testing
```

Each directory includes its own `README.md` with detailed information about its contents and usage, and every file contains comprehensive inline comments to explain the code.

## Dependencies Overview

| Dependency | Repository | What it is |
| :--- | :--- | :--- |
| **CPython** | [python/cpython](https://github.com/python/cpython) | The de facto standard implementation of the Python programming language. |
| **pydantic-settings** | [pydantic/pydantic-settings](https://github.com/pydantic/pydantic-settings) | Pydantic-powered library for managing application configuration and environment variables with strong typing, validation, and seamless `.env` support. |
| **boto3** | [boto/boto3](https://github.com/boto/boto3) | A synchronous Python client for Amazon S3 and other AWS services; compatible with S3-compatible storage systems, including MinIO. [^1] |
| **aioboto3** | [terricain/aioboto3](https://github.com/terricain/aioboto3) | An asynchronous Python client for Amazon S3 and other AWS services; compatible with S3-compatible storage systems, including MinIO. [^1] |
| **just** | [casey/just](https://github.com/casey/just) | Lightweight, cross-platform command runner that replaces complex shell scripts with clean, readable recipes, and natively loads environment variables directly from `.env` files out of the box (unlike `pixi tasks`). |
| **JupyterLab** *(Dev)* | [jupyterlab/jupyterlab](https://github.com/jupyterlab/jupyterlab) | Next-generation web-based interactive development environment for Jupyter notebooks; used here to create interactive documents for testing and verifying code execution. |
| **requests** *(Dev)* | [psf/requests](https://github.com/psf/requests) | A simple, yet powerful HTTP client for Python that makes it easy to send HTTP/1.1 requests; used here to test S3 presigned URL functionality directly from Python code without relying on external tools like `curl`. |

[^1]: I chose `boto3` over `minio-py` because the latter is synchronous-only, and MinIO doesn’t provide an official async client. On the other hand, `aioboto3`—built on top of `boto3` and `aiobotocore`—offers an async client with a nearly identical API. Since this is an educational project, I wanted to keep the codebase consistent and avoid using two entirely different libraries for sync and async operations.

## Quick Start

### I. Prerequisites

| Component | Requirement |
| :--- | :--- |
| **Operating System** | GNU/Linux-based system |
| **Architecture** | `x86_64` |
| **Package Manager** | [`Pixi`](https://pixi.prefix.dev/latest/) |
| **Container Tools** | [Docker and Docker Compose](https://docs.docker.com/engine/install/) |

> **Note:**  
> These are not strict requirements, but rather the baseline environment where the code is 
> guaranteed to run out of the box. Alternative setups are possible but may require manual 
> configuration.

### II. Environment Setup

1. **Clone the repository**

    ```bash
    git clone https://github.com/snegoviol/minio-core.git
    cd minio-core
    ```

2. **Install dependencies**

    ```bash
    pixi install --all
    ```

3. **Activate Pixi environment**

    ```bash
    pixi shell
    ```

4. **Setup environment configuration**

    ```bash
    just copy-env
    ```

5. **Start MinIO**

    ```bash
    just minio-up 1
    ```

6. **Initialize bucket**

    ```bash
    just init-bucket
    ```

### III. Testing

Once the storage is ready, you can run and test the MinIO implementation with interactive Jupyter notebooks in `playground-testing/`:

> **Note:**  
> Ensure the `playground-testing/assets/` directory contains test files with MIME types matching those specified in the `.env` file before running the notebooks.

1. **Launch JupyterLab**

    ```bash
    pixi run -e test jupyter lab
    ```

2. **Test the MinIO implementation**
    - JupyterLab should open automatically in your browser at the default address.
    - In JupyterLab, navigate to the `playground-testing/` folder.
    - Open and execute the notebooks interactively.

3. **(Optional) Verify storage manually**  
    You can open a MinIO client shell to manually verify that everything is working correctly:

    ```bash
    just mc-shell 1
    ```

### IV. Cleanup

When you finish testing:

1. **Stop JupyterLab**  
    In the terminal where JupyterLab is running, press `Ctrl+C` to shut it down.

2. **Stop MinIO**

    ```bash
    just minio-down 1
    ```

## License

Each file in this repository is licensed under the [MIT No Attribution License](LICENSE.txt).
