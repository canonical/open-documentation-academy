# Distribution

In Landscape, a **distribution** refers to the Ubuntu operating system. There are different ways to refer to the distribution depending on your use case.

Distributions are useful for many reasons in Landscape. Many Landscape APIs require you to define a distribution in order to use them. Distributions are also important if you want to perform an action such as [repository mirroring](https://ubuntu.com/landscape/docs/explanation-about-repository-mirroring).

## Landscape APIs That Require a Distribution
The following is a list of some Landscape APIs that need a distribution to function properly:

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