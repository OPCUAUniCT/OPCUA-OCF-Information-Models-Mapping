# ResourceType for an OPC UA - OCF object mapping
This repository contains the OCF *ResourceTypes* defined in a proposal of mapping between the OPC UA (IEC 62541) Information Model and the OCF Resource Model.
The proposal, described [here](https://ieeexplore.ieee.org/document/8387649/), allows to map an OPC UA *Node* as an OCF *Resource*.

In order to enable the mapping of each kind of OPC UA Node, three OCF Resource Type has been defined:

- **"x.opc.object"**: used to map OPC UA Object Nodes
- **"x.opc.datavariable**": used to map OPC UA DataVariable Nodes
- **"x.opc.method"**: used to map OPC UA Method Nodes


---

## Validate JSON with VS Code
This project has been created using **Visual Studio Code** and its native JSON Schema Validator in order to check JSON documents.

1. Clone this project using the following:

    ` git clone https://github.com/OPCUAUniCT/OPCUA-Resource-Types-for-OCF.git `

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