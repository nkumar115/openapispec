**OpenAPI Specification Validator**

OpenAPI Spec Validator is a Python library that validates OpenAPI Specs against the [OpenAPI 2.0 (aka Swagger)](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md) and [OpenAPI 3.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md) specification. The validator aims to check for full compliance with the Specification.

**Module Name:**

| **Library Name** | **Installation** |
| --- | --- |
| openapi-spec-validator | pip install openapi-spec-validator |

**Usage:**

Open api specification can be verify from Command Line Interface or using python library.

**Command Line Interface**
```
openapi-spec-validator openapi.yaml
```

The output will be shown in CLI itself. If it is openapi compliant specification then &quot; **OK**&quot; message will be displayed, otherwise all the relevant error will be displayed in terminal.

**Example:**

1. Below screenshot shows that defined specification is openapi compliant specification.

![](RackMultipart20211102-4-67rfuh_html_533ee6731d47f296.png)

1. Below screenshot shows error message, which we need to resolve in order to follow open api specification.

![](RackMultipart20211102-4-67rfuh_html_6238b630bb872c8d.png)

This error describes that **CreditorAccount** is a required properties, but it is not defined yet. **Below screenshot** from specification file also shows that &quot;CreditorAccount&quot; is a required properties, however it is not defined.

In order to resolve this error, either **CreditorAccount** can be removed from required section oritneeds to be defined in properties section.

![](RackMultipart20211102-4-67rfuh_html_4543f107c7fb271e.png)

1. Another Example, which suggests &quot;Operation ID is not unique&quot; as shown in below screenshot:

![](RackMultipart20211102-4-67rfuh_html_ce0d77880a5388ec.png)

Below screenshot of specification shows that two get methods have same operation id. This error can be resolved by specifying unique operation ID.

![](RackMultipart20211102-4-67rfuh_html_1d6471de573b1ca8.png)
