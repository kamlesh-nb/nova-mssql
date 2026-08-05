# nova-mssql

Microsoft SQL Server driver for Nova (TDS 7.4, TLS). A Nova package — fetch with:

```sh
nova get https://github.com/kamlesh-nb/nova-mssql
```

```nova
import mssql;
```

## Structure (SOLID)

Split by responsibility; consumers only touch the seam (`MssqlDriver` / `MssqlConnection`).

| Module       | Responsibility |
|--------------|----------------|
| `mssql`      | Seam: `MssqlConnection impl Connection` + `MssqlDriver impl Driver` + connect/prelogin/login7. |
| `connection`     | Connection-string parsing (`ConnectionOptions`, `parse`). |
| `codec`   | TDS wire codec — packet framing/chunking, PRELOGIN/LOGIN7/SQLBatch/RPC builders, token+value decoders, `Cur`. |
| `typemap` | TDS type-code→`DbType`, `DbValue`→SQL/bind text, `substituteParams`. |
| `proto`   | Async transport framing (`TdsReader`, `readMessage`, `sendRaw`). |
| `stmt`    | Prepared-statement cache entry (`MsStmt`). |
| `auth`    | TDS password obfuscation (`obfuscatePassword`). |
