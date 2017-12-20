# ResourceType for an OPC UA - OCF object mapping
This is a proposal for an OCF *ResourceType* in order to produce a mapping from an OPC UA *Nodes* in the **Address Space** to OCF *Resources*. This proposal provide interoperability between these two environments trying to maintain semantic, data structure and data encoding.

Only one schema for an OCF ResourceType is provided in this project for a generic OPC UA *Object*. The concepts shown can be extended and/or reused in order to crete new ResourceTypes.

---

## Validate JSON with VS Code
This project has been created using **Visual Studio Code** and its native JSON Schema Validator in order to check JSON documents.

1. Clone this project using the following:

    ` git clone https://gitlab.com/msalafia/opc-json-schema.git `

2. Open the cloned folder using VS Code. This project contains an hidden folder **.vscode** which contains a VS Code workspace setting.

```json
{
    "json.schemas": [
        {
            "fileMatch": [
                "esempio*.json"
            ],
            "url": "/schema/myopcobject.json"
        }
    ]
}
```
This setting says that every JSON file (any file with .json extension) with a name starting with "esempio" must be validated considering the JSON schema **myopcobject.json** in the *"schema"* folder.

3. Create a JSON file with "esempio" at the begin of its name (eg. esempio-1.json, esempioFoo.json, etc.) and start creating a resurce compliant to the schema. All the errors in the schema validation appear in "**Problems**" tab of the VS Code integrated terminal. 