# scan2deploy-android

`Scan2Deploy` is an application devoted to the initial staging of Android devices by reading a barcode sequence. Depending on the content of the staging barcodes `Scan2Deploy` will, in this order:

* configure the device Wi-Fi network,
* control some device inner system settings (e.g. the lock-screen enabled state),
* download a data archive from a HTTP/HTTPS server and unpack it to the device,
* install/update applications and the system image, and
* run a customary script (to perform any additional setup).

It's worth noticing that the application is *not* a resident service. The above-mentioned actions are performed as long as `Scan2Deploy` is active and in use.

## Using the latest Scan2Deploy

Scan2Deploy is pre-installed on Android devices. However, if you want to use the latest version instead, you can create a custom Android Enterprise enrollment barcode that pulls a new version from Github (or another location of your choosing). The list of valid fields you can choose from are [documented here](https://developers.google.com/android/work/play/emm-api/prov-devices#create_a_qr_code).  A sample configuration and pre-generated barcode are provided below.

There are several tools you can use to encode your JSON data into a QR code. A few options are provided below.

* [qrencode](https://fukuchi.org/works/qrencode/index.html.en) - Linux command line utility. Example usage: `qrencode -o s2dupdate.png < s2dupdate.json`
* [QR Code Generator](https://kazuhikoarase.github.io/qrcode-generator/js/demo/) - online tool suitable for copy-paste of JSON data.

### Installing the latest version of Scan2Deploy

#### JSON (Latest version)

```json
{
    "android.app.extra.PROVISIONING_DEVICE_ADMIN_COMPONENT_NAME": "com.datalogic.scan2deploy/com.datalogic.scan2deploy.DeviceOwnerReceiver",
    "android.app.extra.PROVISIONING_LEAVE_ALL_SYSTEM_APPS_ENABLED": true,
    "android.app.extra.PROVISIONING_SKIP_ENCRYPTION": true,
    "android.app.extra.PROVISIONING_DEVICE_ADMIN_SIGNATURE_CHECKSUM": "6CG8ls-pzlKcJNeoZtAC-neobD_ypZCI853n_TBGEHI=",
    "android.app.extra.PROVISIONING_DEVICE_ADMIN_PACKAGE_DOWNLOAD_LOCATION": "https://github.com/datalogic/scan2deploy-android/raw/master/Scan2Deploy.apk"
}
```

#### QR Code (Latest version)

![Scan2Deploy v1.9](media/scan2deploy-master.gif)

### Installing a specific version of Scan2Deploy

You can also download a specific release of Scan2Deploy. For example, here is the process for using version `1.9`:

#### JSON (Version 1.9)

```json
{
    "android.app.extra.PROVISIONING_DEVICE_ADMIN_COMPONENT_NAME": "com.datalogic.scan2deploy/com.datalogic.scan2deploy.DeviceOwnerReceiver",
    "android.app.extra.PROVISIONING_LEAVE_ALL_SYSTEM_APPS_ENABLED": true,
    "android.app.extra.PROVISIONING_SKIP_ENCRYPTION": true,
    "android.app.extra.PROVISIONING_DEVICE_ADMIN_SIGNATURE_CHECKSUM": "6CG8ls-pzlKcJNeoZtAC-neobD_ypZCI853n_TBGEHI=",
    "android.app.extra.PROVISIONING_DEVICE_ADMIN_PACKAGE_DOWNLOAD_LOCATION": "https://github.com/datalogic/scan2deploy-android/raw/v1.9/Scan2Deploy.apk"
}
```

#### QR Code (Version 1.9)

![Scan2Deploy v1.9](media/scan2deploy1-9.gif)

## Creating Scan2Deploy files

You will need to create your Scan2Deploy JSON files using a text editor. A JSON schemas is available to help ensure valid file content. There are several advantages to using an editor that supports this schema:

* Provides help text for each field
* Provides real-time compiler-like messages letting you know when you have made a mistake
* Allows you to write files faster and with fewer mistakes

### $schema

A [JSON schema](https://json-schema.org/) file is used to define the structure and features available in Scan2Deploy. The `$schema` tag is used to indicate which version of the schema you are using. You can versions of schema along with complete field documentation at the [Scan2Deploy Android schema](https://github.com/datalogic/scan2deploy-android-schema) repo.

<br>

🔗 [schema reference](https://github.com/datalogic/scan2deploy-android-schema)

<br>

The latest schema can be referenced like this:

```json
{
    "$schema": "https://raw.githubusercontent.com/datalogic/scan2deploy-android-schema/master/schema.json"
}
```

You can also point to a specific version of the schema by pointing to the desired tag. For example:

```json
{
    "$schema": "https://raw.githubusercontent.com/datalogic/scan2deploy-android-schema/v1-1-0/schema.json"
}
```

### Choosing an editor

There are several good JSON editors available. We reccommend [Visual Studio Code](https://code.visualstudio.com/). It is free and has [many features designed to make writing JSON files easier](https://code.visualstudio.com/docs/languages/json), including utilizing JSON schemas.

By including the aforementioned `$schema` tag in you JSON file, Visual Studio Code will provide automatic tool tips and error underlining:

Some other editors know to use the schema files on schemastore.org when a given file uses a specific file extension that is registered on schemastore.org:  `file-name.dla.json`

### Barcode Generation

Scan2Deploy barcodes are generated with the `Scan2Deploy` UI inside DXU Desktop. From the main *Device Configuration Utility* page, click the *Scan2Deploy (Beta)* button:

![Scan2Deploy button](./media/image81.png)

Once you are at the Scan2Deploy UI page, you can browse to the JSON file you have created and then click `Generate PDF`.

![Scan2Deploy button](./media/image82.png)

A PDF page will open up in your default PDF viewer, ready for you to print, save, or scan.

![Scan2Deploy button](./media/image83.png)

## Examples

### Detailed

```json
{
    "$schema": "http://json.schemastore.org/datalogic-scan2deploy-android",
    "layout": {
        "description": "This is for our Android devices",
        "enroll": true
    },
    "padlock": {
        "valid-until": "20991231235959",
        "key": "ihavenomouthandimustscream",
        "state": "locked",
        "hide-from-launcher": false
    },
    "global": {
        "target-path": "/mnt/sdcard/airwatch",
        "install-path": "/mnt/sdcard/airwatch",
        "update-path": "/mnt/sdcard/airwatch",
        "purge-target-path": true,
        "auto-scan": true,
        "script": "setup.s",
        "action": "close",
        "backup-to-enterprise": true,
        "script": [
            "[Post-Install]",
            "DISABLE_APP com.android.chrome"
        ]
    },
    "settings": {
        "date-time": "Thu, 19 Apr 2018 07:51:37 GMT",
        "auto-time": false,
        "auto-time-zone": false,
        "auto-time-server": "pool.ntp.org",
        "debug-bridge": false,
        "lock-screen": false,
        "status-bar": false,
        "navigation-bar": true,
        "charge-threshold": 0,
        "usb-profile": "NONE",
        "usb-function": "CHARGING"
    },
    "network": {
        "essid": "TESTTKIP",
        "hidden": false,
        "mode": "wpa-psk",
        "mode-key": "datalogic",
        "mode-key-encrypted": false,
        "ephemeral": false,
        "wait-for-connection": true
    },
    "deployment": {
        "scheme": "http",
        "host": "172.19.0.77",
        "port": 8080,
        "path": "/airwatch.zip",
        "ping-timeout": 1000,
        "fetch-timeout": 60000,
        "working-archive": "/mnt/sdcard/target.zip",
        "skip-inflation": true
    },
    "blobs": [
        {
            "file": "/mnt/sdcard/airwatch/credentials.bin",
            "content": "fd09B1iL/k4jRWrjrP0/sO44teY            +B3UafBLsMsCEqd1KOv6b6JYBXLVv70FmHdZhIVoEOQvHu7O4PUJStpZQ+4PYjPqCO+NQr81M7GOF421Ke8L2u4EYyqDE5qXfLy2shEgaRwRpr2f35/38WZkh6edyiWZQJjyLeZcuI7WiaJPpw7Jcw7ye7mb7Rl+ePNFmfvUrpeRFtN+5kUsx/SbB1v0gDyOOuoep"
        }
    ]
}
```

Each section is *optional* in nature. Sections `padlock`, `settings`, `network`, `deployment`, and `blobs` are skipped when missing. Also, for the `settings` section, the configuration parameters are *optional* and when not provided the current setting is kept.

### Update Datalogic Tools

The following example will update the Datalogic tools on the device to the following versions:

* DXU Agent 1.20.168
* SoftSpot 1.8.92

```json
{
    "$schema": "http://json.schemastore.org/datalogic-scan2deploy-android",
    "global": {
        "auto-scan": true,
        "target-path": "/sdcard/Download/"
    },
    "deployment": {
        "scheme": "https",
        "host": "github.com",
        "port": 443,
        "path": "/datalogic/sandbox/raw/master/demo.zip"
    }
}
```