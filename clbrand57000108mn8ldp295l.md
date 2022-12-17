# How to store files in Azure Storage using SAS (Shared Access Signature)

In this post, I want to show you a way to upload files to Azure Storage using an Azure Function that gives you a SAS link that you can use without knowing the connection details to the storage. This example is just to provide you with a basic idea of how to do this, it does not cover every requirement you might have.

We are only creating a link to access a specified file in the storage and with only the `BlobContainerSasPermissions.Create` and `BlobContainerSasPermissions.Write` permissions. The code is slightly different if you wanted to allow access to the Container instead of just a file. The Azure Function is an **HTTP trigger** function.

```csharp
var blobServiceClient = new BlobServiceClient(Environment.GetEnvironmentVariable("StorageAccountKey"));

var blobContainerClient = blobServiceClient.GetBlobContainerClient(Environment.GetEnvironmentVariable("ContainerName"));

if(!(await blobContainerClient.ExistsAsync()))
{
	return new OkObjectResult(null);
}

var blobFile = blobContainerClient.GetBlobClient($"{Path.GetRandomFileName()}.bak");

if(blobFile.CanGenerateSasUri)
{
	var sasBuilder = new BlobSasBuilder(BlobContainerSasPermissions.Create | BlobContainerSasPermissions.Write, DateTimeOffset.UtcNow.AddHours(1))
	{
		BlobContainerName = blobFile.BlobContainerName,
		BlobName = blobFile.Name,
		Resource = "b"
	};

	var sasUri = blobFile.GenerateSasUri(sasBuilder);

	return new OkObjectResult(sasUri.ToString());
}
else
{
	return new OkObjectResult(null);
}
```

Below is a fundamental example of how to call the Azure Function and use the SAS link. Note that in the example we only have to create an `BlobClient` instance and pass the SAS link as a `Uri` type. Then you just call the `Upload or UploadAsync` methods passing the file to upload, in this case, I am passing the path to the file but the method can also receive a `Stream`.

```csharp
using Azure.Storage.Blobs;

var azureFuntionUrl = "azure URL";

var httpClient = new HttpClient();

var blobSASUrl = await httpClient.GetStringAsync(azureFuntionUrl);

var uri = new Uri(blobSASUrl);

var client = new BlobClient(uri);

Console.WriteLine("Starting upload");

var ret = await client.UploadAsync(@"C:\somefile.bak");

Console.WriteLine("Upload Finished");

Console.ReadLine();
```

You can find the complete example on [GitHub](https://github.com/thecodingtips/UploadFilesToAzureStorage).

[Quickstart: Azure Blob Storage client library for .NET](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=environment-variable-windows)

[Create a service SAS for a container or blob](https://docs.microsoft.com/en-us/azure/storage/blobs/sas-service-create?tabs=dotnet)

[Uploading Blobs with the V12 Storage SDK](https://markheath.net/post/upload-blobs-with-v12-sdk)

[Copying Blobs with the V12 Storage SDK](https://markheath.net/post/copying-blobs-with-v12-sdk)