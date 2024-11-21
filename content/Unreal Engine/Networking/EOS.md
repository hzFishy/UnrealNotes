
# Account

> [!info]- Auth Interface VS Connect Interface
> - Auth is for Epic Games accounts
> - Connect supports cross play because instead of using EG account ids it will use a Product User ID (PUID)
> [More]( https://dev.epicgames.com/docs/epic-account-services/auth/auth-interface#differences-between-auth-interface-and-connect-interface)
## Login
Global callstack of a successful call of `Login` on the identity interface.
- `IOnlineIdentity::Login` will call `FUserManagerEOS::Login`
	- will call `CallEOSAuthLogin` (or `ConnectLoginNoEAS`, `LoginViaPersistentAuth`, `LoginViaExternalAuth`)
		- will call `EOS_Auth_Login`, callback at `OnEOSAuthLoginComplete`
			- In callback: (should have Epic Account id (EAID) in `EOS_Auth_LoginCallbackInfo`)
				- if successful will call `ConnectLoginEAS` that calls `EOS_Connect_Login`
					- In callback: (should have PUID in `EOS_Connect_LoginCallbackInfo`)
						- if successful will call `FullLoginCallback` (having both EAID and PUID)
							- calls `AddLocalUser` (with EAID and PUID)
								- calls `FUniqueNetIdEOSRegistry::FindOrAdd` (with EAID and PUID)
								- fills data in the local user struct
								- calls `UpdateUserInfo`
						- if invalid `CreateConnectedLogin`
						- if other error `Logout` wit a Error log
						- will call `TriggerOnLoginCompleteDelegates` as successful

## User Info
Callstack
- `IOnlineUserPtr::GetUserInfo` returns `FOnlineUser`
	- call `FOnlineUser::GetDisplayName`
		- implementation in `TOnlineUserEOS::GetDisplayName`
			- calls `OnlineSubsystemEOSTypesPrivate::GetBestDisplayName`
				- on `FOnlineSubsystemEOS->UserManager` (`FUserManagerEOSPtr`) calls ``