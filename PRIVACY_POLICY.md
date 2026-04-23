# Dobby Application Privacy Policy

**Last Updated:** April 23, 2026

This Privacy Policy describes how the **Dobby** application (hereinafter referred to as the "Application") collects, uses, and protects information when used on Android devices.

## 1. General Provisions
Dobby is a personal AI agent designed for task automation, script execution, and interaction with language models. We value your privacy and strive for transparency in data processing matters.

## 2. Data Collection and Processing

### 2.1. User-Provided Data
* **API Keys:** To use the Application, users independently enter access keys for third-party services (e.g., OpenRouter API). These keys are stored locally on your device in plain text (using Jetpack DataStore) and are not transmitted to our servers.
* **Chat History:** Messages and command execution results are stored exclusively in a local database (Room SQL) on your device (unless the user specifies otherwise).

### 2.2. Technical Data
* **File System Access:** The Application requests file access to execute commands (read, write, delete) based on direct user instructions or AI agent directives.
* **Code Execution:** The Application uses an embedded runtime environment (Chaquopy/Python) to execute scripts on the device. All processing occurs locally.

## 3. Data Transmission to Third-Party Services
To enable intelligent features, the Application interacts with third-party service providers.

* **Third-Party APIs (OpenRouter):** Your text requests (chat messages) are transmitted to external providers via API for response generation.
    * **Important:** Data is only transmitted to providers that you have selected in the settings.
    * When using **OpenRouter** or similar services, your data is processed in accordance with their privacy policies. We recommend reviewing them before entering an API key.
* We **do not transmit** your personal data, device identifiers, or files to third parties, except for text requests necessary for the neural network's operation.

## 4. Data Storage
All application data (settings, history, execution logs) is stored locally on the user's device in a protected application directory. Deleting the Application results in the deletion of all associated local data.

## 5. Security
We implement technical measures to protect your data:
* Use of modern Android libraries for secure storage (DataStore, Room).
* Local code execution in an isolated environment.
* We do not use proprietary servers to collect and store personal data. For application performance analysis and user experience improvement, we use **Google Analytics 4** in accordance with Google's data processing policy.

## 6. Privacy Policy Changes
We may update this policy as new features are added. The current version is always available in the project repository on GitHub.

## 7. Contact Information
If you have questions regarding this policy or the application's operation, you can create a request (Issue) in this repository.
