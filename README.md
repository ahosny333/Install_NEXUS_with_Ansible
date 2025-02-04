# Nexus Repository Manager Installation with Ansible

This Ansible playbook automates the installation and configuration of **Nexus Repository Manager** on a Linux machine. It handles the entire setup process, including Java installation, Nexus installation, user creation, directory setup, and service configuration.

---

## **Table of Contents**
1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Playbook Tasks](#playbook-tasks)
4. [Usage](#usage)
5. [Configuration](#configuration)
6. [Troubleshooting](#troubleshooting)
7. [Contributing](#contributing)
8. [License](#license)

---

## **Overview**
This playbook installs and configures **Nexus Repository Manager 3.x** on a Linux machine. It performs the following tasks:
- Installs **OpenJDK 8** (required for Nexus).
- Creates a dedicated `nexus` user and group.
- Downloads and extracts the Nexus Repository Manager.
- Configures Nexus to run as a `systemd` service.
- Sets up the necessary directory structure and permissions.
- Starts and enables the Nexus service.

---

## **Prerequisites**
Before running the playbook, ensure the following:
1. **Ansible** is installed on your control machine.
2. The target machine is running a **Linux distribution** (e.g., Ubuntu, CentOS).
3. The target machine has **sudo privileges** for the user running the playbook.
4. **Internet access** is available to download the Nexus tarball and dependencies.

---

## **Playbook Tasks**
The playbook performs the following tasks:
1. **Install Java**:
   - Installs OpenJDK 8, which is required for Nexus.

2. **Create Nexus User**:
   - Creates a dedicated `nexus` user and group to run the Nexus service.

3. **Create Directories**:
   - Creates the installation directory (`/opt/nexus`) and data directory (`/opt/sonatype-work`).

4. **Download and Extract Nexus**:
   - Downloads the Nexus tarball and extracts it to the installation directory.

5. **Configure Nexus**:
   - Updates the `nexus-default.properties` file to use the correct data directory.
   - Configures the `nexus.rc` file to run Nexus as the `nexus` user.
   - Sets the `INSTALL4J_JAVA_HOME` environment variable to point to the Java 8 installation.

6. **Set Permissions**:
   - Sets the correct ownership and permissions for the Nexus directories.

7. **Create Systemd Service**:
   - Creates a `systemd` service file for Nexus and ensures it starts on boot.

8. **Start Nexus**:
   - Starts the Nexus service and waits for it to initialize.

---

## **Usage**
### **Step 1: Clone the Repository**
Clone this repository to your local machine:
```bash
git clone <repository-url>
cd <repository-directory>
```

---

### **Step 2: Run the Playbook**
Run the playbook using the following command:
```bash
ansible-playbook -i localhost install_nexus.yml
```

---

### **Step 3: Access Nexus**
After the playbook completes, access Nexus at:

http://localhost:8081

- Default Username: admin

- Default Password: Check the file /opt/sonatype-work/nexus3/admin.password.

---


## **Configuration**

### Variables

The playbook uses the following variables, which can be customized in the `vars` section:

*   `nexus_version`: The version of Nexus to install (default: `3.58.1-02`).
*   `nexus_install_dir`: The installation directory for Nexus (default: `/opt/nexus`).
*   `nexus_data_dir`: The data directory for Nexus (default: `/opt/sonatype-work`).
*   `nexus_user`: The user under which Nexus runs (default: `nexus`).
*   `nexus_group`: The group under which Nexus runs (default: `nexus`).

### Configuration Files

*   `nexus-default.properties`: Configures the Nexus data directory.
*   `nexus.rc`: Specifies the user under which Nexus runs and the Java home directory.
*   `nexus.service`: The systemd service file for Nexus.

## **Troubleshooting**

### Common Issues

*   **Nexus Fails to Start:**
    *   Check the logs for errors:

    ```bash
    journalctl -u nexus.service -f
    ```

    *   Verify that the `nexus` user has the correct permissions:

    ```bash
    sudo chown -R nexus:nexus /opt/nexus /opt/sonatype-work
    ```

*   **Java Version Mismatch:**
    *   Ensure that Java 8 is installed and set as the default:

    ```bash
    java -version
    sudo update-alternatives --config java
    ```

*   **Port Conflict:**
    *   Ensure that port 8081 is not in use by another service.

## **Contributing**

Contributions are welcome! If you find any issues or have suggestions for improvement, please:

1.  Fork the repository.
2.  Create a new branch for your changes.
3.  Submit a pull request.

## **License**

This project is licensed under the MIT License. See the LICENSE file for details.

## Acknowledgments

*   Sonatype Nexus Repository Manager for providing the open-source version of Nexus.
*   Ansible for making automation simple and powerful.
