# Hello Honolulu
Create a simple plain HTML extension in Microsoft Project Honolulu.


### Download the application from here: http://aka.ms/honoluludownload

After installation, all the UI content are copied under the **C:\ProgramData\Server Management Experience\Ux** folder.

Copy the manifest.json from this folder to a place like Desktop. (it's readonly there, we need to modify it).

Open it and copy the following hello manifest info under the modules array as the first item:

```json
{
    "$schema": "../node_modules/@msft-sme/core/dist/manifest/module-schema.json#",
    "name": "html",
    "target": "/modules/html",
    "entryPoints": [
        {
            "entryPointType": "tool",
            "name": "html",
            "urlName": "html",
            "displayName": "HTML Site",
            "icon": "",
            "requirements": [
                {
                    "solutionIds": [
                        "msft.sme.server-manager!servers"
                    ],
                    "connectionTypes": [
                        "msft.sme.connection-type.server"
                    ]
                }
            ]
        }
    ],
    "version": "0.0.1"
},
```

Refer to the the completed example in this repo.

Copy this modified manifest back to the Ux folder.

Copy the hello folder in this repo to **C:\ProgramData\Server Management Experience\Ux\modules** folder.

That's it.

Restart the application and you will have single HTML extension running in the shell.
