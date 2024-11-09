
# Array entry being null OnRep
if you add a replicated UObject in an array, OnRep can be called on client, but the entry can still be null (because the pointer itself isnt replicated yet).
When the pointer will finally point to the replicated UObject OnRep will be called again
