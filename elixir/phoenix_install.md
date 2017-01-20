# Installing Phoenix

Everything went fine when following the instructions here http://www.phoenixframework.org/docs/up-and-running , and the other installation instructions that are linked. The only thing that made trouble were the postgres install. I got the following error when doing `mix ecto.create`:

```bash
** (Mix) The database for HelloPhoenix.Repo couldn't be created: FATAL 28P01 (invalid_password): password authentication failed for user "postgres"

18:14:59.979 [error] GenServer #PID<0.3182.0> terminating
** (Postgrex.Error) FATAL 28P01 (invalid_password): password authentication failed for user "postgres"
    (db_connection) lib/db_connection/connection.ex:148: DBConnection.Connection.connect/2
    (connection) lib/connection.ex:622: Connection.enter_connect/5
    (stdlib) proc_lib.erl:247: :proc_lib.init_p_do_apply/3
Last message: nil
State: Postgrex.Protocol
```

It can be fixed by typing `sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'postgres';"` [0]

[0] http://stackoverflow.com/a/37375810/2156909
