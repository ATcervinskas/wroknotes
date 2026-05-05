#### - uninitialized constant CalendarsHelper

```bash
# Try to find in the plugins directory the file containing the inclusion of the calendar module
grep -r ":calendar"
```

```ruby
# Then replace the instruction with a conditional form (include only if the version of redminie is less than 5.1)

# Replace line
helper :calendars

# with this line:
helper :calendars if Redmine::VERSION.to_s < '5.1'
```

#error #uninitialized_constant_CalendarsHelper

