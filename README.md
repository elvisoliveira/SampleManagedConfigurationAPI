
## Chromium Managed Configuration API Demo

This project demonstrates how to use the `ManagedConfigurationPerOrigin` policy in Chromium-based browsers with a simple web app.

---

### Overview
The Managed Configuration API enables web apps to access externally managed configurations. This example shows how to serve and access a configuration file using the `navigator.managed.getManagedConfiguration()` JavaScript call.

For detailed information about the policy, refer to the official documentation [here](https://chromeenterprise.google/policies/?policy=ManagedConfigurationPerOrigin).

---

### Steps to Run the Demo

#### 1. Start the Local HTTP Server
To serve the repository contents to Chromium, I'm using the PHP's built-in HTTP server:

```bash
php -S 0.0.0.0:8475
```

This command starts a local server to host the web app. Ensure PHP is installed. If not, follow relevant installation instructions for your OS.

#### 2. Create and Add the Policy Configuration
Create a JSON policy configuration file with the following settings:

```json
{
  "WebAppInstallForceList": [
    {
      "url": "http://127.0.0.1:8475/",
      "create_desktop_shortcut": true,
      "default_launch_container": "tab"
    }
  ],
  "ManagedConfigurationPerOrigin": [
    {
      "origin": "http://127.0.0.1:8475/",
      "managed_configuration_url": "http://127.0.0.1:8475/sample.json",
      "managed_configuration_hash": "ac61833450a10510a95e27305571cc6d9d085b32"
    }
  ]
}
```

**Key Configuration Details:**
- **`WebAppInstallForceList`**: Specifies the URL of the web app to be installed.
- **`ManagedConfigurationPerOrigin`**:
  - `origin`: The URL of the web app allowed to access the configuration.
  - `managed_configuration_url`: The URL of the configuration file.
  - `managed_configuration_hash`: A version control hash. This example uses a SHA-1 hash generated with `sha1sum sample.json`, but any valid string is acceptable.

Chromium stores the hash along with the fetched contents and will only request a new configuration file if the hash changes.

#### 3. Apply the Policy
Save the JSON configuration in the appropriate directory for your operating system (paths may vary):
- **Windows**: `C:\Windows\PolicyDefinitions`
- **Linux**: `/etc/chromium/policies/managed/`
- **macOS**: `/Library/Managed Preferences/`

#### 4. Verify the Policy
To confirm that the policy has been applied:
1. Open Chromium and navigate to `chrome://policy`.
2. Check the list of active policies and their current values.

---

### Configuration File
The `sample.json` file in the repository contains the managed configuration data served to the web app via the Managed Configuration API.

**Details:**
- **Local Serving**: This example serves `sample.json` alongside the web app for simplicity.
- **External Hosting**: The file can be hosted externally, as it is fetched directly by Chromium and is not subject to CORS restrictions.

If hosting externally, update the `managed_configuration_url` in the policy to the file's external URL.

---

For more details about the Managed Configuration API, refer to the documentation [here](https://wicg.github.io/WebApiDevice/managed_config/) and [here](https://github.com/WICG/WebApiDevice/).
