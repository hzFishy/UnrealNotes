> There are 2 versions of OSS, one that's based on `OnlineSubsystem` plugin, and a new plugin that started with UE5 called `OnlineServices`
# Logging
- [Info](https://dev.epicgames.com/docs/epic-online-services/eos-get-started/working-with-the-eos-sdk/eossdkc-sharp-getting-started#logging)
- [Log categoires details](https://dev.epicgames.com/docs/en-US/api-ref/enums/eos-e-log-category)
# Account

> [!info]- Auth Interface VS Connect Interface
> - Auth is for Epic Games accounts
> - Connect supports cross play because instead of using EG account ids it will use a Product User ID (PUID)
> - [More info]( https://dev.epicgames.com/docs/epic-account-services/auth/auth-interface#differences-between-auth-interface-and-connect-interface)
## Login
Global call stack of a successful call of `Login` on the identity interface.
- `IOnlineIdentity::Login` will call `FUserManagerEOS::Login`
	- will call `CallEOSAuthLogin` (or `ConnectLoginNoEAS`, `LoginViaPersistentAuth`, `LoginViaExternalAuth`)
		- will call `EOS_Auth_Login`, callback at `OnEOSAuthLoginComplete`
			- In callback: (should have Epic Account id (EAID) in `EOS_Auth_LoginCallbackInfo`)
				- if successful will call `ConnectLoginEAS` that calls `EOS_Connect_Login`
					- In callback: (should have PUID in `EOS_Connect_LoginCallbackInfo`)
						- if successful:
							- calls `FullLoginCallback` (having both EAID and PUID)
								- calls `AddLocalUser` (with EAID and PUID)
									- calls `FUniqueNetIdEOSRegistry::FindOrAdd` (with EAID and PUID)
									- fills data in the local user struct
									- calls `UpdateUserInfo`
								- will call `TriggerOnLoginCompleteDelegates` as successful
						- if invalid user:
							- calls `CreateConnectedLogin`
						- if other error:
							- calls `Logout` and logs an Error
							- will call `TriggerOnLoginCompleteDelegates` as failed

## User Info

### Get DisplayName

> [!warning]- About Platform parameter
> if not passing platform string getting the display name can work but will provoke the following warnings:<br>
> `LogEOSPresence: FPresenceClient::GetTargetPlatformTypePrivate - Target User Cache Not Found!`<br>
> `LogEOSRTC: FRTCClient::GetTargetPlatformTypePrivate: Unable to find local user ....`

Call stack when getting user display name
- `IOnlineUserPtr::GetUserInfo` returns `FOnlineUser`
	- call `FOnlineUser::GetDisplayName`
		- implementation in `TOnlineUserEOS::GetDisplayName`
			- calls `OnlineSubsystemEOSTypesPrivate::GetBestDisplayName`
				- calls `UserManager->GetBestDisplayName` (`FUserManagerEOS`)
					- calls `GetBestDisplayNameStr` and `EOS_UserInfo_BestDisplayName_Release`




# Lobby
- [Schemas](https://dev.epicgames.com/documentation/en-us/unreal-engine/lobbies-interface-in-unreal-engine#configuration)
# Known issues

- `FEpicGamesPlatform::GetOnlinePlatformType - unable to map None to EOS_OnlinePlatformType` : [Explanation](https://eoshelp.epicgames.com/s/article/Why-is-the-warning-LogEOS-FEpicGamesPlatform-GetOnlinePlatformType-unable-to-map-None-to-EOS-OnlinePlatformType-thrown)

