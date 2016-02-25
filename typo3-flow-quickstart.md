## Flow Quickstart

### Create a new project

Follow the instructions at [http://flowframework.readthedocs.org/en/stable/Quickstart/index.html](http://flowframework.readthedocs.org/en/stable/Quickstart/index.html).

```
composer create-project --dev --keep-vcs typo3/flow-base-distribution Quickstart
cd Quickstart
sudo ./flow core:setfilepermissions esis www-data www-data
```

### Create a VirtualHost

In Apache map [http://quickstart](http://quickstart) to `/path/to/Quickstart/Web` and modify `/etc/hosts` accordingly.

### Create a Hello World package

```
./flow kickstart:package MyCompany.MyPackage
```

In the browser: [http://quickstart/mycompany.mypackage/standard/index](http://quickstart/mycompany.mypackage/standard/index)

Bugfix needed!

Again, in the browser: [http://quickstart/mycompany.mypackage/standard/index](http://quickstart/mycompany.mypackage/standard/index)

### Notes (for a Symfony user)

- this framework isn't supported by Netbeans (expecially Fluid, its template engine); 
- the bundle generator crated a good file structure, but having no CRUD nor automatic tests.
