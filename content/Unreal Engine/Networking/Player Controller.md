
## About the dummy player controller

The `ULocalPlayer` always spawns a local & dummy PC (class is `APlayerController`)
The server will later create the _real_ PC instance and assign and replicate it to the client.

You can find this dummy spawn and details about it at `ULocalPlayer::SpawnPlayActor`.
```c++
// The PlayerController gets replicated from the client though the engine assumes that every Player always has a valid PlayerController so we spawn a dummy one that is going to be replaced later.

// Look at APlayerController::OnActorChannelOpen + UNetConnection::HandleClientPlayer for the code that replaces this fake player controller with the real replicated one from the server 
```

# Getting the player controller

See [this](https://discord.com/channels/187217643009212416/221799385611239424/1332735015993348217) message about the various ways (and what I really does) to get one or more player controllers from the server or client.
