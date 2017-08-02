---

copyright:
  years: 2016, 2017
lastupdated: "2017-08-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Retrieve the Web Application Description Language (WADL) summary of this service


OPTIONS http://{PA Bluemix load balancer URL}/pm/v1/wadl

Use this API call to retrieve the WADL for this service.

Request example:

```
    Content-Type: */*
```
{: codeblock}

Response when the WADL request succeeds:

```
    Content-Type: application/vnd.sun.wadl+xml
    Status code: 200
    body: the WADL XML ,eg.
        <?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
            <application>
                <resources base="http://{PA Bluemix load balancer URL}/pm/v1/">
                    <resource path="score">
                        <doc title="Score using the model with given context ID" />
                        <resource path="{id}">
                            <param style="template" name="id" />
                            <method name="POST">
                                <request>
                                    <param style="query" name="accesskey" />
                                    <representation mediaType="application/json;charset=UTF-8" />
                                </request>
                                <response>
                                    <representation mediaType="application/json;charset=UTF-8" />
                                </response>
                            </method>
                        </resource>
                     </resource>
                     ...

             </resources>
        </application>
```
{: codeblock}

Response when the WADL request fails:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        } 
        ```
{: codeblock}