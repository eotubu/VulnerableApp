---
layout: default
title: Design Documentation
nav_order: 4
---
# Design Documentation

While designing VulnerableApp, major emphasis was given on **Ease of adding Vulnerabilities** such that developers of Vulnerability Scanners need to put minimal effort for adding new Vulnerabilities for testing their payload/attack vectors.

## Technologies used
- Java8
- Spring Boot
- Vanilla Javascript

**Note: we are not limited to these technologies and if required, open to expand to other technologies.**

## Design:
UserInterface for VulnerableApp is driven from backend endpoint "/allEndPointJson" which provides entire information about all the Vulnerabilities present in the VulnerableApp. This information is dynamic and is generated from the annotations present over the java class.

### Annotations:
There are 3 annotations which are driving entire VulnerableApp, specifically UserInterface, Vulnerability definition for **Scanners**, Exposing Vulnerabilities and its help etc.

1. VulnerableAppRestController
2. VulnerableAppRequestMapping
3. AttackVector

**Description:**

**VulnerableAppRestController**: 
This annotation is annotated with the Spring's **RestController** annotation and hences exposes the Vulnerability as Rest Endpoint. 
Along with exposing the vulnerability as endpoint this annotation is also used to generate the description of the Vulnerability which is shown in the UI for learning about the vunerability.

For creating a new Vulnerability developers need to add this annotation to there class and a Rest endpoint will get exposed automatically and a UI will be built for the same.

1. [Java Documentation](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/java/org/sasanlabs/internal/utility/annotations/VulnerableAppRestController.java)
2. [Example usage](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/java/org/sasanlabs/service/vulnerability/pathTraversal/PathTraversalVulnerability.java)

**VulnerableAppRequestMapping**:
This annotation is annotated with Spring's **RequestMapping** annotation and is used to add the *Levels* under the Vulnerability which we have specified using the **VulnerableAppRestController** annotation.

Along with adding Levels to vulnerabilities, there are many parameters in this annotation. 
lets talk about some of them:
1. descriptionLabel: It is used to describe the Endpoint requirements in human readable form like if an input is passed as a URL Query param or Cookie etc.
2. htmlTemplate: As UI is driven from annotations so other things in UI like Name of Vulnerabilities, Levels etc are generically populated in the UI but the specific endpoint requirements which are described using **descriptionLabel** field need to be shown in the UI and the template name mentioned here is used to generate that specific portion of the UI. Template name is appended with *.js*, *.css* and *.html* to find the respective javascript/styling and html for the level.

All the other parameters in the annotations are used for generating information for scanners, specifically */scanner* endpoint. 
1. [Java Documentation](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/java/org/sasanlabs/internal/utility/annotations/VulnerableAppRequestMapping.java)
2. [Example usage](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/java/org/sasanlabs/service/vulnerability/pathTraversal/PathTraversalVulnerability.java)
3. [Example Templates](https://github.com/SasanLabs/VulnerableApp/tree/master/src/main/resources/static/templates)

**AttackVector**:
This annotation is mainly used to help users to expose the vulnerability. This annotation is used to help students/security enthusiasts about various payloads/ways to expose the vulnerability.

1. [Java Documentation](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/java/org/sasanlabs/internal/utility/annotations/AttackVector.java)
2. [Example usage](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/java/org/sasanlabs/service/vulnerability/jwt/JWTVulnerability.java)

### VulnerableApp UserInterface Framework
VulnerableApp's UI framework is divided into 3 parts:
1. [index.html](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/resources/static/index.html) 
2. [VulnerableApp.js](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/resources/static/vulnerableApp.js)
3. [VulnerableApp.css](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/resources/static/vulnerableApp.css)

The VulnerableApp's screen is divided into 4 parts where 3 parts are generated using above mentioned files and 1 part is generated using *htmlTemplate* mentioned above.

For more detailed information please look at below labeled images:
![VulnerabilitySelection Page Description](Vulnerability%20list%20Description.jpg)
![VulnerabilityLevel Page Description](VulnerabilityLevel%20Description.jpg)

**Html Template:**

The html template section is generated using *htmlTemplate* parameter of the annotation and VulnerableApp's UI looks for the html/js/css files to generate the User Interface for the selected level in the selected Vulnerability.
VulnerableApp's UI will always look for the htmlTemplate under `src/main/resources/static/templates/<Vulnerability Name>`. Vulnerability Name is same as the value of **VulnerableAppRestController** annotation's value parameter.

VulnerableApp's UI exposes some of the functionality Out-Of-The-Box like generic utility for doing Ajax Calls, generic css for button animation/look and feel etc.

### Currently handled Vulnerability types

1. [JWT Vulnerability](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/java/org/sasanlabs/service/vulnerability/jwt/)
2. [Command Injection](https://github.com/SasanLabs/VulnerableApp/tree/master/src/main/java/org/sasanlabs/service/vulnerability/commandInjection)
3. [File Upload Vulnerability](https://github.com/SasanLabs/VulnerableApp/tree/master/src/main/java/org/sasanlabs/service/vulnerability/fileupload)
4. [Path Traversal Vulnerability](https://github.com/SasanLabs/VulnerableApp/tree/master/src/main/java/org/sasanlabs/service/vulnerability/pathTraversal)
5. [SQL Injection](https://github.com/SasanLabs/VulnerableApp/tree/master/src/main/java/org/sasanlabs/service/vulnerability/sqlInjection)
    1. [Error Based SQLi](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/java/org/sasanlabs/service/vulnerability/sqlInjection/ErrorBasedSQLInjectionVulnerability.java)
    2. [Union Based SQLi](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/java/org/sasanlabs/service/vulnerability/sqlInjection/UnionBasedSQLInjectionVulnerability.java)
    3. [Blind SQLi](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/java/org/sasanlabs/service/vulnerability/sqlInjection/BlindSQLInjectionVulnerability.java)
6. [XSS](https://github.com/SasanLabs/VulnerableApp/tree/master/src/main/java/org/sasanlabs/service/vulnerability/xss)
    1. [Persistent XSS](https://github.com/SasanLabs/VulnerableApp/tree/master/src/main/java/org/sasanlabs/service/vulnerability/xss/persistent)
    2. [Reflected XSS](https://github.com/SasanLabs/VulnerableApp/tree/master/src/main/java/org/sasanlabs/service/vulnerability/xss/reflected)
7. [XXE](https://github.com/SasanLabs/VulnerableApp/tree/master/src/main/java/org/sasanlabs/service/vulnerability/xxe)
8. [Open Redirect](https://github.com/SasanLabs/VulnerableApp/tree/master/src/main/java/org/sasanlabs/service/vulnerability/urlRedirection)
    1. [Http 3xx Status code based](https://github.com/SasanLabs/VulnerableApp/blob/master/src/main/java/org/sasanlabs/service/vulnerability/urlRedirection/Http3xxStatusCodeBasedInjection.java)
