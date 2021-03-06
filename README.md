# XL-Deploy

XL Deploy offers a [REST API](https://docs.xebialabs.com/xl-deploy/latest/rest-api/index.html) to interact with the application.

And we will use this API to get the dictionary content. I assure that you notify that there is no feature offered by the GUI to export the dictionary content. And, there is likely to be confused by the documentation (without any examples !) offered by Xebia, especially when you start to manipulate the API.

## 1. Export the dictionary content

To export the dictionary, you have to use the [Repository Service](https://docs.xebialabs.com/xl-deploy/latest/rest-api/com.xebialabs.deployit.engine.api.RepositoryService.html) and the resource [CI](https://docs.xebialabs.com/xl-deploy/latest/rest-api/com.xebialabs.deployit.engine.api.RepositoryService.html#/repository/ci/{ID:.+}:GET).

The query to execute is __GET /repository/ci/{ID:.+}__

This query uses the GET verb. And the service is __repository__. The used resource is __ci__. And the input parameter is the ID of the dictionary. For example, see the picture above, the ID of the dictionary is : __Environments/StockDictionary__ (the field Id). 
And you can get this form by selecting the dictionary.

![alt text](https://raw.githubusercontent.com/OlivierMounicq/XL-Deploy/master/img/Dictionary.png "XL Deply Dictionary")

And the global the query is : 

http://myxldeployserver/deployit/repository/ci/Environments/StockDictionary  

* myxldeployserver/deployit : the server (root url)
* repository : the service
* ci : the resource
* Environments/StockDictionary : the dictionary ID (its type : udm.Dictionary).

You can type this query in your browser adress textbox, and you will get the dictionary content in XML format. 


In C\#, we can use this snippet.
using System.Net.Http;

```cs
public XDocument GetDictionaryContent(string urlToGetDictionaryContent, string login, SecureString pwd)
{
  XDocument xml;
  
  //Set the login/password to query API  
  var byteArray = Encoding.ASCII.GetBytes($"{login}:{new NetworkCredential(string.Empty, pwd).Password}");
  var hashedCredential = Convert.ToBase64String(byteArray);

  using(var httpClient = new HtppClient())
  {
    httpClient.DefaultRequestHeaders.Clear();
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", hashedCredential);
    
    var response = await httpClient(urlToGetDictionaryContent);
    
    var statusCode = (int)response.StatusCode;
    
    if(statusCode != 200)
    {
      throw new Exception($"HTTP status code {statusCode} - check the credentials or the query. The GET query is {urlToGetDictionaryContent}");
    }
    
    var xmlStr = await response.Content.ReadAsStringAsync();
    xml = XDocument.Parse(xmlStr);
  }

  return xml;
}


```

Remark #1 :  You rather use only one instance HttpClient to perform several queries. [See Remarks paragraph](https://docs.microsoft.com/en-us/dotnet/api/system.net.http.httpclient?view=netframework-4.8)

## 2. Others queries

### 2.1 The query to get package list related to an application

The query is :

http://myxldeployserver/deployit/repository/query?type=udm.DeploymentPackage&parent=Applications/FOLDER/MyApp

* myxldeployserver/deployit : the server (root url)
* repository : the service
* query : the resource
* type=udm.DeploymentPackage : mandatory 
* parent=Applications/FOLDER/MyApp : the ID of the application (its type : udm.Application).

### 2.2 The query to get the environment definition

The query is :

http://myxldeployserver/deployit/repository/ci/Environments/ENV/MyEnv

* myxldeployserver/deployit : the server (root url)
* repository : the service
* ci : the resource
* Environments/ENV/MyEnv : the dictionary ID (its type : udm.Environment).



## 3. How to get more information about a query.

In order to get some hints about the query that you want to execute, I advise you to use the developer console/tools available in each browser. The _Network_ window can provide you details about the HTTP query (the header, the body and the response). In many case, you could get the query's entire url.








