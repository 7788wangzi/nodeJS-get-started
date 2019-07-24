## Use `Node.js` to create a container in Azure Blob

In cloud-shell to create an Node.js app.


Create a new folder.
```
mkdir example-azblob
```

Change into new folder and use `npm` to initialize a new Node.js app, this will create **package.json** file containing metadata that describes the app.

```
cd example-az-blob
npm init -y
```

Create a new source file **index.js**.
```
touch index.js
```

Add the **azure-stroage** and **dotenv** packages, make sure to supply the `--save` option so it persists to **package.json**.
```
npm install azure-storage --save
npm install dotenv --save
```

Create a **.env** file for storaging the connection string.
```
touch .env
```

Type `code .` to open an VS code editor.
