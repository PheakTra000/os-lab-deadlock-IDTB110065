
### Observation Checkpoint 1
The `df -h | grep loop` output shows that the virtual disk image files are successfully attached as loopback devices (e.g., /dev/loop71 and /dev/loop72) and mounted into the file system.

This proves that the system recognizes the image files as usable storage devices, similar to physical disks, and they are now accessible through their respective mount points.
![[Pasted image 20260323133747.png]]



### Observation Checkpoint 2: Local Deadlock
Both scripts froze indefinitely due to a circular wait condition:

- The `sync_up` script held the **Vault Alpha lock** and was waiting to acquire **Vault Beta**.  
- The `sync_down` script held the **Vault Beta lock** and was waiting to acquire **Vault Alpha**.  

Since neither process could release the lock it held until acquiring the other, both processes were stuck, creating a classic deadlock.  

The `ps aux | grep sync_` command confirms that both scripts were still running but unable to make progress.
![[Pasted image 20260323141801.png]]


### Observation Checkpoint 3: Multiplayer Deadlock

Both Player A and Player B scripts froze simultaneously due to a circular wait across two separate user accounts:

- Player A held **Vault Alpha** and waited to acquire **Player B’s Vault Beta**.  
- Player B held **Vault Beta** and waited to acquire **Player A’s Vault Alpha**.  

This caused a **deadlock**: neither script could proceed or release its lock.  
By simulating two users locking each other’s resources, this demonstrates a distributed denial-of-service scenario where the system becomes unresponsive due to resource contention.

- player A
![[Pasted image 20260323145302.png]]

- player B
![[Pasted image 20260323145947.png]]
