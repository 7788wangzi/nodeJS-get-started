## Use `Node.js` to create a container in Azure Blob

In cloud-shell, the detailed steps of creating an Node.js application.

1. Create a new folder.
    ```
    mkdir example-azblob
    ```
1. Change into new folder and use `npm` to initialize a new Node.js app, this will create **package.json** file containing metadata that describes the app.
    ```
    cd example-az-blob
    npm init -y
    ```
1. Create a new source file **index.js**.
    ```
    touch index.js
    ```
1. Add the **azure-stroage** and **dotenv** packages, make sure to supply the `--save` option so it persists to **package.json**.
    ```
    npm install azure-storage --save
    npm install dotenv --save
    ```
1. Create a **.env** file for storaging the connection string.
    ```
    touch .env
    ```
1. Type `code .` to open an VS code editor.
1. Get the connection string in command line.
    ```
    az storage account show-connection-string \
      --resource-group Learn-da62a019-2d83-4799-8875-82017d147d10 \
      --query connectionString \
      --name <name>
    ```
1. Paste the connection string to a variable in .env.
    ```
    AZURE_STORAGE_CONNECTION_STRING="<connection string>"
    ``` 
1. Open **index.js**, add following code:
    ```
      require('dotenv').config();
      const util = require('util');
      const storage = require('azure-storage');
      const blobService = storage.createBlobService();
      const containerName ="photoblobs";

      const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);
      async function main() {
          console.log('Start working...');
          try{
              var result = await createContainerAsync(containerName);
              if(result.created)
              {
                  console.log(`Blob container ${containerName} created successfully.`);
              }
              else{
                  console.log(`Blob container ${containerName} already exists.`);
              }
          }
          catch(err){
              console.log(err.message);
          }
      }

      main();
    ```
