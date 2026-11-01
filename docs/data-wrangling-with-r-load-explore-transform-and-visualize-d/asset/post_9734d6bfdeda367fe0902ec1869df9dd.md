
```python
from platform import python_version
print(python_version())
```

3.8.12

```python
from python38_custom import awswrangler as wr1
```

```python
print(wr1.__version__)
print(wr1.__description__)
print(wr1.__message__)
```

2.17.0
Pandas on AWS. Updated for custom installation.
This was changed as a part of Learning exercise

Figure 3.38: awswrangler SageMaker custom installation – checking the installation status

## Installing AWS SDK for pandas for EMR usage

We can install awswrangler on a new EMR cluster by configuring the bootstrap scripts. Create a bootstrap script with the following commands and upload it into an S3 bucket:

```bash
#!/usr/bin/env bash
set -ex
sudo pip3 install pyarrow==2 awswrangler==2.17.0
```

Create an EMR cluster with the needed software for your project, and also enable JupyterHub and Livy if you want to develop scripts interactively using Jupyter notebooks. We will launch a Jupyter notebook that is associated with this cluster. You can verify awswrangler by printing the version name and description, as we did earlier.

```python
import awswrangler as wr
print(wr.__version__)
print(wr.__description__)
```

Last executed at 2022-10-02 21:16:49 in 71ms

2.17.0
Pandas on AWS.

Figure 3.39: awswrangler EMR installation – checking the installation status

You could also verify the awswrangler installation with the sc.list_packages() command to list the packages from the sparkContext object.

```python
[2]: sc.list_packages()
Last executed at 2022-10-02 21:14:26 in 1.31s
```

```
aenum (3.1.11)
aiohttp (3.8.1)
aiosignal (1.2.0)
asn1crypto (1.5.1)
async-timeout (4.0.2)
asynctest (0.13.0)
attrs (22.1.0)
aws-cfn-bootstrap (2.0)
awswrangler (2.17.0)
backoff (2.1.2)
beautifulsoup4 (4.9.3)
```

Figure 3.40: awswrangler EMR installation – checking the installed packages

You could upload the custom `awswrangler` package on an EMR cluster and install it for usage in an EMR cluster. Refer to the EMR documentation (https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-jupyterhub-install-kernels-libs.html) to install additional packages and kernels.

We have spent a considerable amount of time installing `awswrangler` in different environments to enable different personas across your organization to utilize the `awswrangler` library. Also, we showed how to make changes to `awswrangler` and deploy it across all environments to suit your organization’s needs more appropriately. The AWS SDK for pandas library already provides a lot of capabilities, and most customers can use the official library in their projects, so we provided approaches to using it for different personas. If your project requires the customization of the `awswrangler` library for your needs, we also showed different ways to achieve that. Now, we will explore how to set the configuration options to make `awswrangler` more customized for your project's specific needs.

## Configuration options for AWS SDK for pandas

There are options to configure global variables in `awswrangler`, which will help us to make the `awswrangler` library more customized for our specific use cases. We will look in depth at how to set up configurations to customize the working of `awswrangler`.

### Setting up global variables

Global configuration variables can be set up through two different mechanisms in `awswrangler`:

- **Environment variables**: Create environment variables in the operating system/kernel environment where the `awswrangler` package will be executed. For example, in a Jupyter notebook, use the `%env` command to set values for environment variables. This is loaded when the `awswrangler` object is instantiated.
- **Wrangler config variables**: You can also set up values for environment variables using the `wrangler` object directly. Use the `awswrangler.config.<<variable name>>` syntax to assign a value for that configuration.

### How does a config get applied?

Python has an interesting feature called decorators to add functionality to existing code. This is also called metaprogramming because a part of the program tries to modify another part of the program at compile time. The decorator ensures that configurations are applied before the function is called, using the `@decorator_func` syntax within the `awswrangler` source code for different services.

Refer to the following screenshot, where the decorator function is applied before the Athena `read_sql_query` function, which will set the global variables relevant to Athena in the code. The `apply_configs` function is defined in the `config.py` script, which you can read if needed to understand it in greater depth.

```python
@apply_configs
def read_sql_query(
    sql: str,
    database: str,
    ctas_approach: bool = True,
    unload_approach: bool = False,
    unload_parameters: Optional[Dict[str, Any]] = None,
    categories: Optional[List[str]] = None,
    chunksize: Optional[Union[int, bool]] = None,
    s3_output: Optional[str] = None,
    workaround: Optional[str] = None,
```

Figure 3.41: awswrangler – a config.py code snippet

The following categories of variables are available in the `awswrangler` package. However, users can add configuration variables to the code based on specific project needs:

- **Endpoint URL**: Provide an endpoint URL for various services that will be used by `awswrangler` to connect with an AWS environment. To connect programmatically to an AWS service, you use an endpoint, which is the entry point for an AWS web service. The AWS SDKs and the AWS **Command Line Interface (CLI)** automatically use the default endpoint for each service in an AWS Region, but you can specify an alternative endpoint for your API requests using this variable. One example would be to connect to a particular Region or use an FIPS endpoint for enhanced security using TLS connections.
- **botocore configuration variables**: These are configuration parameters for the boto environment. For more details on the parameters that can be passed to a botocore configuration, refer to the boto3 documentation. Some examples of the parameters in a boto3 configuration are the Region name, output format, connection timeout, and retries. Refer to the botocore documentation (https://botocore.amazonaws.com/v1/documentation/api/latest/reference/config.html) for more details on variables that can be configured.
- **General configuration variables**: These are general configurations that are used in various services to affect the way they behave – for example, we can configure a block size parameter to read data from S3.

## Parameters related to Athena cache behavior

**awswrangler** can retrieve the results from Athena queries that have already been executed within a specific time period. We can configure this behavior using configuration variables to return the pre-executed data within *X* seconds, or *X* queries, so that a user can get valid data more quickly. The following variables help to configure Athena cache behavior:

- **MAX_CACHE_SECONDS**: **awswrangler** can check in Athena’s history whether this query has been run before. If so, and its completion time is less than **max_cache_seconds** from the current query, **awswrangler** skips the query execution and just returns the same results as last time.
- **MAX_CACHE_QUERY_INSPECTIONS**: This is the maximum number of queries that will be inspected from the history to try to find some result to reuse. It only takes effect if **max_cache_seconds > 0**.
- **MAX_REMOTE_CACHE_ENTRIES**: This is the maximum number of queries that will be retrieved from AWS for cache inspection. It only takes effect if **max_cache_seconds > 0** and the default value is **50**.
- **MAX_LOCAL_CACHE_ENTRIES**: This is the maximum number of queries for which metadata will be cached locally. This will reduce latency and also enables keeping more than **max_remote_cache_entries** available for the cache. This value should not be smaller than **max_remote_cache_entries**. It only takes effect if **max_cache_seconds > 0** and the default value is **100**.

## Parameters used across different services

The following parameters are used when interacting with Amazon S3 using the **awswrangler** package. They will help to improve the performance of read/write operations:

- **CONCURRENT_PARTITIONING**: If **True**, this will increase the parallelism (using threads) level when writing to partitions. It will decrease the writing time and increase memory usage.
- **S3_BLOCK_SIZE**: This configuration is used to read files from S3, either in specified block sizes or completely in one shot if the value is set as **-1** for this variable.

## Common use cases for configuring

We can configure **awswrangler** so that the same settings are repeated across multiple commands, meaning that we don’t need to type them every time. Also, when we execute programs across multiple environments, global configurations help us to set default values that will customize an **awswrangler** execution specific to that environment.
