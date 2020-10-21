### Updates

Under "Prerequisites" we have to add:

* With Docker: you have to provide a docker file derived from the "node" docker image containing these global dependencies: "@ui5/cli @sap/ux-ui5-tooling @ui5/logger @ui5/fs"

The corresponding docker file (which we do not publish) would look like

```
FROM node
  
USER root
RUN npm install -g @ui5/cli @sap/ux-ui5-tooling @ui5/logger @ui5/fs
USER node
```

* Without Docker: you have installed the node based fiori toolset for uploading content to the abap frontend server. These dependencies can be installed via

```
npm install -g @ui5/cli @sap/ux-ui5-tooling @ui5/logger @ui5/fs
```

This installs the dependencies under "/usr/local/lib". Hence you need to use a user with write permissions to that folder.





### Additional Remarks:

* Under "Prerequisites": link for cm_client docker file does not work
