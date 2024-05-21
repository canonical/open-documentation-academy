# Distribution

In Landscape, a **distribution** refers to the Ubuntu operating system. There are different ways to refer to the distribution depending on your use case.

There are some Landscape API methods and web portal features that require you to provide the distribution. For example, distributions are important if you want to mirror repositories via the web portal or API. For more information, see an [explanation of repository mirroring](https://ubuntu.com/landscape/docs/explanation-about-repository-mirroring).

## Example API methods that require a distribution
The following is a list of example Landscape API methods that need a distribution:

- **Repositories**: The methods under repositories allow you to manage different repositories. Some of these methods include `CreateDistribution`, and `GetDistributions`. In both cases, you reference the distribution by its name. Read [repository methods for more information](https://ubuntu.com/landscape/docs/api-repositories)

- **Reporting Methods**: These methods allow you to do reporting on selections of computers. They usually take the distribution as a query parameter. For example, here's how to use the `GetCSVComplianceData` method:
    ```shell
    ?action=GetCSVComplianceData&query=distribution:12.04
    ```
    In this case, you reference the distribution by its version number. Read about [reporting API methods](https://ubuntu.com/landscape/docs/api-reporting) for more information.

- **Computers**: The methods under Computers help you retrieve computers and do basic operations such as tagging on them. With these methods, you can reference the distribution with either the version number or the code name. Here's an example of the `AddTagsToComputers` method:
    ```shell
    ?action=AddTagsToComputers&query=distribution:22.04&tags.1=server
    &tags.2=jammy
    ```
    The command above references the distribution with the version number. You can modify it to use the code name if you prefer. Read [Computers](https://ubuntu.com/landscape/docs/api-computers) for more information.