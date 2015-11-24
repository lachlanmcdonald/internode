# internode.py

Python script for retrieving your Internode account usage and outputting as JSON
files.

__Work in progress__.
This script was created by studying [cafuego/internode-php](https://github.com/cafuego/internode-php).
There is no guarantee that it is functional for all account types. Please raise
an issue or pull-request if you encounter problems.

## Setup

Install the required packages with _pip_. _virtualenv_ is highly recommended.

```pip install -r requirements.txt```

Add your username and password to `export.py` by replacing the `USERNAME` and
`PASSWORD` variables with your Internode account details (do not include
`@internode.on.net`), then execute by running `python export.py`.

If you want to write your own script to handle your account information, you can
simply import the `internode` package and interact with the classes directly.

## Output

Each service tied to your account is output as a JSON file, named after your
service ID. i.e. `1234567.json`.

An additional JSON file, `account.json`, is also generated that contains a list
of each of the aforementioned files.

By default, JSON files are output to the `data` directory in the script's
working-directory. If this directory does not exist, the script will attempt to
create it.

## Properties

### `service` properties

These properties are retrived when calling `get_service()` on a `Service` instance. These properties are also output in each service's JSON file by `export.py`.

| Property                 | Description                                                                                                                                                      |
| ------------------------ | -----                                                                                                                                                            |
| `id`                     | Service's Unique ID                                                                                                                                              |
| `carrier`                | The underlying carrier supplying the ADSL service. Often "Internode"                                                                                             |
| `excess-charged`         | Whether the service is charged when usage exceeds the allocated quota.                                                                                           |
| `excess-restrict-access` | Whether the service has its access restricted when usage exceeds the allocated quota.                                                                            |
| `excess-shaped`          | Whether the service has its upload and download speed restricted when usage exceeds the allocated quota.                                                         |
| `plan`                   | Name of the service plan                                                                                                                                         |
| `plan-cost`              | Cost (in AUD) for the service plan                                                                                                                               |
| `plan-interval`          | Frequency in which the service is billed. For example, `Monthly` or `Quarterly`                                                                                  |
| `quota`                  | Quote allocated to this service, in bytes.                                                                                                                       |
| `rollover`               | Start of the next billing period. Also, likely when any quota allocation is reset.                                                                               |
| `speed`                  | Speed of the service                                                                                                                                             |
| `usage-rating`           | Which traffic traffic counts towards quota allocation. `up+down` for when both uploads and downloads consume quota, and `down` for when uploads are not metered. |
| `username`               | Internode username associated with the service                                                                                                                   |

### `usage` properties

A small number of properties from the above table are output by calling `get_usage()` on a `Service` instance. These properties are also output in each service's JSON file by `export.py`.


## Units

All units are output as bytes.

Internode, like many ISPs, uses SI decimal values
— 1,000 bytes per kilobyte (instead of 1,024). To ensure your output matches your understanding of your service, please refer to the table below:

| Unit     | Bytes             |
| -------- | ----------------- |
| Kilobyte | 1,000             |
| Megabyte | 1,000,000         |
| Gigabyte | 1,000,000,000     |
| Terabyte | 1,000,000,000,000 |

## Wishlist

- Add in functionality detailed by the official API specification
- Python 2\3 compatibility
- Write tests (difficult currently since the API does not provide a dummy account for testing)
- A nice static site which can display the JSON output
