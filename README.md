# Mariadb

## About

This Acorn provides a MariaDB database through the Acorn service interface. You can use this Acorn to create a database for your application during development. This launches a single MariaDB container backed by a persistent volume.

The username and passwords for the `root` and `db_user` are randomly generated and stored in the Acorn's secret. The `db_user` is granted all privileges to the database created at startup. Updates to the `--db-name` and `--username` flags will not be applied to the database after the first run.

## Versioning

The version of this Acorn is the version of the MariaDB database server and the Acorn build number for that version. For example, `10.11.6-0` is MariaDB version `10.11.6` and Acorn build number `0`. If there is a breaking change related to the packaging of the Acorn, the Acorn build number will be incremented like so: `10.11.6-1.0`.

## Usage

The examples folder of this repository contains a sample application that uses this Acorn. The application is a simple Python Flask application that uses the MariaDB database to store and retrieve data.

To use this image, add the following to your Acornfile:

```acorn
...
services: db: image: "ghcr.io/acorn-io/mariadb:v#.#.#-#"
...
```

You can customize the database and username name by adding service args to the definition like so:

```acorn
...
services: db: {
    image: "ghcr.io/acorn-io/mariadb:v#.#.#-#"
    serviceArgs: {
        dbName: "wp"
        username: "wp_user"
    }
}
...
```

To connect these services to your container, you can use the following settings:

```acorn
services: db: image: "ghcr.io/acorn-io/mariadb:v#.#.#-#"

containers: app: {
    image: "nginx"
    env: {
        DB_HOST: "@{service.db.address}"
        DB_PORT: "@{service.db.port.3306}"
        DB_NAME: "@{service.db.data.dbName}"
        DB_ADMIN_USER: "@{service.db.secrets.admin.username}"
        DB_ADMIN_PASS: "@{service.db.secrets.admin.password}"
        DB_USER: "@{service.db.secrets.user.username}"
        DB_PASS: "@{service.db.secrets.user.password}"
    }
}
```

The above environment variables are an example, you need to use the correct environment variables for your application.

## Disclaimer

Disclaimer: You agree all software products on this site, including Acorns or their contents, may contain projects and materials subject to intellectual property restrictions and/or Open-Source license (“Restricted Items”). Restricted Items found anywhere within this Acorn or on Acorn.io are provided “as-is” without warranty of any kind and are subject to their own Open-Source licenses and your compliance with such licenses are solely and exclusively your responsibility. [MariaDB](https://mariadb.org) is licensed under GPL-2.0 License which can be found here [License](https://mariadb.com/kb/en/licensing-faq/) and Acorn.io does not endorse and is not affiliated with MariaDB. By using Acorn.io you agree to our general disclaimer here: <https://www.acorn.io/terms-of-use>.
