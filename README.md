
# Get Latest Version
api(dot)getfiddler(dot)com/linux/latest-linux
EDIT: this fork was done with fiddler v3.4.1 in a windows 10 machine

## Linux
## get ilasm (ildasm)

1. dotnet new console -n test
2. cd test
3. dotnet add package Microsoft.NETCore.ILAsm (ILDAsm)
4. dotnet publish -c Release --self-contained --runtime linux-x64
5. export PATH=$(pwd)/bin/Release/netcoreapp3.1/linux-x64/publish:$PATH
6. ilasm (ildasm)

## Windows
## You need to have installed .net and visual studio to get ildasm (decompiler) and ilasm (compiler)
ildasm => from dll to il
ilasm => from il to dll


## main.xxxx.js
Open `fiddler/resources/app/out/WebServer/ClientApp/dist/main.xxx.js` and search for `updateUserLicense`

Add at the beginning of the function: (replace `Ie` with the parameter name)

````javascript

Ie.licenseInfo.currentLicense = "Pro"
Ie.licenseInfo.hasExpiredTrial = false
Ie.licenseInfo.isTrialAvailable = false
Ie.licenseInfo.hasValidLicense = true

````
##Fiddler.WebUi.il

> modify this file to remove file verification

Do the same for the two functions `TryOpenClientMainScript` and `TryOpenElectronMainScript`

Delete all the code before the following code inside the function

````
IL_0208: /* 17 | */ ldc.i4.1
IL_0209: /* 2A | */ ret


````

##FiddlerBackendSDK.il

### method FiddlerBackendSDK.User.UserClient::GetBestAccount

Delete the if statement corresponding to IL_000d - IL_0020
Removed IL_003f - IL_0040 for `return null;` statement

### method '<>c__DisplayClass18_0'::'<GetBestAccount>b__0'

Delete IL_0000 - IL_0019 , insert `ldc.i4.1` before IL_001e (that is, the function body directly returns `true` )
from
````c#
public AccountDTO GetBestAccount(UserWithBestAccountDTO user)
{
        if (user.BestEverywhereAccountId != null)
        {
                return user.Accounts.FirstOrDefault((UserAccountDTO x) => x.Id == user.BestEverywhereAccountId.Value);
        }

        return null;
}


````
to
````c#

public AccountDTO GetBestAccount(UserWithBestAccountDTO user)
{
        return user.Accounts.FirstOrDefault((UserAccountDTO x) => true);

}

````
## disable updates
Modify `fiddler/resources/app/out/main.js`, search for `e.settingsService.get().autoUpdateSettings.disabled` and replace it with `true||e.settingsService.get().autoUpdateSettings.disabled`




