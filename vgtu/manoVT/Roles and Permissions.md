Find user 
```php
$user = App\Models\User::where('username', 'webmaster')->first();
```

Retrieve all permissions

```php
$permissionNames = Spatie\Permission\Models\Permission::all()->pluck('name');
```

Assign all permissions
```php
$user->syncPermissions(Spatie\Permission\Models\Permission::all());
```


Assign role 
```php
$user->assignRole('RoleName');
```

Retrieve role names
```php
$roles = Spatie\Permission\Models\Role::all()->pluck('name');
```

Assignee permissions to the role

```php
$role->givePermissionTo($permissions);
```