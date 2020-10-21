### Updates

1.) Under "Prerequisites" we have to _add_:

* With Docker: you have to provide a docker file derived from the "node" docker image containing these global dependencies: "@ui5/cli @sap/ux-ui5-tooling @ui5/logger @ui5/fs". All these dependencies are available in the default npm registry.

The corresponding docker file (which we do not publish) would look like

```
FROM node
  
USER root
RUN npm install -g @ui5/cli @sap/ux-ui5-tooling @ui5/logger @ui5/fs
USER node
```

In case the communication to the ABAP backend is performed via https it might be required to add custom certificates.

Later in this document we assume that docker image is available with tag "fiorideploy"

* Without Docker: you have installed the node based fiori toolset for uploading content to the abap frontend server. These dependencies can be installed via

```
npm install -g @ui5/cli @sap/ux-ui5-tooling @ui5/logger @ui5/fs
```

All these dependencies are available in the default npm registry.
This installs the dependencies under "/usr/local/lib". Hence you need to use a user with write permissions to that folder.

2.) Under "The ABAP Life-Cycle Management" we have to _replace_ at "To upload a file to your transport" ...

* With Docker
```
docker run --env ABAP_USER=<USER> --env ABAP_PASSWORD=<PASSWORD> -v `pwd`:/project -w /project --rm -it fiorideploy fiori deploy --noConfig -t <TRANSPORT_ID> -u <URL> -p <PACKAGE> -n <APP_NAME> -l <CLIENT> -e "<DESCRIPTION>" -f -y --username ABAP_USER --password ABAP_PASSWORD
```

* Without Docker
```
export ABAP_USER=<USER> ABAP_PASSWORD=<PASSWORD>
fiori deploy --noConfig -t <TRANSPORT_ID> -u <URL> -p <PACKAGE> -n <APP_NAME> -l <CLIENT> -e "<DESCRIPTION>" -f -y --username ABAP_USER --password ABAP_PASSWORD
unset ABAP_USER ABAP_PASSWORD
```

* In both cases we need to be in the project root directory.
* We expect the application has been built prior to the deployment.
* The application needs to be contained in a folder named `dist`.
* The part about creating a transport request remains unchanged.

The parameters are
* URL / Endpoint is something like https://example.org:8000 "The endpoint of your service" is not 100% accurate since the "endpoint" would be something like: "https://example.org:8000/sap/opu/odata/ui5/abap_repository_srv". The later part is added by the toolset internally.
* CLIENT The abap client
* DESCRIPTION An arbitrary description for the app. Only taken into account for initial deployments, not for re-deployments.
* PACKAGE The package to that the application belongs to
* APP_NAME: The name of the application
* The other parameters not explicitly mentioned here remains unchanged.

### Additional Remarks:

* Under "Prerequisites": link for cm_client docker file does not work
* Under "Release and Export your transport request": In our test system A5D I'm not able to launch the Transport Organizer WebUI. I click on Environment >> Transport Organizer Web UI, "HE" asks me to choose a System and nothing happens. I can't see any transport requests. But I can see the transport requests via the transport organizer contained in SE80.
* It is extremly odd to have two different tools at the same time in the context of uploading stuff to ABAP. We have the cm_client for creating a transport request and we have the fiori toolset for performing the upload. In the long run it might happen that the fiori toolset also supports creating transport requests and releasing transport requests. But so far that is not supported by the fiori toolset.
