# Ghost + Google Cloud Storage :ghost::cloud:

The original repository **[is here](https://github.com/thombuchi/ghost-google-cloud-storage)** with the contribution of [@mitchellst](https://github.com/mitchellst), thank for solve the "undefined" error with the images.

But that's not the whole solution.

## Instructions :notebook:

For the external storage, you must create an adapter, in this case with Google Cloud Platform.

1. Create the path **content/adapters/storage** with the next command

```bash
mkdir -p content/adapters/storage
```

2. On the previous path install the [package](https://www.npmjs.com/package/@dfloresdev/google-storage-ghost) of this repo:

```bash
### move to path
cd content/adapters/storage

### install the package
npm i --save @dfloresdev/google-storage-ghost
```

3. Copy the files of package to the folder **storage** with the next command:

```bash
cp -avr node_modules/@dfloresdev/google-storage-ghost .
```

4. In root folder of installation of ghost create/modify the file **config.development.json** and add the _storage config_:

```json
"storage": {
    "active": "google-storage-ghost",
    "google-storage-ghost": {
      "projectId": "Your_project_id",
      "key": "key.json",
      "bucket": "Your_bucket_name",
      "assetDomain": "domain-for-bucket.example.com/name_bucket",
      "insecure": false,
      "maxAge": "2678400"
    }
  },
```

Notes:

- For the key path, if it is in the ghost root directory, just use the name of the file. Otherwise use an absolute path.
- The `assetDomain` is an optional config entry, and is only required if you want to use a [custom domain](https://cloud.google.com/storage/docs/hosting-static-website) for your cloud storage bucket. Note that these instructions only allow for http, not https, as the storage servers do not present a custom certificate for your domain. Here is a [list of workarounds](https://cloud.google.com/storage/docs/static-website#https). Pair with the `"insecure": true` option to use bare http URLs.
- The `insecure` config is also optional, and defaults to false. Set to true if you are using a custom asset domain, and do not have https configured.
- The `maxAge` is an optional config entry, and is only required if you want to set the cache-control's max age property. It defaults to 31 days (in seconds). This is desirable if you will not be deleting and re-uploading the same file multiple times, and will reduce your bandwidth usage when paired with a CDN. It can be overridden to set the max age to something else, smaller if you would like cache entries to be revalidated quicker, larger if you would like the cache entries to last for longer than 1 month.

5. Restart the container or Ghost service

```bash
### docker
docker kill container_name
docker start container_name

### local installation
ghost stop
ghost start
```

### That's all folks :green_heart:

Don't forget follow me, anywhere like **@dfloresdev** :wink:
