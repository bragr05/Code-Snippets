### Steps

1. Get the hash of the publish file, i.e., `bd.publish`, located in the debug folder after compiling the project.
2. Convert that hash into a `varbinary(64)` using the `SHA2_512` hash algorithm.
3. Then execute the procedure `sys.sp_add_trusted_assembly` with the previously generated hash and a description to identify the trusted DLL.
4. This adds the DLL to the trusted list for the entire server, so it is not configured individually per database.

    ```sql
    declare @assembly varbinary(max) = 0x
    declare @hash varbinary(64) = HASHBYTES('SHA2_512', @assembly);

    exec sys.sp_add_trusted_assembly @hash, N'Assembly System.';
    ```

### Other Useful DLL Functions

-   To see which DLLs have been marked as trusted
    ```sql
    select * from sys.trusted_assemblies
    ```
-   To remove a trusted DLL using its `varbinary(64)` hash
    ```sql
    DECLARE @hash varbinary(64);
    SET @hash = 0x;
    exec sp_drop_trusted_assembly @hash
    ```
