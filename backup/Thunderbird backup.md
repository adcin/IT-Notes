# Backup

Executed on:     
  
| distro      | Kubuntu 22.04.2 LTS x86_64 |
| ----------- | -------------------------- |
| kernel      | 5.15.0-71-generic          |
| shell       | bash 5.1.16                |
| de          | Plasma 5.24.7              |
| wm          | KWin                       |
| Thunderbird | 102.10.0                   | 

To backup thunderbird you simply need to copy / archive the profile folder. 

</br>

## Find profile location

On my system it's named `3jgkyq3e.default-release` and located at `$HOME/.thunderbird`. With the Flatpak version of Thunderbird it's located at `$HOME/.var/app/org.mozilla.Thunderbird/.thunderbird`

**Thunderbird:** menu → Help → Troubleshooting Information

**Troubleshooting Information:** Segment "Application Basics" → "Profiles" → "about:profiles"

**About Profiles:** Identify your active profile → Root Directory

</br>

## Create an archive of the profile

1. Close thunderbird
2. Create backup (with timestamp):
```shell
tar cfzvp $HOME/backups/thunderbird_profile_$(date +"%Y%m%d-%H%M").tar.gz -C $HOME/.thunderbird/<PROFILE_FOLDER> ./
```
_This will create a backup of the files inside the profile folder, not the folder itself._

</br>

# Restore

1. Close thunderbird
2. Extract the backup:
```shell
tar xfv $HOME/backups/<BACKUP_FILE.tar.gz> -C $HOME/.thunderbird/<PROFILE_FOLDER>
```
3. Open Thunderbird
4. Go to the profile management (menu → Help → Troubleshooting Information → Segment "Application Basics" → "Profiles" → about:profiles)
5. Create new profile
6. While creating the profile select the "choose folder..." option and navigate to your restored profile folder.
7. Now you should be able to launch a new session with the restored profile.

Of course you can simply override the files in the default profile folder. Then you won't need to create a new one.