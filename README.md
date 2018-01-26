# nagios-plugin-azure-resource

Nagios plugin to monitor Microsoft Azure resource objects.

## Authors

Mohamed El Morabity <melmorabity -(at)- fedoraproject.org>

## Requirements

The plugin is written in Python 2. It requires the following libraries:

* [pynag](https://pypi.python.org/pypi/pynag)
* [msrestazure](https://pypi.python.org/pypi/msrestazure) >= 0.4.15

## Usage

    check_azure_resource.py -C CLIENT -S SECRET -T TENANT -R RESOURCE -M METRIC [-D DIMENSION -V DIMENSION-VALUE] [-H HOST] [-T TIMEOUT]

### Options

    -h, --help

Show help message and exit

    -C CLIENT, --client=CLIENT

Azure client ID

    -S SECRET, --secret=SECRET

Azure client secret

    -T TENANT, --tenant=TENANT

Azure tenant ID

    -R RESOURCE, --resource=RESOURCE

Azure resource ID (in the following format: `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/<resourceProviderNamespace>/<resourceType>/<resourceName>`)

    -M METRIC, --metric=METRIC

Metric (see https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-supported-metrics for a list of all metrics available for each resource type)

    -D DIMENSION, --dimension=DIMENSION

Metric dimension

    -V DIMENSION-VALUE, --dimension-value=DIMENSION-VALUE

Metric dimension value

    -H HOST, --host=HOST

Alternative Azure Management URL (for Azure national cloud instances like Germany or China)

    -t TIMEOUT, --timeout=TIMEOUT

Connection Timeout

    -c CRITICAL, --critical=CRITICAL

Critical Threshhold

    -w WARNING, --warning=WARNING

Warn Threshhold

## Examples

    $ ./check_azure_resource.py \
        --client=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX \
        --secret=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX \
        --tenant=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX \
        --resource /subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVirtualMachine \
        --metric 'Percentage CPU' \
        --warning 50 --critical 75
    OK: Percentage CPU 3.865 percent | 'Percentage CPU'=3.865%;50;75;;

    $ ./check_azure_resource.py \
        --client=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX \
        --secret=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX \
        --tenant=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX \
        --resource /subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/myDBServer \
        --metric storage_used \
        --dimension DatabaseResourceId \
        --dimension-value /subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/myDBServer/databases/myDB \
        --warning 25000000000 --critical 50000000000
    WARNING: Storage used 36783194112.0 bytes | 'storage_used'=36783194112.0B;25000000000;50000000000;;
