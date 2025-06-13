## Data Objects Privileges 

The data governance model within databricks allows you to programmatically grant, deny and revoke access to certain data objects. 

e.g. ```GRANT <privilege> ON <object> TO <user/group>``` or ```GRANT SELECT ON TABLE <table> TO <user>```

### Data Objects 

CATALOG, SCHEMA, TABLE, VIEW, FUNCTION, ANY FILE can be used to grant privileges on to specific users and groups which have been configured in databricks. 

### Privileges 

SELECT, MODIFY, CREATE, READ_METADATA, USAGE, ALL PRIVILEGES are the privileges which can be added or assigned to objects to specific users and groups. 

However, when granting USAGE, this privilege has no effect as it is required to perform any action on a database object. 

### Roles

1. Databricks Admin: can grant access to all objects in the catalog and the underlying file system. 
2. Catalog owner: can grant access to all objects in the catalog.
3. Database owner: can grant access to all objects in the database.
4. Table owner: can grant access to only the table.

More operations: GRANT, DENY, REVOKE, SHOW GRANTS 

To set permissions on ANY FILE, you cannot do this in the data explorer, users will have to use dbSQL to GRANT privileges ON ANY FILE TO users

## Unity Catalog 

Unity catalog is a centralised data governance solution which works across all workspaces on any cloud. Users can unify governance for all data and AI assets. Files, tables, ML models and dashboards can all be unified.

With the addition of unity catalog, the architecture holds all databricks objects in the unity catalog with workspaces then having access to the unity catalog and so the databricks objects are not split across workspaces.

Unity catalog can be accessed at the account level and has a 3 level namespace. 

It also has one extra privilege than hive metastores which is EXECUTE. 

```SELECT * FROM catalog,schema.table```

The hierarchy from the unity catalog is as follows: UC metastore > catalog > schema/db > table/view/function

On the same level as the catalog: 

1. Storage credentials: helps support auth to the underlying cloud storage and apply to an entire storage container. 
2. External locations: represent the storage directories within a cloud storage container.
3. Shares: Collections of tables shared with one or more recepients.

### Identities 

There are three types of identities or principals: 
1. Users: identified by email address and can have an admin role to perform several admin tasks
2. Service Principal: identified by application ids and can be used to automate tools and apps, they can also have admin rights to carry out admin tasks
3. Groups: groups users and service principals into a single entity, groups can be nested with other groups. 

Databricks identities can exist at the account-level and workspace-level which is called identity federation. In the account console, identities can be assigned to one or more workspaces so eliminates the need to copy identities across workspaces. 

Note: The HIVE metastore can still be accesses when unity catalog is used. 