# Raw Configuration

| Name | Type | Description |
|------|------|-------|
| `DISABLE_INTERNAL_DATA_ACCESS` | Bool | When true the private data table cannot be accessed via hosting rewrite rule (`privateCloudData`) |
| `MAX_WETSAND_APP_SUBDOMAINS` | Number | Maximum *.fireweb.app domains for the project. No limitation applied if value is -1 |
| `MAX_DOMAINS` | Number | Maximum domains including wetsand site subdomains. No limitation applied if value is -1 |
| `EMAIL_BORDER_COLOR` | Hex Color (String) | Color for email border |
| `EMAIL_BUTTON_COLOR` | Hex Color (String) | Background color for the email buttons |
| `ENQUIRY_RECIPIENT` | String | Recipient for enquiries sent using wetsand API |
| `EMAIL_CLIENT_LOGO` | String | Logo for the emails sent on behalf of the project |
| `EMAIL_CLIENT_NAME` | String | Alternative name to the logo for the emails sent on behalf of the project. See `EMAIL_CLIENT_LOGO` |
| `FUNCTIONS_LOGGING_LEVEL` | String | The logging level for function box. Possible values are `silly`, `debug`, `info`, `warn`, `error` (winston) |
| `INVOICING_PAUSED` | Bool | If true, the invoices will not be sent to the client. |
| `INVOICE_RECIPIENT` | String | Recipient for the invoices if applicable |
| `DOMAIN` | String | Identifiying domain for the project. With https. This does not necessarily have to be same as hosting domains. e.g, https://muflihun.com |
| `INVOICE_GST_ENABLED` | Bool | Apply GST for invoice or not |
| `EMAIL_DOMAIN` | String | Domain used by Emails page without protocol e.g, muflihun.com |
| `SIMPLE_DOMAIN` | String | Similar to `Domain` except for without protocol. E.g, muflihun.com |
| `CORS_DOMAINS` | String | CSV domains good for development |
| `ADDRESS_LINE_1` | String | This will be included in invoices |
| `ADDRESS_LINE_2` | String | See `ADDRESS_LINE_1` |
| `HAS_CLOUD_DATA` | Bool | |
| `HAS_CONTENT_EDITOR` | Bool | |
| `HAS_FUNCTIONS` | Bool | |
| `HAS_HOSTING` | Bool | |
| `HAS_MEDIA_MANAGER` | Bool | |
| `INVOICE_COMPANY_NAME` | String | |
| `INVOICE_CONTACT_NAME` | String | |
| `MAX_EMAILS` | Number | Enables email services with maximum of these many emails when value is >0 |
| `INVOICE_DAY` | Number | Preferred day of the month to send the invoice |
| `MAX_MEDIA_SIZE_ALLOWED_MB` | Number | Maximum media size allowed per file |
| `PREFERRED_SSL_GROUP` | Number | Preferred SSL group for hosting domains. Value between 1-364 |
| `PRIVATE_DATA_ACCESS_KEY` | String | Key to be used with header `authorization: private-data-access-key <key>` when querying cloud data with private data table |
| `SANDBOX_SERVICE_SECRET` | String | Function box accesses API server internally using this secret. Some routes require this secret to be presented in header `authorization: firewebkit-security <secret>`<br>List of such routes are:<br> • `/clients/:proj/cloud-data/private/:table` (POST & GET)|
| `SANDBOX_IDLE` | Number | Hibernate function box after these many minutes of inactivity |
| `SANDBOX_CPU_SET` | CSV | See `--cpuset-cpus` option in docker. To test with: docker run --rm -it --cpuset-cpus="0,1" --cpus=1.5 deployable/stress -c 1 |
| `SANDBOX_CPU_LIMIT` | Float | See `--cpus` option in docker. See `SANDBOX_CPU_SET` |
| `SANDBOX_MEMORY_LIMIT` | Number | RAM limit for function box in MB|
| `SANDBOX_PROCESS_LIMIT` | Number |  Number of processes allowed for function box |
| `SANDBOX_LOG_FILE_MAX_SIZE` | Number |  Maximum file size for function box log file (in KiB) |
| `TOTAL_SPACE` | Number | Total available space for this project (not really used at the moment) |
