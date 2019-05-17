# XL-Deploy

XL Deploy offers a [REST API](https://docs.xebialabs.com/xl-deploy/latest/rest-api/index.html) to interact with the application.

And we will use this API to get the dictionary content. I assure that you notify that there is no feature offered by the GUI to export the dictionary content. And, there is likely to be confused by the documentation (without any examples !) offered by Xebia, especially when you start to manipulate the API.

### Export the dictionary content

To export the dictionary, you have to use the [Repository Service](https://docs.xebialabs.com/xl-deploy/latest/rest-api/com.xebialabs.deployit.engine.api.RepositoryService.html) and the resource [CI](https://docs.xebialabs.com/xl-deploy/latest/rest-api/com.xebialabs.deployit.engine.api.RepositoryService.html#/repository/ci/{ID:.+}:GET).

The resource is __GET /repository/ci/{ID:.+}__

This query uses the GET verb. And the service is __repository__. The used resource is __ci__. And the input parameter is the ID of the dictionary. For example, see the picture above, the ID of the dictionary is : __Environments/StockDictionary__.

![alt text](https://raw.githubusercontent.com/OlivierMounicq/XL-Deploy/master/img/Dictionary.png "XL Deply Dictionary")


