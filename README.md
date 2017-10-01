# Hello Honolulu
Create a simple plain HTML extension in Microsoft Project Honolulu.


### Download the application from here: http://aka.ms/honoluludownload

#### 1. Get Started
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

#### 2. Behind the Scenes

Extension in project Honolulu has two prerequisites.

1. Extension must have a manifest to discribe its properties. Including the name, target and under which solution. The previous section has an example.

2. To check whether the extension loaded properly and alive, project honolulu's shell will call the extension via the [window.PostMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) method. On the bare minimum, the extension must reply its state = okay (0).

The JavaScript codes in he index.html demonstrates that:

```JavaScript
    function init() {
        window.addEventListener('message', receiveMessage, false);
    }
    function receiveMessage(event) {
        if (event.data.signature !== 'version 0.0.0') {
            return;
        }
        var response = {
            command: event.data.command,
            srcName: event.data.destName,
            srcSubName: event.data.destSubName,
            sequence: event.data.sequence,
            destName: event.data.srcName,
            destSubName: event.data.srcSubName,
            version: event.data.version,
            signature: event.data.signature,
            srcDepth: 1,
            type: 1,
            data: { state: 0 }
        };
        event.source.postMessage(response, event.origin);
    }
```

`response.type = 1` tells it's a response for the shell's request.

If the extension does not listen the PostMessage calls, or does not reply the call properly. The shell will display a "'extension' failed to load" message. So, it's critical to reply to these calls properly.

