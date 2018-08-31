# Oracle General

* Access DB
> `sqlplus db_username/db_password@db_host_name:db_port/service_name_or_sid`

* list table belong to current user
> `select table_name from user_tables`

* list table which current user can access
> `select owner, table_name from all_tables`
