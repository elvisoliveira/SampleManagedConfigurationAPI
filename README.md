
## Chromium Managed Configuration API Demo

This project demonstrates how to use the `ManagedConfigurationPerOrigin` policy in Chromium-based browsers with a simple web app.

### Overview
The Managed Configuration API enables web apps to access externally managed configurations. This example shows how to serve and access a configuration file using the `navigator.managed.getManagedConfiguration()` JavaScript call.

For detailed information about the policy, refer to the [official documentation](https://chromeenterprise.google/policies/?policy=ManagedConfigurationPerOrigin).

### Development Status

The Managed Configuration API is shipped and available in Chromium Desktop starting from version 91 onward. This means that any Chromium-based browser version 91 or higher can utilize the API for managed configuration functionality. For more information, visit the [Chromium Feature Status page](https://chromestatus.com/feature/5755104466567168).

***Important:***
The Managed Configuration API can only be used for force-installed web applications. This requires the web app to be explicitly defined in the WebAppInstallForceList policy. Therefore, the app must be installed as a Web App.

### Steps to Run the Demo

#### 1. Create and Add the Policy Configuration
Create a JSON policy configuration file with the following settings:

```json
{
  "WebAppInstallForceList": [
    {
      "url": "https://elvisoliveira.github.io/SampleManagedConfigurationAPI/",
      "create_desktop_shortcut": true,
      "default_launch_container": "tab"
    }
  ],
  "ManagedConfigurationPerOrigin": [
    {
      "origin": "https://elvisoliveira.github.io/SampleManagedConfigurationAPI/",
      "managed_configuration_url": "https://elvisoliveira.github.io/SampleManagedConfigurationAPI/sample.json",
      "managed_configuration_hash": "ac7c5f03df041f89640a3e64e5d807aa5d654bc0"
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

Chromium stores the hash along with the fetched contents and will only request a new configuration file if the hash changes. [Reference on Source Code](https://github.com/chromium/chromium/blob/4b1b2ae75541695d0821e0754e95021f87a90ad8/chrome/browser/device_api/managed_configuration_api.cc#L263).

#### 2. Apply the Policy
Save the JSON configuration in the appropriate directory for your operating system (paths may vary):
- **Windows**: `C:\Windows\PolicyDefinitions`
- **Linux**: `/etc/chromium/policies/managed/`
- **macOS**: `/Library/Managed Preferences/`

#### 3. Verify the Policy
To confirm that the policy has been applied:
1. Open Chromium and navigate to `chrome://policy`.
2. Check the list of active policies and their current values.

### Configuration File
The `sample.json` file in the repository contains the managed configuration data served to the web app via the Managed Configuration API.

**Details:**
- **Local Serving**: This example serves `sample.json` alongside the web app for simplicity.
- **External Hosting**: The file can be hosted externally, as it is fetched directly by Chromium and is not subject to CORS restrictions.

If hosting externally, update the `managed_configuration_url` in the policy to the file's external URL.

For more details about the Managed Configuration API, refer to the documentation [here](https://wicg.github.io/WebApiDevice/managed_config/) and [here](https://github.com/WICG/WebApiDevice/).

### ChromeOS Enterprise

In enterprises using ChromeOS, the JSON configuration for the Managed Configuration API must be set in the "Managed Configuration" field under the app settings in the Google Admin Console. This allows administrators to centrally define and enforce web app configurations across managed ChromeOS devices. For detailed instructions, check the official ChromeOS documentation: [Web Managed Configuration](https://chromeos.dev/en/enterprise/web-managed-config).
