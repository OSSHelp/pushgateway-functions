# pushgateway-functions

[![Build Status](https://drone.osshelp.ru/api/badges/OSSHelp/pushgateway-functions/status.svg)](https://drone.osshelp.ru/OSSHelp/pushgateway-functions)

## About

Library with functions for sending metrics to [Pushgateway](https://gitea.osshelp.ru/ansible/pushgateway).

## Usage

Include it in your script:

``` shell
pf_lib="/usr/local/include/osshelp/pushgateway/pushgateway-functions.sh"

test -r "${pf_lib}" || { echo "Library ${pf_lib} doesn't exist!"; exit 1; }
test -r "${pf_lib}" && . "${pf_lib}" "${@}"
```

Call functions in your script to shape and push metrics to Pushgateway:

| Function | Args needed | Description |
| -------- | -------- | -------- |
| `pushgateway_register_metric` | Metric name, metric type, description (help) | You need to pre-register every metric you want to send. |
| `pushgateway_set_value` | Metric name, metric value, labels (optional) | On every call adds metric to temporary file. Labels must be passed as separate arguments in a format like `name=value name2=value2`. If no custom labels are needed just don't pass any arguments exept name and value. |
| `pushgateway_send_metrics` | none | Sends metrics from temporary file to Pushgateway and deletes the file afterwards. |

### Examples

Sending two metrics with same name, type and description, but different set of labels:

``` shell
pushgateway_register_metric some_test_metric gauge "Just a test metric"
pushgateway_set_value some_test_metric 15 label1=value1 label2=value2
pushgateway_set_value some_test_metric 23 label2=value2 label3=value3
pushgateway_send_metrics
```

Sending metrics at a different time:

``` shell
pushgateway_register_metric script_is_running gauge "If script is running (1 = running)"

pushgateway_set_value script_is_running 1 somelabel=somevalue
pushgateway_send_metrics

<some code here>

pushgateway_set_value script_is_running 0 somelabel=somevalue
pushgateway_send_metrics
```

## Author

OSSHelp Team, see <https://oss.help>
