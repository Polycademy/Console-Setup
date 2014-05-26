Powershell Setup
================

Launch powershell and run:

```
$profile
```

Create that file specified by $profile.

Add this line to the file:

```
Set-Location $env:USERPROFILE
```

Then run this in powershell:

```
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

You can read more about execution policies here: http://technet.microsoft.com/library/hh847748.aspx and running this: `Get-ExecutionPolicy -List`

Restart Powershell.

Your powershell should now start at your home.