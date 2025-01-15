# Migration Editor Script Documentation

This Zsh script provides functionality to manage database migration files by automatically renumbering them and handling up/down migration files. It supports adding new migrations and reverting existing ones while ensuring the correct sequence of file prefixes.

## Features

1. **Add New Migration Files:**
   - Inserts new migration files (`.up.sql` and `.down.sql`) into the specified directory.
   - Automatically renumbers existing migration files to create space for the new migration.

2. **Revert Migration Files:**
   - Removes both `.up.sql` and `.down.sql` files for a specified migration.
   - Renumbers files following the removed migration to maintain sequential order.

## Usage

```bash
./migration_editor.zsh <folder_path> <new_file_name> [--revert]
```

### Parameters

- `<folder_path>`: The directory containing migration files.
- `<new_file_name>`: The name of the new migration file, starting with a 6-digit prefix.
- `[--revert]`: Optional flag to remove a migration file and renumber subsequent files.

### File Naming Convention

Migration files must follow this naming pattern:

- `000001_description.up.sql` (up migration file)
- `000001_description.down.sql` (down migration file)

### Examples

#### Adding a New Migration

To add a new migration file `000002_add_transactions.sql`:

```bash
./migration_editor.zsh db/migrations 000002_add_transactions.sql
```

**Before:**
```text
000001_create_users.sql
000002_add_column.sql
000003_update_schema.sql
```

**After:**
```text
000001_create_users.sql
000002_add_transactions.up.sql
000002_add_transactions.down.sql
000003_add_column.sql
000004_update_schema.sql
```

#### Reverting a Migration

To remove the migration `000002_add_transactions.sql`:

```bash
./migration_editor.zsh db/migrations 000002_add_transactions.sql --revert
```

**Before:**
```text
000001_create_users.sql
000002_add_transactions.up.sql
000002_add_transactions.down.sql
000003_add_column.sql
000004_update_schema.sql
```

**After:**
```text
000001_create_users.sql
000002_add_column.sql
000003_update_schema.sql
```

## Error Handling

- If the folder does not exist, the script will exit with an error:
  ```
  Error: Folder <folder_path> does not exist!
  ```

- If the new file name does not start with a 6-digit prefix, the script will exit with an error:
  ```
  Error: The new file name must start with a 6-digit prefix!
  ```

## Notes

- The script assumes all migration files in the directory are correctly formatted with a 6-digit prefix.
- It sorts files alphanumerically to determine sequence order.

## License

This script is open-source and available under the MIT License.

