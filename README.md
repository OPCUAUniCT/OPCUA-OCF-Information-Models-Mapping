# Mapping between OPC UA and OCF information models

## Introduction
Since few years, Industry has been featured by a revolution, the fourth one, which has been coined with different names in different countries; the most known is Industry 4.0.

One of the main goal of the Industry 4.0 is the interoperability of industrial applications and the use of enabling ICT technologies, mainly those based on the IoT. This goal may be reached through integration of different communication systems used in the current Industry 4.0 and IoT scenarios. For this reason, during these last years, several solutions dealing with the integration of OPC UA and IoT ecosystems appeared in the literature, due to important role played by OPC UA inside the current Industry 4.0 reference models (see for example the [RAMI4.0](https://www.zvei.org/fileadmin/user_upload/Themen/Industrie_4.0/Das_Referenzarchitekturmodell_RAMI_4.0_und_die_Industrie_4.0-Komponente/pdf/5305_Publikation_GMA_Status_Report_ZVEI_Reference_Architecture_Model.pdf) and [IIRA](https://www.iiconsortium.org/IIC_PUB_G1_V1.80_2017-01-31.pdf) reference architectures which both indicate OPC UA as one of the main connectivity standard on Industry 4.0).

The research group at University of Catania, DIEEI, has recently proposed a novel solution to make interoperable OPC UA and IoT ecosystems; some preliminary results have been presented [here](https://ieeexplore.ieee.org/document/8387649/) and [here](https://ieeexplore.ieee.org/document/8352412). Among the current IoT ecosystems, OCF has been chosen for the integration with OPC UA, as it seems a promising solution to standardise the exchange of information into IoT. It is important to point out that no other solutions of interoperability between OPC UA and OCF are present in the current literature. 

## Research goal
Interoperability between OPC UA and OCF has been realised in both direction. This repository contains documents about the mapping from OPC UA to OCF in the folder [OCFResourceType]() and documents about the mapping from OCF to OPC UA in the folder [OCF OPC UA Information Model]()

Considering the interoperability from OPC UA to OCF, the proposal is based on the idea to make available information maintained by an OPC UA Server to whatever IoT devices compliant with OCF specifications. This is realised through the ad-hoc definition of a new OCF Device Type, called in the paper **"x.opc.device"**. According to the solution here presented, each information maintained by an OPC UA Server may be mapped into an OCF Device of **"x.opc.device"** type; acting as a server, this device may expose this information to whatever client devices in the OCF ecosystem. 

The figure in the following gives a graphical representation of the proposed solution. As it can be seen on the left of the figure, the OCF ecosystem is present with several OCF Devices exchanging information though OCF communication protocol. 

<p align="center">
<img src="imagegithub.png" width="500">
</p>
    
As shown, in the OCF ecosystem it is assumed the presence of at least an OCF Device of **"x.opc.device"** type. On the right side, the OPC UA ecosystem is shown, made up by OPC UA Servers and one or more OPC UA Clients and Subscribers. The arrow from the OPC UA Server to the OCF Device of **"x.opc.device"** type represents the information flow needed to populate this last device with the information maintained by the OPC UA Server. Information flow is realised on the basis of the mapping defined in the research carried out by the research group at University of Catania (DIEEI) and presented in the remainder of this paper. The mapping specifies how each element of the OPC UA AddressSpace is mapped in the correspondent element of the OCF Resource Model inside the OCF Device of **"x.opc.device"** type.

It is important to point out that the proposal involves only the mapping rules from OPC UA to OCF information models. Details about how each information from OPC UA Server is actually transferred to the OCF Device of **"x.opc.device"** type (e.g., communication protocols and services used to realise the mapping) are outside the scope of the research carried on.

Considering the interoperability from OCF to OPC UA, the proposal is based on the definition of an OPC UA Information Model having the aim to represent OCF Resource Model. It is built on the top of the existing OPC UA Device Model and is called *OCF OPC UA Information Model*, as graphically represented in the following figure. 

<p align="center">
<img src="OCFOPCInformModel.png" width="500">
</p>

This novel information model offers several novel OPC UA ObjectTypes defined to map elements of OCF Resource Model (e.g., Device Type, Resource Type, Resource). Most of the novel ObjectTypes inherit from the types of the OPC UA Device Model. The novel ObjectTypes here defined are: **OCFResourceType**, **OCFResourceInstanceType** and **OCFDeviceType**.

# Mapping from OPC UA to OCF

Mapping from OPC UA to OCF may involve the entire OPC UA AddressSpace of a specific OPC UA Server or its subset. As said before, the proposal is based on the use of an OCF Device belonging to the ad-hoc defined **“x.opc.device”** Device Type. Such Device is made up by several OCF Resources which may be related each other by OCF Links. OCF Resources are aimed to model OPC UA Nodes and belongs to one of three ad-hoc defined Resource Types: **“x.opc.object”**, **“x.opc.datavariable”** and **“x.opc.method”**, described in the following.

It is worth noting that the **“x.opc.device”** Device Type has been defined to represent the environment where the elements of OPC UA AddressSpace (or its subset) are mapped. A Device of this type shall expose a mandatory OCF Resource of the **“x.opc.object”** Resource Type named *“AddressSpaceSubset”*. It aims to aggregate OCF Resources mapping OPC UA Nodes.

## OCF Resource Types folder

The folder *OCF Resource Types* contains a subfolder named *schema* where the JSON Schema relevant to the three ad-hoc defined OCF Resource Types **“x.opc.object”**, **“x.opc.datavariable”** and **“x.opc.method”** are stored in the files *"opc.object-schema.json*, *"opc.databariable-schema.json* and *"opc.method-schema.json*, respectively. Furthermore, the basic OCF Resource Type needed by these Resource Types are also included in the folder.

Finally, some JSON examples are provided in the *OCF Resource Types* folder in order to better understand the JSON Schema provided.

The OCF Resource Types defined extend the “oic.core” OCF Resource Type. For this reason, each Resource representing an OPC UA Node contains the OCF common properties filled as follows: 
- Name “n” property is filled using the OPC UA attribute DisplayName,
- Resource Type (“rt”) property is filled with the name of the Resource Type used to represent the Resource (i.e.  **“x.opc.object”**, **“x.opc.datavariable”** and **“x.opc.method”**),
- Identifier of the Resource (“id”) property is filled using a string representation of the OPC UA NodeId of the mapped Object Node, made up by the concatenation of the value ns (NamespaceIndex) and value i (Identifier) separated by a dash; for example, the NodeId made up by ns=2 and i=12 becomes “2-12”.

### "x.opc.object" Resource Type
The “x.opc.object” Resource Type has been defined to map OPC UA Object Nodes of any ObjectType (including FolderType). 
In addition to the OCF common properties, a Resource belonging to the “x.opc.object” Resource Type exposes several properties, described in the following table.

| Property Name | OCF data type | Mandatory | Description
| ------------- | ------------- | ------------- | ------------- |
|  <Common properties\>  | - | - | Properties defined by "oic.core" Resource Type|
| OPCNodeType  | string  | Yes | It specifies if the Resource represents an OPC UA Node of ObjectType or FolderType type|
| OPCDescription  | string  | No | It maps the Description attribute of the OPC UA Object Node represented|
| OPCProperties  | array of objects  | No | An array of JSON objects representing the OPC UA Property Nodes of the OPC UA Object Node represented|
| links  | string  | array of OCF Links | An array of OCF Links mapping OPC UA OPC UA Organizes and HasComponent References|

*OPCProperties* is defined as an array of JSON objects. Each JSON object mapping an OPC UA Property Node, named **OPCPropertyObject**, is made up by the property defined in the following table.

| Property Name | OCF data type | Mandatory | Description
| ------------- | ------------- | ------------- | ------------- |
|  <Common properties\>  | - | - | Properties defined by "oic.core" Resource Type|
| opc-property-name  | string  | Yes | It contains the BrowseName attribute of the OPC UA Property Node|
| opc-property-value  | \<type>  | Yes | \<type> represents the OCF data type mapping the OPC UA DataType of Value attribute of the OPC UA Property Node. The property contains the representation of the Value attribute of OPC UA Property Node according to the \<type>. Only in the case of OPC UA Property with attribute Value of array type, the \<type> is the OCF data type array. In this case, the OCF data type of each element of the array is given by the *innermost-type* property and this property contains an array.|
| value-type  | string  | Yes | It specifies the OCF data type used for the representation of the Value attribute of OPC UA Property Node. In case of attribute Value of array type, this property specifies OCF array data type.|
| enum-values  | array of objects  | No | Array of JSON objects each containing the enumeration values. It is present only if the Value attribute of the mapped OPC UA Property Node is of Enumeration DataType; in this case \<type> is number and *opc-property-value* represents the integer representation of the enumeration.|
| num-dimensions  | array of number  | No | It is present only if \<type> is array and it specifies the relevant dimensions.|
| innermost-type  | string  | No | It is present only if \<type> is array and it contains the OCF data type mapping the OPC UA DataType of each element of the array.|

If the Value attribute of the OPC UA Property Node is an enumeration, the optional *enum-values* property contains an array of JSON objects each containing the enumeration values. Each object is made up by the attributes described in the following table.

| Property Name | OCF data type | Mandatory | Description
| ------------- | ------------- | ------------- | ------------- |
| enumeration-index | number | Yes | The integer representation of an Enumeration |
| enumeration-value | string | Yes | A human-readable representation of the Value of the Enumeration |

### "x.opc.datavariable" Resource Type
OPC UA DataVariable Nodes are mapped as instances of the “x.opc.datavariable” OCF Resource Type. In addition to the OCF common properties, a Resource belonging to the “x.opc.datavariable” Resource Type exposes several properties, described in the following table.

| Property Name | OCF data type | Mandatory | Description
| ------------- | ------------- | ------------- | ------------- |
|  <Common properties\>  | - | - | Properties defined by "oic.core" Resource Type|
| OPCValue  | \<type>  | Yes | \<type> represents the OCF data type mapping the OPC UA DataType of Value attribute of the OPC UA DataVariable Node. The property contains the representation of the Value attribute of OPC UA DataVariable Node according to the \<type>. Only in the case of OPC UA Property with attribute Value of array type, the \<type> is the OCF data type array. In this case, the OCF data type of each element of the array is given by the *innermost-type* property and this property contains an array.|
| value-type  | string  | Yes | It specifies the OCF data type used for the representation of the Value attribute of OPC UA DataVariable Node. In case of attribute Value of array type, this property specifies OCF array data type.|
| enum-values  | array of objects  | No | Array of JSON objects each containing the enumeration values. It is present only if the Value attribute of the mapped OPC UA DataVariable Node is of Enumeration DataType; in this case \<type> is number and *OPCValue* represents the integer representation of the enumeration.|
| num-dimensions  | array of number  | No | It is present only if \<type> is array and it specifies the relevant dimensions.|
| innermost-type  | string  | No | It is present only if \<type> is array and it contains the OCF data type mapping the OPC UA DataType of each element of the array.|
| OPCProperties  | array of objects  | No | An array of JSON objects representing the OPC UA Property Nodes of the OPC UA Object Node represented|
| links  | string  | array of OCF Links | An array of OCF Links mapping OPC UA OPC UA Organizes and HasComponent References|

If the Value attribute of the OPC UA DataVariable Node is an enumeration, the optional *enum-values* property contains an array of JSON objects each containing the enumeration values. Each object is made up as described for **OPCPropertyObject**.

### "x.opc.method" Resource Type
OPC UA Method Nodes are mapped as instances of the “x.opc.method” OCF Resource Type. In addition to the OCF common properties, a Resource belonging to the “x.opc.method” Resource Type exposes several properties, described in the following table.

| Property Name | OCF data type | Mandatory | Description
| ------------- | ------------- | ------------- | ------------- |
|  <Common properties\>  | - | - | Properties defined by "oic.core" Resource Type|
| OPCInputArg | array of objects | No | An array of JSON object representing the input arguments of the Method (i.e. OPCInputArg OPC Property Node). |
| OPCOutputArg | array of objects | No | An array of JSON object representing the output arguments of the Method (i.e. OPCOutputArg OPC Property Node). |
| Executable | boolean | Yes | It is used to map the Executable and UserExecutable attributes of the OPC UA Method Node |
| OPCProperties  | array of objects  | No | An array of JSON objects representing the OPC UA Property Nodes of the OPC UA Object Node represented|
| links  | string  | array of OCF Links | An array of OCF Links mapping OPC UA OPC UA Organizes and HasComponent References|

### Mapping Value of OPC UA Variable Nodes

The following table contains the conversion rules among OPC UA, JSON and OCF data types.

| OPC UA DataType | JSON base type | OCF data type
| ------------- | ------------- | ------------- |
| **Built-in**: Integer, Float, Double, StatusCode| number | number |
| **Enumeration** |string | string |
| **Built-in**: Boolean| literal names (*true* and *false*) | boolean |
| **Built-in**: NodeId, ExpandedNodeId, DiagnosticInfo DataValue, Variant, ExtentionObject, LocalizedText, QualifiedName | object | object |
| **Structured**| object | object |
| **Array** | array | array |

The current value of the Value attribute of OPC UA Variable Node (both Property and DataVariable) is encoded according to the OCF data type and the value so obtained is assigned to the property *opc-property-value* in the case of OPC UA Property Node or to the property *OPCValue* in the case of OPC UA DataVariable Node. Particular cases occur when the Value attribute of OPC UA Variable belongs to OPC UA Enumeration or Structured DataTypes.

In case of Enumeration DataType, the property *enum-values* of an **OPCPropertyObject** and *x.opc.datavariable* Resource Type is filled with an array of JSON objects containing the enumeration values. Each JSON object in the array is made up by two properties named *enumeration-index* and *enumeration-value* as previously described.

In the case of Structured DataType, an ad-hoc JSON object has been defined to represent the original OPC UA Structured DataType. It is made up by only one property named *fields*. This property is an array of JSON objects, each of which models a single OPC UA Field of an OPC UA Structured DataType. Yhe following table describes all the properties of this JSON object.

| Property Name | OCF data type | Mandatory | Description
| ------------- | ------------- | ------------- | ------------- |
| field-name  | string  | Yes | It contains the Field Name value of the OPC UA Field |
| field-value  | \<type>  | Yes | \<type> represents the OCF data type mapping the OPC UA DataType specified by TypeName value of of the OPC UA Field. The property contains the representation of the current value of the OPC UA Field encoded according to the \<type>. Only in the case of attribute Value of array type, the \<type> is the OCF data type array. In this case, the OCF data type of each element is given by the *innermost-type* property and the property contains an array.|
| value-type  | string  | Yes | It specifies the OCF data type used for the the representation of current value of the OPC UA Field. In the case of attribute Value of array type, this property specifies OCF array data type.|
| enum-values  | array of objects  | No |An array of JSON objects each containing the enumeration values. It is present only if the OPC UA Field Value is of Enumeration DataType; in this case \<type> is number and “field-value” represents the integer representation of the enumeration.|
| num-dimensions  | array of number  | No | It is present only if \<type> is array and it specifies the relevant dimensions.|
| innermost-type  | string  | No | It is present only if \<type> is array and it contains the OCF data type mapping the OPC UA DataType of each element of the array.|

As for example, the following JSON represent an OPC UA Object with a structured Property named EURange.

<p align="center">
<img src="example-eu-range.png" width="500">
</p> 

---

### Validate JSON with VS Code
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

# Mapping from OCF to OPC UA

Mapping from OCF to OPC UA specifies how each element of the OCF Resource Model is mapped in the correspondent element of the OPC UA AddressSpace of an OPC UA Server. Through this mapping, information maintained by a generic OCF Device may be published by an OPC UA Server making this information available to whatever OPC UA Client connected with the OPC UA Server.

As said before, the proposal is based on the definition of an OPC UA Information Model called *OCF OPC UA Information Model*. The following figure gives a graphical representation of the novel information model.

<p align="center">
<img src="ocfinformationmodel.png">
</p>

The novel ObjectTypes defined are: **OCFResourceType**, **OCFResourceInstanceType** and **OCFDeviceType**. Before deepening the description of these ObjectTypes, it is important to point out the main assumptions assumed.

First of all, it is worth noting that OCF Resource Model allows multiple inheritance. In fact, an OCF Resource can be instance of one or more OCF Resource Types. Each OCF Resource Type defines a set of properties owned by the OCF Resource. Mapping of each of the OCF Resource Types has been realised defining the abstract ObjectType named **OCFResourceType**. A subtype of **OCFResourceType** is used to define a new OPC UA ObjectType mapping an OCF Resource Type. Each subtype features components made up by OPC UA DataVariables, mapping all the properties of relevant OCF Resource Type.

The obvious mapping of an OCF Resource in OPC UA ecosystem should consist of defining an OPC UA ObjectType subtype **OCFResourceType** for each of its OCF Resource Type and create an OPC UA Object instance of all these ObjectTypes. Although a solution of this kind seems to be perfect to realise the mapping of an OCF Resource, in the OPC UA ecosystem multiple inheritance is forbidden and a different approach must be provided.

The solution adopted is the definition of a particular OPC UA ObjectType, called **OCFResourceInstanceType**. For each OCF Resource, an instance of **OCFResourceInstanceType** is created; the goal is to use this instance to aggregate instances of the **OCFResourceType** subtypes modelling OCF Resource Types relevant to the OCF Resource. For this reason, it has been assumed that these instances contain the actual values of the properties relevant to the OCF Resource represented. In this way, the solution adopted allows to distribute the state of a Resource among these instances.

The last novel ObjectType defined in this mapping is **OCFDeviceType**. It is a subtype of the OPC UA DeviceType ObjectType and its instances will be used to represent OCF Devices. As an OCF Device contains OCF Resources, the instances of **OCFResourceInstanceType** will be component of an **OCFDeviceType** instance.

## OCF OPC UA Information Model folder

The folder *OCF OPC UA Information Model* contains the XML file defining the novel *OCF OPC UA Information Model*. This file, named *ocfinformationmodel.xml* has been created using the tool *UaModeler*. This tool is developed by Unified-Automation and is able to generate the code of a custom information model; this code can be used to import the information model in an OPC UA AddressSpace.

It is worth noting that the folder contains also the files needed to import and edit the UAModeler project relevant to the *OCF OPC UA Information Model*.

## Publications

- Cavalieri S, Salafia M G, Scroppo M S (2018). Realising Interoperability between OPC UA and OCF. “IEEE Access” Journal, Pending publication
- Cavalieri S, Salafia M G, Scroppo M S (2018). Towards interoperability between OPC UA and OCF. “Journal of Industrial Information Integration” Journal, Pending Review
- Cavalieri S, Salafia M G, Scroppo M S (2018). Interoperability between OPC UA and OCF. Pending Review of ICIT 2019, 20th International Conference on Industrial Technology, 13-15 February 2019, Melbourne (Australia).
- Cavalieri S, Scroppo M S (2018). [A proposal to make OCF and OPC UA interoperable](https://ieeexplore.ieee.org/document/8352412). Proceedings of ICIT 2018, 19th International Conference on Industrial Technology, 20-22 February 2018, Lyon (France).
- Cavalieri S, Salafia M G, Scroppo M S (2018). [Mapping OPC UA AddressSpace to OCF resource model](https://ieeexplore.ieee.org/document/8387649/). Proceedings of ICPS 2018, 1st International Conference on Industrial Cyber-Physical System, 15-18 May 2018, Saint-Petersburg (Russia).
