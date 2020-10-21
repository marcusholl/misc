### Updates

1.) Under "Prerequisites" we have to _add_:

* With Docker: you have to provide a docker file derived from the "node" docker image containing these global dependencies: "@ui5/cli @sap/ux-ui5-tooling @ui5/logger @ui5/fs"

The corresponding docker file (which we do not publish) would look like

```
FROM node
  
USER root
RUN npm install -g @ui5/cli @sap/ux-ui5-tooling @ui5/logger @ui5/fs
USER node
```

Later in this document we assume that docker image is available with tag "fiorideploy"

* Without Docker: you have installed the node based fiori toolset for uploading content to the abap frontend server. These dependencies can be installed via

```
npm install -g @ui5/cli @sap/ux-ui5-tooling @ui5/logger @ui5/fs
```

This installs the dependencies under "/usr/local/lib". Hence you need to use a user with write permissions to that folder.

2.) Under "The ABAP Life-Cycle Management" we have to _replace_ at "To upload a file to your transport" ...

* With Docker
```
docker run --env ABAP_USER=<YOUR_USER> --env ABAP_PASSWORD=<YOUR_PASSWORD> -v `pwd`:/project -w /project --rm -it fiorideploy fiori deploy --noConfig -t <TRANSPORT_REQUEST_ID> -u <URL> -p <PACKAGE> -n <APP_NAME> -l <CLIENT> -e "<DESCRIPTION>" -f -y --username ABAP_USER --password ABAP_PASSWORD
```

* Without Docker
```
export ABAP_USER=<USER> ABAP_PASSWORD=<PASSWORD>
fiori deploy --noConfig -t <TRANSPORT_ID> -u <URL> -p <PACKAGE> -n <APP_NAME> -l <CLIENT> -e "<DESCRIPTION>" -f -y --username ABAP_USER --password ABAP_PASSWORD
unset ABAP_USER ABAP_PASSWORD
```

* &lt;URL&gt; is something like https://example.org:8000
* In both cases we need to be in the project root directory.
* We expect the application has been built prior to the deployment.
* The application needs to be contained in a folder named `dist`.
* The part about creating a transport request remains unchanged.

The parameters are
* URL / Endpoint is something like https://example.org:8000 "The endpoint of your service" is not 100% accurate since the "endpoint" would be something like: "https://example.org/sap/opu/odata/ui5/abap_repository_srv". The later part is added by the toolset internally.
* CLIENT The abap client
* DESCRIPTION An arbitrary description for the app. Only taken into account for initial deployments, not for re-deployments.
* PACKAGE The package to that the application belongs to
* APP_NAME: The name of the application
* The other parameters not explicitly mentioned here remains unchanged.

### Additional Remarks:

* Under "Prerequisites": link for cm_client docker file does not work
